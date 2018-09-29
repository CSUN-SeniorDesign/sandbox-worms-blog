---
title: "Nick's Blog Week 5"
date: 2018-09-28T23:32:14-07:00
draft: false

categories: [Project 2]
tags: []
author: "Nicholas Yoon"
---
Looked up the CircleCI website, how it works, tried to get a grasp of the general idea, but didn't divulge into it too much because I wasn't tasked with this part of the project. Did some research on DataDog - what it is, what it does, how it's used, how to get it installed on our webservers properly - and signed up for an account since this was one of my tasks for this project. It didn't seem too difficult, matter of fact, it looks like as long as we're able to get it installed properly, the drag and drop UI seems extremely user friendly. After further discussion, I was asked to setup an environment like the one we'd be setting up on AWS, except I’d be using CentOS 7 spun up through a VM on VirtualBox on my local machine. Once installed, I was asked to figure out how to grab the tarball (.tar.gz) from our organization’s proper s3 bucket, extract it to a directory I created, search and locate a specific file, compress it using proper formatting, and send it back to the s3 bucket.
Since I was mainly working on creating a VPC instance using Terraform in the last project, I was asked to work on the Ansible portion of this project. So I setup CentOS 7 on my local Windows machine using VirtualBox as stated above. Installed and configured both Apache and AWSCLI. I was able to pull the tarball from the proper AWS s3 bucket, but ran into an error about the file not being a bzip2 file. After brief Googling, I found that the file was already extracted, which my groupmate was able to confirm, and saw that I was running an incorrect command. My groupmate was able to point me in the right direction. 
Below are a collection of the websites (at least the ones I remembered to save) I used for research.