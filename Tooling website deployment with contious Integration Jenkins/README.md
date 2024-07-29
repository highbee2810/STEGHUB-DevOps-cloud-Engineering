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
   1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.

2. **Configure Instance Details**:
   - Choose instance type, network, subnet, and other settings as per your requirements.

3. **Add Storage**:
   - Allocate storage space according to your needs.

4. **Add Tags**:
   - Optionally, add tags for better organization.


5. **Configure Security Group**:
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your IP address.
![Screenshot (57)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c2e660e8-e954-4dd4-88e7-b96749c55fc5)

6. **Review and Launch**:
   - Review the configuration and launch the instance.

![created EC2 instance](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f0c3258d-8ee6-42e3-adc5-aa9daaea10a4)
    

