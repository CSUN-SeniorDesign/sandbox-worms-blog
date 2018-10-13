---
title: "Mark's Blog Week 7"
date: 2018-10-12T22:59:01
draft: false
Categories: [Project 3]
Tags: []
Author: "Mark Siegmund"
---

Marks Blog Week 7								October 12, 2018

Working on Project 3 this week.
Working on Docker this week.
I started by building a docker container using an Alpine.  First downloading an alpine image with the pull command or run command.  The easy way is to type
docker run alpine -it /bin/sh
This gives an root prompt on an alpine docker image. 
Next install open RC to start services.  
The command for this is apk add openrc --no-cache
Then install apace
apk add apache2 
Then start apache
rc-service apache2 start
The server is now started.  Only thing I cant test is the browser as my host machine for docker his a non GUI system.  That is next.

The way to exit the container and leave it running is with CTRL-P, and CRTL-Q
This puts you back on the host system with the container still running.
Any other way you exit the system is with the docker container being terminated.  A container must have an active process or it dies.

The next step is to find out about multistage builds.
The idea here is
With multi-stage builds you use multiple FROM statements in your Dockerfile.  Each FROM instruction can use a different base, and each of them begins a new stage of the build.


Also started learned about AWS EC2 containers.   