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

6. **Review and Launch**:
   - Review the configuration and launch the instance.

7. **Connect to the Instance**:
   - Use Mobaxterm to connect to the instance via SSH.

## Installing LAMP Stack

1. **Update Package Repository**:
   ```bash
   sudo apt update

