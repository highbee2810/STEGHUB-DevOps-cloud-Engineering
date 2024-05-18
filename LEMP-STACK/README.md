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
5.**Test how Nginx can respond to request over the internet**
```
http://<public-ip address>:80
```
![Screenshot (105)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e60890e0-f9e3-418b-97dd-1920ece9122f)
__6.__ __Another way to retrieve the public ip address other than check the aws console__

```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
## Installing Mysql
1. **mysql is popular relational database**:
   ```bash
   sudo apt install mysql-server
   ```
![Screenshot (106)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a7e7ee31-9761-498f-ad17-cf91c023d19e)

2.**log into mysql**:
   ```bash
   sudo mysql
   ```
   
3.**running a security script to set password for the root user using  mysql_native_password***:
   ```
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admin123$';
   ```
![Screenshot (107)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/92766b3e-80a9-4c83-a3ea-fce97f2df76f)
Exit the MySQL shell
```
exit
```


__5.__ __Run an Interactive script to secure MySQL__

The security script comes pre-installed with mysql. This script removes some insecure settings and lock down access to the database system.
```
sudo mysql_secure_installation
```
![Screenshot (108)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/de33e9e2-53b8-4d91-9bb3-3ce01743f11c)

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
while Apache embeds PHP interpreter in each request, Nginx requires an external program to handle PHP processsing and act as bridge between PHP interpreter itself and web server. 
you need to Install php-fpm (PHP fastCGI process manager) and tell nginx to pass PHP requests to this software for processing. Also, install php-mysql, a php module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
```
sudo apt install php-fpm php-mysql -y
```
![Screenshot (109)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/891c3b63-614a-46d0-aafb-0901010d2b50)
## Configuring Nginx to use PHP processor
1.**Create a root web directory for your_domain**
```
sudo mkdir /var/www/projectLEMP
```
2.**Assign the directory ownership with $USER which will reference the current system user**
```
sudo chown -R $USER:$USER /var/www/projectLEMP
```
![Screenshot (110)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/7532fa89-6bda-460c-aee4-035f828cd311)
3.**Create a new configuration file in Nginx’s “sites-available” directory.**
```
sudo nano /etc/nginx/sites-available/projectLEMP
```
paste the following codes 

```
server {
  listen 80;
  server_name projectLEMP www.projectLEMP;
  root /var/www/projectLEMP;

  index index.html index.htm index.php;

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
  }

  location ~ /\.ht {
    deny all;
  }
}
```
![Screenshot (112)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c9b2cd5b-3812-4083-b5ba-83136ab32632)


### Here’s what each directives and location blocks does:

- __listen__ - Defines what port nginx listens on. In this case it will listen on port 80, the default port for HTTP.

- __root__ - Defines the document root where the files served by this website are stored.

- __index__ - Defines in which order Nginx will prioritize the index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page for PHP applications. You can adjust these settings to better suit your application needs.

- __server_name__ - Defines which domain name and/or IP addresses the server block should respond for. Point this directive to your domain name or public IP address.

- __location /__ - The first location block includes the try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate result, it will return a 404 error.

- __location ~ \.php$__ - This location handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.

- __location ~ /\.ht__ - The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root, they will not be served to visitors.
  __4.__ __Activate the configuration by linking to the config file from Nginx’s sites-enabled directory__

```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
This will tell Nginx to use this configuration when next it is reloaded.

__5.__ __Test the configuration for syntax error__

```
sudo nginx -t
```
![Screenshot (114)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e3f954fe-5b2d-4846-b569-366e03adee84)


__6.__ __Disable the default Nginx host that currently configured to listen on port 80__

```
sudo unlink /etc/nginx/sites-enabled/default
```
__7.__ __Reload Nginx to apply the changes__
```
sudo systemctl reload nginx
```

__8.__ __The new website is now active but the web root /var/www/projectLEMP is still empty. Create an index.html file in this location so to test the virtual host work as expected.__

```
sudo echo ‘Hello LEMP from hostname’ $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) ‘with public IP’ $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```
![Screenshot (115)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e1adc8e2-e9bc-4b17-981b-d66f543e8ac2)
## Testing PHP with Nginx
**1. Create a test PHP file in the document root. Open a new file called info.php within the document root.**
```
sudo nano /var/www/projectLEMP/info.php
```
paste the code below
```
<?php
phpinfo();
```
**2. Access the page on the browser and attach /info.php**
```
http://51.20.64.70/info.php
```
![Screenshot (116)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/ee13f0b2-605a-477a-aad0-015e08532c87)
after checking the details about PHP is best to remove the file as it contain sensitive information
```
sudo rm /var/www/projectLEMP/info.php
```
## Step 6 - Retrieve Data from MySQL database with PHP
1.**connect to MYSQL database** 
```
sudo mysql
```

2.**create a new Database**
```
CREATE DATABASE Mytodo_database;
```

3.**create a user and grant the user full priviledge with password**
```
CREATE USER 'Ib'@'%' IDENTIFIED WITH mysql_native_password BY 'Admin123$';

GRANT ALL ON Mytodo_database.* TO 'todo_user'@'%';
```

4. **exit**
```
exit
```

5. **check if the user has proper permission by logging**
```
mysql -u Ib -p

SHOW DATABASES;
```
**show database**
**create a table called todo_list**
```
CREATE TABLE todo_database.todo_list (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY(item_id)
);
```

**insert few rows**
```
INSERT INTO todo_database.todo_list (content) VALUES ("My first important item");

INSERT INTO todo_database.todo_list (content) VALUES ("My second important item");

INSERT INTO todo_database.todo_list (content) VALUES ("My third important item");
INSERT INTO todo_database.todo_list (content) VALUES ("fourth important item");
INSERT INTO todo_database.todo_list (content) VALUES ("My fifth important item");
INSERT INTO todo_database.todo_list (content) VALUES ("and this one more thing");
```
**check if that the data was succesfully saved into the table**
```
SELECT * FROM todo_database.todo_list;
```
![Screenshot (125)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/3d50703e-52e1-44bf-8658-f689a23a5062)
**exit**
```
exit
```
**create a PHP script**
```
sudo nano /var/www/projectLEMP/todo_list.php
```
**copy this content into it**
```
<?php
$user = "Ib";
$password = "Admin123$";
$database = "todo_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
?>
```
![Screenshot (126)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/7842fdc5-412e-48d7-a936-c848fb7127dc)

**save and close**
**Access the page by browser using ip address or domain name**
```
http://51.20.64.70/todo_list.php
```
![Screenshot (127)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1b3c0c54-9b43-4a3a-8e93-e93b7105a468)

## conclusion














