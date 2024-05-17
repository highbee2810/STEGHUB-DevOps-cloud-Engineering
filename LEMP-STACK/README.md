# Project Documentation: LEMP Stack on EC2 Instance

## Overview

This project is a documentation guide for setting up a LEMP (Linux, Nginx, MySQL, PHP) stack on an Amazon EC2 instance. The LEMP stack is a popular web development environment that provides the necessary components to run dynamic websites and web applications.

### Components

- **Linux:** Ubuntu Server 20.04 LTS (Linux distribution)
- **Nginx:** Nginx HTTP Server (Web Server)
- **MySQL:** MySQL Database Server (Relational Database Management System)
- **PHP:** PHP (Hypertext Preprocessor) for server-side scripting

## Setting up the EC2 Instance

1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.
![Screenshot (93)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/5e6475a6-11da-4c7a-a7f0-c7affb362206)

2. **Configure Instance Details**:
   - Choose instance type, network, subnet, and other settings as per your requirements.

3. **Add Storage**:
   - Allocate storage space according to your needs.

4. **Add Tags**:
   - Optionally, add tags for better organization.


5. **Configure Security Group**:
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your IP address
   ![Screenshot (104)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/9aed6d66-a25b-4b3d-85f3-fdeae7b2a53e)

6. **Review and Launch**:
   - Review the configuration and launch the instance.
![Screenshot (95)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e1a80729-80bc-468d-a6c5-5b95efc8229e)

    
7. **Connect to the Instance**:
   - Use Git Bash to connect to the instance via SSH.
     ![Screenshot (96)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/91d89a66-4423-4569-8e71-069ccaf3c1da)
8. **Grant Permission for the private key and SSH to the instance**
```
chmod 400 my-ec2-key.pem
ssh -i "my-ec2-key.pem" ubuntu@18.209.18.61
```
![Screenshot (99)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/81da729e-cc44-4fe4-88da-335c6c1729fc)

## Installing LEMP Stack

1. **Update Package Repository**:
   ```bash
   sudo apt update
   ```
![Screenshot (100)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/28d4bdbc-f310-49e8-b8f5-471b8350c479)

2.**Install Nginx**
   ```bash
sudo apt install Nginx -y
```
![Screenshot (101)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f092c539-e873-4e26-8297-4eb796d103b5)

3.**Enable and verify that Nginx is running on as a service on the OS**:
```bash
sudo systemctl status Nginx
```
If it green and running, then apache2 is correctly installed
![Screenshot (102)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/30f64306-9a7d-4758-8677-748a225a81a4)


__4.The server is running and can be accessed locally in the ubuntu shell by running the command below:__

```
curl http://localhost:80
OR
curl http://127.0.0.1:80
```
![Screenshot (103)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/43842955-0740-4536-85cc-6820bf37a43a)
