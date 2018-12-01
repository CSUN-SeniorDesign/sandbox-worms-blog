---
title: "John's Blog 13"
date: 2018-11-20T11:26:24-08:00
draft: false
Categories: [Project 7]
Tags: ["John Philip Vinuya", "Sandbox Worms", "Week 13"]
Author: "John Vinuya"
---
This week, due to our limited time because of the holiday, most of the time spent with the environment was done with the intent to create documentation for the dhcp, dns, and other services. We also looked into alternative services for redundancy/failover- options to be explored include PXE server for network booting, and a new service called 'Metal as a Service' that facilitates deployment/provisioning of the environment. We also discussed ways to determine if dhcp/dns services are running from one firewall to the other via ingress on certain ports to prevent duplicate address assignment; other methods to determine server status from the other firewall are being researched. To further flesh out the network layout in the future, we may catalogue running services and network interfaces using nmap and netstat to see how the iptables are configured. We may also need to discuss with the professor what interfaces and services are used for what.
The routing issue from last week still persists, as we still need to log in through firewall B to access the other machines.