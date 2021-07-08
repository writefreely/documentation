# Varnish Cache

WriteFreely includes [Cache-Control](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) headers on a few important [ActivityPub](https://activitypub.rocks/) endpoints, these do nothing on their own but with a Cache like [Varnish](https://www.varnish-software.com) they reduce the load on your backend services, in particular database requests.

> This document describes a basic configuration for using Varnish with WriteFreely.

## Security

Varnish only supports HTTP, no SSL. So if you want your site served securely over HTTPS you need to put something in front of Varnish, like [nginx](https://www.nginx.com/).

You could also [use Apache2](https://www.varnish-software.com/wiki/content/tutorials/varnish/varnish_ubuntu.html#step-3-configure-apache2-to-work-with-varnish), but this guide will demonstrate nginx.

Follow the [configuration and installation guide](https://www.varnish-software.com/wiki/content/tutorials/varnish/varnish_ubuntu.html) for Varnish. You will need to run Varnish on a port other than `80`, nginx will handle those requests. This guide uses `8888`.

Modify the port in these files:
- `/etc/default/varnish`
```
DAEMON_OPTS="-a :8888 \
             -T localhost:6082 \
             -f /etc/varnish/default.vcl \
             -S /etc/varnish/secret \
             -s malloc,256m"
```
- `/etc/systemd/system/varnish.service`
```
ExecStart=/usr/sbin/varnishd -j unix,user=vcache -F -a :8888 -T localhost:6082 -f /etc/varnish/default.vcl -S /etc/varnish/secret -s malloc,256m
```

### nginx

nginx site configuration is the same as with [writefreely.org/start](https://writefreely.org/start) with the exception of the `location /` block for SSL:

```
server {
      listen 443 ssl; 
      listen [::]:443 ssl; 
...

location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP  $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header X-Forwarded-Port 443;
        proxy_pass http://0.0.0.0:8888;
        proxy_redirect off;
}

...
```

The `proxy_pass` directive should point at your Varnish service. You can double check what IP/port it is running on with `netstat -plntu` on linux.

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:8888            0.0.0.0:*               LISTEN      14867/varnishd      
tcp        0      0 127.0.0.1:6082          0.0.0.0:*               LISTEN      14867/varnishd      
tcp6       0      0 :::8888                 :::*                    LISTEN      14867/varnishd
```

The service on `tcp:6082` is the localhost only port.

## /etc/varnish/default.vcl

The following is a modified version of the default vcl configuration.

```
vcl 4.0;

import std;

# the default backend points at the content server
# point this at the IP and port of your WriteFreely service
backend default {
    .host = "127.0.0.1";
    .port = "8084";
}

sub vcl_recv {
  # this check passes request for anything OTHER than activity+json straight to the backend
  if (! req.http.Accept ~ "application\/activity\+json") {
    return(pass);
  }
  # we don't want to take requests from external IPs or port 80 directly with Varnish
  # the hostname in the below regex should match YOUR instance domain
  if ((client.ip != "127.0.0.1" && std.port(server.ip) == 80) &&
      (req.http.host ~ "^(?i)(www\.)?write.as")) {
    set req.http.x-redir = "https://" + req.http.host + req.url;
    return (synth(750, ""));
  }
}

sub vcl_synth {
  if (resp.status == 750) {
    // Redirect to HTTPS with 301 status.
    set resp.status = 301;
    set resp.http.Location = req.http.x-redir;
    return(deliver);
  }
}

sub vcl_deliver { 
    # Happens when we have all the pieces we need, and are about to send the 
    # response to the client. 
    # 
    # You can do accounting or modifying the final object here. 
}
```

## Notes

The current `Cache-Control` header in WriteFreely is set to a `60s` TTL. This may change or become configurable in the future.

The endpoints that provide caching are:

- Collections fetched via canonical URL with `activity+json` header (e.g. /matt/)
- Collections fetched via API (e.g. /api/collections/matt)
- Post fetched via canonical URL with `activity+json` header (e.g. /matt/my-post)
- Post fetched via API (e.g. /api/collections/matt/posts/123someid456)
