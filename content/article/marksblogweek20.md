---
title: "Mark's Blog Week 20"
date: 2019-02-22T21:58:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 20]
Author: "Mark Siegmund"
---

Marks Blog Week 20					February 22, 2019


This week was a real eye opener.  I thought the server switch over would be a relatively easy process. 
Test iLo ports on new servers. 
Copy over the needed files for DHCP, DNS, and the firewall.
Turn off the old servers and change the IPs of the new servers.
Turn on all 3 services.
Test all 3 services.

But it has not worked out that easy.
Before we turned off the old DHCP server we tried to force IPs to be assigned to all the network interfaces on the new servers.  This did not work as easy as I thought it would.  Testing is still not done and will continue into this weekend.  We added the necessary lines to the dhcpd.conf file and 

subnet 130.166.47.0 netmask 255.255.255.0 {
  option domain-name-servers 130.166.47.2, 130.166.47.3;
  option routers 130.166.47.1;
  pool {
       failover peer "failover-partner";
       range 130.166.47.200 130.166.47.219;
       }
}

subnet 10.47.0.0 netmask 255.255.0.0 {
  option domain-name-servers ns1-be.techlab.csun.edu, ns2-be.techlab.csun.edu;
  pool {
       failover peer "failover-partner";
       range 10.47.1.1 10.47.1.240;
       }
}

subnet 130.166.240.16 netmask 255.255.255.248 {
}

subnet 10.0.47.0 netmask 255.255.255.252 {
}

From the subnet lines in the dhcpd.conf file one can see that we are using 4 different networks.  Of those 4 only 2 are serving IPs.  They are

10.47.1.1  to 10.47.1.240
**** We changes this so that testing could be done on the dedicated IPs outside of the pool range.  This network is actually much larger.  It is 10.47.0.0/16

The only other network that serve IPs is the 130.166.47.0/24
The range it serves is 130.166.47.200 to 130.166.47.219
This is pretty small, only 20 addresses.

We had 2 problems with handing out the IPs.
It took a long time before the IP seems to work.  On both server and client we restarted the DHCPd service on the server and networking on the client.  After 5 minutes or a little more it seemed to be working…. But
For each IP we changed in the dhcpd.conf file each new address worked on the client but for sum reason the old ip continued to function.  After 2 changes 3 ips were responding to ssh connections but the old IPs were removed from the dhcpd.conf
Looking into both of these things.

Until we get DHCP to function as expected I felt we should wait on the turning off of the old servers.

This week we got the new servers reversed in their physical locations in the A and B racks, to we reversed them.  Everyone was involved in the physical transfer of the systems.  

I was able to get 2 new network ports activated by campus IT for the 130.166.240.16/29 network.  One in each rack. 
The ports are Y12-6 and Y13-6  
Where the Y12 and 13 are the rack numbers and the-6 is the 6th port that is connected to the campus infrastructure.  Located at the top of each rack.  (The highest U location in the rack)
The campus is responsible for the activation of each of these ports and the network configuration on these ports and for any hardware problems.  The equipment is not actually in our rack just the wire termination.

------------------------
We added several new entries into the dhcpd.conf file to add static ip for hosts…

#########################
host new-fwa-enp4s0f0 {
  hardware ethernet 1c:c1:de:e6:b4:00;
  fixed-address 130.166.47.243;
}

host new-fwa-enp3s0f1 {
  hardware ethernet 1c:c1:de:e6:b4:0a;
  fixed-address 10.47.21.21;
}
########################################################

The first entry 47.243 worked but took a much longer time than we expected.
The 21.21 did not work at all.  


I am looking at both entries to figure out the issues.

I found a few commands that I thought I would add some notes on.
Ifdown, ifup, and ifquery can be used to turn on networking and also force IPs to be reloaded.  Ifconfig will not force the IP changes.  The problem with this command is that is does not work for us.  I think this has to do the the new version of Ubuntu that is the problem.  The port have been renamed from Eth0 - Eth3 but now they have name like enp3s0f0 for the Eth0 port.  All the ports are using a new designation.  This was not installed on our version and resulted in a port not found error no matter how we identified the port.

It was a little challenging when we killed a port that our network connection was working on.  
*** One thing that would be bad is if we issued a command like if down -a (all).  This would kill all network interfaces (except iLo)  and it would be impossible to reconnect.  If iLo was not working we would have to manually go to the MDF and use the console to restart the connections.  Luckily this did not happen.

Dhclient -r (and -v verbose) should also cause a network reset
On the server we issued    systemctl restart isc-dhcp-server
