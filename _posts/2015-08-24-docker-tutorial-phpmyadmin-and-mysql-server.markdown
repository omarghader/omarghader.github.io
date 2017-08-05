---
layout: post
title: "Docker Tutorial : PhpMyAdmin and MySQL Server"
modified:
categories:
excerpt: connect PhpMyAdmin container  to MySQL server container
comments: true
tags: [tutorial docker mysql,tutorial,docker, devops,docker linking containers]
image:
  feature:
date: 2015-08-24T23:57:49+02:00
---

If you are new to Docker or you are wondering why docker is very popular today, I recommend you to read my previous posts [Introduction to Docker]({{site.url}}).

## Pull Containers from Docker registry

#### Searching for containers
Docker have a public registry where you could find images of dockerized application that are ready to use. All information and indications for using container are specified too.

In this post, I will show you how to connect **PhpMyAdmin** container  to **MySQL server** container. First of all, we must pull the images from repository. In order to find the image that you want, you can :

* Go to **[Docker Hub](http://hub.docker.com)** and search for the corresponding image
* Using docker command-line to search : docker search **image**

I recommend you to look at Docker Hub website for the first time. Then, try to understand how the image work by reading author instructions. Now we will pull 2 containers that i chose from Docker hub.

{% highlight bash %}
#  Pull containers
$ docker pull mysql/mysql
$ docker pull phpmyadmin/phpmyadmin

# Verify that images are pulled correctly
$ docker images
{% endhighlight %}

### Running MySQL Server container

After downloading the image, we must configure and run the container. You'll notice that a lot of containers is configured by pre-defining ENVIRONNEMENT VARIABLES before running the application. For the first time, i agree that it's not easy to manipulate containers and to understand how it runs. But later, i think that docker will be your best friend ^^.

Docker command line have many options
{% highlight bash %}
$ docker run --name name-of-container -e YOUR_ENV_VARIABLE=var1 -p host-port:container-port name-of-image (command)
{% endhighlight %}

* -e : to indicate the declaration of Environnemnt variable inside the container
* -d : run the container in background
* -p : bind container port to host port
* --name : to name the container generated
* --link : link 2 containers together.
* command : optionnally we can add a command to be executed.

Now, let's run mysql container now :

{% highlight bash %}
$ docker run --name mysql -e MYSQL_ROOT_PASSWORD=0000 -d mysql
{% endhighlight %}

### Running PhpMyAdmin container
Phpmyadmin must point to MySQL Server. So that we must link both containers by adding the option : --link name-of-container:name-of-imag.
{% highlight bash %}
$ docker run --name myadmin -d --link mysql:db -p 8080:80 phpmyadmin/phpmyadmin
{% endhighlight %}

## Testing connectivity

Now go to your browser , tap [http://localhost:8080](http://localhost:8080) and login with root/0000 .
Voilà your containers work fine.
