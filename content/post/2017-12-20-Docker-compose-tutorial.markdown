---
title: Docker Compose Tutorial
modified:
categories:
comments: true
tags: [docker,compose,tutorial,mysql,phpmyadmin,devops]
description: docker compose tutorial phpmyadmin mysql
date: 2017-12-20
---

If you are tired of configuring your docker containers every time you want to run it, docker compose is the solution. Docker compose is a tool that will read a configuration file, and will translate it to docker command easily.

If you like this article, leave a comment and share it with your friends :)

#### Install Docker compose

```sh
$ sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose

# Test the installation
$ docker-compose --version
docker-compose version 1.18.0, build 1719ceb
```

## Mysql PhpMyadmin example
In a [previous post](http://omarghader.github.io/docker-tutorial-phpmyadmin-and-mysql-server/), we have seen how to connect phpmyadmin to mysql server container.
Let's create the same with docker-compose. In order to do that, create the configuration file `docker-compose.yml`.
```yaml
# docker-compose.yml file

version: '3' ### version of docker compose
services:
  mysql:
    image: mysql
    environment:
      - MYSQL_ROOT_PASSWORD=0000

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    links:
      - mysql:db  ## PhpMyadmin needs the hostname "db" to connect to Mysql
    ports:
      - 8080:80 ## Map Port: host 8080 => container 80
```

If you want to run the script, `be sure to be in the same directory as your docker-compose.yml file`.
```sh
$ docker-compose up -d
```
Arguments:
 >  -d : run the containers in background

Now open your browser and go to [http://localhost:8080](http://localhost:8080) and login with `username: root , password: 0000` . Voilà your containers work fine.

#### Stopping the containers
If you want to stop the containers:
```sh
$ docker-compose down
```
