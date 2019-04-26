---
title: "Johnblog28"
date: 2019-04-25T23:09:06-07:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
Looked further into firewall builder and the compile creates a script that contains a series of iptables commands that when executed, add configurations in addition to existing rules. Saving and flushing the previous ruleset helps to apply the new exceptions that will be reset upon reboot. New services we looked into are Corosync accompanied with Pacemaker for IP failover via cluster membership and management; these services may take the place of our current configuration with keepalived. By using Corosync, we would have the 2 firewalls set up in an active/passive configuration using 2 floating ips - .240.18 being our original and .47.x being the additional for the .47 subnet. It is because of the .47 subnet that we believe many of our issues are caused as there is no gateway for .47, thus anything routed from the .240 subnet to the .47 subnet is lost. It is yet to be determined that this is also why we cannot get into the iLo for the new firewalls. 
On Thursday we completed an ubuntu installation on one of the servers in the room next door and disabled the BIOS password using a breaker inside the machine. Other things we observed include different the type of RAM installed and needed to be ordered, and how 1 of the 4 hard drives returned a failure status; currently we set up RAID 5, that way if a new drive were to be added it would be rebuilt automatically. Another issue with the machine was how it was unable to detect USB connection certain times as we tried to boot from the stick; eventually we were able to install ubuntu 14 successfuly but were unable to get networking as that prevented us from installing packages or even pinging.  
