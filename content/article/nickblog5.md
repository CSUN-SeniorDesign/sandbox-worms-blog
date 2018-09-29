---
---
Looked up the CircleCI website, how it works, tried to get a grasp of the general idea, but didn't divulge into it too much because I wasn't tasked with this part of the project. Did some research on DataDog - what it is, what it does, how it's used, how to get it installed on our webservers properly - and signed up for an account since this was one of my tasks for this project. It didn't seem too difficult, matter of fact, it looks like as long as we're able to get it installed properly, the drag and drop UI seems extremely user friendly. After further discussion, I was asked to setup an environment like the one we'd be setting up on AWS, except I’d be using CentOS 7 spun up through a VM on VirtualBox on my local machine. Once installed, I was asked to figure out how to grab the tarball (.tar.gz) from our organization’s proper s3 bucket, extract it to a directory I created, search and locate a specific file, compress it using proper formatting, and send it back to the s3 bucket.
Since I was mainly working on creating a VPC instance using Terraform in the last project, I was asked to work on the Ansible portion of this project. So I setup CentOS 7 on my local Windows machine using VirtualBox as stated above. Installed and configured both Apache and AWSCLI. I was able to pull the tarball from the proper AWS s3 bucket, but ran into an error about the file not being a bzip2 file. After brief Googling, I found that the file was already extracted, which my groupmate was able to confirm, and saw that I was running an incorrect command. My groupmate was able to point me in the right direction. 
Below are a collection of the websites (at least the ones I remembered to save) I used for research.   

Websites:
	CircleCI:
		https://circleci.com/
		https://segment.com/blog/rebuilding-our-infrastructure/

	Packer:
		https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh
		https://www.packer.io/docs/templates/index.html
		https://www.packer.io/docs/templates/builders.html

	Setting up a staging environment:
		https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
		https://dltj.org/article/software-development-practice/

	Extract tarball from s3 bucket on simulation environment - in our case, CentOS 7: 
		https://acloud.guru/forums/aws-certified-developer-associate/discussion/-L-Sao1-GUHsxsA0Vbv_/How%20to%20decompress%20a%20file%20in%20s3%20using%20CLI
		https://docs.ansible.com/ansible/2.3/s3_module.html
		https://stackoverflow.com/questions/46236776/aws-cli-is-there-a-way-to-extract-tar-gz-from-s3-to-home-without-storing-the-t
		https://unix.stackexchange.com/questions/61461/how-to-extract-specific-files-from-tar-gz
		https://forums.aws.amazon.com/thread.jspa?messageID=599647 (specifically the dialogue located in the middle of the page titled "Re: CentOS 7 and CloudFormation")
		https://www.interserver.net/tips/kb/extract-tar-gz-files-using-linux-command-line/
		https://superuser.com/questions/362525/tar-shows-is-not-a-bzip2-file-error-when-uncompressing-a-tar-bz-file

	DataDog:
		https://www.youtube.com/watch?v=mpuVItJSFMc
		https://www.youtube.com/watch?v=uI3YN_cnahk&list=PLdh-RwQzDsaOoFo0D8xSEHO0XXOKi1-5J
		https://www.youtube.com/watch?v=xKIO1aWTWrk&list=PLdh-RwQzDsaOoFo0D8xSEHO0XXOKi1-5J&index=2
		https://www.youtube.com/watch?v=U5RmKDmGZM4&list=PLdh-RwQzDsaOoFo0D8xSEHO0XXOKi1-5J&index=3
		https://docs.datadoghq.com/agent/basic_agent_usage/amazonlinux/?tab=agentv6
		https://app.datadoghq.com/account/settings#agent/aws
		https://www.datadoghq.com/blog/monitoring-ec2-instances-with-datadog/
		https://hub.docker.com/r/datadog/docker-dd-agent/
		https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh