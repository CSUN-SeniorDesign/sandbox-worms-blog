---
title: "John's blog 14"
date: 2018-11-27T15:34:10-08:00
draft: false
Categories: [Project 7]
Tags: ["John Philip Vinuya", "Sandbox Worms", "Week 14"]
Author: "John Vinuya"
---
Despite our lack of meetings, we have been able to work on individual cataloguing of services, dhcp records, and in-progress documentation. We looked through the dhcpd.conf file and found certain entries that we find questionable, specifically in the private ip ranges.

	...
	subnet 10.x.x.x netmask 255.255.0.0 {	### /24 address
		option domain-name-servers ns1-be.xxx.csun.edu, ns2-be.xxx.csun.edu;
		pool {
			failover peer "failover-partner";
			range 10.x.x.1 10.x.x.254;		### 10.x.x.06 - 10.x.255.250 local vm correction /26 range 
		}
	}
	...
	
We noticed the ip address originally entered was /24, which caused the range to contain 253 addresses rather than the larger amount from the /26 address. Further investigation in the netplan file/directory also showed different IPs for the same interface with the default ip not listed using 'ifconfig'. This issue is also something that we may need the professor to address before fixing.
We also researched different failover alternatives which include Maas, which provides bare-metal provisioning, PXE for network booting, and development of a script for checking running services on remote machines.