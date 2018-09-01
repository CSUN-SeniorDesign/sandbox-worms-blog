---
title: "Team Setup Domain Name"
date: 2018-08-29T22:59:35-07:00
draft: false

categories: [Project 0]
tags: []
author: "Aubrey Nigoza"
---
As we setup our team’s infrastructure, we divided the tasks between each other. I have been tasked to purchase the domain name and certificate for our team. To make sure that the access to the account will be available without any interruption because of member change, I have decided to create a new email address that will be shared with the entire team. 

I have chosen to use namecheap.com as the domain registrar. I also purchased a certificate together with the domain name purchase. This certificate will be used to secure the team’s website. However, I think that we should still give Let’s Encrypt free certificate a try once we have our EC2 instance. This will allow us to be exposed to using a purchased certificate (namecheap.com) and free certificate (Let’s Encrypt). Namecheap also offered a whois privacy filter to mask the information that I have provided. 

I also started to read about AWS Organization to familiarize myself. From my understanding, building an AWS Organization allows a group of people to apply policy to multiple AWS account. The master account will be billed for the expenses of all the member account. We can setup different Organizational Unit that can be applied different policies. Policies such as allowing or denying access to certain AWS. In a real-life scenario, one account can be assigned to be the production, one account can be the Staging, and another can be the QA. For our purposes, we can build OU for project to be submitted and an OU for lab testing.  Each member’s AWS account can be in the lab testing. This will allow all of us to be able to experience the entire setup of our AWS account (VPC, subnet, internet gateway, route53, elastic ip, EC2, etc). We can have one account that will contain the servers that will be bound of our official domain. It can be the official account for the submission of project. 
