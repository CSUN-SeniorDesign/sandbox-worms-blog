---
title: "Mark's Blog Week 18"
date: 2019-02-08T18:51:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 18]
Author: "Mark Siegmund"
---

Marks Blog Week 18					February 8, 2019

More changes for the group this week.  Nick, Yerden and Neel are moving to the AWS project.  Nick did a great job cleaning up the documentation for the equipment in the 2 MDF racks!  
Network ports config in MDF.  The plan to connect the 2 new servers NICs was not completed yet.  We got two of the networks connected.  Found some very confusing network documentation.  On the heartbeat network, 10.0.47.x we got a very strange reading on the network with a device made by Fluke used to discover network switch information.  When using the Fluke to identify the switch port info the Fluke showed a completely different network than when the port was plugged into our ETH

We discovery of reversal of install order for the 2 new servers.  We will have to swap them next week.  Just one more thing to do.

Organizing work was a major priority this week.  
iLo config and some uses/ web access
Discussion with Jeff and Advancing tech group.
More understanding of DNS secondary updating.
Recovery for DNS is not so important since secondaries are set up.  I found out this week that this is done with zone transfers.  The key is the line
allow-transfer { 130.166.240.19; };
In the file /etc/bind/named.conf.local
This needs to be set to the primary’s IP on both the primary and secondary(s).

A second very import thing we will have to do watch is for the serial number in zone files.  Fo every change we make the SN must be changed (incremented) before the update can work on the secondary.  There is the procedure that we need to follow after making the zone file changes. 


/usr/sbin/rndc freeze
sleep 30
service named stop
sleep 30
rm -f /etc/named.data/bak.*
rm -f /etc/named.data/*.jnl
sleep 10
service named start
sleep 30
/usr/sbin/rndc thaw
 
I just found this and need to test it out over this weekend.


<<<<<<< HEAD


There is simply no way that we can complete the migration to the new servers this week.  
Files for DNS, DHCP and firewall can be copied over to the new servers and are in our home/ect dir.  The network configuration for Eth0 and Eth3 NICs are not done.  I was thinking that we need to have 2 ports turned on by campus IT for the front end connections to the campus network but just thinking about it we only need to take the 2 connections from the old server and move it to the new server.  We could do this also on the heartbeat network (Eth3).
Maybe we could try this after the presentation on Monday.  If not then this same week.
=======
There is simply no way that we can complete the migration to the new servers this week but we can get it ready  for early next week.  
Files for DNS, DHCP and firewall can be copied over to the new servers and are in our home/ect dir.  The network configuration for Eth0 and Eth3 NICs are not done.  I was thinking that we need to have 2 ports turned on by campus IT for the front end connections to the campus network but just thinking about it we only need to take the 2 connections from the old server and move it to the new server.  We could do this also on the heartbeat network (Eth3).
Maybe we could try this after the presentation on Monday.  If not then sometime this same week.
>>>>>>> b6fd02671baf2ccde8e4f9f51b811377e580ef01

 On Thursday (yesterday) Jeff came and talked to our group.  All the guys talked to him about different things they needed.  It was very good to see this.  Jeff talked about the VPN server that  was used to get to the iLo ports.  This needs to be check out and tested.  This is going to be done this week end.  This will be the first time I have had to generate a certificate on my own initiative. 