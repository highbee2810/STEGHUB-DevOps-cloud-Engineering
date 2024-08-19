# Ansible refactoring and static assignment
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
