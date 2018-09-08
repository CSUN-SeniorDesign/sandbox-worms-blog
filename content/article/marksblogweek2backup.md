---
title: "Marksblogweek2"
date: 2018-09-08T02:35:11-07:00
categories: [Project 0]
author: “Mark Siegmund”
draft: false
---

Marks Blog-Week 2
This week I got a lot done.  My responsibility was to set up the EC2 instance and setup the web server.  I decided to use Redhat 7.5 because 3 of our members had used it in their past  Linux trials.  Setting it up was pretty easy.  The problem was connecting up to it.  I had set up an instance in my personal account previously.  It was straight forward.  After starting the instance I was able to connect up to it with Putty just as the instructions had said.  This new instance that I had just created in our shared environment and no matter what I tried I just could not connect up using Putty.  Well after a bit of trying different changes in user names and changing keys I asked Aubrey what I might be missing…. He asked if I had added my IP into the security group that someone else in my group created….. Duh… I found the group added my IP and tried putty again but it still was not working…. I rechecked everything one item at a time.  I found that I had confused the key that was sent to me with my key from personal instance.  This was an easy fix too and found the correct key ran it through PuttyGen  got my private key and guess what, it worked on the first try.  :-)

The webserver install was very straight forward.  Installed Apache using yum. We also needed to setup ssl service and found that we needed to install mod_ssl.  Additionally we needed to allow tcp traffic on port 443.  This was also the same security group that needed to modified for the ssh entry for my IP.
Port 443 and 0.0.0.0/0  for IP4
Port 443 and ::/0  for IP6

I am working on finding out the ip range for the CSUN wireless so I can put that in our access list.  Currently I have added the entire CSUN class be for wired networks… 130.166.0.0/16

Got more familiar with Git.  I did not use it enough this week and will put more effort into using it for the coming week. 
Looked at creating subnets… Began to realize that given 64K address we were using about 3 times 4K and 3 time 1K or 15K addresses.  That left 49K addresses un used! 


