---
title: "John's Blog Week 4"
date: 2018-09-19T11:51:18-07:00
draft: false
Categories: [Project 1]
Tags: []
Author: "John Vinuya"
---
This week we created and configured the application load balancer which can decrypt ssl/tls communcation.

First, the certificate had to be added to Amazon:
-resource "aws_iam_server_certificate" "sandboxwormscert" {
-  name      = "sandboxwormscert"
-  certificate_body = "${file("certificates/www_sandboxworms_me.crt")}"
-  private_key      = "${file("certificates/sandboxwormsKey.key")}"
-  certificate_chain = "${file("certificates/www_sandboxworms_me.pem")}"
-}

Then the load balancer is created and configured as an ALB along with references to the security group and public subnets with our service instances:

-resource "aws_alb" "sandbox-ALB" {
-  name               = "sandbox-ALB"
-  internal           = false
-  load_balancer_type = "application"
-  security_groups	=	["${data.aws_security_group.ALBSG.id}"]
-  subnets		=	["${data.aws_subnet.public_sub1.id}","${data.aws_subnet.public_sub2.id}"]
-
-#  enable_deletion_protection = true
-}

Target groups are then configured:
-resource "aws_lb_target_group" "sandbox-target" {
-  name     = "sandbox-target"
-  port     = 80
-  protocol = "HTTPS"
-  vpc_id   = "${data.aws_vpc.selected.id}"
-  health_check { ...

2 Listeners are made, one for HTTP and one for HTTPS, on ports 20 and 443 respectively:
-resource "aws_alb_listener" "alb_front_https" {
-	load_balancer_arn	=	"${aws_alb.sandbox-ALB.arn}"
-	port			=	"443"
-	protocol		=	"HTTPS"
-	...

-resource "aws_alb_listener" "alb-listener" {
-	load_balancer_arn	=	"${aws_alb.sandbox-ALB.arn}"
-	port			=	"80"
-	protocol		=	"HTTP"
-	default_action {
-   type = "redirect" ...
 
The listeners had to be configured with the ARN from the certificate we added earlier to be able to decrypt SSL/TLS traffic from outside.  