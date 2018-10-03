---
title: "John's Blog 6"
date: 2018-10-02T17:54:33-07:00
draft: false
Categories: [Project 2]
Tags: []
Author: "John Vinuya"
---
This week we implemented Hashicorp's Packer to create a new, base AMI to be later used for our Autoscaling Group. 
After installing the packages for packer, we needed to specify the template file to build the AMI accordingly. However, in order to add the ansible playbooks to the template, Packer needed to be run in the ansible controller VM to be able to build properly. 

	"provisioners": [{
		"type": "ansible",
		"playbook_file": "configapache.yml"
	}]

Once installed on the VM, the ansible provisioner is able to run correctly but with the drawback of using only one playbook. Other ansible provisioners provide json arrays to accommodate multiple playbooks, however certain provisioners are meant to be implemented on a guest host, not the controller. 
			"region": "us-east-1",
			"source_ami_filter": {
			"filters": {
			"virtualization-type": "hvm",
			"name": "/dev/sda1",
			"root-device-type": "ebs"
			},
			"owners": ["309956199498"],
			"most_recent": true
			},  
			"instance_type": "t2.micro",
			"ssh_username": "ec2-user",
			"ami_name": "packer-example {{timestamp}}",
			"tags": {"name": "baseAMI"}
		}],
Other specifications made on the template include the AMI name and tags to provide further identification which can be used in terraform documents such as the autoscaling group.
And lastly, the access keys had to be specified, as they were declared as variables in the template.
		"variables": {
			"aws_access_key": "",
			"aws_secret_key": ""
		},
	"builders": [{
			"type": "amazon-ebs",
			"access_key": "{{user `aws_access_key`}}",
			"secret_key": "{{user `aws_secret_key`}}",
Rather than implementing a variables file, we can simply use the -var option when running the packer build command, which is a safer, more convenient alternative.

