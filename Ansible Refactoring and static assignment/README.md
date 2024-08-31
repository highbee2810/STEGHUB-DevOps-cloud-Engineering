![image](https://github.com/user-attachments/assets/5e53f97d-c357-442f-8f2b-8788b236b7ea)# Ansible refactoring and static assignment
In this project, we will continue working with ansibleconfig-mgt. repository and make some improvements of our code.
we need to refactor our Ansible code, create assignments, and learn how to use the imports functionality. Imports allow to effectively re-use previously created playbooks in a new playbook - it allows us to organize our tasks and
reuse them when needed.
## Code Refactoring
Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper
comments without affecting the logic
## Step 1 - Jenkins job enhancement
now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us
enhance it by introducing a new Jenkins project/job - we will require **Copy Artifact** plugin.
1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact - we will store there all artifacts after each build
   ```
   sudo mkdir /home/ubuntu/ansible-config-artifact

   ```
![Screenshot (447)](https://github.com/user-attachments/assets/dc713e77-0912-4df6-927b-f455f94ec922)
2. Change permissions to this directory, so Jenkins could save files there 
```
 chmod -R 777 /home/ubuntu/ansible-config-artifact
```
3. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins
   ![Screenshot (448)](https://github.com/user-attachments/assets/ac002ef5-c69b-48bc-9ee1-c242ac1eef9c)
![Screenshot (450)](https://github.com/user-attachments/assets/526b54f6-7e9b-486c-b896-0ef361a9caf9)

4. Create a new Freestyle project (you have done it in Project 9) and name it save_artifacts.
   ![Screenshot (451)](https://github.com/user-attachments/assets/0b2f3c4b-68fa-4f13-a8c2-b287549dfd26)
![Screenshot (452)](https://github.com/user-attachments/assets/7abdafff-b1d5-4641-8501-789ee45d7401)
![Screenshot (453)](https://github.com/user-attachments/assets/2ae3610b-fe12-4295-b617-d5accddbd387)
5. This project will be triggered by completion of our existing ansible project. 
6. The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory.
7. Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).
   If both Jenkins jobs have completed one after another - you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your master branch.
## Step 2 - Refactor Ansible code by importing other playbooks into site.yml
DevOps philosophy implies constant iterative improvement for better efficiency - refactoring is one of the techniques that can be used.
create a new branch, name it refactor
```
git branch refactor
git branch -l
```
![Screenshot (456)](https://github.com/user-attachments/assets/9e1b0d1a-c59f-421c-b2d3-12c709939431)
In previous project, we wrote all tasks in a single playbook common.yml. it will become tedious to use one playbook for multiple servers.
1. Within playbooks folder, create a new file and name it site.yml
   ```
   cd playbooks
   touch site.yml
   ls
   ```
   ![Screenshot (457)](https://github.com/user-attachments/assets/a49d488c-fee1-46da-9db1-9d6a82eab885)
   This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously.
   2. Create a new folder in root of the repository and name it staticassignments
      ```
      cd ..
      mkdir staticassignments
      ls
      ```
      ![Screenshot (458)](https://github.com/user-attachments/assets/a3da7f6a-abaa-42fd-99a3-7b986d15a003)
The static-assignments folder is where all other children playbooks will be stored
3. Move common.yml file into the newly created static-assignments folder.
   ![Screenshot (459)](https://github.com/user-attachments/assets/858c7c7a-afb9-4fe0-ab05-8bdbda8d456a)
4. Inside site.yml file, import common.yml playbook.
   ```
   ---
- hosts: all
- import_playbook: ../static-assignments/common.yml
  ```
![Screenshot (460)](https://github.com/user-attachments/assets/ddffe7cc-e5ab-475c-aa4b-c3478a0b5e6f)
5. Run ansible-playbook command against the dev environment
create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.
```
touch common-del.yml
vi common-del.yml
```
paste the following code in it
```
---
- name: Update web, NFS, and DB servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: Delete Wireshark
      yum:
        name: wireshark
        state: removed

- name: Update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Delete Wireshark
      apt:
        name: wireshark-qt
        state: absent
        autoremove: yes
        purge: yes
        autoclean: yes

```
![Screenshot (461)](https://github.com/user-attachments/assets/ee5661c8-9f6e-49d5-ae8f-515e5a6de6d4)
update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers
```
cd /home/ubuntu/ansible-config-mgt/
ansible-playbook -i inventory/dev.yml playbooks/site.yaml
```
![Screenshot (462)](https://github.com/user-attachments/assets/efdf96e8-e0dc-4f21-8e1c-9b30ebfa0fa0)

wireshark has been deleted in all of the servers

![Screenshot (463)](https://github.com/user-attachments/assets/5e4ec4e6-b543-48e8-871a-18614825d007)

## Step 3 - Configure UAT Webservers with a role 'Webserver'
We have our nice and clean dev environment, so let us put it aside and configure 2 new Web Servers as uat. 
1. Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly - Web1-UAT and Web2-UAT
   ![Screenshot (464)](https://github.com/user-attachments/assets/ab998bd2-054c-4c12-8708-9df79e9a52cb)
2. To create a role, we must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory.
There are two ways how you can create this folder structure:
 - Use an Ansible utility called ansible-galaxy inside ansible-configmgt/roles directory (you need to create roles directory upfront)
```
mkdir roles
cd roles
ansible-galaxy init webserver
```

 - Create the directory/files structure manually
   since we store all our codes in GitHub, it is recommended to create folders and files there rather than locally on Jenkins-Ansible server
   
   ![Screenshot (465)](https://github.com/user-attachments/assets/b3f105e3-8d37-419f-bc27-1f3cbbdba9d9)

   remove tests,files and vars directories
   ![Screenshot (466)](https://github.com/user-attachments/assets/a961f2a6-8992-4daf-aecc-54e7cb72a2cd)
3. Update your inventory ansible-config-mgt/inventory/uat.yml file with IP
addresses of your 2 UAT Web servers
```
[uat-webservers]
<Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
```
4. In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path = /home/ubuntu/ansible-configmgt/roles, so Ansible could know where to find configured roles.

5. It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:
 - Install and configure Apache (httpd service)
 - Clone Tooling website from GitHub https://github.com/highbee2810/tooling.git.
 - Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
 - Make sure httpd service is started
   ```
   ---
---
- name: Setup Apache and deploy a website
  hosts: all
  become: true
  tasks:
    - name: Install Apache
      ansible.builtin.yum:
        name: "httpd"
        state: present

    - name: Install Git
      ansible.builtin.yum:
        name: "git"
        state: present

    - name: Clone a repository
      ansible.builtin.git:
        repo: https://github.com/highbee2810/tooling.git
        dest: /var/www/html
        force: yes

    - name: Copy HTML content to one level up
      command: cp -r /var/www/html/html/ /var/www/

    - name: Start Apache service
      ansible.builtin.service:
        name: httpd
        state: started

```
![Screenshot (467)](https://github.com/user-attachments/assets/2f2be267-8416-4c9b-8e89-5aab23eef436)


## Step 4 - Reference 'Webserver' role
Within the static-assignments folder, create a new assignment for uatwebservers uat-webservers.yml. This is where we will reference the role.
```
---
- hosts: uat-webservers
roles:
- webserver
 ```
the entry point to our ansible configuration is the site.yml file. Therefore, we need to refer your uat-webservers.yml role inside site.yml
So, we should have this in site.yml
```
---
- hosts: all
- import_playbook: ../static-assignments/common.yml
- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml
  ```
## Step 5 - Commit & Test
Commit your changes, create a Pull Request and merge them to master branch, make sure webhook triggered two consequent Jenkins jobs, they ran successfully and copied all the files to your JenkinsAnsible server into /home/ubuntu/ansible-config-mgt/ directory

Now run the playbook against your uat inventory and see what happens:
**NOTE**: Before running your playbook, ensure you have tunneled into your Jenkins-Ansible server via ssh-agent
```
cd /home/ubuntu/ansible-config-mgt
ansible-playbook -i /inventory/uat.yml playbooks/site.yaml
```
