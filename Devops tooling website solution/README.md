# DevOps Tooling Website Solution- Documentation.
This project introduces a set of DevOps tools that will help  in day to day activities in managing, developing, testing, deploying and monitoring different projects.

## In this project a solution that consists of following components will be implemented:
### 1.**Infrastrucure**: AWS
### 2.**Web server**: : Red Hat Enterprise Linux 8
### 3.**Database server**: Ubuntu 24.04 + MySQL
### 4.**Storage server**: Red Hat Enterprise Linux 8 + NFS Server
### 5.**Programming Language**: PHP
### 6.**Code Repository:** Github

## Step 1 - Prepare NFS Server
1. Spin up a new EC2 instance with RHEL Linux  Operating System.
![Screenshot (284)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1ea03554-d42c-4667-88c1-e17d708cce7d)

Configure the inbound rules to allow http and https and attached 3 volumes before launching
![Screenshot (285)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1c4bcd7b-0714-4601-867f-d2a035231dd6)

3. Configure LVM on the Server. format the disks as xfs
Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs
![Screenshot (286)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/fb8681e2-2791-4ed1-ae0f-6902880170a9)

```
lsblk
gdisk
```
![Screenshot (287)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/82af34df-32c0-4bfe-b7cc-07623c2fdb92)

```
sudo lvcreate -n lv-app -L 9G NFS-vg
sudo lvcreate -n lv-logs -L 9G NFS-vg
sudo lvcreate -n lv-opt -L 9G NFS-vg

sudo lvs
```
![Screenshot (288)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8f2fad7c-f1cc-4311-9253-08406db4a292)
format the logical volumes with xfs
```
sudo mkfs -t xfs /dev/NFS-vg/lv-app
sudo mkfs -t xfs /dev/NFS-vg/lv-logs
sudo mkfs -t xfs /dev/NFS-vg/lv-opt
```
![Screenshot (289)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/09c9e5e9-4472-4387-b39e-06d3b82122e0)

5. Create mount points on /mnt directory for the logical volumes as follows:
Mount lv-apps on /mnt/apps - To be used by webservers
Mount lv-logs on /mnt/logs - To be used by webserver logs 
Mount lvopt on /mnt/opt - To be used by Jenkins server in Project 8

```
sudo mkdir /mnt/apps
sudo mkdir /mnt/logs
sudo mkdir /mnt/opts

cd /mnt
ls
```
```
sudo mount /dev/NFS-vg/lv-app /mnt/apps
sudo mount /dev/NFS-vg/lv-opt /mnt/opt
sudo mount /dev/NFS-vg/lv-logs /mnt/logs
```
![Screenshot (290)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/0fa6f639-89b2-471b-914d-4bc6cea27d91)

6. Install NFS server, configure it to start on reboot and make sure it is up and running
 ```
 sudo yum update -y
 sudo yum install nfs-utils -y
```
![Screenshot (291)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8611dcb9-9928-4574-8f81-94a9d679b71e)

```
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![Screenshot (292)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/5e70524e-c120-40b1-98de-bbddc7d97373)

7. Export the mounts for webservers' subnet cidr to connect as clients. For simplicity, you will install your all three Web Servers inside the same subnet, but in production set up you would probably want to separate
each tier inside its own subnet for higher level of security. To check your subnet cidr - open your EC2 details in AWS web console and locate 'Networking' tab and open a Subnet link:

Set up permission that will allow the Web Servers to read, write and execute files on NFS

```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service
```
![Screenshot (293)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/349a4241-40ae-45c7-8196-5f098f69c8ae)

Configure access to NFS for clients within the same subnet (example Subnet Cidr - 172.31.32.0/20)

```
sudo vi /etc/exports

/mnt/apps 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)

sudo exportfs -arv
```
![Screenshot (294)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/46df85fb-3464-4459-aa72-4557cbddebbf)

![Screenshot (295)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/4be8de28-10e5-445f-a935-d83a0316abb2)


8.Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)

```
rpcinfo -p | grep nfs
```
![Screenshot (296)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e206f827-11f2-4ed1-8152-bb8c560e2476)

Important note: In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049

![Screenshot (297)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/cc61f5e9-b124-4ca1-89ce-1d8eb4230b35)

## Step 2 — Configure the database server
launch an Ec2 instance with neccessary configurations

![Screenshot (298)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e9706e72-9736-4bce-b868-45b8cd1356d9)

ssh into the instance
![Screenshot (299)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/79e5bfe7-3a47-4cd9-897b-aa9268ac0221)

Install MySQL server

```
sudo apt install mysql-server
```
Run mysql secure script

```
sudo mysql_secure_installation
```
![Screenshot (301)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/807c9e51-b8fb-4f35-8ee7-662ce0325c90)

Create a database and name it tooling
Create a database user and name it webaccess
Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr

```
sudo mysql

CREATE DATABASE tooling;
CREATE USER 'webaccess'@'172.31.16.0/20' IDENTIFIED WITH mysql_native_password BY 'Admin123$';
GRANT ALL PRIVILEGES ON tooling.* TO 'webaccess'@'172.31.16.0/20' WITH GRANT OPTION;
FLUSH PRIVILEGES;
show databases;

use tooling;
select host, user from mysql.user;
exit
```

![Screenshot (302)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/dc1c61ba-665b-4a9d-96b3-a3473eb94a58)

Set Bind Address and restart MySQL

```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

sudo systemctl restart mysql
sudo systemctl status mysql
```
![Screenshot (303)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8f8179c3-7389-4402-9264-3c952ffe7e16)
![Screenshot (304)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/ddf0fc45-8c89-44f7-a829-f41a15f42e7b)
![Screenshot (305)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/b8ab9986-e25c-42a0-887f-bfd6fd8f2b68)



## Step 3 — Prepare the Web Servers
We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case - NFS Server and MySQL database.
During the next steps the following will be done:
1. Configure NFS client (this step must be done on all three servers)
2. Deploy a Tooling application to our Web Servers into a shared NFS folder
3. Configure the Web Servers to work with a single MySQL database.

1. Launch a new EC2 instance with RHEL 8 Operating System
   ![Screenshot (298)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f8e7994e-6ba9-4336-8fac-f848d5756d0d)
2. Install NFS client
   ```
   sudo yum install nfs-utils nfs4-acl-tools -y
   ```
   ![Screenshot (306)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/6e4b315c-b456-489f-8b70-e5ef3f470498)

   
3. Mount /var/www/ and target the NFS server's export for apps
```
sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid 172.31.30.235:/mnt/apps /var/www
```
![Screenshot (307)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1e33c1de-99ea-4970-a54a-2391e620364d)

4. Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot:
![Screenshot (308)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/87144672-e42e-431d-bbab-b2ea44f4c4ac)

```
sudo vi /etc/fstab
```
add the following

```
172.31.30.235:/mnt/apps /var/www nfs defaults 0 0
```
![Screenshot (309)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/41452173-c114-42ed-b856-070919d84da8)

5. Install Remi's repoeitory, Apache and PHP
   ```
   sudo yum install httpd -y
   ```
  ![Screenshot (310)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f32e7f2b-141d-4164-af82-e3f2198a89bc)

```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
![Screenshot (311)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/7a70a4ee-a883-4aba-8054-67f42ed26ef2)

```
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm
```
