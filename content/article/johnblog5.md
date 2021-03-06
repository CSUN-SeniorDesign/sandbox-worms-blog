---
title: "Johnblog5"
date: 2018-09-27T14:31:20-07:00
draft: false
Categories: [Project 2]
Tags: []
Author: "John Vinuya"
---

This week we implemented the terraform code to create a new s3 bucket, in which we will be placing our .tar for our sites.
We used IAM policies to grant the new user, CCI, used by Circle Ci to use the s3 PUT action to place the .tar in our bucket.
	
	/*===============DATA TAG====================*/
	data "aws_s3_bucket" "sandboxworms-packages-92618"{
    bucket = "sandboxworms-packages-92618"
	}	
	/*data.aws_s3_bucket.sandboxworms-packages-92618.arn*/

	/*===================USER + GROUP===================*/
	resource "aws_iam_user" "CCI" {
		name = "CCI"
	}

We had to utilize some JSON in our 2 policies, one for PUT and one for GET from the bucket. Respectively, the Circle Ci user should place the .tar while the EC2 Webserver has permission to use the GET action to pull from the bucket.

	{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::sandboxworms-packages-92618",
                "arn:aws:s3:::sandboxworms-packages-92618/*"
            ]
        }
    ]
	}
	EOF

Issues with JSON included how policies were structured on AWS and modifying the JSON generated by default policies to suit our needs.
These policies would be attached to roles, which also had their respective JSON code.

	{
		"Version": "2012-10-17",
			"Statement": [
			{
			"Action": "sts:AssumeRole",
			"Principal": {
				"Service": "ec2.amazonaws.com"
			},
			"Effect": "Allow",
			"Sid": ""
			}
		]
	}
	EOF	

Next step to be implemented is the incorporation of Hashicorp Packer and the development of the base AMI.	