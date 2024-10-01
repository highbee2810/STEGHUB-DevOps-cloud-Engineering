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

Here is our created pipeline
![Screenshot (95)](https://github.com/user-attachments/assets/7aafb6c9-450b-4c9b-a692-50d5ccf1998c)
## Jenkins file creation
Inside the Ansible project, create a new directory deploy and start a new file Jenkinsfile inside the directory.
![Screenshot (96)](https://github.com/user-attachments/assets/af351d85-36b7-4282-a21d-1b0c6e4b9459)
Add the code snippet below to start building the Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage

```
pipeline {
    agent any


  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }
    }
}
```
Now go back into the Ansible pipeline in Jenkins, and select configure, Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile
![Screenshot (97)](https://github.com/user-attachments/assets/b1114de8-ba4c-4d0e-8a41-f43aa77c49e5)
Back to the pipeline again, this time click "Build now"
This will trigger a build and you will be able to see the effect of our basic Jenkinsfile configuration by going through the console output of the build.
To really appreciate and feel the difference of Cloud Blue UI, it is recommended to try triggering the build again from Blue Ocean interface.
1. Click on Open Blue Ocean
2. Select your project
3.Click on the play button against the branch
![Screenshot (98)](https://github.com/user-attachments/assets/67fc740f-3bab-4547-a3c7-888b0cd55e3f)

## Let us see this in action.
Create a new git branch and name it feature/jenkinspipeline-stages
![Screenshot (99)](https://github.com/user-attachments/assets/a96526eb-0182-4fb0-905e-5e5d8fe7acc2)

Currently we only have the Build stage. Let us add another stage called Test. Paste the code snippet below and push the new changes to GitHub.
```
   pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
    }
}
```
![Screenshot (100)](https://github.com/user-attachments/assets/c128dd02-1928-4b8e-97d2-f1879f6e3f5b)

To make your new branch show up in Jenkins, we need to tell Jenkins to scan the repository 
i. Click on the "Administration" button 
ii. Navigate to the Ansible project and click on "Scan repository now"
![Screenshot (101)](https://github.com/user-attachments/assets/57ddd5fc-be40-4bd8-97b6-981fac83a8ce)

iii. Refresh the page and both branches will start building automatically. You can go into Blue Ocean and see both branches there too.
![Screenshot (102)](https://github.com/user-attachments/assets/ad45b7f2-5a5c-423a-860e-73bb52338e11)
iv. In Blue Ocean, you can now see how the Jenkinsfile has caused a new step in the pipeline launch build for the new branch.
![Screenshot (103)](https://github.com/user-attachments/assets/30cae592-3ae9-41e6-87d8-37eac37e9f2f)
## A quick task
Create a pull request to merge the latest code into the main branch
![Screenshot (104)](https://github.com/user-attachments/assets/cef21d2f-de1e-4473-8581-750e1333bd5a)
![Screenshot (105)](https://github.com/user-attachments/assets/b062e90d-cd37-4329-96ab-de076d63ed69)
![Screenshot (106)](https://github.com/user-attachments/assets/0bc8aa97-f7eb-46d1-826e-40c366c25065)
Merge the PR
![Screenshot (107)](https://github.com/user-attachments/assets/3ab5bcbf-2020-4d51-b923-55d0fe5e979c)
go back into your terminal and switch into the main branch and pull the latest change
![Screenshot (108)](https://github.com/user-attachments/assets/2e9a42f8-3556-4210-9d71-b934bb5edfc4)

Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
- Package
- Deploy
- Clean up
![Screenshot (109)](https://github.com/user-attachments/assets/08567ad4-6c90-4881-ad5e-86740ee8216a)
  
Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch
Eventually, your main branch should have a successful pipeline like this in blue ocean
![Screenshot (112)](https://github.com/user-attachments/assets/787678ea-fe8f-4325-95d3-65d427bc9cef)


## Running Ansible playbook from jenkins
Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:
Installing Ansible on Jenkins
```
sudo apt update
sudo apt install ansible
ansible --version
```
![Screenshot (111)](https://github.com/user-attachments/assets/83f0a29b-90bd-4055-a3f6-90eaa65b6900)

Installing Ansible plugin in Jenkins UI
![Screenshot (113)](https://github.com/user-attachments/assets/1f171d0f-43dc-460c-9f11-f8abac1df389)
Creating Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)
```
pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/deploy/ansible.cfg"
    }

    stages {
        stage('Initial cleanup') {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/highbee2810/ansible-config-mgt.git'
            }
        }

        stage('Prepare Ansible For Execution') {
            steps {
                sh 'echo ${WORKSPACE}'
                sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg'
            }
        }

        stage('Test SSH Connections') {
            steps {
                script {
                    def allHosts = [
                        'ubuntu@172.31.27.181',
                        'ubuntu@172.31.29.154',
                        'ubuntu@172.31.16.91',
                        'ec2-user@172.31.27.77'
                    ]

                    sshagent(['private-key']) {
                        allHosts.each { host ->
                            sh "ssh -o StrictHostKeyChecking=no ${host} exit"
                        }
                    }
                }
            }
        }

        stage('Run Ansible playbook') {
            steps {
                sshagent(['private-key']) {
                    ansiblePlaybook(
                        become: true,
                        credentialsId: 'private-key',
                        disableHostKeyChecking: true,
                        installation: 'ansible',
                        inventory: '${WORKSPACE}/inventory/dev.yml',
                        playbook: '${WORKSPACE}/playbooks/site.yml'
                    )
                }
            }
        }

        stage('Clean Workspace after build') {
            steps {
                cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
            }
        }
    }
}
```
![Screenshot (114)](https://github.com/user-attachments/assets/2792a3fd-e6a8-4f25-bd0d-a16624804b3f)
## Note: Ensure that Ansible runs against the Dev environment successfully.
**Possible errors to watch out for:**

Ensure that the git module in Jenkinsfile is checking out SCM to main branch instead of master (GitHub has discontinued the use of Master due to Black Lives Matter. You can read more here)
Jenkins needs to export the ANSIBLE_CONFIG environment variable. You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory. This way, anyone can easily identify that everything in there relates to deployment. Then, using the Pipeline Syntax tool in Ansible, generate the syntax to create environment variables to set. **https://wiki.jenkins.io/display/JENKINS/Building+a+software+project*

**Possible issues to watch out for when you implement this**

Remember that ansible.cfg must be exported to environment variable so that Ansible knows where to find Roles. But because you will possibly run Jenkins from different git branches, the location of Ansible roles will change. Therefore, you must handle this dynamically. You can use Linux Stream Editor sed to update the section roles_path each time there is an execution. You may not have this issue if you run only from the main branch.

If you push new changes to Git so that Jenkins failure can be fixed. You might observe that your change may sometimes have no effect. Even though your change is the actual fix required. This can be because Jenkins did not download the latest code from GitHub. Ensure that you start the Jenkinsfile with a clean up step to always delete the previous workspace before running a new one. Sometimes you might need to login to the Jenkins Linux server to verify the files in the workspace to confirm that what you are actually expecting is there. Otherwise, you can spend hours trying to figure out why Jenkins is still failing, when you have pushed up possible changes to fix the error.

Another possible reason for Jenkins failure sometimes, is because you have indicated in the Jenkinsfile to check out the main git branch, and you are running a pipeline from another branch. So, always verify by logging onto the Jenkins box to check the workspace, and run git branch command to confirm that the branch you are expecting is there.

If everything goes well for you, it means, the Dev environment has an up-to-date configuration.
## Configure ansible on Jenkins
Click on Dashboard > Manage Jenkins > Global Tool Configuration > Add Ansible. Add a name and the path ansible is installed on the jenkins server.
![Screenshot (115)](https://github.com/user-attachments/assets/941436f2-7f81-4120-9a6d-f3eb493021b5)
**To ensure jenkins properly connects to all servers, install another plugin called ssh agent**
![Screenshot (116)](https://github.com/user-attachments/assets/d56edf23-cbe1-4dd5-bf79-eaf9f230e065)
**Then go to manage jenkins > credentials > global > add credentials**
Then follow the steps below:

- Kind: SSH Username with private key
- Scope: Global (Jenkins, nodes, items, all child items, etc)
- ID: private-key (or any ID you prefer)
- Username: Leave it blank or set a default value (e.g., defaultuser) # This is because we are using servers of different username i.e ubbuntu and ec2-user. This value won’t be used because the actual usernames will be specified in the Ansible inventory file.
- Private Key: Enter the private key directly
