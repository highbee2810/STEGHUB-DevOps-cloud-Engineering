# Understanding Client-Server Architecture
Computers connected to the internet are called clients and servers
![Client-server-model svg](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c48513c5-d231-4d5a-a7a9-b9da56307d33)
**• Clients are the typical web user's internet-connected devices (for example, your computer connected to your Wi-Fi, or your phone connected to your mobile network) and web-accessing software available on those devices (usually a web browser like Firefox or Chrome).
• Servers are computers that store webpages, sites, or apps. When a client device wants to access a webpage, a copy of the webpage is downloaded from the server onto the client machine to be displayed in the user's web browser.**

The setup on the diagram above is a typical generic Web Stack
architecture that we have already implemented in previous projects (LAMP,
LEMP, MEAN, MERN), this architecture can be implemented with many other
technologies - various Web and DB servers, from small Single-page
applications SPA to large and complex portals.
## a quick client-server architecture in action
Open up your Ubuntu or Windows terminal and run curl command:
```
curl -Iv steghub.com
```
![Screenshot (204)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/92df2060-28ad-4f48-b731-c7eff55ea46f)

**Note: If your Ubuntu does not have 'curl', you can install it by running sudo
apt install curl**
In this example, your terminal or ubuntu will be the client, while steghub.com will be
the server.
**the response from the remote server in above output. You can also see
that the requests from the URL are being served by a computer with an IP
address 160.153.133.153 on port 80**

Another simple way to get a server's IP address is to use a simple
diagnostic tool like 'ping', it will also show round-trip time - time for packets
to go to and back from the server, this tool uses ICMP protocol.
##  - Implement a Client Server Architecture using MySQL Database Management System (DBMS).
To demonstrate a basic client-server using MySQL RDBMS, follow the below instructions

**1. Create and configure two Linux-based virtual servers (EC2 instances in AWS).**

Server A name - `mysql server`
Server B name - `mysql client`

## Setting up the  'mysql server'

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
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), 3306 (mysql) and 443 (HTTPS) from your IP address.
![Screenshot (57)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c2e660e8-e954-4dd4-88e7-b96749c55fc5)

6. **Review and Launch**:
   - Review the configuration and launch the instance.

![Screenshot (205)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/70d42e91-d9c1-4c91-ad92-fa71e166babf)

## Setting up the 'mysql client'

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
![Screenshot (206)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/163c9be7-dc0c-4b7c-a124-6ba40a5e9eb2)

### The two servers are up and running
![Screenshot (207)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e44505c2-0785-470c-821d-5b60f64b629b)

## conect to mysql server using windows terminal
**ssh to the instance**
```
ssh -i steghub.pem ubuntu@13.51.205.236
```
![Screenshot (209)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/562574a4-3a3a-4012-9531-98a181d9c469)


1. **Update Package Repository**:
   ```bash
   sudo apt update
   ```
   ![Screenshot (210)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/79b646c3-f79d-4ff3-867d-1984ff8d7744)

   
2. **Upgrade ubuntu**:
   ```
   sudo apt upgrade
   ```
## On mysql server Linux Server install MySQL Server software.
use the command below to install mysql 
```
sudo apt install mysql-server -y
```
![Screenshot (211)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f4f36e1f-6ad5-4451-afa9-0a45bd5de0cd)

MySQL is an open-source relational database management system. Its name is a combination of "My", the name of the cofounder Michael Widenius's daughter, and "SQL", the abbreviation
for Structured Query Language.
## On mysql client Linux Server install MySQL Client software

## conect to mysql client using windows terminal
**ssh to the instance**
```
ssh -i steghub.pem ubuntu@13.60.83.92
```

![Screenshot (212)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/fcf9e930-087d-4a0a-9634-ec88dc9f3e6c)


1. **Update Package Repository**:
   ```bash
   sudo apt update
   ```
   ![Screenshot (210)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/79b646c3-f79d-4ff3-867d-1984ff8d7744)

   
2. **Upgrade ubuntu**:
   ```
   sudo apt upgrade
3. **Install MySQL Client software**
   ```
   sudo apt install mysql-client -y
   ```
   ![Screenshot (213)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a924f813-9f07-4c85-bc05-3dfbd5fe0959)
By default, the two EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses.
   
