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
