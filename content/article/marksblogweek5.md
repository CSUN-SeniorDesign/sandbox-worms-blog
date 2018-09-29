---
title: "Mark's Blog Week 5"
date: 2018-09-28T00:00:01-07:00
draft: false
Categories: [Project 2]
Tags: []
Author: "Mark Siegmund"
---

Marks Blog Week 5								September 28, 2018

I am Project leader for Project 2. The main change I wanted to accomplish was to have more dialog between group members.  I did this by talking with all members each day.  I also wanted to have some kind of backup for each issue we had.  This way training happens by one member who has researched his issue going to another team member.  A secondary reason for this is to foster camaraderie between group members. 
   The first thing to do was to create a  task list.  I worked  with Aubrey to create the task list.  Fo the most part the order was that given in the project description.  
     One thing that I needed to do was to get my local VM running for using Ancible.  This was an unfinished element from project 1 that needed to be done.  Ubuntu was what I choose for my OS.  The first install did not have a bunch of the tools and Gui that I was used to seeing so I re-downloaded the “Everything DVD” and re-installed.  This time when I completed the install I had many more tools.  Nevertheless I still had to add powershell, aws-cli, and, amazon-ssm-agent.  Then hugo was installed by 

snap install hugo --channel=extended .  

I then had to add my ssh keys to .ssh/authorized_keys.  To do  this file transfer I used curl.  
The permissions to the keys had to be changed.  This was done with the command chmod 700 .ssh/authorized_keys, which allows ONLY the owner to read, write and execute.  
Python was then installed using pip.  Next awsconfig needed to be run to create my credentials.  Next I copied my private key to .ssh/id_rsa.  The next step is to clone the repos.  Now Ansible is ready to be installed.
These four commands will download Ansible and set up correct permissions.
sudo curl -o ec2.py https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.py
 sudo curl -o ec2.ini https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.ini
 sudo chmod a+x ec2.py
 sudo chmod +x ec2.py
Ancible is now ready to use.
I worked on the Autoscaling Group.  The main idea here is that the health of machines in this group are monitored to see how busy they are.  If the threshold is met of how much traffic the machine is handling then a new web server
 will be generated to offload some of the traffic.  In addition the “health” of the web server is monitored.  If something is going wrong with the webserver and new one will be generated and the one with the problem will be removed.  The ASG will sit behind an ALB (Application Load Balancer).  The code for this was much more complicated than I originally thought.  It is still not working.
