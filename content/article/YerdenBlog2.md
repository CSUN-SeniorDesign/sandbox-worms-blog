---
title: "Yerden's blog 2'"
date: 2018-09-07T23:32:26-07:00
draft: false
categories: [Project 0]
author: "Yerden Zhursinbek"
---
Setting up AWS and GIT accounts and getting closer with the platforms during the first week took more time than expected, since most of us were absolutely new to it. After everyone has finished their individual tasks of the project we decided to create IAM user for each organization member, so everyone could have access to the running EC2 instance and also run their own instances to exercise and understand all the steps of the workflow. 
In order to create a new user in EC2 instance we need to have putty installed. First, we have to create a Key Pair in AWS EC2. After logging in AWS console, choose Key Pairs in EC2 Services menu. Create a Key Pair, name it and download the Private Key. Then, using PuttyGen, select the private key and save Public Key as .pem file. Finally save Private Key as .ppk extension.
After the steps mentioned above are done we can start creating user in EC2, firstly by login to the instance. You can then add a new user be running simple sudo command “sudo useradd username -d /home/username”. Some configurations need to be done in sudoers file to allow access without admins password. Then by running “sudo su username”, switch to the new user and then navigate to its directory, where .ssh file has to be configured. 
I also went through the steps of deploying the post on Hugo. After the static html files are generated in public folder (blog’s repo), open WinSCP and connect the server using Private Key. Then, copy html files to the home directory and login to the EC2 using Putty. After that, delete files in the DocumentRoot and move static html files there. And finally deploy the blogpost by running “sudo restorecon” command.
