---
title: "John's Blog 10"
date: 2018-10-31T16:21:15-07:00
draft: false
Categories: [Project 5]
Tags: []
Author: "John Vinuya"
---
For project 5, we decided to implement the CloudFlare CDN design. A large part of the project is in changing the config.yml file in the .circleci directory to redirect output to a new S3 bucket, where it will be hosted. 
Looking at older config files, we realized we can run all the commands in a single job, and since we are just redirecting to S3, the only files needed from the repo are in the /public folder.
	`aws sync </public> s3://<bucket_name>`
Provisions needed for S3 deployment are a revised or appended IAM policy with resources targetting the new S3 bucket and a Cloudfront distribution. 
JSON policy must be appended like so:
	`Resources: [
	"arn:aws:s3:::bucket_name",
	"arn:aws:s3:::bucket_name/*"
	]`
As a result of this design, this removes the need for EC2 instances and eliminates our largest cost in our monthly bill.