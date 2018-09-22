---
title: "Nick's Blog Week 4"
date: 2018-09-21T22:03:54-07:00
draft: false
Categories: [Project 1]
Tags: []
Author: "Nicholas Yoon"
---
Still trying to figure out how to properly spin an EC2 instance up with a NAT. I was able to find helpful code from these websites:

https://linuxacademy.com/howtoguides/posts/show/topic/13922-a-complete-aws-environment-with-terraform
https://www.terraform.io/intro/getting-started/build.html
https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html
https://www.terraform.io/docs/providers/aws/r/eip_association.html
https://nickcharlton.net/posts/terraform-aws-vpc.html

To understand the importance of the NAT instance, it's a secure method for handling routing correctly. It's setup in the public subnet to allow outbound traffic for instances in the private subnet while preventing unauthorized access from users on the internet. I was having trouble finding the correct ami id required to properly spin up the instance, but our group leader was able to point me in the right direction. I also had some issues with the script since I'm not a strong coder, but our group members were extremely helpful in assisting me get it all figured out. There was definitely some issues learning Terraform and how to properly configure the directories to get everything to run smoothly. For instance, in order to get the windows_amd64 plugin I needed to run an initial Terraform init command on my local Terraform directory which downloaded the proper plugin for provider "aws". Then I ran a Terraform plan which creates the dynamodb table, the kms keys, and the s3 buckets. Now, our group leader showed initially showed us what files were needed inside that directory in order for this process to go smoothly, but that's where I need to brush up on my Terraform skills. We also went over how to move the tfstate file to the s3bucket and how to create IAM users on my own personal AWS account. We went over how to generate key pairs, updating credential files, and adjusting the backend files.    

The Bastion Host was setup in the public subnet to allow SSH traffic from the internet gateway to the webservers - which will be done through Ansible on our Ubuntu controllers. Fail2ban has also been installed to prevent brute force attacks.  

aws ec2 describe-images --filter Name="owner-alias",Values="amazon" --filter
Name="name",Values="amzn-ami-vpc-nat*"

Things started off a bit rocky for Project 1 since our account was hacked and within a matter of 20 minutes or less, 20+ instances were created throughout the various regions on AWS which ended up costing a total of $70+. Luckily, we were able to catch it pretty quick - can't imagine what the damage could've been if we didn't catch that as fast as we did. Having to learn the material, apply our knowledge, build an instance, configure the environment, create documentations, and create a presentation in a two week span was quite difficult, but thankfully our group leader is quite well-versed with this stuff, which ended up saving us. With several other classes that each of us group members are taking, along with the homework and assignments, tests to study for, and our own lives, it's a very tight deadline to meet, especially without any instructions to get us going. Nonetheless, we're here to learn and apply our knowledge, but please understand that this is the first time we've ever been exposed to this type of material. It's not fair to hold that against us either, because the University should take full responsibility for the lack of education and material that was introduced to us throughout the several years we've been here at CSUN.

Moving forward, I plan on brushing up on as much material as possible for Terraform and Ansible because I'm sure we'll be using both again here shortly. I haven't been able to commit as much time as I had liked for these past two projects because of personal reasons, but I plan on spending more time going over course materials to better equip myself with the knowledge necessary to be successful in this career. I feel that I've been letting my team down a little, but I'm determined to pick things back up and keep pushing forward!

