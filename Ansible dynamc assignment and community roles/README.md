# Ansible dynamic assignment and community roles
In this project we will introduce dynamic assignments by using include module. what is the difference between static and dynamic assignments?
from previous project, you can already tell that static assignments use **import* Ansible module. The module that enables dynamic assignments is **include*.
```
import = Static
include = Dynamic
```
When the import module is used, all statements are pre-processed at the time playbooks are parsed. Meaning, when you execute site.yml playbook, Ansible will process all the playbooks referenced during the time it is
parsing the statements On the other hand, when include module is used, all statements are processed only during execution of the playbook. Meaning, after the statements are parsed, any changes to the statements encountered during
execution will be used.
## NOTE
Take note that in most cases it is recommended to use static assignments for playbooks, because it is more reliable. With dynamic ones, it is hard to debug playbook problems due to its dynamic nature. However, you can use
dynamic assignments for environment specific variables as we will be introducing in this project
## Introducing Dynamic Assignment Into Our structure
In our https://github.com/highbee2810/ansible-config-mgt GitHub repository start a new branch and call it dynamic-assignments.
![Screenshot (474)](https://github.com/user-attachments/assets/d65a6aaf-05fe-4779-a45e-f8ecdfb36751)
Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml.
![Screenshot (475)](https://github.com/user-attachments/assets/80acd787-eb63-43eb-a1a6-f9906d68ce15)
We will instruct site.yml to include this playbook later. For now, let us keep building up the structure

Since we will be using the same Ansible to configure multiple environments, and each of these environments will have certain unique attributes, such as servername, ip-address etc., we will need a way to set values to
variables per specific environment. For this reason, we will now create a folder to keep each environment's variables file. Therefore, create a new folder env-vars, then for each
environment, create new YAML files which we will use to set variables.
![Screenshot (476)](https://github.com/user-attachments/assets/a7e61ac9-1fc1-4917-866b-dde239c77bf5)

Now paste the instruction below into the env-vars.yml file.
```
---
- name: collate variables from env specific file, if it exists
hosts: all
tasks:
- name: looping through list of available files
include_vars: "{{ item }}"
with_first_found:
- files:
- dev.yml
- stage.yml
- prod.yml
- uat.yml
paths:
- "{{ playbook_dir }}/../env-vars"
tags:
- always
```
![Screenshot (477)](https://github.com/user-attachments/assets/b80a39ee-f6c2-42d9-abfa-043096b5dcbe)
## 3 things to notice here:
1. We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and
variants of include_* must be used. These are:
include_role
include_tasks
include_vars
In the same version, variants of import were also introduces, such as: import_role, import_tasks
2. We made use of a special variables {{ playbook_dir }} and {{ inventory_file }}. {{ playbook_dir }} will help Ansible to determine the location of the running playbook, and from there navigate to other
path on the filesystem. {{ inventory_file }} on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder

3. We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment
specific env file does not exist.

## Update site.yml with dynamic assignments
Update site.yml file to make use of the dynamic assignment. (At this point, we cannot test it yet. We are just setting the stage for what is yet to come. So hang on to your hats)
site.yml should now look like this.
```
---
- hosts: all
- name: Include dynamic variables
tasks:
import_playbook: ../static-assignments/common.yml
include: ../dynamic-assignments/env-vars.yml
tags:
- always
- hosts: webservers
- name: Webserver assignment
import_playbook: ../static-assignments/webservers.yml
```
![Screenshot (478)](https://github.com/user-attachments/assets/86198c9a-d504-45aa-80c4-facdce0d4286)

## Community Roles
Now it is time to create a role for MySQL database - it should install the MySQL package, create a database and configure users. But why should we re-invent the wheel? There are tons of roles that have already been
developed by other open source engineers out there. These roles are actually production ready, and dynamic to accomodate most of Linux flavours. With Ansible Galaxy again, we can simply download a ready to use
ansible role, and keep going.
## Download Mysql Ansible Role
We will be using a MySQL role developed by geerlingguy.
Inside roles directory create your new MySQL role with
```
ansible-galaxy install geerlingguy.mysql
```
and rename the folder to mysql
![Screenshot (479)](https://github.com/user-attachments/assets/189321ce-a1d3-4d25-bbd1-96bc42081713)

```
mv geerlingguy.mysql/ mysql
```
Read README.md file, and edit roles configuration to use correct credentials for MySQL required for the tooling website.
Now it is time to upload the changes into your GitHub:
```
git add .
git commit -m "Commit new role files into GitHub"
git push --set-upstream origin dynamic-assignment

```
## Load Balancer roles
We want to be able to choose which Load Balancer to use, Nginx or Apache, so we need to have two roles respectively:
1. Nginx
2. Apache
we can Decide if we want to develop our own roles, or find available ones from the community Update both static-assignment and site.yml files to refer the roles.
loadbalancers.yml file
```
- hosts: lb
roles:
- { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
- { role: apache, when: enable_apache_lb and
load_balancer_is_required }
```
site.yml file
```
- name: Loadbalancers assignment
hosts: lb
- import_playbook: ../static-assignments/loadbalancers.yml
when: load_balancer_is_required
```
Now we can make use of env-vars\uat.yml file to define which loadbalancer to use in UAT environment by setting respective environmental variable to true
```
enable_nginx_lb: true
load_balancer_is_required: true
```
The same must work with apache LB, so you can switch it by setting respective environmental variable to true and other to false. To test this, you can update inventory for each environment and run Ansible
against each environment

