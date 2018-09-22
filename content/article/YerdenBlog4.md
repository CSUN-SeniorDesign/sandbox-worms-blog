---
title: "Yerden's Blog 4"
date: 2018-09-21T23:34:40-07:00
draft: false
categories: [Project 1]
author: "Yerden Zhursinbek"
---
Over the weekend I went over some material on how to create service instances with terraform and came up with code sample that I found on terraform resources. My task was to create two web servers that would take care of our website’s traffic. 
In the code line of .tf file for web servers I had to specify availability zone (it has to be in the private subnet) as well as subnet’s ID. In my first (successful) attempt of applying code and launching the instance I did not specify “vpc_security_group_ids”, however it is required since we are creating instance In our VPC. Also I would like to bring up the importance of being in the right directory. Once I ran “terraform plan” command to check for errors in the code, I tried to apply the code by “terraform apply” command which asked for permission to destroy some files and my user. I had to do the screenshare with my group leader to lately figure out that I was not in the right directory when applying code.


