---
title: "John's Blog 18"
date: 2019-02-07T23:29:19-08:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
This week, we primarily focused on preparing to make the transition between the old firewalls and the newly installed firewalls. In accordance to the Advancing Tech document, I attempted to replicate the DNS primary-secondary configuration on VMs before moving the needed zone and config files to the new firewalls. I've had mixed results trying to establish a similar setup from scratch at first, but after reading through each zone and named.conf file, I found that while named.conf has "master" set up to be the current Firewall A, Firewall B has be set up to be the "slave" along with allowed transfers to other machines, some of which I found to be connected on the interface and others we assume to be within CSUN and outside the campus. Since I copied the bind directory and other requisite files last week, I will now start transferring those files to the newly installed firewalls and set up the primary-second dns configuration there.
We attempted turning off each of the current firewalls to test whether the other will take over in the absence of the other, but we still needed to install traceroute to know whether the packets go from one firewall as opposed to the other. Upon shutting down each firewall, the other firewall's connections were still live and were able to ping so rough conclusions include that the primary-secondary works or that they are acting as independent, redundant primary-s.