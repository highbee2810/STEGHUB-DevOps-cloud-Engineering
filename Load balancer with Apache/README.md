# This project Aims to demonstrate how Apache can be used as load balance to spread incoming traffices accross multiple web servers
## requirements:
1. Web servers(three RHEL 8 Ec2 instances)
2. NFS server
3. an ubuntu server(the load balancer server)
4. Db server.
## This project is the continuation of the devops tooling website solution where we have our web servers up and running , the NFS server and the DB server.
you might wonder how a user will be accessing each of the webservers using 3 different IP addreses or 3 different DNS names. You might also wonder
what is the point of having 3 different servers doing exactly the same thing. When we access a website in the Internet we use an URL and we do not
really know how many servers are out there serving our requests, this complexity is hidden from a regular user, but in case of websites that are 
being visited by millions of users per day (like Google or Reddit) it is impossible to serve all the users from a single Web Server (it is also
applicable to databases, but for now we will not focus on distributed DBs).

When you have just one Web server and load increases - you want to serve more and more customers, you can add more CPU and RAM or completely
replace the server with a more powerful one - this is called "vertical scaling". This approach has limitations - at some point you reach the
maximum capacity of CPU and RAM that can be installed into your server. Another approach used to cater for increased traffic is "horizontal
scaling" - distributing load across multiple Web servers. This approach is much more common and can be applied almost seamlessly and almost
infinitely (you can imagine how many server Google has to serve billions of search requests). Horizontal scaling allows to adapt to current load by adding (scale out) or
removing (scale in) Web servers. Adjustment of number of servers can be done manually or automatically (for example, based on some monitored
metrics like CPU and Memory load). Property of a system (in our case it is Web tier) to be able to handle growing load by adding resources, is called "Scalability".
**In our set up in Project-7 we had 3 Web Servers and each of them had its own public IP address and public DNS name. A client has to access them by
using different URLs, which is not a nice user experience to remember addresses/names of even 3 server, let alone millions of Google servers.
In order to hide all this complexity and to have a single point of access with a single public IP address/name, a Load Balancer can be used. A Load
Balancer (LB) distributes clients' requests among underlying Web Servers and makes sure that the load is distributed in an optimal way.**

![Screenshot (339)](https://github.com/user-attachments/assets/47aba1b4-963f-439c-8f23-c6fec9634eca)

In this project we will enhance our Tooling Website solution by adding aLoad Balancer to disctribute traffic between Web Servers and allow users to access our website using a single URL.

## Configure Apache as a load balancer
**1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb**

1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.
   - 
![Screenshot (340)](https://github.com/user-attachments/assets/cfd6e4f6-57b1-43c9-8d88-10ea75fc2c48)


2. **Configure Instance Details**:
   - Choose instance type, network, subnet, and other settings as per your requirements.

3. **Add Storage**:
   - Allocate storage space according to your needs.

4. **Add Tags**:
   - Optionally, add tags for better organization.


5. **Configure Security Group**:
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your anywhere.


6. **Review and Launch**:
   - Review the configuration and launch the instance.
![Screenshot (341)](https://github.com/user-attachments/assets/abfaf47d-652f-4096-acb3-d84c656d95bc)

**Make sure you Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.**
7.**SSH into the instance**
![Screenshot (342)](https://github.com/user-attachments/assets/d12747c0-d36a-42e1-a9c9-9d97d5c9356b)

## Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:
update  and upgrade the packages
```
sudo apt update -y && sudo apt upgrade -y
```
![Screenshot (343)](https://github.com/user-attachments/assets/9e982cad-34f1-4271-96c4-b8ccc4d30e2f)

install apache
```
sudo apt install apache2
```
![Screenshot (344)](https://github.com/user-attachments/assets/2597a04e-1f03-451e-aca8-41b2b62b7340)

install the development files for the libxml2 library
```
sudo apt-get install libxml2-dev
```
![Screenshot (345)](https://github.com/user-attachments/assets/d74ef8e2-c765-4488-87aa-4b94bd8a6d80)
**#Enable following modules:**
```
sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethod_bytraffic
```
![Screenshot (346)](https://github.com/user-attachments/assets/c9e245e8-21c5-4d9e-b44c-9a24eebbb7c8)
**#Restart apache2 service**
```
sudo systemctl restsrt apache2
```
**Make sure apache2 is up and running**
```
sudo systemctl status apache2
```
![Screenshot (347)](https://github.com/user-attachments/assets/e667b083-b005-4d22-9f79-786eb08f1e78)
**Configure load balancing**
```
sudo vi /etc/apache2/sites-available/000-default.conf
```
#Add this configuration into this section <VirtualHost *:80>
</virtualhost>
```
<Proxy "balancer://mycluster">
    BalancerMember "http://172.31.28.103:80" loadfactor=5 timeout=1
    BalancerMember "http://172.31.30.125:80" loadfactor=5 timeout=1
    ProxySet lbmethod=bytraffic
</Proxy>

ProxyPreserveHost On
ProxyPass "/" "balancer://mycluster/"
ProxyPassReverse "/" "balancer://mycluster/"

```
![Screenshot (348)](https://github.com/user-attachments/assets/2d0b3d34-eacf-4661-a6dd-cece835c12cf)

**Restart apache**
```
sudo systemctl restart apache2
```
check apache status
```
sudo systemctl status apache2
```
![Screenshot (349)](https://github.com/user-attachments/assets/d555e99b-a4a5-431c-872c-d2c0e062c7c6)

by traffic balancing method will distribute incoming load between your Web Servers according to current traffic load. We can control in which
proportion the traffic must be distributed by loadfactor parameter.

## 4. Verify that our configuration works - try to access your LB's public IP address or Public DNS name from your browser:
```
35.94.41.8
```
![Screenshot (350)](https://github.com/user-attachments/assets/1c21f1a6-8fde-4e00-8b09-7a7c1bbc6c85)

**Unmount the the nfs server log file**

**Open two ssh/Putty consoles for both Web Servers and run following
command:**
```
sudo tail -f /var/log/httpd/access_log
```



    
