---
title: "John's Blog 11"
date: 2018-11-08T11:21:47-08:00
draft: false
Categories: [Advancing Tech Lab]
Tags: []
Author: "John Vinuya"
---
It did take time to assemble our group for the Advancing Tech Lab, despite most of our group carrying over to the this new project, we did some difficulty at first with communication with new group members. While we were arranging that, we also had to learn about the existing configuration Prof. Wiegley had for the server racks. It is important to note that there is documentation that is yet to be written for the setup as well as proper identification for what needed to be implemented in the system. 
Our first task, once our group was organized, was to look into and understand the how the firewalls, DNS, and DHCP was configured. Once we received our credentials to ssh into the system, our group had chosen to look into the DNS and DHCP configuration. The first issue we encountered involved the stability of the firewalls; we found that SSHing into Firewall A was successful most of the time, whereas Firewall B was found to have some instability and frequently timedout when pinging or SSHing. This posed a minor problem to both groups as while both firewalls are supposed to be duplicates of one another, we have not done enough investigation into Firewall B to see if that was the case. 
To map out what interfaces were present with the system, we had installed and ran nmap, which returned different results from different subnets. We also noticed that some machines that had pinged back were actually located off campus and considering how the system should technically be encapsulated in our environment, we decided that was something to take note of.
Further investigation into the dhcpd service also revealed that the service had been stopped and that current records came from both this semester and last spring; however further investigation is needed to see the purpose of this setup. However, running `systemctl status isc-dhcp-server.service` does show running dhcpd service information; a similar command can be run to find dns services as well.
In order to better understand certain aspects of the DNS and DHCP, I personally would need to review older material to see what other avenues I could investigate to see what other interfaces the firewall affects.