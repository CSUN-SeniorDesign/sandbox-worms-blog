---
title: "John's Blog 12"
date: 2018-11-14T15:38:28-08:00
draft: false
Categories: [Project 6]
Tags: ["John Philip Vinuya", "Sandbox Worms", "Week 12"]
Author: "John Vinuya"
---
This week, we had some problems accessing the 130.166.47.10 machine directly. This, combined with the holiday on monday left us with little time to be able to investigate the machines and firewalls to the fullest extent, however we were able to elucidate certain important details regarding firewall rules, dns, and dhcp configurations.
Running `ifconfig` showed 3 interfaces with IPs 130.166.240.x, 10.0.47.x, 10.47.0.x, and the loopback. These IP ranges are reflected in the dhcpd.conf file where 2 subnet pools were specifically defined: 130.166.47.200 - 130.166.47.219 which eth0 interface falls into and 10.47.1.1 - 10.47.1.254 which eth1 falls into. Firewall entries for both firewall A and B contain fixed addresses with `include` lines at the end pointing to a handful of different .dhcp files in the /Techlab directory which contain additional records as well as a failover .conf file.
Other .dhcp files contain records for the different hosts and machines within the given subnets.

###fixedhosts.dhcp###
Provides fixed/static address to services/devices, ie. firewall entries

*  moodle
*  www
...

###interconnects.dhcp###
*  vcconnect 
*  Cisco machines

###HPc7000s.dhcp###
*  HP c7000 Chassis web interfaces

From these entries, we concluded that a good amount of these records had been "hard-coded" or entered manually. While the dhcpd service is still running, addresses aren't being dynamically assigned to machines normally. Further investigation must be made to see the extent of this is reaches.
 