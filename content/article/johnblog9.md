---
title: "John's Blog 9"
date: 2018-10-24T13:44:29-07:00
draft: false
Categories: [Project 4]
Tags: []
Author: "John Vinuya"
---
A large part of project 4 involved research into which different cloud-hosting service is viable for hosting our webservers. As the base account manager of our AWS organization, I have monitored our costs over the past 2 monthes and gained an understanding of what services are or are not cost-intensive. It is to my understanding that EC2 services, while they do charge by the minute, make up a significant portion of our bill. The number of instances we do have actually use up the time AWS free-tier offers much faster due to the number of instances we have running at all times.
###Problem
How can we make our blog more cost-efficient?
What can be done to mitigate EC2 charges? 
###Options
One option can be to move our webservers off AWS entirely, and instead use a separate cloud-hosting service to keep our webservers, thereby removing the EC2 instance charges entirely. This does seem to be good option, however there is the matter of redirecting TLS traffic from AWS with third party load balancing services. As of my research, I have yet to find a service similar to what is used on AWS, however it would be ideal if those features were to be available on the same service. 
Another option is to partially migrate our setup to a separate cloud-hosting service, that way we can still use AWS load balancing while forwarding traffic to our production webserver remotely.
###Caveats
By using a separate cloud-hosting service, we may be subject to additional fees, should the service require additional payment to utilize many of the same features AWS offers. The feasibility of this largely depends on the cost, whether or not it exceeds the original EC2 costs on AWS. 
Security is also another concern as this largely depends on what external service we choose and whether or not their security specifications and procedures are similar to Amazon's. For the purposes of this blog, no sensitive information is part of the content of the site, therefore most risks with using external services would be largely be in repository management.