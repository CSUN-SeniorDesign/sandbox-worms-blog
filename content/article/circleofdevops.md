---
title: "Circle of DevOps"
date: 2018-09-25T23:56:03-07:00
draft: false

categories: [CircleCI]
tags: [Project 2,CircleCi, CI, CD]
author: "Aubrey Nigoza"
---
**Background**
Continuous Integration (CI) and Continuous Delivery (CD) and two concepts that are often discussed together. CI is the process of integrating new codes as soon as possible and as often as possible to help improve collaboration. This is an automated process that integrates a new part of the source code right away. Unit testing is another concept that makes CI possible. The idea behind unit testing is that you break up the codes in multiple smaller units. Each units will have its own test. If a developer worked on a code, he can push it up to a branch and the CI  system will run its workflow to integrate the new code. This gives immediate feedback to the developer whether the submitted code will compile and build and if it broke the functionality of the particular unit that he worked on. This automated process of pushing the code, building the code, testing the functionality also allows for immediate feedback from other developers who may review the code. 

The other piece of this puzzle is the CD. CD is the process of delivering the application that passed the CI. This is an automated system that constantly deliver products for QA tester. In this way, every step of the way, testing and QA can happen immediately. 

Circle CI does all of these. In our project 2, I setup our CircleCI config file. 

**Key Terms**  

- *jobs*: A collection of steps that we want to run. Each job is its own container or machine (executor). It is like each job runs on its own computer (in this case a container). As such, configuration done on one job will not be passed or seen on the other jobs. However, you may persist certain data between jobs using workspace.  
- *executor* : Type of computer that the job should run under. It can be machine, docker or macos.
- *steps*: A way to define the tasks you want to do.
- *workflow*: An idea of running multiple jobs in parallel or serial or a combination. There are certain reasons why you want to do this. 

**Use Case**  


For example, let's say you have a list of different testing that you want to run. As the goal of CI/CD is to deliver as quick as possible and the tests are not dependent on each others, you may run the tests in parallel. You may create multiple jobs to run in parallel to test different aspects of the source codes. Ok, that's fine. Imagine you have 10 tests to run. You create 10 jobs that will run in parallel. Remember, each job is its own container and each job will go thru the process of building and compiling. So, you have 10 executors (containers/machine/macos) simultaneously building, compiling and testing. Since they are all building the same code, if there's an error in building, all 10 will fail. I think that is a fair assumption. Now, if you are paying for runtime of the executors, that will be wasting resource. This is where workflow comes into place.

Workflow will allow us to run a job that only builds, if it fail, great, you only ran 1 executor. If it succeeds, ok, lets go ahead and start building and testing on all the other 10 jobs. 

**Our Implementation for Project 2**

We are using workflow instead of just building out the steps in 1 job. In our case, since we only have 1 testing, it slows down the process. But, since this is a school project and the goal is to learn, it suits as well to try to understand workflow.

We have a build workflow:

	build:
	    docker: 
	     - image: felicianotech/docker-hugo:0.47.1 #use this particular image with hugo .47.1 . According to the documentation, it is best practice to always specify the version instead of using the keyword latest.
	    working_directory: /hugo
	    environment: 
	      HUGO_BUILD_DIR: /hugo/public
	    steps:
	     - checkout #this will get the code from our repo
	     - run:
	         name: "Run Hugo" #will run hugo on the container to build the static html
	         command: HUGO_ENV=production hugo -v -d $HUGO_BUILD_DIR --enableGitInfo --verbose --debug 

Our next job is test. We want to test first before we deploy:

	test:
	    docker: 
	     - image: felicianotech/docker-hugo:0.47.1
	    working_directory: /hugo
	    environment: 
	      HUGO_BUILD_DIR: /hugo/public
	    steps:
	     - checkout
	     - run:
	         name: "Run Hugo"
	         command: HUGO_ENV=production hugo -v -d $HUGO_BUILD_DIR --enableGitInfo --verbose --debug
	     - run:
	          name: Test Hugo Build Static HTML
	          command: |
	            htmlproofer $HUGO_BUILD_DIR --allow-hash-href --check-html \
	             --empty-alt-ignore --assume-extension --disable_external        

Next is a deploy job. If it is staging, it will deploy to staging. If it's master it will deploy in master but will require for a team member to approve it by clicking approve in CircleCI page.

  	deploystaging:
	    docker: 
	     - image: felicianotech/docker-hugo:0.47.1
	    working_directory: /hugo
	    parallelism: 3
	    environment: 
	      HUGO_BUILD_DIR: /hugo/public
	    steps:
	     - checkout
	     - run:
	          name: Install AWS CLI 
	          command: |
	            apk add --update python python-dev py-pip build-base
	            pip install awscli
	     - run:
	         name: "Run Hugo"
	         command: HUGO_ENV=production hugo -v -d $HUGO_BUILD_DIR --enableGitInfo --verbose --debug
	     - deploy:
	          filters: 
	            branches:
	                ignore: /pull\/.*/
	          name: Tarball the public
	          command: tar -zcvf /hugo/${CIRCLE_SHA1}.tar.gz -C $HUGO_BUILD_DIR . 
	     - deploy:
	          filters: 
	            branches:
	                ignore: /pull\/.*/
	          name: deploy to AWS
	          command: |
	            if [ "${CIRCLE_BRANCH}" = "project2_staging" ]; then
	              aws s3 cp /hugo/${CIRCLE_SHA1}.tar.gz \
	              s3://sandboxworms-packages-92618/staging/${CIRCLE_SHA1}.tar.gz
	            else
	              echo "Not master branch, dry run only"
	            fi
  	deploymaster:
	    docker: 
	     - image: felicianotech/docker-hugo:0.47.1
	    working_directory: /hugo
	    parallelism: 3
	    environment: 
	      HUGO_BUILD_DIR: /hugo/public
	    steps:
	     - checkout
	     - run:
	          name: Install AWS CLI 
	          command: |
	            apk add --update python python-dev py-pip build-base
	            pip install awscli
	     - run:
	         name: "Run Hugo"
	         command: HUGO_ENV=production hugo -v -d $HUGO_BUILD_DIR --enableGitInfo --verbose --debug
	     - deploy:
	          filters: 
	            branches:
	                ignore: /pull\/.*/
	          name: Tarball the public
	          command: tar -zcvf /hugo/${CIRCLE_SHA1}.tar.gz -C $HUGO_BUILD_DIR .
	     - deploy:
	          filters: 
	            branches:
	                ignore: /pull\/.*/
	          name: deploy to AWS
	          command: |
	            if [ "${CIRCLE_BRANCH}" = "master" ]; then
	              aws s3 cp /hugo/${CIRCLE_SHA1}.tar.gz \
	              s3://sandboxworms-packages-92618/master/${CIRCLE_SHA1}.tar.gz
	            else
	              echo "Not master branch, dry run only"
	            fi
		workflows:
		  version: 2
		  setup-build-package-and-deploy:
		    jobs:
		      - build
		      - test: #useful if there are multiple tests, so we can run them together
		          requires:
		            - build
		      - deploystaging:
		          filters:
		            branches:
		              only: project2_staging
		          requires:
		            - test
		      - hold:
		          filters:
		            branches:
		              only: master
		          type: approval
		          requires: 
		            - test
		      - deploymaster:
		          filters:
		            branches:
		              only: master
		          requires:
		            - hold


**Upcoming features**

- Create a text file that indicates the latest build and upload to s3
- Figure out whether to test commit in other branches or only when it is doing pull request.
- Alert team members of on hold jobs.