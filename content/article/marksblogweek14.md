---
title: "Mark's Blog Week 14"
date: 2018-11-30T21:01:01
draft: false
Categories: [Project 7 Sprint 3]
Tags: [Mark Siegmund / Sandboxworms / Week 14]
Author: "Mark Siegmund"
---

Marks Blog Week 14								November 30, 2018

This week I concentrated on MaaS or Metal as a service.  I am not clear if it will do exactly what I want.  The main goal being to create a backup of a bare metal machine.  The info on the web site seems to indicate that it will do what I want…. But it does a bunch more stuff.  Services like Dhcp, DNS, Pixe, provide network discovery, machine discovery, provisioning,   I tried to install on several different machines.  None worked.  Each had different problems.  So I started from scratch and did the install at the same time I installed the OS.  The 3rd install…. This one a brand new clean install using the 18.04 MaaS from the install ISO and the Region install worked but when complete the website was not working.  I think I will try a clean install and use “apt install maas”.

Building a network map has been a crucial step in understanding and planning.  It is critical to leave this document for anyone that follows our group to help understand and make changes to the infrastructure.  
There are 4 networks
130.166.47.x/24     CSUN subnet - given to Advancing Tech Lab?
130.166.240.x/29    CSUN network given to Advancing Tech Lab
10.47.x.x/16        (firewalled)
10.1.47.x/24        (Firewalled - heartbeat)

There are 2 racks. One on the left hand side and the other is on the right hand side.
HP L1a  (10.47.0.11)             HP R1a  (10.47.0.12)
HP L2a  (10.47.0.13)             HP R2a  (10.47.0.14)
HP L3a  (10.47.0.15)             HP R3a  (10.47.0.16)

Moodle (130.166.47.10)  Moodle project is now defunct - server is working but Moodle is not being used)  The interface is a bridge interface.  Not sure why this is.  Possible use with VMs?


Firewall A (4 interfaces)
Eth0  130.166.240.19
Eth0  130.166.240.18
Eth1  130.166.47.2
Eth1  130.166.47.1
Eth2  10.47.0.2
Eth3  10.0.47.2 (heartbeat)



Firewall B
Eth0  130.166.240.29
Eth0  130.166.240.28
Eth1  130.166.47.3
Eth1  130.166.47.4
Eth2  10.47.0.3
Eth3  10.0.47.3 (heartbeat)


I spoke with Dr Wiegly and arranged to receive and install 2 new servers in the MDF.  The goal is to replace Firewall A and B.   The plan for now is to put an OS on them and install them in the MDF.  Finding and configuring the exact network configuration is not import to start with.  The important thing is to just get a network connection that we can rely on and work on the systems remotely. 

