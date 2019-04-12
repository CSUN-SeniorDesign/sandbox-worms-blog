---
title: "Johnblog26"
date: 2019-04-12T14:04:09-07:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
Looking at the firewall builder files and the ip tables, we found that on top of being largely complicated, it also used many chains which made reference to rules somewhere else. Firewall Builder files are .xml, making them readable on a webpage, which firewall builder illustrates in a spreadsheet with draggable objects. Certain objects like TechlabFW have properties like platform with security options of iptables, packet filter, and ipfilter. Objects inside the same rule include `frontend` and `backend` which are referenced in other rules, Address Tables like `Abusers` that are based on the `blocklist.txt` file, of which are used in rules to keep out unauthorized users, among others. Objects for the interfaces refer to eth0 as `uplink` with the floating ip .18 assigned to it, eth1 as `frontend` with 47.1 assigned, eth2 as `backend`, and eth3 as `heartbeat`. Our current goal is to scrap the older .fwb files and create a new file that simply allows traffic without the clutter of the older rules. The rules for FWA currently mention HTTPS in rule 22 in the .fwb files and in the iptables; it is somewhere in here that prohibits FWA from performing push and pull git operations for versioning. Further research into Firewall Builder's documentation may help distinguish how the iptables are changed versus how the routing blocks traffic in certain ports.
