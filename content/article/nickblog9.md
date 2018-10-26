---
title: "Nick's Blog 9"
date: 2018-10-26T15:54:34-07:00
draft: false
Categories: [Project 4]
Tags: []
Author: "Nicholas Yoon"
---
This week, we collectively focused on optional changes that can be made to the way we host our website to the most cost effective method. First, we discussed the plan of how the functionalities of the infrastructure would correspond with one another in order to bring up our blog both effectively and efficiently. From what I understood is that our webservers would be listening via HTTP or HTTPS for requests, the latest build of the website would be pushed to our Github repository, CirclCi grabs the code and builds an image which is then pushed to our S3 bucket. Meanwhile, as the requests come through via port 80/443, Route 53 would be redirect the path to the proper image from the S3 bucket and deploy an output so the person making the request can access the website successfully. Now that might not be completely correct, but that's the general idea of what I got from the discussion we had as a group.

So I did some digging and found that there's similar other CDNs available to the public besides AWS. I looked into Akamai, CloudFlare, and also the option of hosting anywhere from 1-4 instances on AWS. For the most part, the cost of hosting a website using any one of these CDNs is quite similar. I saw prices ranging from $20-$34 for 1-2 instances total. I estimated the plans based on 1.8 GBs of data usage per day which is equivalent to our website getting 1000 hits per day (stipulation advised to use by professor for project 4). Plus, it looks like many of these CDNs offer a free tier similar to Amazon, which brings the costs down a little more if we're including discounts. 

Also, this week we were asked to come to class to listen to presentations about specific projects that are being offered to our senior design class. I was honestly expecting more projects to be presented to us, but from the one's that I saw, I'd have to say I'm leaning towards the SAPS, irrigation system, or Professor Wiegley's "Advancing in Technology" idea. I found these three to be the most beneficial to us, as CIT majors, because this is the type of work I feel we will be doing moving forward into our careers. Maybe not so much the irrigation system, that seems more of a "start-up venture" type scenario, but as far as working on a system that's already been built, but can use improvements like the SAPS system or even virtualizing some components within a network that has old, outdated hardware is a viable career choice as well.

We've also collectively agreed as a group that some of us would try re-doing some of the older projects, on our own, in hopes of being able to gain a better understanding of the entire process and functionalities. I also need to update my resume, complete project 4, and upload this blog post so I've got quite a full plate. 


Websites: 
	https://aws.amazon.com/s3/pricing/
	https://www.alibabacloud.com/product/ecs
	https://aws.amazon.com/getting-started/projects/host-static-website/services-costs/
	https://www.cloudflare.com/plans/#compare-features
	https://calculator.s3.amazonaws.com/index.html#r=IAD&s=EC2&key=calc-F427A247-9731-4DEC-9797-D3648611AF61
	https://www.akamai.com/us/en/cdn/what-is-a-cdn.jsp