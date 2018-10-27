---
title: "Nick's Blog 8"
date: 2018-10-19T16:42:15-07:00
draft: false
Categories: [Project 3]
Tags: []
Author: "Nick Yoon"
---
This week, we were asked to turn our attention towards trying to get Docker running on our VMs. I was able to get it installed, but couldn't get a webserver spun up properly. I still need to divulge in the Docker material some more, along with CircleCi and how they work with one another. Since the presentation needed to be finished by Thursday, I adjusted my focus to trying to get the slides done. I was able to find several diagrams online to help explain how our infrastructure is designed and details of the workflow. Searching for relative material in regards to our infrastructure was actually quite helpful because I first needed to grasp a good idea of how our infrastructure works in order to find the proper diagrams for our workflow. 

As I got deeper into looking for relative material, I started to get a better idea of how all of the different systems work with one another - CircleCi uses the Dockerfile to create an image and pushes the image to both the ECR and S3. The S3 will then triggers an event and notifies Lambda. Lambda will then grab the image from the ECR and spin up an instance that'll live within the ECS Cluster. I believe that's the general idea of how CircleCi, Docker, ECR, S3, Lambda, and ECS work together. Once again, I tried to help our group leader, John, with the ECR and ECS Cluster material, but I found that everything had already been created before I was able to finish. Now that we'll be able to take a brief hiatus from the Projects, I'll focus on trying to get caught up as much as possible starting from the basics.

Throughout the next few weeks, I'll be focusing on trying to get project 0 finished on my own. I plan on purchasing my own domain name using namecheap and will be using my own AWS account outside of my group's organization. Once I'm able to knock that project out, I'll turn my attention to getting my VM setup properly with a Linux distro, setup Apache webserver, and have a cronjob setup to do something basic. 


Websites:
	ECS - 
	http://blog.shippable.com/setup-a-container-cluster-on-aws-with-terraform-part-2-provision-a-cluster
	https://www.lynda.com/Docker-tutorials/Getting-started-ECS/606069/643352-4.html?srchtrk=index%3a2%0alinktypeid%3a2%0aq%3aaws+ecr%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2
	https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create_cluster.html
	https://www.terraform.io/docs/providers/aws/r/ecr_repository.html
	https://linuxacademy.com/community/posts/show/topic/26403-terraform-object-expected-closing-rbrace-got-eof
	http://wayne-yuen.blogspot.com/2017/03/1-why-use-containers-instead-of-ec2.html


	Docker -
	https://www.lynda.com/Docker-tutorials/Docker-Toolbox/721901/779040-4.html?srchtrk=index%253a1%0alinktypeid%253a2%0aq%253adocker%0apage%253a1%0as%253arelevance%0asa%253atrue%0aproducttypeid%253a2
	https://docs.docker.com/install/linux/docker-ce/ubuntu/#supported-storage-drivers
	https://www.blog.labouardy.com/pushing-a-docker-image-to-the-ec2-container-registry/ps were also added, targetting the staging and production instances respectively. Upon implementation of the terraform, the Dockerfile and CirclCi config.yml were being configured.