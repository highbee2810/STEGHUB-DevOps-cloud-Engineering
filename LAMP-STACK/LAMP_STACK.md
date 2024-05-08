# Project Documentation: LAMP Stack on EC2 Instance

## Overview

This project is a documentation guide for setting up a LAMP (Linux, Apache, MySQL, PHP) stack on an Amazon EC2 instance. The LAMP stack is a popular web development environment that provides the necessary components to run dynamic websites and web applications.

### Components

- **Linux:** Ubuntu Server 20.04 LTS (Linux distribution)
- **Apache:** Apache HTTP Server (Web Server)
- **MySQL:** MySQL Database Server (Relational Database Management System)
- **PHP:** PHP (Hypertext Preprocessor) for server-side scripting

## Setting up the EC2 Instance

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
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 3306 (MySQL) from your IP address.
![Screenshot (57)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c2e660e8-e954-4dd4-88e7-b96749c55fc5)

6. **Review and Launch**:
   - Review the configuration and launch the instance.

![created EC2 instance](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f0c3258d-8ee6-42e3-adc5-aa9daaea10a4)
    

7. **Connect to the Instance**:
   - Use Mobaxterm to connect to the instance via SSH.
     ![connecting to server using mobaxterm](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/4cadb9fc-f4d2-4243-b56f-d843bc15686f)


## Installing LAMP Stack

1. **Update Package Repository**:
   ```bash
   sudo apt update
![Screenshot (52)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/cc9de0d2-111a-4a2d-8c6e-472427144398)
2.**Install apache**
   ```bash
sudo apt install apache2
![Screenshot (53)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a66fa535-c17c-4b31-815a-50348afd0c53)
3. **Enable that apache is running**
 ```bash
   sudo systemctl status apache2


### if it green and running it means everything is running smoothly
![Screenshot (54)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/08044572-fa7f-4430-8408-c2e30a4a6cbb)


