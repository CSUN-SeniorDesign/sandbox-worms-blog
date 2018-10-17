---
title: "Troubleshooting Week8"
date: 2018-10-18T17:14:29-07:00
draft: false

categories: [Project 3]
tags: [ECS, cluster, docker]
author: "Aubrey Nigoza"
---


### ERROR ###
aws_ecs_service.sbw-ecs-service: InvalidParameterException: The container sbw-blog does not exist in the task definition.  

status code: 400, request id: 90fea37a-cd1e-11e8-aad1-f1dcb60ef91e "sbw-ecs-service"  

The problem was we were defining a container_name in my service that doesn't exist. It doesn't exist because I didn't use the same name as in the task definition.

    resource "aws_ecs_service" "sbw-ecs-service" {
	    name            = "sbw-ecs-service"
	    iam_role = "${aws_iam_role.ecs-service-role.name}"
	    cluster         = "${aws_ecs_cluster.sbw-ecs-cluster.id}"
	    task_definition = "${aws_ecs_task_definition.sbw-blog.family}:${max("${aws_ecs_task_definition.sbw-blog.revision}", "${data.aws_ecs_task_definition.sbw-blog.revision}")}"
	    desired_count   = 2
	
	    load_balancer {
	    target_group_arn  = "${aws_alb_target_group.sbw-ecs-target-group.arn}"
	    container_port    = 80
	    container_name    = "sbw-frontend" #<----doesnt match with below###
	    }
    }

    data "aws_ecs_task_definition" "sbw-blog" {
	    task_definition = "${aws_ecs_task_definition.sbw-blog.family}"
	    depends_on = [ "aws_ecs_task_definition.sbw-blog" ]
    }

    resource "aws_ecs_task_definition" "sbw-blog" {
	    family                = "sbw-blog"
	    network_mode          = "bridge"
    container_definitions = <<DEFINITION
    [
    {
    "name": "sbw-frontend", #<-----doesn't match with above####
    "image": "429784283093.dkr.ecr.us-east-1.amazonaws.com/sandboxworms:latest",
    "essential": true,
    "portMappings": [
    {
    "containerPort": 80,
    "hostPort": 80
    }
    ],
    "memory": 500,
    "cpu": 10
    }
    ]
    DEFINITION
    }

In our case, we didn't have a name at all in your aws_ecs_task_definition of the container_definitions.


I also worked on our lambda function. It was a good challenge. I had forgotten how to write in python. But, luckily it was not that hard to pick it up again.

My code logic is:

1. Get the key that triggered the event. (the text file)
	1. If production, set value for production deployment.
	2. if staging, set value for staging deployment.
2. Grab the file that triggered the event
3. Read the content. Trim new line
4. Use the content to get the manifest of the image in ECR
5. Use the manifest to tag the image with the value set on step 1.
6. Trigger a new revision of task definition
7. Update the service to use the new revision
8. 6 and 7 will refresh the container rollout  


		import boto3
		import botocore
		import pprint
		import os
		
		region = "us-east-1"
		ecr_client = boto3.client('ecr')
		s3 = boto3.resource('s3')
		ecs_client = boto3.client('ecs', region_name=region)
		
		def lambda_handler(event, context):
		    if event:
		        print("Event : ", event)
		        file_obj = event["Records"][0]
		        filename = str(file_obj['s3']['object']['key'])
		        print("Filename: ", filename)
		        object = s3.Object('sandboxworms-packages-92618', filename)
		        response = object.get()
		        imagesha=object.get()['Body'].read().decode('utf-8').rstrip("\n")
		        manifest=get_manifest(imagesha)
		        if filename == "master/prodbuild.txt":
		            tag_name = "production"
		            service_name = "sbw-ecs-service-production"
		            task_defintion = "sbw-task-production"
		            task_family = "sbw-task-production"
		            container_name = "sbw-frontend-production"
		        else:
		            tag_name = "staging"
		            service_name = "sbw-ecs-service-staging"
		            task_defintion = "sbw-task-staging"
		            task_family = "sbw-task-staging"
		            container_name = "sbw-frontend-staging"
		        try:
		            tag_image(manifest, tag_name)
		        except Exception:
		            print("Image already tagged")
		        
		        revision = revise_task_definition(tag_name, task_family, container_name)
		        taskDefinitionRev = task_defintion + ':' + str(revision)
		        update_sbw_service(taskDefinitionRev, service_name)
		
		def revise_task_definition (tag_name, task_family, container_name): 
		    response = ecs_client.register_task_definition(
		        family=task_family,
		        #taskRoleArn='string',
		        networkMode='bridge',
		        containerDefinitions=[
		            {
		                'name': container_name ,
		                'image': '429784283093.dkr.ecr.us-east-1.amazonaws.com/sandboxworms:' + tag_name,
		                'cpu': 1024,
		                'memory': 512,
		            #'memoryReservation': 123,
		            #'links': [
		            #    'string',
		                'portMappings': [
		                    {
		                        'containerPort': 80,
		                        'hostPort': 80,
		                        'protocol': 'tcp'
		                    },
		                ],
		                'essential': True,
		            },
		        ],
		    )
		    return (response['taskDefinition']['revision'])
		    
		def update_sbw_service (taskDefinitionRev, service_name):
		        
		    #print taskDefinition
		    response = ecs_client.update_service(
		        cluster='sbw-ecs-cluster',
		        service= service_name,
		        desiredCount=1,
		        taskDefinition=taskDefinitionRev,
		        deploymentConfiguration={
		            'maximumPercent': 200,
		            'minimumHealthyPercent': 50
		        }
		    )
		    #pprint.pprint(response)
		    print ("service updated")
		
		
		def get_manifest(imagesha):
		    response = ecr_client.batch_get_image(
		        registryId='429784283093',
		        repositoryName='sandboxworms',
		        imageIds=[
		            {
		                'imageTag': imagesha
		            },
		        ],
		    )
		    return response
		    
		def tag_image(manifest, tag_name):
		    response = ecr_client.put_image(
		        registryId='429784283093',
		        repositoryName='sandboxworms',
		        imageManifest=manifest['images'][0]['imageManifest'],
		        imageTag=tag_name
		    )    


