---
title: "Johnblog19"
date: 2019-02-15T13:04:28-08:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
This week we mainly focused on preparing for the server migration. In addition to my research on DNS primary-secondary configuration, I also did further research on DHCP and what needed to be done before transferring the files. For DNS, the zone files need to have their serial attribute incremented to register the change when a zone transfer occurs; this is something to consider when creating the automation for deployment. For DHCP, because the MAC address in the new machine is different, we need to change the MAC address listed in the named.conf.local file. For our convenience, we made 2 blocks and commented out one with the old address; this is so that in the event that we need to rollback and use the old firewalls, we have the option to do so. This and more is mentioned in the documentation added to Gitlab this week, which can be edited should more steps need to be recorded. This documentation is a part of a larger directory that should contain the documents on the rest of the services that are moved during the migration.
