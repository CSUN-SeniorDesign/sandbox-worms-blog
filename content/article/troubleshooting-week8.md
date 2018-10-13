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
