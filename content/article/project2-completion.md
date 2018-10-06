---
title: "Project2 Completion"
date: 2018-10-02T20:31:55-07:00
draft: false

categories: [Project 2]
tags: [packer, ansible, bash]
author: "Aubrey Nigoza"
---
# Putting it All together #

In this week, we had to piece together all the different parts of the project that was done separately. The Ansible playbook that I modified from Project 1 to fit the requirements in Project 2 was used by John for creating a base AMI for Packer. The baseAMI was used by Mark on the Launch Configuration of the Auto Scaling Group. This gave our Infrastructure to automatically bring up new webservers that are already configured. 

One of the key thing that we discovered was that when Packer runs the playbook, even though we put host: all, it only targets the instance that it creates. This is important because it allows us to easily configure the instance and not modify the other instances.

The Playbook:

- 2 Roles: geerlingguy.apache and datadog.datadog
- The apache configurations were saved in a variable file that was being used by the role: geerlingguy.apache

        vars_files:
            - group_vars/main.yml
        roles:
                - { role: geerlingguy.apache, become: yes }
                - { role: Datadog.datadog, become: yes}

- After Apache and Datadog has been installed, different modules were ran to perform multiple tasks.


        
        tasks:
            - name: add authorized keys
            authorized_key:
                user: ec2-user
                state: present
                key: "{{ item }}"
            with_items:
                - https://github.com/Mark-Quark.keys
                - https://github.com/aubreynigoza.keys
                - https://github.com/nky9514.keys
                - https://github.com/jpvinuya.keys

- Python-pip was installed in the server to handle the installation of awscli. AWSCLI will be used by the script to get the s3 files.

            - name: install epel-release repo for python
            become: yes
            yum:
                name: https://dl.fedoraproject.org/pub/epel/epel-release-latest-{{ ansible_distribution_major_version }}.noarch.rpm
                state: present
            - name: Import EPE GPG Key
            become: yes
            rpm_key:
                key: https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-{{ ansible_distribution_major_version }}
                state: present
            - name: 'Install Python PIP'
            #tags: 'aws-cli'
            become: 'yes'
            yum:
                name: python-pip
                state: latest
            - name: 'Install AWS CLI'
            #tags: 'aws-cli'
            become: 'yes'
            pip: >
                name=awscli
                state=latest

- Directories were created for the 2 websites and for staging the tarball files. 

            - name: Creates directory
            become: yes
            file: path=/home/packages state=directory
            - name: Creates staging_sandboxworms
            become: yes
            file: path=/var/www/vhosts/staging_sandboxworms state=directory
            - name: Creates prod_sandboxworms
            become: yes
            file: path=/var/www/vhosts/prod_sandboxworms state=directory
- The scripts were copied to the instance. These scripts will be ran by the cron job. The scripts would deploy the website files from S3. 

            - name: copy staging script
            become: yes
            copy:
                src: stagingdeploy.sh
                dest: /etc/cron.d/
                owner: root
                group: root
                mode: 0744
            - name: copy prod script
            become: yes
            copy:
                src: proddeploy.sh
                dest: /etc/cron.d/
                owner: root
                group: root
                mode: 0744
            - name: Run staging script every 5 minutes
            become: yes
            cron:
                name: "staging deployment"
                minute: "*/5"
                job: "/etc/cron.d/stagingdeploy.sh &>> /home/packages/stagingcron.log"
                state: present
            - name: Run prod script every 5 minutes
            become: yes
            cron:
                name: "prod deployment"
                minute: "*/5"
                job: "/etc/cron.d/proddeploy.sh &>> /home/packages/prodcron.log"
                state: present

## Deployment Script Logic
1. Download latestbuild.txt file from S3.
2. If it doesn't exists locally, it will download the tarball listed in the latestbuild.txt file.
3. If latestbuild.txt exists locally, use diff to compare.
4. If same, exit.
5. If different, download the tarball listed in the downloaded latestbuild.txt from S3.
6. Delete the existing latestbuild.txt.
7. Unpack the tarball to the correct location.
