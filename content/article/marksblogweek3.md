Marks-Blogpost-Week3                                                      Sept 14,2018

This week was another challenging week.  I have worked on 
Installed Terraform on my laptop.
Basic format for using terraform is to run
   init
   plan
   apply
init command needs to be run the first time in each folder
After that we only need to make changes and run plan and apply.

Terraform code for 
   VPC network  configuration this includes set up of:
       Internet Gateway 
       Route tables for private and public subnets
       Private subnets
       Public subnets
       Security group for limiting access from the public subnets to the private subnets

This part was quite daunting.  There are so many pieces to think about.  The subnets were pretty straightforward to get.   Using variables made life a bit more complicated in the short run.  The 3 public ones were 
10.0.16.0/22
10.0.32.0/22
10.0.48.0/22
And the private ones
10.0.64.0/20
10.0.80.0/20
10.0.96.0/20
The route tables were confusing and I needed help from Aubrey.

    Found out that each of the directories had to have the key files for access to AWS.  The trick to not make them public is to use gitignore in the root of our local files.  Our keys got pushed to our Git repository and someone found them within ½ hour.  We had charges for the next 24 hours that came to over 65$.  Luckily we discovered it quickly and invalidated the keys within the next 30 min but it took over 4 hours to find all the instances in other zones. Aubrey ended up helping me with most of the VPC.

   Bastion instance
I started the coding for this.  Very simple on the surface.  We will be bringing up our instances tomorrow if all goes well.
   Web server instance
I started the coding for this.  Very simple on the surface.  We will be bringing up our instances tomorrow if all goes well.


     One of our biggest problems is that each of our schedules is very different.  Even if we are online at approximately the same time we still seem to be waiting a long time for responses.  We  need to communicate more effectively and to offer help more frequently. 

     I also set up my first VM of Cent OS.  My first install was a minimal install that only included command line ability.  The networking did not work from the start.
I found the by default the network interface was disabled. 
 **** Had to run the command
 ifup erp0s3 
to enable the interface.
I then downloaded “the everything” ISO (8 gigs) and did a reinstall to get all the gui tools that I could find that were somewhat useful to me. 
Lol, did not think I had much to write, guess I did.
