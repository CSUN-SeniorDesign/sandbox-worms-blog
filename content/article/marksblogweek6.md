---
title: "Mark's Blog Week 6"
date: 2018-10-5T00:00:01-21:00
draft: false
Categories: [Project 2]
Tags: []
Author: "Mark Siegmund"
---

Marks Blog Week 6								October 5, 2018
Lots for this week

Finally began to understood about variables and passing them from different directories and files in the same directory.  When passing in info from outside the current file I need to use a  line like this one,
data "aws_iam_instance_profile" "instance_profile"{
    name = "instance_profile"
}

I can then reference it this way
iam_instance_profile = "${data.aws_iam_instance_profile.instance_profile.name}"

The key is to use the “data” syntax to get it to work.

When I created all the issues at the beginning of this project I should have use task lists in each issue.  It makes is look nicer and is easy to show that something is done.  Don't know how I missed this.  I will definitely use it in the next project for my issues.

Struggled immensely with group dynamics.  Two members are more advanced.  One has worked with AWS before.  The others, including myself in there,  have to work harder to catch up and be more helpful.  I tried pairing up people so that one on one work would be easier to do in terms of communication.  It worked somewhat.  There was more communication but we will have to do better.  This is exactly what the project 2 topic on “silos” was about.  

Project came together at last minute, literally.  I finished with my section on AWS and the launch configuration largely due to understanding about variables, (from my first paragraph above)  I feel that the pieces are so integrated that unless we were all together working and talking that progress was very slow.

Helped Nick with deployment of sites.  Learned some bash scripting.  This is sad to say but I learned about bash variables for the first time.  Created my first if statement.  Such a weird syntax to have it end “fi”.  Very different from any language I have used.  It will just take time like everything else.  The script we needed was to be run on each of the web servers.   In concept was pretty easy.   

DataDog has a pretty interface.  It needed to have some additional data displayed or accessable like number of web hits.  This was not clear how to implement and it is not done yet.  The basic site is up and working.  It just sits there and we really did not do anything with it.  Already working in IT the normal daily work does not allow for looking a log files “just because we should”.  People only look a log files when there is a problem.  That is the main use that i can see for DataDog.  It can have alerts set and sent out when a certain threshold is met.  So the benefit is that I don't have to parse logs.  This service does that and sends out a notification when there is a problem detected.
