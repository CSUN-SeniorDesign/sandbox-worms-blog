---
title: "Marks's Blog Week 3"
date: 2018-09-21T19:15:01
draft: false
Categories: [Project 1]
Tags: []
Author: "Mark Siegmund"
---
Marks Blogpost-Week4						Sept 22, 2018

Preformed my first destroy on Terrafrom.  It was intimidating and I was worried that I was going to damage work done by my group.  Apply brought it all back and Bastion instance was using variables for  the key_name, subnet ID, and the security group.  I do not understand hot to find an ami-xxxxx for the instance.  Aubre helped me with it.  (he gave it to me)  However I searched using Google, and AWS search but found nothing helpful.  By using filters for finding the ami# when making the instance I was able to find one for CentOS, but I could not find the RedHad one that Aubrey found. 

I was the first one to get an instance up this week.  It was kinda cool that I was able to help two other members of our group with making their instances.

We needed to put sectrity files
                backend.auto.tfvars
                credential.auto.tfvars
                Credential-backend.auto.tfvars
In each folder that we wanted to run terraform in.  These files needed to be listed with git ignore so that my private key would not get posted to our Git repository.


Changes were made to the security groups and I could not get terraform to work.  Got some generic errors and I finally thought about the certificates that we generated and the pem file that we used to access the instances in the private subnets.  I needed 3 different files to completed all the keys and cert that was needed.  I put them in the certificate file
                Certificate folder with
		sandboxwormsKey.key
		www_sandboxworms_me.crt - website cert
		www_sandboxworms_me.pem  - access key for ssh for all instances

With time running out for the week I was looking at the network we had set up and the placement of the web servers.  Both web servers were placed into private subnet 1.  This worked fine but I think if we have room to improve the design in the next project I will make 3 web servers and put one into each of the private subnets.  This will improve system uptime for the web servers.  The ALB will send traffic to each of our servers in private subnets distributing the network load and mitigating downtime if there is a network problem in one of the subnets.like a DDoS.  This may not be the best example but it is possible that only one web server may be attacked in zone and having the others spread out in different subnets means that the others would continue to function while the first was attacked.

