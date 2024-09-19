# Experience Continuous Integration with Jenkins | Ansible |Artifactory |SonarQube | PHP- 101
In this project, you will understand and get hands on experience around the entire concept around CI/CD from applications perspective. To fully gain real expertise around this idea, it is best to see it in action across different programming languages and from the platform perspective too. From the application perspective, we will be focusing on PHP here; there are more projects ahead that are based on Java, Node.js, .Net and Python. By the time you start working on Terraform, Docker and Kubernetes projects, you will get to see the platform perspective of CI/CD in action.
## What is Continuous Integration?
In software engineering, Continuous Integration (CI) is a practice of merging all developers' working copies to a shared mainline (e.g., Git Repository or some other version control system) several times per day.Frequent merges reduce chances of any conflicts in code and allow to run tests more often to avoid massive rework if something goes wrong. This principle can be formulated as Commit early, push often.
The general idea behind multiple commits is to avoid what is generally considered as Merge Hell or Integration hell. 
CI concept is not only about committing your code. There is a general workflow, let us start it...
 - Run tests locally
 - Compile code in CI
 - Run further tests in CI(Static Code Analysis, Code Coverage Analysis, Code smells Analysis, and Compliance Analysis)
 - Deploy an artifact from CI
   ![Screenshot (63)](https://github.com/user-attachments/assets/046d9fb8-ac30-4103-b02f-944b93097e80)
## continous integration in the real world
Version Control
Build
Unit Test
Deploy
Auto Test
Deploy to production
Measure and Validate
## Common Best Practices of CI/CD
Before we move on to observability metrics - let us list down the principles that define a reliable and robust CI/CD pipeline:
Maintain a code repository
Automate build process
Make builds self-tested
Everyone commits to the baseline every day
Every commit to baseline should be built
Every bug-fix commit should come with a test case
Keep the build fast
Test in a clone of production environment
Make it easy to get the latest deliverables
Everyone can see the results of the latest build
Automate deployment (if you are confident enough in your CI/CD pipeline and willing to go for a fully automated Continuous deployment)
## Why are we doing everything we are doing? - 13 DevOps Success Metrics
 - Deployment frequency
 - Lead time
 - Customer tickets
 - Percentage of passed automated tests
 - Defect escape rate
 - Availability
 - Service level agreements(functional and non-functional requirement)
 - Failed deployments
 - Error rates
 - Application usage & traffic
 - Application performance
 - Mean time to detection (MTTD)
 - Mean time to recovery (MTTR)
## Simulating a typical CI/CD Pipeline for a PHP Based application
As part of the ongoing infrastructure development with Ansible started from Project 11, you will be tasked to create a pipeline that simulates continuous integration and delivery. Target end to end CI/CD pipeline is
represented by the diagram below
![Screenshot (64)](https://github.com/user-attachments/assets/b94b5fb2-2063-4a18-a3e2-916f1112a194)

## Setup
This project is partly a continuation of your Ansible work, so simply add and subtract based on the new setup in this project.
To get started, we will focus on these environments initially.
 - Ci
 - Dev
 - Pentest
Both SIT - For System Integration Testing and UAT - User Acceptance Testing do not require a lot of extra installation or configuration. They are basically the webservers holding our applications. But Pentest - For Penetration testing is where we will conduct security related tests, so some other tools and specific configurations will be needed. In some cases, it will also be used for Performance and Load testing. Otherwise, that can also be a separate environment on its own. It all depends on decisions made by the company and the team running the show.

![Screenshot (69)](https://github.com/user-attachments/assets/92c0139a-0d4a-4f66-8c9c-7ff9b4484fc1)

What we want to achieve, is having Nginx to serve as a reverse proxy for our sites and tools. Each environment setup is represented in the below table and diagrams:
## CI-Environment
![Screenshot (65)](https://github.com/user-attachments/assets/3e42591f-f655-4272-8289-b1ed32ac0866)

## Other environment from lower to higher.
![Screenshot (66)](https://github.com/user-attachments/assets/52c7b576-0375-486e-87de-244291695472)

## DNS requirements
Make DNS entries to create a subdomain for each environment. Assuming your main domain is darey.io
You should have a subdomains list like this:
![Screenshot (68)](https://github.com/user-attachments/assets/f4d3b14c-0992-400d-a283-cf5eed57fc03)

![Screenshot (74)](https://github.com/user-attachments/assets/30bac76c-5c9d-4304-9c48-b934bc1badc2)

ci inventory
```
[jenkins]
<Jenkins-Private-IP-Address>
[nginx]
<Nginx-Private-IP-Address>
[sonarqube]
<SonarQube-Private-IP-Address>
[artifact_repository]
<Artifact_repository-Private-IP-Address>
```

dev inventory
```
[tooling]
<Tooling-Web-Server-Private-IP-Address>
[todo]
<Todo-Web-Server-Private-IP-Address>
[nginx]
<Nginx-Private-IP-Address>
[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python
[db]
<DB-Server-Private-IP-Address>
```
pentest inventory file
```
[pentest:children]
pentest-todo
pentest-tooling
[pentest-todo]
<Pentest-for-Todo-Private-IP-Address>
[pentest-tooling]
<Pentest-for-Tooling-Private-IP-Address>
```
**Observations**
1. You will notice that in the pentest inventory file, we have introduced a new concept pentest:children This is because, we want to have a group called pentest which covers Ansible execution against both pentest todo and pentest-tooling simultaneously. But at the same time, we want the flexibility to run specific Ansible tasks against an individual group.
2. The db group has a slightly different configuration. It uses a RedHat/Centos Linux distro. Others are based on Ubuntu (in this case user is ubuntu). Therefore, the user required for connectivity and path to python interpreter are different. If all your environment is based on Ubuntu, you may not need this kind of set up. Totally up to you how you want to do this. Whatever works for you is absolutely fine in this scenario.

## Ansible Roles for CI Environment
Add two more roles to ansible:
1. SonarQube (Scroll down to the Sonarqube section to see instructions on how to set up and configure SonarQube manually)
2. Artifactory
**Why do we need SonarQube??**
SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality, it is used to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities.
**Why do we need Artifactory?**
Artifactory is a product by JFrog that serves as a binary repository manager. The binary repository is a natural extension to the source code repository, in that the outcome of your build process is stored. It can be
used for certain other automation, but we will it strictly to manage our build artifact.

## Configuring Ansible For Jenkins Deployment
In previous projects, you have been launching Ansible commands manually from a CLI. Now, with Jenkins, we will start running Ansible from Jenkins UI. To do this,
1. Navigate to Jenkins URL
2. Install & Open Blue Ocean Jenkins Plugin
3. Create a new pipeline
![Screenshot (75)](https://github.com/user-attachments/assets/150b7b29-4df4-42f5-a96d-25735f3fc0ae)




