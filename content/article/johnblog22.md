---
title: "Johnblog22"
date: 2019-03-08T04:13:29-08:00
draft: false
Categories: [Comp 481]
Tags: ["John Philip Vinuya", "Sandbox Worms"]
Author: "John Vinuya"
---
This week we had little time to work together on the new firewalls. First, we attempted to shutdown the old firewall A to see if the new firewall A is able to assign IPs in the absence of old A. However, there were complications when accessing ILO on the old firewall, as we had often accessed and documented methods for accessing ILO for the new firewalls, but the only time we had accessed ILO in the old firewalls was when we physically connected to them in the MDF. Interesting thing to note, when documenting, record the machine-specific ILO usernames to prevent confusion. It turns out that the usernames used to access ILO were different between the old and new firewalls and as we didn't set up the ILO in the old firewalls, we were unsure how the setup was made. Fortunately, we were able to access ILO eventually, but for this week, I set out to document the differences and specific options needed to ssh into ILO in the case we have to remotely shut off or bring back the firewalls. The documentation is currently on Gitlab, but is more or less available to append if issues come up.
