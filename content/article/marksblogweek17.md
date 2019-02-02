---
title: "Mark's Blog Week 17"
date: 2019-02-01T23:01:01
draft: false
Categories: [CIT481]
Tags: [Mark Siegmund / Sandboxworms /  Week 17]
Author: "Mark Siegmund"
---

Marks Blog Week 17					February 1, 2019

This week we formed our project scope and how the group members.  This was difficult as we broke up the group we had last semester.  On Tuesday 4 of us replaced the motherboard where the ethernet port was not functioning.  This involved moving the 56 gigs of RAM and the 2 processors over to the new motherboard.  We had to remove the heat sink grease and apply new grease.  To remove the old motherboard from the case we had to remove all the fan assemblies, the riser card adapter (this is for adding a PCIe option card) the power connector for the hard disk enclosure, and the cables for data transfer to the hard disk case.  The only issue we had was that one RAM module was not fully inserted in its slot.  The system came up with a memory error.  We simply turned off the system and fully inserted the memory module and restarted.
The memory module type is PC3 (or DDR3) ECC unbuffered.

On Wed Jon is going to work on the motherboard install and we are going to configure the RAID configuration to RAID 1 (Mirrored) with a spare.  We only have 3 146G SAS drives.  And last is to install Ubuntu Server 18.04.

John, Yerden, Mario, Thomas, and Neel worked on the servers in JD1109 with me on both Wednesday and Thursday.  Nick worked on the documentation of equipment in the MDF.  

In JD1109 we finished up the motherboard swap.  John got his hands dirty and performed almost an entire swap by himself.  We needed some heatsink grease for the CPUs and all the grease that I had was very pasty and dry.  It had to be trashed.  Mario brought some in on Thursday and it was much softer and very easy to apply.  We examined the memory configuration and tried working out the config details but it seems that the directions for populating the slots was not followed.  But it came from the factory this way and has been working without a problem.  We just need to find the exception to the rules posted on the case cover.  It is 56G for the RAM.  This is an unusual number.  I would expect that we went to 64 G and there are 2 empty slots and we are using 4G modules.  I wonder why it was not fully populated.  Still looking for some reason.



Raid configuration
On Wednesday we worked on the Raid configuration for motherboard we replaced.  In a rush to set it up on Tuesday the default RAID config was set to Raid0, or to stripe the drives.  We ended up with approximately 440G for the drive.  146GX3drives.  What we wanted was only 146G but using Raid 1 or 2 drives mirrored.  The third drive was for a spare.  3 drives would work if we wanted to set up Raid 5 but the internal raid controller only supports Raid 1 and 2 not 5.

OS install - Ubuntu 1804 LTS.
After getting the raid configurations set we installed Ubuntu 18.04.  The install went perfectly no problems were encountered during the install.  When done with the install we began testing the network ports, just as we had before.  We were surprised to find that NIC 0 was not working on the new motherboard, exactly as the problem was last semester that led to Professor Wiegley suggesting that the motherboard be replaced.  We now had 2 motherboards with NIC 0 getting an IP but would not even ping the gateway.  This seems so very unlikely that we tried a the latest Ubuntu OS v 18.10.  We found that the NIC0 was now working.  It was not a motherboard problem as we thought at first.  It seems to have something to do with the 18.04 OS.  So our solution was to upgrade the OS on both servers to 18.10.   Now we have an extra motherboard.

Lilo
We set the Administrator password on Lilo port to the password on the case.  We did this for both servers.  