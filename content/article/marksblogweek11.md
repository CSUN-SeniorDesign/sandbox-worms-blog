---
title: "Mark's Blog Week 11"
date: 2018-11-09T22:49:01
draft: false
Categories: [Senior Project Sprint 1]
Tags: [week11]
Author: "Mark Siegmund"
---

Marks Blog Week 11								November 9, 2018

This week I was assigned to Advancing Tech Lab that is run by Dr Weigley.  Of the 7 people assigned to the Advancing Tech Lab we broke up into a group of 3, Team A, working on the two firewalls and a group of 4, Team B to work on DHCP and DNS.  Further Team B separated out to two persons to work on DHCP and two to work on DNS.  I was assigned to DNS working with Nick.   Even before doing the split I did some research on DHCP as I am familiar with using Microsoft DHCP.  I found the /etc/dhcp/dhcpd.conf config file on both firewall A and B.  
Using:      systemctl status isc-dhcp-server.service
Lets you know if the service is up and running.  It was running on firewall A.
Only one DHCP server can only be running on a single subnet at any point in time.  If more than one was running duplicate IPs would be handed out.  (assuming the address pools overlapped) 

There seems to be a problem with Firewall B.  It randomly kicks me off.   Sometimes it works for a long time, like an hour or more.  Sometimes it only works for 10 minutes and just disconnects me.  For some reason Firewall B has an update installed that requires a reboot.  Professor Weigley was surprised by this saying that only himself and our group had root access and that no automatic updated should be turned on.

I got my Git-Lab account linked with my github account.  Somehow I did not complete the account creation and it took 3 days and Professor hamilton to pushing me to check for a problem.

I started by finding the conf config file for DNS.  Command for checking on whether bind is running is:   systemctl status bind9
I found It is running on both firewall A and B.  This makes sense because there is no limit to the number of name servers you can be running at the same time as long as they are configured correctly.

I have made a VM running Ubuntu 16.04.  I now have bind installed and will start setting up the named.conf. The main idea for next week is to duplicate the services on the current systems Firewall A and B.

I spoke with Professor Weigley and he has just received 2 replacement machines for firewall A and B.  The hardware for both machines is identical.  The old machines are not identical.  When we have the new systems looking correct we will bring down the old systems.
