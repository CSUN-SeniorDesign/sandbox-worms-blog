---
title: "John's Blog 7"
date: 2018-10-11T16:53:55-07:00
draft: false
Categories: [Project 3]
Tags: []
Author: "John Vinuya"
---
For this project, we had to get familiar with utilizing clusters and containers to automate the deployment of our site. This involved getting to know how Amazon's Elastic Container Service worked and what resources needed to be defined in terraform. I first manually created the ECS cluster, task definitions, and ECR to see what resources were added by ECS to implement the cluster. Upon deletion of the cluster, Amazon gracefully removed all resources added by ECS, and it was from there I was able to base the terraform code.
 
	resource "aws_ecs_service" "sbw-ecs-service" {
		name            = "sbw-ecs-service"
		iam_role = "${aws_iam_role.ecs-service-role.name}"
		cluster         = "${aws_ecs_cluster.sbw-ecs-cluster.id}"
		task_definition = "${aws_ecs_task_definition.sbw-blog.family}:${max("${aws_ecs_task_definition.sbw-blog.revision}", "${data.aws_ecs_task_definition.sbw-blog.revision}")}"
		desired_count   = 2
		...
		}

The service defined above is responsible for the managing the task definition and when configured, provides service autoscaling which can be implemented for the staging and production images.

	resource "aws_ecs_task_definition" "sbw-blog" {
		family                = "sbw-blog"
		network_mode          = "bridge"
		container_definitions = <<DEFINITION
		...
		}

The task definition, depending on the how it is structured, contains JSON which includes what image to pull from the ECR as well as other options such as cpu, memory, etc. Other ways to implement this include referencing to another JSON file which contains the same code.
Aside from the ECS resources, we also had to create new security groups, roles, and instance profiles to accomodate the ECS instances and what resources they needed. We also needed to add new rules for the listen to redirect outside traffic either to our cluster or to our other instances. This will be changed later to redirect to the staging or production sites along with their respective staging and production task definitions.
After implementing the appropriate resources, we will now focus on the automation using CircleCi.
