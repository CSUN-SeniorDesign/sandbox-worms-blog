---
title: "Mark's Blog Week 9"
date: 2018-10-26T23:13:01
draft: false
Categories: [Project 3]
Tags: []
Author: "Mark Siegmund"
---

Marks Blog Week 9								October 26, 2018

  This week was slow.   For project 4 we performed a cost analysis of our project 3 as it was set up at completion.  The main goal of project 4 was to think about minimizing the cost to run the services as we had running at the end of project 3.  There were many ways to lower the cost.  The easiest way was to eliminate services in AWS and replace them with nothing.  This simply lowers the cost by eliminating services.  There was no reason to have the staging as it was not even in the scope of work.  Autoscaling is a very good feature but on it was responsible for the largest share of AWS cost.  By far the largest cost was for each instance that was running.  Very roughly each instance was costing about 10$ per month.  We really did not have to have the autoscaling group and having only one web server that server all our needs.  With only one web server there was no need for the ALB.  If we put the web server in a public subnet and then we could eliminate the NAT server.  There was no need for the bastion station by the same type of reasoning.  This is not a good way to run services but is reduces cost to a minimum.
  
  I think a better approach is to us the private subnet.  This requires that we use NAT.  The group decided to go the more spartan way.
  
  One thing that I did not think about but Aubrey came up with using the s3 bucket for hosting our web content.  This allowed us to run our services with no instances running.  The cost ending up about 5-6$ per month.  Wow after our initial start place of 130 that is a huge savings!!!
  
  One additional thought is that with all the services cut one thing that would improve performance is to add a content delivery network.  One such service is from a company called Cloudflare it has a free tier.
  
  This week I went back to basics and started planning a setup of Project 0.  Just to make sure I got the basics down.  I will work on this over this weekend and next week.
  
  I could not believe how horrible my resume looked.  I have use the same basic format since 1990!  I completely reworked the content and used a new nice looking template.
