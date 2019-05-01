---
title: Docker Compose Nginx Tutorial
modified:
categories:
comments: true
excerpt: "Docker compose nginx configuration tutorial"
description: Docker compose nginx configuration tutorial
tags: [docker compose nginx config, docker nginx proxy, docker compose tutorial, docker compose nginx reverse proxy]
image: img/nginx_logo.png
date: 2019-05-01
---

## 1. What is nginx?
Nginx is a reverse proxy, HTTP cache and load balancer. It aims to enhance performance by using less memory. Nginx is event-driven. That measns it uses one thread to receive requests, then it dispatch it to one or multiple threads asynchronously.

In this post, you will learn how to deploy a load balancer using nginx in docker.

## 2. Requirements
* Docker
* Docker compose

## 3. How it works
Supposing you have 2 servers `server1` and `server2` that expose a web service. You want to dispatch the load on these 2 server. Using nginx, 50% of the requests will be handled by the server1 and 50% by server2. To do that you need to configure nginx.

```
request -------> nginx ----------> server1
                   |    
                   |
                   |-------------> server2
```

## 4. Configuration

Nginx has a specific configuration syntax. You can check [nginx official documentation](https://nginx.org/en/docs/) for more details.

Create the configuration file `nginx.conf`.
```
events { worker_connections 1024; }

http {

    upstream app_servers {    # Create an upstream for the web servers
        server server1:80;    # the first server
        server server2:80;    # the second server
    }

    server {
        listen 80;

        location / {
            proxy_pass         http://app_servers;  # load balance the traffic
        }
    }
}

```

## 5. Preparing servers pages
We will create a minimal page for each server to test that load balancer works. To do that, create the file `server1.html`.
```
<html><body>server1</body></html>
```

Then, create the file `server2.html`:
```
<html><body>server2</body></html>
```

## 6. Docker compose file
Create the `docker-compose.yml` file in the same folder where you have create nginx.conf, server1.html and server2.html.

```
# docker-compose.yml file
version: '3'

services:
  # The load balancer
  nginx:
    image: nginx:1.16.0-alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    ports:
      - "8080:80"

  # The web server1
  server1:
    image: nginx:1.16.0-alpine
    volumes:
      - ./server1.html:/usr/share/nginx/html/index.html

  # The web server2
  server2:
    image: nginx:1.16.0-alpine
    volumes:
      - ./server2.html:/usr/share/nginx/html/index.html
```

## 7. Test
Run the docker compose stack defined above with the command `docker-compose up -d`.

In your browser, go to [http://localhost:8080](http://localhost:8080) and refresh the page multiple times.
You should see `server1`, then `server2`, then `server1`...

Congratulations, you have configured your nginx as a load balancer.
