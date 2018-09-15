---
title: "Nick's Blog Week 3"
date: 2018-09-14T21:14:01-07:00
draft: false
Categories: [Project 1]
Tags: []
Author: "Nicholas Yoon"
---
Issues we faced:

	-AWS acct got hacked, 20 instances started within 30 mins-1 hour. Initial charge was about $15, next morning there was a charge for $132. Sent multiple messages to Amazon customer service requesting to review the charges and instructions on how to contest. 
	-still hard to collaborate with the entire group. We plan on getting together in the following week, to sit down as a group for hopefully a couple of hours, to go over everything we need to know about the environment we're building via terraform and ancible. 
	-We have an idea of how everything works, but trying to put it all together in reality seems to be difficult.

What I learned:
	
	-Installing Terraform:
		-download and install
		-change environment variables and set path to the location of the terraform.exe file
	-Installing CentOS 7 on a VM 
		-setting up VM with GUI with all the necessary applications preinstalled and system preconfigured
	-Terraform code:
		-researching throughout the Hashicorp website, specifically the guides to obtain proper script format and language. Obviously some changes to the variables needed to be changed, but an overall good learning experience. 
		-all .tf files should be under one single directory known as a Terraform config file
	-NAT Instance:
		-found multiple sources with information regarding how to properly implement a NAT instance. The NAT is built within the public subnet initially, then transferred over to the private subnet using an Elastic IP address to enforce higher levels of security to deny unauthorized access. The code wasn't hard to get a hold of, just had to be careful of the variables that needed to be changed appropriately according to the specs of our instances.
		-wasn't aware of the credential files I needed, along with the backend files, to get the instance up and running properly. Also had to change the "ami", but luckily our group leader was able to point me straight to the proper guidelines.

