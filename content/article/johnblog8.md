---
title: "John's Blog 8"
date: 2018-10-17T12:27:32-07:00
draft: false
Categories: [Project 3]
Tags: []
Author: "John Vinuya"
---
This week we implemented the automation using AWS Lambda and CircleCi, however in order to set it to project specifications, ECS had to be revised.
First of the changes, 2 ECS services needed to be added specifically for staging and production. Unique task definitions also had to be udated to pull different images according to their respective tag.

	resource "aws_ecs_service" "sbw-ecs-service-staging" {
		name            = "sbw-ecs-service-staging"
		iam_role = "${aws_iam_role.ecs-service-role.name}"
		cluster         = "${aws_ecs_cluster.sbw-ecs-cluster.id}"
		task_definition = "${aws_ecs_task_definition.sbw-task-staging.family}:${max("${aws_ecs_task_definition.sbw-task-staging.revision}", "${data.aws_ecs_task_definition.sbw-task-staging.revision}")}"
		desired_count   = 2
		...
	}

ECS services specific to the staging and production are created and attached to their corresponding task definitions and managed via ALB and unique ECS target group.

		container_definitions = <<DEFINITION
		[
	{
		"name": "sbw-frontend-staging",
		"image": "429784283093.dkr.ecr.us-east-1.amazonaws.com/sandboxworms:staging",
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
	
In addition to changes made to ECS, ALB listener rules had to be added to forward based on site url. Url used varies according to www., blog., staging., etc

	resource "aws_alb_listener_rule" "sbw-forward-blog-staging" {
		listener_arn = "${data.aws_alb_listener.sbw-alb-listener.arn}"
		priority = 94
		action {
			type = "forward"
			target_group_arn = "${aws_alb_target_group.sbw-ecs-target-group-staging.arn}"
		}
		condition {
			field = "host-header"
			values = ["blog.staging.sandboxworms.me"]
		}
	}	
	
Six more listener rules were added according to url. Two additional target groups were also added, targetting the staging and production instances respectively. Upon implementation of the terraform, the Dockerfile and CirclCi config.yml were being configured.