---
title: "Nick's Blog Week 6"
date: 2018-10-05T20:56:19-07:00
draft: false

categories: [Project 2]
tags: []
author: "Nicholas Yoon"
---
I continued working within my own local Linux environment setup through my VirtualBox VM. Still working on the bash scripting trying to figure out how to pull the correct object from our organization's s3 bucket, extract the contents, compare to current build, if different destroy old build and deploy new build. It's extremely frustrating because the pace of this class isn't slowing down and I keep falling more and more behind because I'm not understanding the material. There's really only one person that fully understands the entire process of what's going on within our group, another person that's able to keep up, but for the rest of us we're constantly playing catch-up and it's getting to be increasingly frustrating. I've been exposed to much more industrial related projects within six weeks of attending this class, than I have my entire tenure here at CSUN. The exposure is great, but once again, if we could get a bit of guidance it would be very much appreciated. Nonetheless, I've got a better idea of bash scripting, still novice status, nowhere near where I need to be. The most frustrating part is not being able to produce as much as I'd like for the group. I'm not one to take credit for work that I haven't performed and the fact that I feel obsolete to this group doesn't sit well with me. I was able to pull the latestbuild.txt file from our organization's s3 bucket. Then I extracted the contents and pushed it to another text file within a staging directory that I created on my local VM. From there, I was a bit confused as to what I was supposed to compare the tarball with, since there was nothing to compare to. From there I needed to figure out how to deploy it to the webservers, but also ran into issues with that part. I need to work on logically listing out all the steps that go into making the task we need done, work as a script. I have to learn to walk through my code, one step at a time, instead of getting too ahead of myself.  

Type tar xvfz <file.tar.gz> to extract tarball to current directory. 

Download tarball and extract using this command:
	
	aws s3 cp s3://sandboxworms-packages-92618/staging/3392ce19055491c93e75c939547ed0fd17cc7d91.tar.gz 3392ce19055491c93e75c939547ed0fd17cc7d91.tar

List contents of a tar file:
	
	$ tar -tvf file.tar

Websites:  
	View contents of tarball:
		https://www.cyberciti.biz/faq/list-the-contents-of-a-tar-or-targz-file/
		https://www.pendrivelinux.com/how-to-open-a-tar-file-in-unix-or-linux/
		http://www.rebol.com/docs/unpack-tar-gz.html
		https://www.pendrivelinux.com/how-to-open-a-tar-file-in-unix-or-linux/
		https://www.cyberciti.biz/faq/list-the-contents-of-a-tar-or-targz-file/
		http://how-to.wikia.com/wiki/How_to_untar_a_tar_file_or_gzip-bz2_tar_file
		https://unix.stackexchange.com/questions/96410/search-for-a-file-inside-a-tar-gz-file-without-extracting-it-and-copy-the-result
		https://stackoverflow.com/questions/30651502/how-to-get-contents-of-a-text-file-from-aws-s3-using-a-lambda-function

	Grab text file from s3 bucket:
		https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html
		https://cloudacademy.com/blog/how-to-deploy-application-code-from-s3-using-aws-codedeploy/
		https://www.botmetric.com/blog/ultimate-cheat-sheet-deployment-automation/
		https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/s3-example-bucket-website.html
		https://stackoverflow.com/questions/30876123/script-to-download-file-from-amazon-s3-bucket
		https://stackoverflow.com/questions/31077593/how-do-i-use-shell-script-to-check-if-a-bucket-exists
		https://gist.github.com/chrismdp/6c6b6c825b07f680e710
		https://github.com/mietek/bashmenot/tree/master/src
		https://stackoverflow.com/questions/44026080/shell-script-for-deployment-in-aws-s3
		https://stackoverflow.com/questions/33112081/upload-to-s3-via-shell-script-without-aws-cli-possible
		http://tmont.com/blargh/2014/1/uploading-to-s3-in-bash
		http://tmont.com/blargh/2014/1/uploading-to-s3-in-bash

	AWS CLI:
		https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html
		https://aws.amazon.com/tools/

	DataDog:
		https://app.datadoghq.com/account/settings#agent/debian
		https://app.datadoghq.com/account/settings#agent/aws
		https://app.datadoghq.com/screen/461711/project2?page=0&is_auto=false&from_ts=1538709000000&to_ts=1538712600000&live=true
		https://docs.datadoghq.com/agent/?tab=linux
		https://github.com/DataDog/datadog-agent/blob/master/docs/agent/upgrade.md

	Apache: 
		https://httpd.apache.org/docs/2.4/env.html