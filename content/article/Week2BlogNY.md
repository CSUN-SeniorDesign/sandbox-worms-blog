---
title: "Week2BlogNY"
date: 2018-09-07T18:41:15-07:00
draft: false
categories: [Project 0]
tags:[]
author: "Nicholas Yoon"
---
Creating Key Pairs to allow users access without having to administer passwords was a bit extensive, but makes plenty of sense. By giving users access is necessary, 
but allowing full root access is not. It's also a preliminary step we can take to ensure any ill experienced user damage to the EC2 instance. I said that it was quite 
extensive because we had to first create the key pairs and download both Private/Public Keys (that part was easy). It took some research to figure out why the original 
PEM file wasn't allowing proper access to the system, but we soon figured it's because we had to convert it to a PPK file. Then there's the step of creating the actual
new user account in the EC2 instance. That was done using some yum and sudo commands, creating some directories, storing the keys in an authorized keys file in our 
Ubuntu EC2 Instance and voila ~ we were able to give proper access to each individual in the group. We decided not to use the Amazon Secret Store option because individual
keys were distributed to each individual with the understanding that everyone would secure their own data. 

Then there was the task of deplying the Hugo blog by initially generating the static html via GitBash. Upon confirming the /templates folder was properly cloned and contains
material, we were able to generate the static html in the public folder and connect using WinSCP. Afterwards, we simply delete all the files in the DocumentRoot and move the 
static html files to the DocumentRoot. Then we ran the sudo restorecon command which properly deployed the blog. 

It's been a rough couple of weeks because we weren't able to meet as a group in person as much as we had liked to. Distributing tasks properly is definitely an issue we'll
need to focus on, but it looks like we'll be going into the start of project 1 with the strong understanding of what changes will need to be made in order to improve the
workflow as a group. Several of us are still not as familiar as we'd like to be with accessing and configuring the EC2 Instance so we plan on trying to re-work project 0 this week. 
Throughout this weekend, I plan on doing some research on Ansible and Terraform in hopes of being able to get my feet a little wet with the material.
