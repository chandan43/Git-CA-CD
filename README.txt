Getting started with GitLab CI/CD
================================

 GitLab offers a continuous integration service. If you add a .gitlab-ci.yml file
 to the root directory of your repository, and configure your GitLab project to 
 use a Runner, then each commit or push, triggers your CI pipeline.

 So in brief, the steps needed to have a working CI can be summed up to:

	1. Add .gitlab-ci.yml to the working directory of your repository
	2. Configure a Runner

Ref:- http://sj1gitsw1.caveonetworks.com/help/ci/quick_start/README  

1.Creating a .gitlab-ci.yml file
  ------------------------------
# vim .gitlab-ci.yml

#This file is a template, and might need editing before it works on your project.
#see https://docs.gitlab.com/ce/ci/yaml/README.html for all available options

# you can delete this line if you're not using Docker
#image: busybox:latest
stages:
    - build
    - testing
    - cleanup

before_script:
  - git config --global user.name "chandan"
  - git config --global user.email "cjha@xyz.com"

after_script:
  - echo "After script section"

build_n5drv:
  stage: build
  tags:
    - n5_drv
  script:
    - /home/git_build/nitrox_build_test.sh

drvload_tests:
  stage: testing
  tags:
    - n5_drv
  script:
    - /home/git_build/nitrox_drv_loadtest.sh

sample_tests:
  stage: testing
  tags:
    - n5_drv
  script:
    - /home/git_build/nitrox_drv_sample_test.sh

cleanup:
  stage: cleanup
  tags:
    - n5_drv
  script:
    - /home/git_build/nitrox_cleanup.sh

Copy .gitlab-ci.yml  file to your working directory and do git push to your 
working git branch. 
  
Ref:  https://docs.gitlab.com/ce/ci/yaml/README.html  


2. Installing the Runner
   ---------------------
   1. Add GitLab's official repository:
   # For Debian/Ubuntu/Mint
	curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash

   # For RHEL/CentOS/Fedora
	curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash   
   2. Install the latest version of GitLab Runner, or skip to the 
	next step to install a specific version:
	# For Debian/Ubuntu/Mint
	 sudo apt-get install gitlab-runner
	
	# For RHEL/CentOS/Fedora
	sudo yum install gitlab-runner

Ref : https://docs.gitlab.com/runner/install/
3.Configuring GitLab Runners
  --------------------------

Registering a specific Runner with a project registration token

To create a specific Runner without having admin rights to the GitLab instance, 
visit the project you want to make the Runner work for in GitLab:

    Go to Settings > CI/CD > Runners settings to obtain the token
  
  # To register a Runner under GNU/Linux:

    Run the following command:

    #sudo gitlab-runner register

	Enter your GitLab instance URL:

	Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
	==> Go to Settings > CI/CD to obtain the token
 
	Enter the token you obtained to register the Runner:

	Please enter the gitlab-ci token for this runner
	==> Go to Settings > CI/CD > Runners settings to obtain the token

	Enter a description for the Runner, you can change this later in GitLab's UI:
        ==>project-name
	Please enter the gitlab-ci description for this runner [hostame] 
	==>my-runner
	
	Enter the tags associated with the Runner, you can change this later in GitLab's UI:
	Please enter the gitlab-ci tags for this runner (comma separated):
	==>my_tag

	Enter the Runner executor:

	Please enter the executor: ssh, docker+machine, docker-ssh+machine, 
	kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
	==>shell

	# Start GitLab Runner
          #gitlab-runner start

Note : If you are facing any linking issue then please do the following steps
	-> Go to Settings > CI/CD > Runners settings
	   You will get following 
		Runners activated for this project
		Edit Active runner and provide same tag which is mentioned in 
		yml file.   

Ref : 	https://docs.gitlab.com/ee/ci/runners/ 
	https://docs.gitlab.com/runner/register/
