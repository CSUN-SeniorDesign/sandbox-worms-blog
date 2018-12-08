---
title: "Mark's Blog Week 15"
date: 2018-12-06T22:01:01
draft: false
Categories: [Project 7 Sprint 4]
Tags: [Mark Siegmund / Sandboxworms /  Week 15]
Author: "Mark Siegmund"
---

Marks Blog Week 15								December 06, 2018
Wow, here we are, the last blog of the semester.  I took the the network map Neel made and modified it.  I started with the image Neel used for the presentation last week.  I tried using Visio but got bogged down on the connectors.  First they had arrows.  I could not find a way to get rid of the arrows.  Then I found the arrows and tried several arrow types ending with the Multi-Arrow and the Multi-line.  Both of the multies did very strange things when manipulating their bending points.  They simply did not work the way I wanted.  Then I tried Lucid charts.  This looked promising but quickly ran into a snag.  This a form of cripple ware.  There is a max of 60 elements that can be added.  I might have been able to complete it but if anyone wanted to modify it later they may have run into a problem, so I scrapped this one.  I then tried a Google Drawing.  I got a basic framework set up.  This would work but I decided to continue with a mock up like Neel did, adding in all the changes that I wanted.  I thought it would be a good idea to have John use his Photoshop skills in making the document.

There are 5 networks
130.166.47.0/24     CSUN subnet - given to Advancing Tech Lab?

130.166.240.16/29  CSUN network given to Advancing Tech Lab
130.166.240.24/29  CSUN network given to Advancing Tech Lab
Having a CIDR network of  /29  means that only 3 bits can be use for the network nodes. Ie If we started with a full /24 subnet then breaking it up into /29s would mean that 
0-7 is one network
8-15
16-23
24-31
Dividing up the network this way may help with some intention that I do not understand at this time.  It may have been implemented by Professor Wiegly or some student that worked on it in the past.  One downside of this choice is that for each network of 8 two address can not be used.  One is for the network or a .0 (all bits set to 0) and the last is the the broadcast (or all bits set to 1).  In a /24 we would lose only 2 addresses but if network is divided into all /29s we lose 32 addresses.  It looks like the addresses are set to /29 only on the firewall A and B machines.

10.47.0.0/16  (firewalled)
10.1.47.0/24  (Firewalled - heartbeat)  connected in the MDF rack by a single network cable- no switch

There are 2 racks
HP L1a  (10.47.0.11)              HP R1a  (10.47.0.12)
HP L2a  (10.47.0.13)             HP R2a   (10.47.0.14)
HP L3a  (10.47.0.15)             HP R3a   (10.47.0.16)

Moodle (130.166.47.10)  i believe this is a left over machine from Moodle project is now defunct - server is working but Moodle is not being used)  The interface is a bridge interface.  Not sure why this is.  Possible use with VMs?


Firewall A (5 interfaces)
Eth1  130.166.47.2
Eth1  130.166.47.1
Eth0  130.166.240.19
Eth0   130.166.240.18
Eth2    10.47.0.2
Eth3   10.0.47.2 (heartbeat)
Ib0      192.168.47.2  Iinfiniband
*** InfiniBand (abbreviated IB) is a computer-networking communications standard used in high-performance computing that features very high throughput and very low latency. It is used for data interconnect both among and within computers. InfiniBand is also used as either a direct or switched interconnect between servers and storage systems, as well as an interconnect between storage systems.[Wikipedia]  I has special network cables that are different from standard CAT5 twisted pair for ethernet.



Firewall B
Eth1  130.166.47.3
Eth1  130.166.47.4
Eth0  130.166.240.29
Eth0   130.166.240.28
Eth2    10.47.0.3
Eth3   10.0.47.3 (heartbeat)
(no InfiniBand interface)





---------------------------------------------------------------------------------------------

New names will be:   firewall2a and firewall2b

Lilo  press F8 key every few seconds after Thermal Calibration
***** if you miss the window for pressing F8 you will have to start over.   There is no prompt for pressing  the F8 key!!!!!

Reset Lilo password to the one on the case.
Default DNS name:    ILOUSE051N7VC
Default user name is:  administrator (not case sensitive)
Password is on the case cover

P410i RAID controller (and config)  **** also needs F8 to be pressed.  When you go into Lilo exit and start pressing the F8 key immidately.

When you get into the menu system choose view config so you can see what is there.

We are going to then delete the old config
With only 3 drive the best to do is to Mirror 2 drives and use the 3rd as a hot spare.

The setting is like this:
    Delete the 3rd drive (remove the X)  Panel 1
    Select Mirror RAID 1+0
    Select spare
    Select 8GB max boot partition
    Save
    Exit




Installed Ubuntu 18.10 LTS
    Boot from USB
    **** If starting from a powered off state this will take2-5 minutes of waiting just for the screen to flicker (this is normal)
     Select English language and keyboard
     Leave Proxy blank
     For Ubuntu archive mirror address - choose the default
     Choose entire disk for the install
     
    

 Final install screen info
         ServerYour name:  Techlab
         Server name:   firwall2a
         User name:    techlab
         Password:    (standard one)
         Snap-in:  Microsoft Powershell
         
     

System problem:
One server seems to have a problem with one of the NICs.
eth0 kinda works.... it does DHCP and gets an address but will not ping ANYTHING (except itself).  Not even the gateway.

All 3 other interfaces are working fine.  I use the same network cable and wall jack on all 4 interfaces.  Only eth0 is a problem.

I have restarted the machine several times.  I have installed and reinstalled the OS (Ubuntu 18.10 LTS) 2 additional times.

Problem is always the same.  I can’t ping in or out but IP, netmask, gateway all looks fine.  The only conclusion I can come to is that the interface is not functioning correctly.  Again, today I worked on this and got not further.  To work around the problem I am going to get a PCIe NIC (single, dual, or quad port).  After discussing with Professor Weigly in using this solution a duplicate card needs to be setup and used in the 2nd server.  Today I went over to the MDF and looked at the networking and power and space that was needed to install the servers.



Need to do:
Get 2 NICs, one for each of the servers, keeping the hardware configuration exactly the same.
Follow up on MaaS
Work on final presentation
For next semester. 
    Finish DHCP
    Finish DNS
    PXE server
    LDAP configuration 
   Gluster configuration 
   MaaS
