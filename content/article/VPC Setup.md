---
title: "VPC Setup"
date: 2018-08-30T16:25:20-07:00
draft: false 

categories: [Project 0]
tags: []
author: "John Philip Vinuya"
---

To start, I first downloaded Git on my local machine and then proceeded to use Git Bash for subsequent exercises. To install Hugo, I first installed Chocolately to run the install through Windows CMD, then ran the install packages for Hugo. From there, Hugo is able to make the .md files needed for the blog post as well as generate the static site. 

What is needed next is to set up the Amazon Web Services account to set up the EC2 instances and the VPC. I then created the Virtual Private Cloud on AWS with the first attempt using an elastic IP and a recent attempt using a created IGW with the IP 10.0.0.0/16. I then made the 3 public subnets with the IPs 10.0.16.0/22 - 10.0.48.0/22. I also created three private subnets using the IPs 10.0.64.0/20 - 10.0.96.0/20 with auto-assigned IP addresses for all subnets. The proper Route Tables were then configured, one with both local and igw routes for the public subnets, and one with only local routes for the private subnets. The public subnets were associated with the route table with both the local 10.0.0.0\16 route and the 0.0.0.0/0 internet gateway while the private subnets were linked to the route table that had only the local 10.0.0.0/16 route. The first attempt using an elastic IP ran with similar results with the exception that the elastic IP had opened both a NAT gateway as well as a two more route table for both the internet gateway and the NAT gateway. Upon looking at the subnet Network Access Control Lists, the other connected subnets can be viewed and analyzed. Subsequent EC2 instance tests using the VPC resulted in both successful ssh as well as ping to google.com from inside. More EC2 instance tests will be ran to test VPC's reliability.