---
layout: post
title: Introduction to Docker
modified:
categories: 
excerpt: Docker is a new open-source project that makes deployment of applications easier.
tags: [docker,devops,golang,apache-server,docker tutorial]
image:
  feature: sample-image-1.jpg
date: 2015-08-23T15:14:25+02:00
---

Docker is a new open-source project that makes deployment of applications easier. The idea of this technology is very simple : "the application runs inside a software container. This container is acting like a mini Operating System". Basics are very similar to Virtual machine : An OS is running independently of host's OS. Networking is working using bridges... 
Docker is using 2 terms : image and container:

* An image is a packaged application that is not running.
* A container is an instance of image. It's a running application.

In addition, Docker have a lot of advantages. let's discover them! 

![Docker](http://blog.docker.com/wp-content/uploads/2013/08/KuDr42X_ITXghJhSInDZekNEF0jLt3NeVxtRye3tqco.png){: .center-image }

#Docker Advantages
* **Lightweight** comparing to normal Virtual machine. You can run many containers in parallel without having memory problems.
* Docker can run on **most operating systems**. Containers running have no dependencies with host's OS. So it simpifies developer's tasks by exporting 1 application for all OS instead of 1 application for each specific operating System.
* **Deployment is easier** using docker. Once your Docker image is built, your applications can be exported without any dependenncies. So that , you could gain a lot of time. 
* Docker have a large community. You can find the answers to all your questions and wonders on : StackOverflow, Github issues ...
* Docker have a public repository where you can find all applications that you want easily, how to configure it and how the image was built. Per example: you could fine Apache server image, or MySQL Server..
* Docker is used today by very well know companies such as : Microsoft (that is a partner of Docker too), eBay, Baidu, Groupon... So that's why you should have the curiousity about Docker and to give a try.

##How to Install Docker

For Ubuntu , run this command in your terminal :
{% highlight bash %}
curl -sSL https://get.docker.com/ | sh
{% endhighlight %}

For other OS , please refer to [docker documentation](https://docs.docker.com/installation/)

####Verify Docker is installed correctly
Run this command in your terminal :
{% highlight bash %}
sudo docker run hello-world
{% endhighlight %}

####Run docker as non-sudo user
Docker is used as root normally. But you can still use docker as non-root user. In order to do that, run this command in your terminal :
{% highlight bash %}
sudo usermod -aG docker $USER
{% endhighlight %}


###But How to use Docker ?

In my next tutorial, we will maipulate docker containers. and I will show you how we connect a PhpMyAdmin to MySQL Server, both working on different docker containers on the same host.