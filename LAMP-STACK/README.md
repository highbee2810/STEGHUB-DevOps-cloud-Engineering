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
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your IP address.
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
```

3.**Enable and verify that apache is running on as a service on the OS**:
```bash
sudo systemctl enable apache2
sudo systemctl status apache2
```
If it green and running, then apache2 is correctly installed
![Apache Status](./images/check-status.png)

__4.The server is running and can be accessed locally in the ubuntu shell by running the command below:__

```
curl http://localhost:80
OR
curl http://127.0.0.1:80
```
## Installing Mysql
1. **mysql is popular relational database**:
   ```bash
   sudo apt install mysql-server
   ```
   ![Screenshot (61)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/acb46e79-7ea8-49c8-ae96-3cd29ec3ea86)
2.**log into mysql**:
   ```bash
   sudo mysql
   ```
   ![Screenshot (64)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e50d903b-d438-4155-b231-8512b8b256b5)
   3.**running a security script to set password for the root user using  mysql_native_password***:
   ```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admin123$';
```
![User Password](./images/access-mysql-shell.png)
Exit the MySQL shell
```
exit
```
__5.__ __Run an Interactive script to secure MySQL__

The security script comes pre-installed with mysql. This script removes some insecure settings and lock down access to the database system.
```
sudo mysql_secure_installation ```


__6.__ __After changing root user password, log in to MySQL console.__

A command prompt for password was noticed after running the command below.
```sudo mysql -p ```
Exit MySQL shell
```
exit
```

## Step 3 - Install PHP
1.**Install php**:
Apache is installed to serve the content and MySQL is installed to store and manage data.
PHP is the component of the set up that processes code to display dynamic content to the end user.

The following were installed:
- php package
- php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases.
- libapache2-mod-php, to enable Apache to handle PHP files.
  ```sudo apt install php libapache2-mod-php php-mysql```
 2.**confirm php installation**:
  ``` php -v```  







