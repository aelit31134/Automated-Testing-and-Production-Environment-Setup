# Automated-Testing-and-Production-Environment-Setup

## Aim
This setup aims at creating an automated production and testing environment using 
__Jenkins as the automation tool, 
Docker for Web Server Provisioning and, 
Git/Github as SCM.__

This setup is a small glimpse of Continuous Integration that can be achieved using Jenkins

## Introduction
This setup will work with two branches setup in the same git repo on the developer workstations, named Master and Dev1. While Master branch is linked to the Production Server through the github remote Master branch where clients will reach directly, Dev1 is linked to the Testing server through the github remote Dev1 branch which will in-turn be accessible to the Quality Assurance Team(QAT) who when certify new features or developments deployed on Dev1 Server for testing, will send a remote signal to merge the features with master branch to have it deployed on the Production server.

__Here's a Diagram I made to make it easy to understand:__
![Automation Diagram](https://raw.githubusercontent.com/aelit31134/Automated-Testing-and-Production-Environment-Setup/master/Automated%20Setup(jenkins%2C%20docker%2C%20git)1.jpg)

## Setup Creation
## Requirements:
* Jenkins
* Docker
* Git
* Github
* ngrok(for Internet hosting)

The development will take place on a local git repo existing on the local Workstation. Their will be two branches, Master and Dev1, branched to the same repo.

Different collaborating developers working on different features will work together and the verified features are committed to the master branch and pushed to remote master for direct deploying on Production Server and the features still to be tested are committed to the Dev1 branch and pushed to remote Dev1 to be deployed on the Testing Server for QAT team to verify. After a certification from QAT team is received, the two branches area merged so as to deploy the new features on the Production Server.

__Docker is used to Provision the Production server and the Testing Server,__

__For the sake of creating identical Environments for Production and Testing, two different docker containers are created using the same httpd image pulled from the docker hub.__

## Jenkins Job Deployment:
* The first job Job1, will automatically setup a fresh Testing Environment, pull the latest commit files from github remote Dev1 branch and deploy it for QAT testing. It will continuously keep checking github for new commits and as soon as it finds one, it will deploy it on the Testing Server.
* The second job Job2, will automatically setup the Production Environment, pull the latest commit files from github remote Master branch and deploy it for clients. It will continuously keep checking github for new commits and as soon as it finds one, it will deploy it on the Production Server.
* The third job Job3, will send a signal to github to merge the Dev1 branch with the Master Branch which in turn will trigger the Job2 as Job2 is continuously looking for new commits and due to merger, it runs and deploys the changes, directly on the Production Server. This job will also delete the current running Testing Environment when it is triggered as it is always better to begin new testing phase on a fresh Test Environment setup This job will be triggered by the QAT team through a remote trigger link provided to them.

__Now, let's start with the Practical Setup:__

## Git/Github:

* Setting up the git repo locally,

      mkdir /newproject
      git init

* Now you can create all the files in this folder and then add and commit them,

      git add index.html
      git config --global user.email '<github email>'
      git config --global user.name '<username>'
      git commit index.html -u '<comment>'

* Now you can push it to the github repo,
      
      git add remote origin <github URL>
      git push -u origin master

* Now to create another branch,

      git branch dev1

* To switch Branches,
      
      git checkout dev1
      
## Docker setup:

__Refer this for Initial docker setup and more:__

https://github.com/aelit31134/DockerExamPortal

* To download httpd docker image for server setup,
      
      docker pull httpd

* To run the docker container for Production and mounting the container's web page storage directory to a directory on the host system so that we can update all files on the host system in that directory and they will be made available to the container as well,

      sudo docker run -dit -p 8081:80  -v /webmme:/usr/local/apache2/htdocs  --name prod_web  httpd:latest
                                                                  
* To run the docker container for Testing and mounting the container's web page storage directory to a directory on the host system so that we can update all files on the host system in that directory and they will be made available to the container as well,

      sudo docker run -dit -p 8081:80  -v /webmme:/usr/local/apache2/htdocs  --name prod_web httpd:latest
                                                                   
__Here, -p means we'll we doing PATing so as to make webserver accessible to the outside world but on the same network using the host IP__

* Now, we also need to give jenkins user some root powers for it to run all these commands
  You can do so by making some additions in the configuration file sudoers

      vim /etc/sudoers

![sudors configuration for jenkins user](https://raw.githubusercontent.com/aelit31134/Automated-Testing-and-Production-Environment-Setup/master/sudoers.PNG)

__NOPASSWD means that password for jenkins user will not be asked for__

__Note: This configuration file should be modified with caution.The above setup will give jenkins user all root permissions by just writing sudo before any command__

* Jenkins should be run as a service,

      systemctl start jenkins
      

__Jenkins portal is accessible at__ <__host IP>:8080__, __and here u can download github plugins then create the jobs as explained above.__

__If any of you faces any issues in implementing or understanding, message me at https://www.linkedin.com/in/aayush-goel-2a394618b 
and I'll make a tutorial Video or provide help.__

__U can also refer to the images attached for reference.__

## References and Downloads:
* www.jenkins.io
* https://git-scm.com
* https://plugins.jenkins.io/git/#GitPlugin-AdvancedFeatures
* https://www.linkedin.com/pulse/docker-based-project-online-exam-portal-easy-setup-aayush-goel
