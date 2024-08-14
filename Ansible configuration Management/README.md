# Ansible Configuration Management (Automate Project 7 to 10)- 101.
This Project will Automate most of the routine tasks  with Ansible Configuration Management.
## Ansible Client as a Jump Server (Bastion Host):
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided.
we will dvelop the use of ansible script to simulate the  use jump/ bastion host to access our web servers.
## Tasks
1. Install and configure Ansible client to act as a Jump Server/Bastion Host
2. Create a simple Ansible playbook to automate servers configuration.
## Step 1 - Install and Configure Ansible on EC2 Instance.
1. Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.
   ![Screenshot (413)](https://github.com/user-attachments/assets/06ec939b-c170-4a56-a96d-430cafcb3d7b)

2. In your GitHub account create a new repository and name it ansibleconfig-mgt.


3. Install Ansible (see: install Ansible with pip)

