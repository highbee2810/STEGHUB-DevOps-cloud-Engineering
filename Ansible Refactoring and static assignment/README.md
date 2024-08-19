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
