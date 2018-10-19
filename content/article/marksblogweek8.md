---
title: "Mark's Blog Week 8"
date: 2018-10-18T14:03:01
draft: false
Categories: [Project 3]
Tags: [Project3]
Author: "Mark Siegmund"
---

Marks Blog Week 8								October 18, 2018
Docker Containers
This was the first thing that caught my attention this week.  A docker container starts with a core image that could be a linux flavor or even windows.  I have yet to try it but it looks as if you can even run a windows container if you are on a Win 10 Pro Operating System… for free?  I guess MS is losing out to Linux due to the Docker movement.   Anyway we choose to run Alpine Linux.  The basic image file to start with is about 4 Meg!!  This did not work for our needs and we opted to use httpd alpine linux which is a 128M and has apache in the image and the services it needs.  I setup docker on a vm running Ubuntu.  Actually the one I used for Ancible.  I did this mostly for the reason that running docker for windows would stop me from using virtualbox.  To begin with I downloaded some linux images.  The first of which was Alpine (the  4 meg image).  I was able to install bash (only bourn shell was there).  The first basic concept I need to understand is that when you run a command on a docker container it is done and dead.  So the trick is to run a bash session but not to end it.  To leave it running with stuff installed you need to do and CTRL p and then a CRTL q.  This puts you back to your host system and leave the container still running.   I left a bunch of used docker containers and found an easy command to use to clean them up.
   sudo docker rm $(sudo docker ps -a -f status=exited -q)


Trying to understand Circle CI stuff this week.
Circle Ci seems to be a central service that is key to Changes in the git repository.  When a changes is posted in our blog 

The circleci file config.yml controls so much….
For this part of the Project 3 it controls docker image creation and saving to the ECR.
Tagging also takes place and is needed for separation of staging and productions sites of our blog.

Sets up environment variables
What machine does this happen on?
Install awscli
Export repository env
Builds a docker image

- run:

         name: load archived image


         command: |


           docker load -i docker-image/image.tar


run:


         name: Tag stage S3


         command: |


             echo "$CIRCLE_SHA1" > stagebuild.txt && aws s3 cp stagebuild.txt s3://sandboxworms-packages-92618/staging/stagebuild.txt --sse=aws:kms --sse-kms-key-id ec0a605d-17e9-4ac8-a2e3-e299c1ee1a93


Saves(pushes)  docker image to ECR (Elastic Container Registry)


Tested our blog website to see if a blog could be edited or deleted.  There should be no problem doing this just verifying that it works as expected.  I created a bogus entry and posted it, then removed it.  This leaves a trail in Git but the website is changeable.


I also worked on Cricle CI this week.
This runs for when autoscaling takes place or when system is restarting.

This runs when someone push a post in the staging or master branch of our blog repository.

Circle ci is at the heart of this system.  On this project I got to learn about Circle CI.  

***** It took awhile to figure out how to build the image in CircleCi. At first we thought that we need to push the locally-built image to docker hub and then access the image from circleci. That doesnt make sense because that means we always have to make an image locally. The answer was simple. We put the Dockerfile in the blog repository. Let’s go thru step by step and see how this helped us.

Spin up the environment: 
This is where the primary container starts up.  If cached this is quick.  If not cached this can take a significant amount of time.  Our choice of container is an image maintained by CircleCi. I believe it si Alpine based with python and some other development app. It has a size of 170 meg.  Just a thought but a docker Windows image would probably be much larger…. A bad choice.

Checkout code :
Special step used to check out source code to the configured path (defaults to the working_directory). The reason this is a special step is because it is more of a helper function designed to make checking out code easy for you.


Setup a remote docker engine:
a new remote environment is created, and your primary container is configured to use it.


Install awscli:
We chose an image with python so we can pip install  aws cli.


Export repository env:
This step is just a different way of exporting environment variable.


Build image installs these items: THese are some of the utilities that we need to build our code. Some of them to push to AWS, some for Hugo Extended to build our site properly. We are also pull the official hugo binaries.    (and more)
Misc utils
Make
G++
Open ssh
Curl
Rsync


Save image to an archive:
We are saving the image to 
docker save -o docker-image/image.tar $FULL_IMAGE_NAME
Because an image consists of multiple files or layers and a JSON manifest, in order for docker to save it, it compresses all the file in a tar file. The reason we chose to save the image to an archive instead of pushing it directly to ECR at this point is because we wanted to have a separate jobs for deployment. This will also allow us in the future to put more testing in place. The idea is to pass the along the image to the next job so it doesnt have to be rebuilt. If this was a more complex application, like a Python based application, this will allow us to move the built code to the next job without pushing over the other utilities. 


Persisting to workspace:
To increase the speed of your software development through faster feedback, shorter reruns, and more efficient use of resources, configure Workflows.
This is the key to moving the image around. In this step we tell CicleCi to persist a folder. Persisting a folder means that we can attach the folder on a different job. We chose the folder that contains our docker image.

Moving on to the next jobs.
We have deployed staging and deploymaster. They are almost the same except the deploymaster requires an approval before it can go thru. The first step is to attached the persisted workspace and use docker load to uncompress the image. 

We are using aws cli and docker built in command to push the image. We are pushing the image to AMAZON ECR. 

After that, we are pushing a text file that contains the sha of this image to S3.



