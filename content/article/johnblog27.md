---
title: "Johnblog27"
date: 2019-04-19T12:15:36-07:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
This week we discussed how we should use firewall builder going forward and came to the agreement that we'd be better off directly working with iptables for routing by saving the current rules to a file and flushing it to start from scratch. After discussing with Prof. Wiegley, it seems like we can still go forward with directly changing the iptables on the old servers, but wants fwbuilder to be used with the new servers. We also were able to install a service called `keepalived` that acts as an ip failover service; it allows the floating ip, refered to in the configuration file as the virtual ip, to be available to its respective firewall based on the priority set in the `keepalived.conf`. Upon testing, running `ip addr` showed which firewall had routing for the floating ip; we tested changing the priority to switch it to the other firewall and ran `ip addr` again, which properly reflected the results. The floating ip showed up according to the priority and upon ssh-ing to .240.18, we were connected to firewalla or firewallb respectively. Keepalived, in addition to ip failover, is used for load-balancing and provides notifications using email, which is also specified in the configuration file along with virtual router id and state. During testing, we had trouble accessing iLo on firewalla when we shut it down, only to see it come back up minutes after; we assume this is an issue caused by the various changes to the network, but we don't understand how the firewall restarted despite doing a full shutdown.
