# Automated-Testing-and-Production-Environment-Setup

# Aim
This setup aims at creating an automated production and testing environment using 
__Jenkins as the automation tool, 
Docker for Web Server Provisioning and, 
Git/Github as SCM.

This setup is a small glimpse of Continuous Integration that can be achieved using Jenkins

# Introduction
This setup will work with two branches setup in the same git repo on the developer workstations, named Master and Dev1. While Master branch is linked to the Production Server through the github remote Master branch where clients will reach directly, Dev1 is linked to the Testing server through the github remote Dev1 branch which will in-turn be accessible to the Quality Assurance Team(QAT) who when certify new features or developments deployed on Dev1 Server for testing, will send a remote signal to merge the features with master branch to have it deployed on the Production server.

__Here's a Diagram I made to make it easy to understand:__
