---
title: "John's Blog Week 3"
date: 2018-09-12T15:11:01-07:00
draft: false
Categories: [Project 1]
Tags: []
Author: "John Vinuya"
---
Project 1 started with the installation and setting up of terraform using our infrastructure repo on github. This required the use of AWS CLI to gain access to the aws account. Unfortunately, a .tf file containing sensitive information was pushed to a branch on github and make access keys available to the public. Not long after, we discovered multiple instances running in multiple regions along with other unauthorized resources. Upon realizing this, we deleted the access keys on github to prevent further unauthorized access to our account. We also went through each region and terminated all running instances, VPCs, volumes, and other unauthorized resources being used. 
We sent a support ticket and Amazon support responded within hours verifying that all unauthorized resources had been removed and that the case is under review. Luckily, all charges incurred seem to fall within the amount of credits Amazon Educate provided earlier, however, should the rest of the calculated bill exceed those credits, other group members have agreed to use their respective credits as well. Overall damage caused by the compromised account resulted in unexpected charges, a day's work, and little else. Currently the account if re-secured and our case still under review by Amazon support.
Our Terraform setup now has s3 and dynamoDB configured to lock whenever #terraform plan or config is being run to prevent conflicts in terraform scripts. Access keys are now referenced in different credential files which are included in the .gitignore file so the files are excluded from being pushed onto the branch. Currently we are configuring the Elastic Load Balancing for ssl/tls decryption.

