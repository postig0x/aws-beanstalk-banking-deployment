# Retail Banking Application Deployment Using AWS Elastic Beanstalk

This repository contains a retail banking application that is deployed and managed by AWS Elastic Beanstalk and Jenkins.

In order to create a Continuous Integration/Continuous Deployment (CI/CD) pipeline, it's important to make sure that the application software builds successfully and all associated tests pass to ensure that there are no problems with the application itself. For that reason, we created an AWS EC2 instance with Jenkins installed. This AWS EC2 instance (called "Jenkins server" from here on)
