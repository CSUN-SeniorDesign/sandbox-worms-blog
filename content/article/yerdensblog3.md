---
title: "Yerden's blog 3"
date: 2018-09-14T22:27:21-07:00
draft: false
categories: [Project 1]
author: "Yerden Zhursinbek"
---
Basically, Project 1 is redoing of first project but using some software tools and automation techniques to build up the environment. I was assigned to set up EC2 Service instances (2 is expected to be enough), which will contain our blog site and take care of the traffic. But first software tools had to be installed.
This time I had pleasure to get a new experience with Terraform and Ansible tools which will help us set up our infrastructures in more safe and efficient manner. Since I used Homebrew to install Hugo for previous projects, I decided to stick with it and by running “brew install terraform” command in terminal installed Terraform on my pc. Then, initialize the terraform directory with “terraform init” command. Next step is configuring the backend which will use S3 bucket to save .tf files and finally “terraform plan” command to check if changes were applied as expected. 
Currently I am doing some research on configuration of EC2 service instances.

