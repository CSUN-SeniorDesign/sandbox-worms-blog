---
title: "Week 4 Playbook"
date: 2018-09-20T20:44:57-07:00
draft: false

categories: [Project 1]
tags: [ansible playbook plays]
author: "Aubrey Nigoza"
---
# Playbook on AWS Instances #

My goal for this week was to run a play on our 2 EC2 instances in the private subnet. Because this instances are not reachable from the Internet directly, access to them has to go thru the Bastion Host. The first step is getting a good understanding of the scenario and a basic understanding of how Ansible functions. 

- The 2 EC2 instances are in the private subnet. It is reachable via 2 ways, on port 80/443 thru ALB and port 22 via the bastion host. 
- Ansible uses SSH to run tasks in the hosts.
- Ansible uses an inventory file to figure out which hosts to run the play on.
- Since, our hosts/instances are in AWS, we should assume that at any given time, a particular host may be replaced, removed or added to the network. 

Now that I have this understanding, I started looking for answers.

### How to access EC2 instances in private subnet via Bastion Hosts ###
1. Make sure that the necessary route and access control via Security Groups are in placed.
	- Bastion: ingress port 22 from internet | egress port 22 to Ec2 Instances
	- Ec2 Instance: ingress port 22 from Bastion Host
2. Add the private key that can be used to authenticate to the EC2 instances in the ~/.ssh folder of the source machine(client) and secure it (no other user can access it)
3. Configure ssh identity:   

		ssh-agent bash
		ssh-add (if private key is not at the default location, specify it)
4. Download ec2.py and ec2.ini
	- Ec2.py will serve as the dynamic inventory for Ansible. It will help us query the EC2 instances with certain tag.
5. Configure ~/.ssh/config
	- User account to use
	- Private network to proxy thru the bastion host
	- Explicitly define bastion host public ip, user, location of the identity file (private key) and to forward agent.


			Host 10.*.*.*
			User ec2-user
			IdentityFile ~/.ssh/keypair #location of private key.
			ProxyCommand ssh -q -W %h:%p 13.50.123.45 
			Host 13.50.123.45
			User ubuntu
			IdentityFile ~/.ssh/keypair
			ForwardAgent yes

### Developing my first playbook ###
1. Install httpd using the Yum module.
2. Start httpd
3. make sure httpd starts at bootup.

		- hosts: tag_Type_WebServer
		  tasks:
		    - name: install Apache
		      become: yes
		      yum: name={{ item }} state=present
		      with_items:
		        - httpd
		    - name: start services
		      service: name={{ item }} state=started enabled=yes
		      become: yes
		      with_items:
		        - httpd
4. done

