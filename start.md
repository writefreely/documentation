# Getting Started

This guide will walk you through setting up your own WriteFreely instance. It requires technical knowledge and access to a server you control.

Run into problems along the way? Ask for help [on our forum](https://discuss.write.as/c/writefreely/installation).

Don't need to self-host? The Write.as team provides [fully-managed WriteFreely hosting](https://writefreely.org/services/hosting), which also funds our work on the project. You can also join an [existing community](https://writefreely.org/instances).

## Requirements

WriteFreely has minimal requirements to get up and running. You'll only need:

- The ability to run an executable
- About 30 minutes

**Requirements for MySQL-backed installs**

- A MySQL (5.6+) database
- Database server timezone set to UTC

## Set Up

First, [download the latest release](https://github.com/writefreely/writefreely/releases/latest) for your operating system. If your OS and system architecture are not listed, you'll need to [build from source](https://writefreely.org/docs/latest/developer/setup), instead.

> If you plan to use **MySQL** for your database, separately connect to your MySQL server now and run:
>
> ```
> CREATE DATABASE writefreely CHARACTER SET latin1 COLLATE latin1_swedish_ci;
> ```

Extract the files and go into the project folder. Next, start the configuration process by running the following command. This will walk you through the various ways you can configure your instance:

```
writefreely config start
```

Generate the encryption keys for your instance by running:

```
writefreely keys generate
```

And finally, run:

```
writefreely
```

🎉

## Running in Production

To run your instance publicly, you can either run WriteFreely as a standalone server or put it behind a reverse proxy. ([Learn the difference here](https://discuss.write.as/t/difference-between-standalone-and-reverse-proxy-install/663).)

### Standalone server

To configure the application to run as a standalone server, run:

```
writefreely config start
```

...and choose **Production, standalone** for your _Environment_.

Next, choose whether the site will be **Insecure** or **Secure**. If you choose **Secure**, you'll need to enter the absolute paths to your Certificate and Key. With those configured, WriteFreely will serve traffic on port 80 and 443, redirecting all insecure traffic to your https:// address.

### Behind a reverse proxy

Here's an example [nginx](https://www.nginx.com/) configuration. It compresses certain files, allows federation endpoints on `/.well-known/` to co-exist with other uses (like Let's Encrypt), and serves static files with nginx instead of the application. Values you should change to match your own configuration are in **bold**:

```
server {
    listen 80;
    listen [::]:80;

    server_name example.com;

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
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
    }

    location ~ ^/(css|img|js|fonts)/ {
        root /var/www/example.com/static;
        # Optionally cache these files in the browser:
        # expires 12M;
    }

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
    }
}
```

At this point, it'd be a great time to [get a certificate](https://certbot.eff.org/) from Let's Encrypt for your instance.

### Starting the service

Lastly, you'll want to set the application up so it continues running. Here's how to do it on Ubuntu.

**Ubuntu 14.10 or earlier**. You can create an Upstart service by creating a file at `/etc/init/writefreely.conf`:

```
description "WriteFreely Instance"
author "Write.as "

start on runlevel [2345]
stop on runlevel [016]
respawn

chdir /var/www/example.com
exec /var/www/example.com/writefreely >> /var/log/writefreely.log 2>&1
```

Then start the service and you're live!

```
sudo start writefreely
```

Verify this by following the application log with:

```
tail -F /var/log/writefreely.log
```

**Ubuntu 15.04 or later**. You can create a Systemd service by creating a file at `/etc/systemd/system/writefreely.service`:

```
[Unit]
Description=WriteFreely Instance
After=syslog.target network.target
# If MySQL is running on the same machine, uncomment the following 
# line to use it, instead. 
#After=syslog.target network.target mysql.service

[Service]
Type=simple
StandardOutput=syslog
StandardError=syslog
WorkingDirectory=/var/www/example.com
ExecStart=/var/www/example.com/writefreely
Restart=always

[Install]
WantedBy=multi-user.target
```

Then start the service and you're live!

```
sudo systemctl start writefreely
```

Verify this by following the application log with:

```
sudo journalctl -f -u writefreely
```