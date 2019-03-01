---
title: "Johnblog21"
date: 2019-02-28T18:25:16-08:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
This week comprised primarily of research for the nmap command to better use it to properly fill out a rough but accurate network map and what the config.yml file for netplan contains to help with changing the settings on the network interfaces. The current config.yml has a gateway specified for the first interface along with the domain for techlab.csun.edu and csun.edu. In regards to the DHCP issue last week, we set the dhcp4 property to true to enable dhcp assignment as well as set the appropriate IP range on the appropriate interfaces. Once we shut off the old firewalls, we would have to make sure that the netplan configuration settings should be the same to achieve similar ip assignments. We later started to change the IPs via the config.yml in the netplan directory; depending on which of the interfaces was changed, we had to either shell into the new firewall from a different ip or shell into ilo and power down the machine from there as running netplan apply would sometimes cause the session to freeze if the interface that was shelled into had its ip changed. Currently we have hard-coded new ips and were able to have other machines successfully ping them.
