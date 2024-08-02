# Retail Banking Application Deployment Using AWS Elastic Beanstalk

This repository contains a retail banking application that is deployed and managed by AWS Elastic Beanstalk and Jenkins.

## Testing before Deploying with AWS EC2 and Jenkins

In order to create a Continuous Integration/Continuous Deployment (CI/CD) pipeline, it's important to make sure that the application software builds successfully and all associated tests pass to ensure that there are no problems with the application itself. For that reason, I created an AWS EC2 instance with Jenkins installed.

A new "multibranch pipeline" item is created in order to create a job that will build and install the application software using the developers Github credentials and the specified Github repository URL.

After building and testing as specified in this repositories "Jenkinsfile", we get the following successful build logs:

![Jenkins Results](/screenshots/jenkins_result.png)

The result of the actions specified in the Jenkinsfile can be found within the Jenkins server in the `/var/lib/jenkins/workspace/<app-name>/test-reports` directory. Similarly, the application software retrieved from Github by Jenkins can be found in the `/var/lib/jenkins/workspace/<app-name>` directory.

## Deploying a Managed Service with AWS Elastic Beanstalk

After verifying a successful build with Jenkins, I created an Application and Environment through AWS Elastic Beanstalk. AWS EB manages application software by autoscaling server instances, assigning storage volumes to each instance, providing logs, metrics, and events, among other things that make it easy for a developer to focus on only building their application.

In order to build the environment, IAM roles were created to allow the AWS EB environment to access other AWS services (e.g AWS EC2 to create required instances). After setting a minimum instance of 1 t3.micro, a 10GB SSD volume, basic health metrics, and uploading the application code in a compressed zip file, the environment automatically launches an EC2 instance for the application, and deploys the application to the EC2 server.

As of 08/01/2024, the application can be accessed through `http://awsbeanstalkbankingdeploymentv2-env.eba-r4qw7kmk.us-east-1.elasticbeanstalk.com/`.

A relationship diagram of the developer, client, and workload internals are provided in the screenshot below:

![Workload 1 Diagram](/screenshots/workload1diagram.png)

## Issues

One main issue that I encountered was with AWS EB. When attempting to access the application through the URL provided, I received a 502 Gateway error. This was due to the fact that the zip file had been compressed incorrectly. In other words, the application files were nested in an additional directory within the zip file, so the deployment was unsuccessful. After compressing the application files directly instead of compressing the parent directory, AWS EB was able to deploy the application successfully.

Another issue I ran into was when trying to clone the initial workload repository. Creating an access token was giving me issues, so I decided to create an SSH key within my personal EC2 instance to be able to access github remotely.

## Optimization

Using managed services like AWS Elastic Beanstalk for cloud infrastructure provides the convenience of deploying scalable software applications in desired geographical regions without the need to worry about networking or managing the servers that serve the application.

On the other hand, a retail bank opting to use AWS Elastic Beanstalk as their managed service may want to consider implementing additional security measures within their infrastructure so as to meet regulatory requirements and enhance security overall. If a retail bank is large, AWS EB may become expensive if the service is constantly managing larger instances in various regions. Finally, a retail bank may become locked into the AWS ecosystem if they are dependent on AWS EB to run all of their application software, which would make it difficult to switch to a different service.

