---
title: "Containing Week7"
date: 2018-10-12T17:09:06-07:00
draft: false

categories: [Project 3]
tags: [docker, container]
author: "Aubrey Nigoza"
---
Key Terms and Concepts:

**A. Docker concepts** 

On the previous projects, we push the codes (html files) to our webservers. which in turn delivers our new posts or updates. In project 3, on every update, we will push the entire system, OS, application and code. We can achieve this by leveraging CONTAINERS. 
The docker term represents many layers of docker. Docker can refer to the client command, the docker engine that runs on Windows, Linux or MAC, the docker container and the docker company. The company itself is called docker.
A Container is like a virtual machine but instead of getting a piece of the hardware resource (like memory, cpu, disk), a container gets a piece of the Operating System resource. In this way, a container has just enough of what it needs to run. Just enough here is really just enough. A linux based container will not even have vim or any utilities that it does not need. Imagine a really really really small version of linux. 
We will write a docker file to create an image of the container.  
We can save the image of the container in Amazon ECR (per project 3 specs)
A container can run on a windows, linux or mac machine (phyiscal or virtual machine). 
A container is supposed to be portable and can be shipped to any machine that is running docker engine. Let's say you build an image of a container, you should be able to run it on WIndows, Linux or MAC docker engine. Because the container has all the things that it needs to run itself, it is very portable and will work cross platform.

**B. Amazon ECS**  
ECS is the service by Amazon to host docker containers. ECS is an alternative to DOCKER SWARM. You might come across this concept as you research Docker. We are NOT using Docker Swarm. Docker Swarm is the native container clustering technology provided by Docker. We will be using AWS ECS which is the container clustering service provided by Amazon.

(Assumption) Google Cloud Kubernetes is another example of container clustering service.
It is not a managed service where you can just bring up containers.
Refer to point A.6. - ECS will allow us to create a EC2 instance(s) that will host our containers. 
(Assumption) - I think that we will need to put our EC2 instances in multiple availability zone in order to have our container be highly available.
There are many different layers in an ECS cluster.   

- EC2 instance(s) 
- Tasks
- Service

EC2 instance(s) are the machine that will run our containers.  
Tasks is the construct that will define which image we want to run. It will tell the cluster where to get the image. In our case, it should point to our ECR repo and to the appropriate image. We will have 1 for production and 1 for staging.  
The tasks will pull image based on tags of the image, production or staging.  
Service will tell the cluster how many of the tasks we want to run. I believe this is where we specify the desired state and maximum state. Let;s say, we our desired state is 2 containers, and our max is 10.   

(Assumption) Service- if we have multiple EC2 instances, the cluster should automatically load balance and autoscale the creation of the containers between the EC2 instances. We might need to setup a policy for this.  

(Assumption) Service - should also automatically register the new containers it creates to the target group of the ALB. 

Everytime we have a new post, we will deploy a new image to the ECR.
We need to do a rolling update so that not all the containers will be updated at the same time. This should keep our website uptime high.

**C. CircleCI**  
CircleCi will handle getting the code from the Github repository

Build our image from the docker file. 
Test our image
(Extra Credit) Do unit testing
Tag the image with either production or staging.
Deploy the image to the ECR repo

**D. Lambda**  
Perform any function that needs to be done manually so that developers only need to push code to the repo and it should show up in the website.

(Assumption) Run a code to change the tasks or refresh the task in the ECS cluster when an event is triggered in ECR.
This means the ECR must be setup to generate an event when a new image has been posted.
