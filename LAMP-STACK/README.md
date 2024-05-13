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
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
   ```
Exit the MySQL shell
```
exit
```
![Screenshot (66)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/31c21c29-c50d-4523-b144-6346f56cb3b1)

__5.__ __Run an Interactive script to secure MySQL__

The security script comes pre-installed with mysql. This script removes some insecure settings and lock down access to the database system.
```
sudo mysql_secure_installation
```
__6.__ __After changing root user password, log in to MySQL console.__

A command prompt for password was noticed after running the command below.
```
sudo mysql -p
```
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
  ```
  sudo apt install php libapache2-mod-php php-mysql
  ```
2.**confirm php installation**:
  ``` 
  php -v
  ```  
![Screenshot (68)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/af7d7a7c-db45-4a95-a1e1-fb7d3342f745)
At this point LAMP is fully operational we have L-linux , A-apache, M- mysql and P-php.
## Creating a Virtual Host for the website using Apache
1.**set up a domain name called projectlamp**:
Apache on default has one server block that serve content from /var/www/html directory we will leave this and create our own directory next to the default one
```
mkdir /var/www/projectlamp
```
2. **Assign ownership of the directory with the user$ environment which will reference the current user**
   ```
   sudo chown -R $USER:$USER /var/www/projectlamp
   ```
   ![Screenshot (69)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/79596929-5dd8-4c87-9c94-6f37162ceae5)

3.**Creation of configuration file in Apache's sites-available directory using vi or vim**
```
sudo vi /etc/apache2/sites-available/projectlamp.conf
```
this will create a new blank file paste the following configuration

```
<VirtualHost *:80>
  ServerName projectlamp
  ServerAlias www.projectlamp
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/projectlamp
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
to save press ESC key + :+wq!
use ls command to view new file in the sites-available
```
sudo ls /etc/apache2/sites-available
```
with this virtual host we are telling the server to serve content from /var/www/projectlamp
4**enable virtual host**
you can use a2ensite command to enable the virtual host
```
sudo a2ensite projectlamp
```
5.**disable the default that comes with apache using the below command**
```
sudo a2dissite 000-default
```
confirm that your configuration file is free from syntax error
```
sudo apache2ctl configtest
```
6.**finally reload apache**:
```
sudo systemctl reload apache2
```
7.**the new website is now active but the web root is still empty**
create an index.html file in /var/www/lampproject
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
go to your browser and try open your website using ip address
```
htpp://13.60.44.229:80
```
![Screenshot (77)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/7a20f453-d140-4454-8110-29f7b6eed785)
This file can be left in place as a temporary landing page for the application until an index.php file is set up to replace it. Once this is done, the index.html file should be renamed or removed from the document root as it will take precedence over index.php file by default.

## Step 5 - Enable PHP on the website

With the default DirectoryIndex setting on Apache, index.html file will always take precedence over index.php file. This is useful for setting up maintenance page in PHP applications, by creating a temporary index.html file containing an informative message for visitors. The index.html then becomes the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root bringing back the regular application page.
If the behaviour needs to be changed, /etc/apache2/mods-enabled/dir.conf file should be edited and the order in which the index.php file is listed within the DirectoryIndex directive should be changed.

__1.__ __Open the dir.conf file with vim to change the behaviour__
```
sudo vim /etc/apache2/mods-enabled/dir.conf
```

```
<IfModule mod_dir.c>
  # Change this:
  # DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
  # To this:
  DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
![Screenshot (81)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/ad01ee05-eb7e-443c-aa2f-02808fc05aff)

__2.__ __Reload Apache__

Apache is reloaded so the changes takes effect.
```
sudo systemctl reload apache2
```
__3.__ __Create a php test script to confirm that Apache is able to handle and process requests for PHP files.__

A new index.php file was created inside the custom web root folder.

```
vim /var/www/projectlamp/index.php
```

__Add the text below in the index.php file__
```
<?php
phpinfo();
```
![Screenshot (82)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/08caa33b-f8ee-46cc-9da9-0d2d6a7e1e7d)

__4.__ __Now refresh the page__
![Screenshot (80)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8b5c4888-9073-403c-bb42-edea7fc80bd3)

This page provides information about the server from the perspective of PHP. It is useful for debugging and to ensure the settings are being applied correctly.

After checking the relevant information about the server through this page, It’s best to remove the file created as it contains sensitive information about the PHP environment and the ubuntu server. It can always be recreated if the information is needed later.
```
sudo rm /var/www/projectlamp/index.php
```
**Conclusion:
we have successfully set up a LAMP stack on an Amazon EC2 instance using Mobaxterm for remote server access. You can now deploy and host your web applications on this environment.**












