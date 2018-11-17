---
title: "Mark's Blog Week 12"
date: 2018-11-16T03:49:01
draft: false
Categories: [Project 6 Sprint 1]
Tags: [week12]
Author: "Mark Siegmund"
---

Marks Blog Week 12								November 17, 2018
This week was eye opening.  I thought I knew mostly how DHCP works and a cursory idea of how DNS worked.  I knew that I had much ground to cover on DNS.  I am familiar with DHCP on Windows but the config was much different when setting it up with a config text file.  There is a basic file that controls DHCP,dhcpd.conf.  It has basis info about global options that is sets and all the subnets that it controls.  What I was not sure about is how to control subnets that the server did not have a physical connection to.  The basic idea to DHCP is a lease to a client for a specific time period.  Static leases are forever.  They are use on systems that require a specific IP like web servers and even the interface of the DHCP server itself.  These are excluded from the ip range that can be leased out.  Some of the global settings that might affect all subnets could  be a DNS server, a lease period, a net mask, a time server.  For each subnet that has a specific need to configure something the entry can be removed from the global setting and added to the configuration under each subnet.  I wonder what happens if there are entries for both global and subnet settings which one wins out over the other.  My guess is subnet wins because if the global exists and is over written by a subnet setting that would be ok if it was replaced.  I will have to try that out.

DNS 
I found the config file was located in /etc/bind.  It is called named.conf.  Even though I have been around people who talk about DNS configurations and databases I have no experience with DNS maintenance or configuration.  The basic idea is that a DNS server is given a DNS name and it give back or resolves it to an IP address.  I learned about reverse DNS and how it gives a domain name if you give it an IP address.  This requires the lookup files to the organized by students.  There are not many uses for reverse lookup but one important one in nslookup.  If reverse is not working then nslookup is not going to work.

I decided that we will post the conf files for services on our repository.  This waw anyone can see it and make changes to it and they are logged. We can go back in time to use a old configuration.  One thing to consider is that if we post changes to the file there is no way to know if it was really used on a server.  It is possible that I could post a change that was never used or ever tested and it would stay up in the repository for someone to use in the future.  I guess it would just take documentation line to added to the file...

I spoke with Dr Weigley a few times this week.  To day I asked about MAAS, which is a service installed on a linux machine and it makes an image of a linux server and saves it so it can be redeployed.  It is a cloud service and it appears to be free if you do not need support.  Even if you need support it is pretty inexpensive at 50$ for a year.  Jeff’s opinion is that anything we think is good to recommend and we document it for anyone that will use it then he is fine with it.  I will bring it up with the group to be considered for the next sprint.  I will set up a new Physical machine and install Ubuntu on it so I can test out MAAS next week.  Jeff indicated that it was complicated to setup.  Next sprint will tell.

This week I got my testing VM up and working for DNS.  Got a lot done this week.

