# Tooling website deployment with continous integration: Introduction to Jenkins
Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy.
In this project we are going to start automating part of our routine tasks with a free and open source automation server - Jenkins. It is one of the mostl popular CI/CD tools, it was created by a former Sun Microsystems
developer Kohsuke Kawaguchi and the project originally had a named "Hudson".
we are going to utilize Jenkins CI capabilities to make sure that every change made to the source code in GitHub https://github.com/highbee2810/tooling will be automatically be updated to the Tooling Website.
## Task:
Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server
Here is how your updated architecture will look upon competion of this project

![Screenshot (359)](https://github.com/user-attachments/assets/17557f16-d235-4672-88ee-5eff84a07954)

**STEP -1: Install Jenkins server**
1. Create an aws ec2 ubuntu server 20.04 Lts and name it jenkins
2. 
