---
title: Configuring Node js on Jenkins using docker
modified:
categories:
comments: true
description: "Installing jenkins from scratch + configuring Nodejs"
tags: [jenkins, node, nodejs, npmrc, docker, jenkins_nodejs]
date: 2018-08-22
---

Jenkins is a devops tool for continuous intergration.

## Install jenkins
Requirements for this tutorial :
- Docker
- Docker compose

We will user docker compose to configure jenkins docker container:

```yaml
version: '3.5'

services:
  jenkins:
    image: 'jenkins/jenkins:lts'
    container_name: 'jenkins'
    restart: always
    ports:
      - '8080:8080'
      - '50000:50000'
    volumes:
      - jenkins-data:/etc/gitlab
      - /var/run/docker.sock:/var/run/docker.sock

volumes:
  jenkins-data:
    name: jenkins-data

```

Then, run the command :
```sh
$ docker-compose up -d
```

## Run Jenkins
Once you had run the docker compose file, test that your jenkins is running by opening this url on your browser [http://localhost:8080](http://localhost:8080)

## Install NodeJS Plugin

When jenkins is ready, go to "Manage Jenkins" => "Manage Plugins"  and Install NodeJs Plugin and NPM Plugin.

Later, go to "Manage Jenkins" => "Global Tool configuration", go down to NodeJS and choose the version you want to use.
**Dont forget to check the "automatic installation" checkbox.**




**Important**: At this point, node and npm are not installed yet.
In order to install them:
- create a "Freestyle project"
- In the project configuration page: Check the box "Provide Node & npm bin/ folder to PATH", and choose the version of node you want.
- In the project configuration page: In the "Build" part, click "a shell script"  and add the command "node -v"
- Run the project

If node is installed, the jenkins job will pass and you will see in the console output: "vXX.XX.XX"

## Configuring NPMRC Optional

You can add your npm settings from jenkins.
Go to "Manage Jenkins" => "Managed Files" => choose npmrc and set your configuration

## Set A Variable environment

Previously, Nodejs have been installed. But, the installation was not global.
In order to make nodejs accessible for all jobs, we should add an Environment Variable for the job runner.
Go to "Manage Jenkins" => "Manage Nodes" => For all the nodes, add this environment :
```sh
PATH=$PATH:/var/jenkins_home/tools/jenkins.plugins.nodejs.tools.NodeJSInstallation/node/bin
```
## Test your own code

Now that you have configured nodejs, create a new project with a jenkinsfile and run it.
