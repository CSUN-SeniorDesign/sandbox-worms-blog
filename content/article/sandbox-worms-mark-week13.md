---
title: "Mark's Blog Week 13"
date: 2018-11-22T03:30:01
draft: false
Categories: [Project 7 Sprint 2]
Tags: [week13]
Author: "Mark Siegmund"
---

Marks Blog Week 13								November 22, 2018

This week I fell back to look at the larger project.  Lots of thinking about organization of the project and what our goals really are.  Asking myself basic questions about stuff that we want to cover and what we need to focus on.  I posted to the whole ATL group my thoughts in the hope it would spark some conversation.  Even if it is only in the network layout and discovery process we need to divide the work so that we do not duplicate it.  Here are some of my thoughts.

IP V6? Do we really need/want?
Network layout - subnets
  130.166.47/24
    10.47.0.0/16
    What services are needed … DHCP, DNS, 
    A network diagram showing how the firewall works and its connections
         
Split this work among all members so that everyone gets to undersdand the discovery process.
Using Nmap 
ip route grep | default   -  shows default gateway
Cd to /etc  then     -   cat hostname
 ifconfig, 
netstat -lt
Netstat -l     shows all listening ports
netstat -ie   similar to ifconfig
netstat -r   - shows routing table
, ping (IP)  is some (IP) system up (alive)
Nslookup (IP) - given IP - gives back DNS name 
nmap-sP 130.166.47.1-254   - show computers with running services
Nmap 130.166.47.10   shows ports in use on a computer 
map (IP) list all ports being used
route
ip adder - lists network information

Server Layout - (and clients) - (nmap)
      Service(s) enabled on servers
      Firewall A - 130.166.240.19

     Firewall B - 130.166.240.20
 
      Moodle? -  130.166.45,10  (is this server doing anything?)  Can we repurpose it for the PXE  server?

Security points
     Vlans
     DHCP
           Subnet configured

 Security by obscurity (masking service ports by substitution )
  Never trust default port settings- someone may try to mislead you by forcing a port to be used on a unaccepted port like ssh running on port 443.

Scope of Work for Project 6 - Advancing Tech Lab
     Provisioning - 
           (MaaS) - Metal as a Service look into using this
     
            Redundancy (failover services)
                   . create script for first round
                   . If we are running a cluster service this will go away or be modified.

           PXE server - setup so we can provide a service to do a network install

           Firewall  - look at firewall network connections create visual map
  
           DHCP (Dynamic) configure it to serve
 a range
Netmask
DNS Server
PXE Server
Set lease time
Set gateway address


           DNS (ddns) - dynamic DNS
               Set zones

           LDAP - Join to campus domain

           Gluster
