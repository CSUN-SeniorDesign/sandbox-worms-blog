---
title: "Johnblog24"
date: 2019-03-29T16:48:09-07:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
After coming back from break, we completed networking on the new firewall A; we changed each of the IPs to mirror the old firewall and at one point left old A off. At the moment, there are still unresolved problems with DNS resolving, one of which was demonstrated during the presentation on monday. Currently we are working implementing the same changes on new firewall B, and turning off the old B to test these changes. At the moment, we also changed the IPs on old A to mirror the former netplan of new A. New A is accessible through the 240.19 and Old A is accessible through 240.22. This week we did the same for new B and old B, changed the netplan of new B to old B's former netplan; they are currently accessible at 240.20 for new B and 240.22 for old B. Regarding the floating IP .18, we tested the redirect and ssh-ing into it leads to old A, despite changing the netplan on both old and new A, .18 is always redirected to old A. There are a few theories to this, that there is an IP table or rule causing anything that accesses the .18 to be redirected to old A, or if the redirect is made via MAC address, which is another configuration entirely.
