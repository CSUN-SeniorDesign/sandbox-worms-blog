---
title: "Week2 - Putting it all together"
date: 2018-09-05T22:46:48-07:00
draft: false

categories: [Project 0]
tags: [vpc, aws]
author: "Aubrey Nigoza"
---
2nd week is just coming to close. Project 0 is 98% done, pending some documentations. It feels like we just finished the "main" project of a course. We built everything, from purchasing the domain, nameserver setup, setting up the network, setting up workflow for collaboration, setting new virtual machine, setting up apache, and ending with a finished website accessible in the internet. But, this is just the beginning of the course. It is hard to put in to words the excitement that I and most likely my remaining classmates are feeling. In this post, I am going to talk about two concepts that I hope will stick to my long term memory better because I wrote about it.

Let's start with SELINUX. Because we chose to use Red Hat Linux 7 for our EC2 instance, SELINUX comes configured. 
        
     
	# This file controls the state of SELinux on the system.  
    # SELINUX= can take one of these three values:  
	#     enforcing - SELinux security policy is enforced.  
    #     permissive - SELinux prints warnings instead of enforcing.  
    #     disabled - No SELinux policy is loaded.  
	SELINUX=enforcing  
	# SELINUXTYPE= can take one of three two values:  
	#     targeted - Targeted processes are protected,  
	#     minimum - Modification of targeted policy. Only selected processes are protected.  
	#     mls - Multi Level Security protection.  
	SELINUXTYPE=targeted

Above is the default configuration of SELinux. I came across SELINUX when I could not figure out why APACHE would not serve my html files. I checked the file and folder permissions and they were correct, as far as I know. After few rounds of google searching, I came across SELinux. SELINUX puts a context on files. I understood context as the way SELINUX understands the purpose of the files. By default the files in the /var/www/html have the correct context as web files. SELINUX will allow APACHE to serve the files in there. 

What happened in my case was that, I copied the html files from my local computer to my home directory in the instance. From the home directory, I moved them to /var/www/html. With this process, the context on the files would be wrong. To fix the issue, I need to put them to the right context, http_sys_context. Because the files are in /var/www/html, I can just restore the context without specifying what I want it to be and Linux will put the correct context. This is done by ```sudo restorecon -R /var/www```. 

```ls -Z``` will show the context of the files in the working directory.

 
```sudo semanage fcontext -a -t httpd_log_t "/var/www/html/.*\.log.*"``` This command will set the context for any .log files in the /var/www/html . With the correct permission and acl, this will allow APACHE to write to a log file in the /var/www/html. 

As we can see, SELINUX expects certain files to be in a certain folder. If the files are not expected to be in a folder, then the context must be set. There are more to learn about this subject but this knowledge was enough to bring up our websites.

Another thing that I wanted to talk about is our experience in collaboration. Because of our conflicting schedules, we, as a team, did not have many face to face meetings. We had to work remotely and collaborate remotely at different times. There are few challenges in this scenario but our experience showed us the benefits of being able to collaborate remotely. Without tools like github and slack, we would not have been able to properly complete the project. Besides the use of these tools, we also needed to setup our system that allows access to multiple people and do a peer review of the work done. Peer review in a small team like ours and relatively small project is a little bit annoying (to say the least). For example, I made a batch of changes. Pushed it to the repo. I asked for pull request. All good at this point. But, moments later, I noticed that I made a typo on my post. I only need to fix one word, do I really have to create new branch and request for peer review? 

An example of setting up a system with respect to multiple people collaborating is the folder permissions in our DocumentRoot. Permissions had to be set so that, if memberA deployed our static html on day 1. On day 2, all other team members should be able to delete or modify these files without sharing credentials and without having to change the permission on the file. It becomes a little bit complicated when we factor in that we also need to give Apache r-- access to the files and r-X access to the folders. This was a welcome challenge and forces us to think back to lessons learned in CIT 210, 360 and other classes. 

We settled in to creating a webadmin group. Give user rwx access, group r-- and other r-X on File permission. In the ACL, give user rw, group rwx and other r--. The capital X, allows for execute on directories only. We also set it so that any files or directories created in the folder will have a group of webadmin.

It looks like this

	# file: index.html
	# owner: john
	# group: webadmins
	user::rw-
	user:ec2-user:rwx               #effective:r--
	group::rwx                      #effective:r--
	mask::r--
	other::r--


	-rw-rw-r--+ 1 john webadmins  8413 Sep  6 06:45 404.html
	drwxrwxr-x+ 9 john webadmins  4096 Sep  6 07:43 article
	drwxrwxr-x+ 7 john webadmins  4096 Sep  6 07:43 author
	drwxrwxr-x+ 3 john webadmins    58 Sep  6 07:43 categories
	drwxrwxr-x+ 3 john webadmins    82 Sep  6 07:44 dist
	drwxrwxr-x+ 2 john webadmins    26 Sep  6 07:44 img
	-rw-rw-r--+ 1 john webadmins 15763 Sep  6 06:45 index.html
	-rw-rw-r--+ 1 john webadmins   972 Sep  6 06:45 index.json
	-rw-rw-r--+ 1 john webadmins  5080 Sep  6 06:45 index.xml
	drwxrwxr-x+ 4 john webadmins    24 Sep  6 07:44 page
	-rw-rw-r--+ 1 john webadmins    13 Sep  6 06:45 robots.txt
	-rw-rw-r--+ 1 john webadmins  2942 Sep  6 06:45 sitemap.xml
	drwxrwxr-x+ 4 john webadmins    63 Sep  6 07:44 tags



  
