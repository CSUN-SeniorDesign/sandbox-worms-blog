---
title: "Terraforming AWS"
date: 2018-09-13T20:31:02-07:00
draft: false

categories: [Project 1]
tags: [terraform]
author: "Aubrey Nigoza"
---
My goal for this week is to setup our terraform environment to use Remote State with Locking. As the project specs dictate, I used S3 and DynamoDB to provide locking. Terraform writes the state of the environment in a JSON file that has an extension of .tfstate. Initially this file is located on the PC that has terraform (locally). My understanding of the task is to configure remote backend that targets S3. This way terraform will use the S3 bucket to save the tfstate file. Since we have multiple directories which could be considered multiple state of the environment, we will be saving multiple tfstate in the same S3 bucket. DynamoDB will handle the locking of the tfstate file to avoid simultaneous change from happening that could present conflict. 

#### First Scenario ####
I initially created an S3 bucket by going to the AWS Management Console. I gave our IAM account access to the bucket. I also created a table in the DynamoDB. After this, I setup the backend on our terraform to use the S3. This is simple enough as terraform already has built-in integration with both of these services.

	terraform {
	    backend "s3" {
	        access_key = "${var.aws_access_key}"
	        secret_key = "${var.aws_secret_key}"
	        region     = "${var.region}"
	        encrypt = true
	        bucket = "sandboxworms-rstate-0911218-22"
	        key="terraform.tfstate"
	        region="us-east-1"
	        dynamodb_table = "sandboxworms-lockdb-09112018-22"
	    } 
	}

#### Issue ####
However, I ran to an issue here when I did terraform init to initialize our environment to bring the tfstate to S3. Terraform cannot interpolate the variable during initialization. After some google search, I found that I can pass the access key and secret key on the terraform init command.   
#### Solution ####
    terraform init -backend-config="access_key=xxxxx" -backend-config="secret_key=xxxx"

This works but it means I have to type the access key and secret key during init and I have to manually configure S3 and DynamoDB.

#### Second Scenario ####
So, I decided to change the process:  
1. Initialize the environment locally.  
2. Run terraform with configuration to create S3, create table in Dynamo and give IAM user access to S3.  
3. Initialize the environment again to use the remote backend S3. Setup a .tfvar file to pass the configuration needed for the S3 backend.

Number 3 looked like this: 3 files.  
- backend.tf (tells terraform to use S3)  
- backend.auto.tfvars (tells terraform which S3 bucket and Dynamodb to use. This is useful so I can have multiple version of this file depending on if I am working on our root account AWS or my own AWS)  
- credential-backend.auto.tfvars (included in .gitignore and contains the access key and secret key)  

	terraform init -backend-config=credential-backend.auto.tfvars -backend-config=backend.auto.tfvars

backend.tf:  

	terraform {
	    backend "s3" {
	    } 
	}

backend.auto.tfvars

	bucket = "sandboxworms-rstate-0911218-24"
	dynamodb_table = "sandboxworms-lockdb-09112018-24"
	key = "terraform.tfstate"
	profile = "terraform"
	region = "us-east-1

credential-backend.auto.tfvar

	access_key = "" 
	secret_key = ""  


### Issue ###
Now, my issue is that when I run terraform init, the S3 bucket and dynamoDB doesn't exist yet. So, I just get an error. 

#### Workaround ####

I rename the backend.tf to backend.tfno . It could be any extension. This just forces terraform to not use this file as part of the configuration. Without this file, my terraform will initialize locally. I will be able to run terraform apply. After my S3 bucket and DynamoDB have been created, I can change the extension of backend.tf to the proper extension. I ran:  

	terraform init -backend-config=credential-backend.auto.tfvars -backend-config=backend.auto.tfvars

It prompted me if I want to upload my current tfstate to s3. After this, remote backend is setup. 

#### Result ####


- My group mate, could just initialize directly to the Remote state as the s3 bucket and tfstate file exists already.
- Terraform is also tracking the state of our S3 bucket and DynamoDB.
- We can easily see the setup of our remote backend infrastructure because they were created using codes.
- We can make changes to it such as removing permission by modifying the code. 
- It allows us to extend the features and benefits of terraform to our remote backend as well.


*Aubrey* 