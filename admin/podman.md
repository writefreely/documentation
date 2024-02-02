{:toc}

# Podman

## Using Podman for Production

### Assumptions and Name Scheme
#### Assumptions
For production, this documentation is assuming the following:
* Nginx is the reverse proxy.
* Certbot is used for HTTPS.
* The reverse proxy has its own pod and network that writefreely's pod will need to connect to.
* SELinux is enabled.
* Podman's DNS plugin is installed (often as dependency).

#### Name Scheme
##### Container/Pod Names
* pod-writefreely: Writefreely pod with the writefreely service.
* writefreely: The container running writefreely's code, corrisponding to the docker.io/writeas/writefreely image.
* writefreely-db: The container running mariadb/mysql, corrisponding to the docker.io/library/mariadb image.

##### Network Names
* net-proxy: The CNI network connecting writefreely-pod and proxy-pod.

##### Volume/Mounted File Names
* writefreely-keys: The volume with Writefreely's web keys.
* writefreely-db: The volume for Writefreely's database, which is also where user data is stored.
* writefreely-config.ini: The configuration file for Writefreely, corrisponding to `config.ini` within the container.
* site-confs: The volume holding imported nginx configuration files, such as `/etc/nginx/conf.d/`.

### Creating the Writefreely Pod
#### Pod
First create the pod and make sure it is connected to the netowrk with the pod you are using as your proxy.
```
podman pod create \
	--network proxy-net \
	--name pod-writefreely
```
As stated in "Assumptions", the proxy pod will be directing traffic, so there is no need to set a port on this pod.

#### Database Container
Your database command will likely be setup differently, but here is a template to work with:
```
podman run \
	--name "writefreely-db" \
	--pod "pod-writefreely" \
	--volume "writefreely-db:/var/lib/mysql/writefreely:Z" \
	--env MYSQL_DATABASE="writefreely" \
	--env MYSQL_ROOT_PASSWORD="$WRITEFREELY_MYSQL_ROOT_PASSWORD" \
	--env MYSQL_USER="writefreely" \
	--env MYSQL_PASSWORD="$WRITEFREELY_MYSQL_PASSWORD" \
	--label "io.containers.autoupdate=image" \
	--detach \
	--restart=always \
	docker.io/library/mariadb:latest
```
`--volume "writefreely-db:/var/lib/mysql/writefreely:Z"` assures it the volume is not shared with other containers.  
If you need sharing, make the `:Z` lowercase `:z`.  

If `podman-auto-update.service` is enabled, `--label "io.containers.autoupdate=image"` will check for and update the container any time a new image is available.   
If this is a problem, remove the label line.  

The database will need a few seconds to startup, so make sure it has finished initial setup, such as with `podman logs -f writefreely-db` before continuing.  

#### Writefreely Container
##### Making the writefreely-config.ini
Remember: The hostname for accessing writefreely-db and writefreely will be `pod-writefreely`, since the pod handles the networking for the containers within.
So when prompted, use `pod-writefreely` as the hostname behind the proxy.  
If you need traffic to reach a container directly for some reason, `<container-name>.dns.podman` might work.

One needs to make the file for the host to share with the container:
```
touch writefreely-config.ini
```
Next, users can either run the interactive creation utility (`--config`) or simply have writefreely make a template for you (`--create-config`).  

```
podman run \
	--name "writefreely" \
	--pod "pod-writefreely" \
	--mount type=bind,source="$HOME"/.local/share/containers/storage/volumes/writefreely-config.ini,destination=/go/config.ini,rw,relabel=private \
	--rm \
	-it \
	docker.io/writeas/writefreely:latest --config
```
In `--mount`, keep in mind that `rw` is changed to `ro` in further instructions and `relabel=private` is necessary for SELinux labeling but can optionally be changed to `relabel=shared`. 

Alternatively, one may wish to create the config file on a test machine, and push the file to the production server.

##### Initializing the Database
If you make `writefreely-config.ini` from `--create-config` or from another machine, you will need to initialize the database.
```
podman run \
	--name "writefreely" \
	--pod "pod-writefreely" \
	--mount type=bind,source="$HOME"/.local/share/containers/storage/volumes/writefreely-config.ini,destination=/go/config.ini,ro,relabel=private \
	--rm \
	-it \
	docker.io/writeas/writefreely:latest db init
```

RUNNING THIS TWICE MAY RESULT IN DATABASE PROBLEMS... for now.  
If you did, just stop `writefreely-db`, remove the `writefreely-db` volume `podman volume rm writefreely-db`, and go through the Database Container instructions again.

##### The Service Container
Lastly, the actual container needs to be avilable to provide the web app.
```
podman run \
	--name "writefreely" \
	--pod "pod-writefreely" \
	--volume "writefreely-keys:/go/keys:Z" \
	--mount type=bind,source="$HOME"/.local/share/containers/storage/volumes/writefreely-config.ini,destination=/go/config.ini,ro,relabel=private \
	--label "io.containers.autoupdate=image" \
	--rm \
	--detach \
	docker.io/writeas/writefreely:latest
```
The server will check if the web keys are created on boot.
If keys are already present, key creation will be skipped.

This container will exit durring startup if the database is not active and able to connect.

### Nginx HTTPS Reverse Proxy Config
As usual, this is a template, change to your own needs.  
Unlike on the main server installation page, the nginx container and the writefreely container do not share a static files directory, a `proxy_pass` is added to this config, too.

```
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    server_name <YOURDOMAIN>;

    ssl_certificate     /etc/letsencrypt/live/<YOURDOMAIN>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<YOURDOMAIN>/privkey.pem;
    # verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /etc/letsencrypt/live/<YOURDOMAIN>/chain.pem;

    gzip on;
    gzip_types
      application/javascript
      application/x-javascript
      application/json
      application/rss+xml
      application/xml
      image/svg+xml
      image/x-icon
      application/vnd.ms-fontobject
      application/font-sfnt
      text/css
      text/plain;
    gzip_min_length 256;
    gzip_comp_level 5;
    gzip_http_version 1.1;
    gzip_vary on;

    location ~ ^/.well-known/(webfinger|nodeinfo|host-meta) {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://pod-writefreely:8080;
        proxy_redirect off;
    }

    location ~ ^/(css|img|js|fonts)/ {
    
        # Next line may be unecessary for this setup
        root /var/www/blog.cloudart.moe/static; 
        
        proxy_pass http://pod-writefreely:8080;
        # Optionally cache these files in the browser:
        # expires 12M;
    }

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://pod-writefreely:8080;
        proxy_redirect off;
    }
}
```
