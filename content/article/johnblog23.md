---
title: "Johnblog23"
date: 2019-03-15T11:32:35-07:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
This week was pretty hectic in terms of scheduling with Gradfest and other matters, so we had little time to discuss as a group what our future actions are. We investigated why were unable to ping outside the week prior, but this week we found that we were able to ping google.com and the name would be resolved properly. Further research yielded that pinging direct IPs for external hosts do not always connect. However we can now access the new firewalls directly from the outside using .21 for new A and .22 for new A & B. Checking the 50-cloud-init.yaml file, the .21 and .22 IPs are assigned to are enp4s0f1, the third interface. Considering how we are able to access the new firewalls directly from the outside, we may be able to do further tests on shutting off the old firewalls. We are also doing further ping tests to see if DNS resolving is working. We will also continue to test turning off services on the old firewalls and see if the new firewall's services take over and work as intended. We also performed some power down tests on the old firewalls and tried to connect to the other machines in the network. Briefly, we were unable to connect to other hosts from the new firewalls, but we needed to verify which IPs were actually in the internal network. 
