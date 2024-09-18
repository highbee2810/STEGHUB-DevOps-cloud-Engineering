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

What we want to achieve, is having Nginx to serve as a reverse proxy for our sites and tools. Each environment setup is represented in the below table and diagrams:

## CI-Environment

