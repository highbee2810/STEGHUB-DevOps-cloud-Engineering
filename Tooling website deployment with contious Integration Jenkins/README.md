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
![Screenshot (360)](https://github.com/user-attachments/assets/05843db2-975f-4029-a1a7-6fff430cc14f)


2. **Install JDK (since Jenkins is a Java-based application)**
   ```
   sudo apt update
   sudo apt install default-jdk-headless
   ```
![Screenshot (363)](https://github.com/user-attachments/assets/85d721bb-867c-4b31-8200-f20189fda46a)
![Screenshot (362)](https://github.com/user-attachments/assets/4a50e2d6-204d-4c94-b0be-f16724665eb0)

3.**Install Jenkins**
```
#add the Jenkins Debian repository key to your system
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

#Add the Jenkins repository to your sources list
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
#Update your package lists
sudo apt update

#install jenkins
sudo apt-get install jenkins
```
![Screenshot (364)](https://github.com/user-attachments/assets/5da13bff-26a4-4d93-8b93-f30dba078f80)

**Make sure Jenkins is up and running**
```
sudo systemctl status jenkins
```
![Screenshot (365)](https://github.com/user-attachments/assets/ca0b5215-3bee-4e01-970f-9609052432d9)

4. By default Jenkins server uses TCP port 8080 - open it by creating a new Inbound Rule in your EC2 Security Group
![Screenshot (367)](https://github.com/user-attachments/assets/be0b39e2-a216-43f0-b8df-bf183757933f)

5. Perform initial Jenkins setup
   From your browser access http://54.185.44.240:8080
![Screenshot (368)](https://github.com/user-attachments/assets/95683310-471a-4b25-9ac6-b02fb8712143)


```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Retrieve paasword from your server.

**Install suggested pluggins  and set up an account**
![Screenshot (370)](https://github.com/user-attachments/assets/68dbbdee-24f3-4d84-bc18-fad866316747)

6. Go to Jenkins web console, click "New Item" and create a "Freestyle project"
   ![Screenshot (371)](https://github.com/user-attachments/assets/eb94a690-69ac-47a9-9599-cdf3b5888850)
To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself, In configuration of your Jenkins freestyle project choose Git repository,
provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

Save the configuration and let us try to run the build. For now we can only do it manually. Click "Build Now" button, if you have configured everything correctly, the build will be successfull and you will see it under #1
![Screenshot (372)](https://github.com/user-attachments/assets/222db175-8c80-4c21-97cf-de7868d0455c)

3. Click "Configure" your job/project and add these two configurations Configure triggering the job from GitHub webhook Configure "Post-build Actions" to archive all the files - files resulted from a build are called "artifacts"

![Screenshot (373)](https://github.com/user-attachments/assets/f335c02f-bbdf-4283-937d-61154c53de65)

make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the main branch.

![Screenshot (374)](https://github.com/user-attachments/assets/d48edd3d-b010-4c39-ac1d-af2b5145990d)

You will see that a new build has been launched automatically (by webhook) and you can see its results - artifacts, saved on Jenkins server
![Screenshot (375)](https://github.com/user-attachments/assets/53a676d8-5d9f-42a8-a97f-7cdf5c09fa1f)

we have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as 'push' because the changes are being 'pushed' and files transfer is initiated by GitHub)
By default, the artifacts are stored on Jenkins server locally
```
ls -lrt /var/lib/jenkins/jobs/tooling_github/builds/3/archive/
```
![Screenshot (376)](https://github.com/user-attachments/assets/2ec20ec2-7e01-4d1a-9584-12ba5858de60)

## Step 3 - Configure Jenkins to copy files to NFS server via SSH
Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.
1. Install "Publish Over SSH" plugin
   On main dashboard select "Manage Jenkins" and choose "Manage Plugins" menu item. On "Available" tab search for "Publish Over SSH" plugin and install it
![Screenshot (377)](https://github.com/user-attachments/assets/13b4366d-c372-4b47-84cc-148deab12b52)

click install

![Screenshot (378)](https://github.com/user-attachments/assets/5f5e08a5-20cd-4f5b-84e7-a3a821c4b775)

## 2. Configure the job/project to copy artifacts over to NFS server

On main dashboard select Manage Jenkins > Configure System menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

Provide a private key (content of .pem file that we use to connect to NFS server via SSH/Putty)

Arbitrary name

Hostname - can be private IP address of our NFS server

Username - ec2-user (since NFS server is based on EC2 with RHEL 9)

Remote directory - /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server

![Screenshot (380)](https://github.com/user-attachments/assets/bd29d180-ffe8-4235-bb0e-5eddc17c9d9b)

Test to confirm if the configuration is okay
![Screenshot (381)](https://github.com/user-attachments/assets/47f64b1c-dc50-4971-af05-afec18107dea)

Save the configuration, open your Jenkins job/project configuration page and add another one Post-build Action (Send build artifact over ssh).
Also, Configure it to send all files produced by the build into our previouslys define remote directory In our case we want to copy all files and directories, so we use ** If you want to apply some particular pattern to define which files to send 

![Screenshot (382)](https://github.com/user-attachments/assets/61901e8b-8c76-47cc-889b-db4366661051)

Save this configuration and go ahead, change something in README.MD file in our GitHub Tooling repository

