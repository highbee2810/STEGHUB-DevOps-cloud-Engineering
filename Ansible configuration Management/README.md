# Ansible Configuration Management (Automate Project 7 to 10)- 101.
This Project will Automate most of the routine tasks  with Ansible Configuration Management.
## Ansible Client as a Jump Server (Bastion Host):
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided.
we will dvelop the use of ansible script to simulate the  use jump/ bastion host to access our web servers.
## Tasks
1. Install and configure Ansible client to act as a Jump Server/Bastion Host
2. Create a simple Ansible playbook to automate servers configuration.
   ## The Architecture
   ![Screenshot (417)](https://github.com/user-attachments/assets/75db4612-b3ee-4022-953d-f1e2fdc97d29)

## Step 1 - Install and Configure Ansible on EC2 Instance.
1. Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.
   ![Screenshot (413)](https://github.com/user-attachments/assets/06ec939b-c170-4a56-a96d-430cafcb3d7b)

2. In your GitHub account create a new repository and name it ansibleconfig-mgt.

![Screenshot (415)](https://github.com/user-attachments/assets/82b00835-9262-47d3-82be-285ba66e93c7)
![Screenshot (414)](https://github.com/user-attachments/assets/a489e824-b81d-40d2-a98e-bbc5af7d14ac)

3. Install Ansible (see: install Ansible with pip)
```
sudo apt update
sudo apt install ansible
```
check if ansible is installed
```
ansible --version
```
![Screenshot (416)](https://github.com/user-attachments/assets/685f257c-b29d-43e4-ad5d-684cb9a36f3c)

4. Configure Jenkins build job to archive your repository content every time you change it.

setup a webhook on ansibleconfig-mgt repository on github

![Screenshot (420)](https://github.com/user-attachments/assets/1cf1e31c-e7cb-4ce7-8fe4-eeb3dd19b045)

Create a new Freestyle project ansible in Jenkins and point it to your 'ansible-config-mgt' repository
![Screenshot (421)](https://github.com/user-attachments/assets/0a2d792b-12be-4610-ae9d-1bc0da139125)

Configure a Post-build job to save all (**) files
![Screenshot (422)](https://github.com/user-attachments/assets/072da84c-89da-495a-af51-996180a1d91b)

5. Test your setup by making some change in README.md file in main branch.The file is saved alraedy in  /var/lib/jenkins/jobs/ansible/builds/2/archive/
   ```
   ls /var/lib/jenkins/jobs/ansible/builds/2/archive/
   ```
   Note: Trigger Jenkins project execution only for main (or master) branch.
## Step 2 - Prepare your development environment using Visual Studio Code
1. Install and launch visual studio code
   ![Screenshot (425)](https://github.com/user-attachments/assets/15ec10dc-c194-4129-af7a-fe537acebe9e)

2. After you have successfully installed VSC, configure it to connect to your newly created GitHub repository.
   - install git pull request extension
   - create a new folder nam ansibleconfig-mgt open it with visual studio code
   - clone the repository from github into the folder using
   - ```
     git clone https://github.com/highbee2810/ansibleconfig-mgt..git
     ```
## Step 3 - Begin Ansible Development.
1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.
   ![Screenshot (427)](https://github.com/user-attachments/assets/06f8005c-5abc-4829-b7a5-79cad9a02e83)
2. Checkout the newly created feature branch to your local machine and start building your code and directory structure
run the command below on vscode terminal
```
git branch -m feature/ans-prj01
```
![Screenshot (428)](https://github.com/user-attachments/assets/7aa55d4f-543c-466a-9513-cb6a9ee20219)
run git branch to confirm the branch you are using
![Screenshot (429)](https://github.com/user-attachments/assets/a73a026f-1b9a-47ea-8c1a-d2059bd465e0)

3. Create a directory and name it playbooks - it will be used to store all your playbook files
   ```
   mkdir playbooks
   ```
4. Create a directory and name it inventory - it will be used to keep your hosts organised.
```
mkdir inventory
```
5. Within the playbooks folder, create your first playbook, and name it common.yml
```
cd playbooks
touch common.yml
```
6. Within the inventory folder, create an inventory file () for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively. These inventory files use .ini languages style to
configure Ansible hosts
```
cd /ivnventory
touch dev staging uat prod
```
## Step-4 Setup an Ansible inventory 
An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate

Save the below inventory structure in the inventory/dev file to start configuring your development servers.
Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host
```
ssh-add devops.pem
```
![Screenshot (431)](https://github.com/user-attachments/assets/f0c9ea56-de6e-4d10-8310-087e8dbb70b2)

Confirm the key has been added with the command below, you should see the name of your key.
```
ssh-add -l
```
![Screenshot (432)](https://github.com/user-attachments/assets/f9b70470-06c4-48be-b86d-8199e2f72c17)

Now, ssh into your Jenkins-Ansible server using ssh-agent
```
ssh -A ubuntu@54.202.149.118
```
![Screenshot (433)](https://github.com/user-attachments/assets/de92306a-27f2-4b3f-b4d0-ca321ae1b805)

Update your inventory/dev.yml file with this snippet of code:
```
[nfs]
172.31.28.107 ansible_ssh_user=ec2-user
[webservers]
172.31.28.103 ansible_ssh_user=ec2-user
172.31.30.125 ansible_ssh_user=ec2-user
[db]
172.31.24.101 ansible_ssh_user=ubuntu
[lb]
172.31.26.59 ansible_ssh_user=ubuntu
```
![Screenshot (434)](https://github.com/user-attachments/assets/e63673c0-d8bc-4b4b-b442-68bd1ec72fe5)
## Step 5 - Create a Common Playbook
It is time to start giving Ansible the instructions on what you need to be performed on all servers listed in inventory/dev
In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.
Update playbooks/common.yml file with following code:
```
---
- name: Update web, NFS, and DB servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: Ensure Wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: Update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repository cache
      apt:
        update_cache: yes
      
    - name: Ensure Wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
```
![Screenshot (435)](https://github.com/user-attachments/assets/2a2c3a89-ce63-405c-844d-314dcc79b6bb)

The Ansible playbook  updates Wireshark on web, NFS, and DB servers using yum, and on the LB server using apt
lets update this playbook with following tasks:
Create a directory and a file inside it
Change timezone on all servers
Run some shell script

## Step 6 - Update GIT with the latest code
Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.
In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes - it is also called "Four eyes principle".
Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.
Commit your code into GitHub:
1. Use git commands to add, commit and push your branch to GitHub.
   ```
   git add .
   git status
   git commit -m"ansible playbook"

   ```
   push to remote repository branch
   ```
   git push origin featur/ans-prj01
   ```
   
2. Create a Pull Request (PR)
click on compare and pull request.
![Screenshot (436)](https://github.com/user-attachments/assets/89963f9b-6742-4815-8c24-3becf7aa9374)
3. lets act as a reviewer.
![Screenshot (437)](https://github.com/user-attachments/assets/30559653-9e73-431c-a026-22b7517e1619)

add a comment
![Screenshot (439)](https://github.com/user-attachments/assets/cb6b982d-e4d8-47fc-957b-393feaf973f4)

and merge the pull request
![Screenshot (440)](https://github.com/user-attachments/assets/9dd26cb4-f2bc-44a2-b483-84ddfaafbff7)
5. Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes
```
git checkout -b main
git pull
```
![Screenshot (441)](https://github.com/user-attachments/assets/98fdc23f-466c-4d55-8aee-98f8696d1f36)

Once your code changes appear in master branch - Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory
on Jenkins-Ansible server
## Step 7 - Run the first Ansible test.
Now, it is time to execute ansible-playbook command and verify if our playbook actually works:
1.Setup your VSCode to connect to your instance
![Screenshot (442)](https://github.com/user-attachments/assets/d3046837-8d2f-4ac4-a397-012c2e15703d)
2. run the code below in ansibleconfig-mgt. directory
```
ansible-playbook -i inventory/dev.yml playbooks/common.yml
``` 
