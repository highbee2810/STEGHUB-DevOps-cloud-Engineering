# Project Documentation: Web solution with wordpress
In this project storage infrastructure will be prepared on two Linux servers and a basic web solution will be implemented using WordPress.
 **WordPress:** WordPress is a free and open-source content management system written in PHP and paired with MySQL or MariaDB as its backend Relational Database Management System (RDBMS)
 ## Project Tasks:
 1. Configuration of storage subsystems for Web and Database servers based on Linux OS.
 
 2. Installation of WordPress and connection to a remote MySQL database server.
 
## Three-Tier Architecture
![images](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/743b4345-50ee-4d25-a9eb-8a8a5a5b2529)

Three-tier architecture is a well-established software application architecture that organizes applications into three logical and physical computing tiers: the presentation tier, or user interface; 
the application tier, where data is processed; and the data tier, where the data associated with the application is stored and managed.The chief benefit of three-tier architecture is that because each
tier runs on its own infrastructure, each tier can be developed simultaneously by a separate development team, and can be updated or scaled as needed without impacting the other tiers.

### **Presentation tier:**
The presentation tier is the user interface and communication layer of the application, where the end user interacts with the application. Its main purpose is to display information to and collect
information from the user. This top-level tier can run on a web browser, as desktop application, or a graphical user interface (GUI), for example. Web presentation tiers are usually developed using
HTML, CSS and JavaScript. Desktop applications can be written in a variety of languages depending on the platform.

### **Business or Application tier**
The application tier, also known as the logic tier or middle tier, is the heart of the application. In this tier, information collected in the presentation tier is processed - sometimes against other
information in the data tier - using business logic, a specific set of business rules. The application tier can also add, delete or modify data in the data tier.
The application tier is typically developed using Python, Java, Perl, PHP or Ruby, and communicates with the data tier using API calls.

### **Data tier**
The data tier, sometimes called database tier,or data access tier is where the information processed by the application is stored and managed. This can be a relational database management
system such as PostgreSQL, MySQL, MariaDB, Oracle, DB2, Informix or Microsoft SQL Server, or in a NoSQL Database server such as Cassandra, CouchDB or MongoDB.

## The three-tier setup
1. A Laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where you will install
WordPress)
3. An EC2 Linux server as a database (DB) server
## Web server setup
1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose RedHat Server as the operating system.
![Screenshot (224)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8f1580fd-2d60-43c7-b4b1-154ee0fa60af)


2. **Configure Instance Details**:
   - Choose instance type, network, subnet, and other settings as per your requirements.

3. **Add Storage**:
   - Allocate storage space according to your needs.

4. **Add Tags**:
   - Optionally, add tags for better organization.
5. **Configure Security Group**:
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your IP address
 
6. **Review and Launch**:
   - Review the configuration and launch the instance.
         
7. **Connect to the Instance**:
   - Use windows terminal to connect to the instance via SSH.
8.**". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.**
![Screenshot (225)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c4d5d685-7821-499d-af70-782cc83fdcca)

The 3 volumes are created
![Screenshot (226)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/9a380a71-b83f-4b66-aad4-7265b975c76f)
 
 Attach the 3 volumes to the running Ec2 instance
 ![Screenshot (228)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1d718f6a-c261-46f6-8938-969fbc55b3e4)

9.  **SSH to the instance**
 Note: for Ubuntu server, when connecting to it via SSH/Putty or any other tool, we used ubuntu user, but for RedHat you will need to use ec2-user user.
Connection string will look like ec2-user@<Public-IP>
```
ssh -i "steghub.pem" ec2-user@16.171.12.79
```
![Screenshot (229)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/f7673d99-e58b-4a6f-9716-165afe788768)

**Use lsblk command to inspect what block devices are attached to the server.**
```
lsblk
```
![Screenshot (230)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/6751789c-f7d1-436f-acda-0c3eebd0b434)
**Use df -h to see all mounts and free space on the server.**
```
df -h
```
![Screenshot (231)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/79d6f596-98c0-4dc3-a4e1-f0a50f44381a)

**Use gdisk utility to create a single partition on each of the 3 disks**
```
sudo gdisk /dev/
```
![Screenshot (232)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/91896689-41e9-4e09-b250-89de473d0e64)

**Use lsblk utility to view the newly configured partition on each of the 3 disks.**
```
lsblk
```
![Screenshot (233)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/d8b0644c-0a6b-4c2a-ad32-6ca58782329d)

**6. Install lvm2 package using 
```
sudo yum install lvm2
```
![Screenshot (234)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a2958dc9-2921-48f0-9b32-d6239ceaab41)

 Run``` sudo lvmdiskscan ``` command to check for available partitions.
![Screenshot (235)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/ae360713-08b8-4a27-9dab-4efaf7bb95a5)


Note: Previously, in Ubuntu we used apt command to install packages, in RedHat/CentOS a different package manager is used, so we shall
use yum command instead.
**Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**
```
sudo pvcreate /dev/nvme1n1p1
sudo pvcreate /dev/nvme2n1p1
sudo pvcreate /dev/nvme3n1p1
```
![Screenshot (236)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8e450053-6f3e-4d24-bf4c-ce5e515f145a)
**Verify that your Physical volume has been created successfully by running sudo pvs**
```sudo pvs```

![Screenshot (237)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1143f0ae-7a2d-4f4c-9fc1-ebfcb88b98a1)

**Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg**
```
sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
```
![Screenshot (238)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/94d81a76-545e-46f0-982f-849d36f86864)

**Verify that your VG has been created successfully by running ```sudo vgs``**
![Screenshot (239)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/b4f5e994-8e6c-4472-bd04-54f08b3ccacb)

**Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE:
apps-lv will be used to store data for the Website while, logs-lv will beused to store data for logs.**
```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![Screenshot (240)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1fac9647-d966-412c-b7e1-efeab0356318)

**Verify that your Logical Volume has been created successfully by running ```sudo lvs```**
![Screenshot (241)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/656ab030-e879-4d2c-8e7d-c4c0c9aaf067)

**Verify the entire setup**
```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk
```
![Screenshot (242)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/d976d7d6-dbcd-4bc1-9940-761f42b34506)

![Screenshot (243)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8c580cac-0ca0-4314-882c-42efa150ed7e)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem**
```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
![Screenshot (244)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/3acf0027-c579-4e26-ba06-942ae3c31677)

**Create /var/www/html directory to store website files ```sudo mkdir -p /var/www/html```**

**Create /home/recovery/logs to store backup of log data ```sudo mkdir -p /home/recovery/logs```**

**Mount /var/www/html on apps-lv logical volume**
```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```
**Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before
mounting the file system)**
```
sudo rsync -av /var/log/ /home/recovery/logs/
```
![Screenshot (245)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8a53b58c-85f6-402f-80d5-82b62ef9c913)

**Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why the step above is very
important)**
```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
**Restore log files back into /var/log directory**
```
sudo rsync -av /home/recovery/logs/ /var/log
```
**Update /etc/fstab file so that the mount configuration will persist after restart of the server.**
```
sudo blkid
```
![Screenshot (246)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/5674ab96-a07c-499b-b6f6-0777ccdd8c7c)

```sudo vi /etc/fstab```
Update /etc/fstab in this format using your own UUID and rememeber to remove the leading and ending quotes.
![Screenshot (248)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/9573d2c9-942a-4f41-a1a3-a2d6ce621b6d)

**Test the configuration and reload the daemon**
```
sudo mount -a
sudo systemctl daemon-reload
```
**Verify your setup by running ```df -h```, output must look like this:**
![Screenshot (249)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/6ff7a00f-430b-4fa4-9330-b0a5750d7da7)


## Database Server setup

1. **Launch an EC2 Instance**: 
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose RedHat Server as the operating system.
![Screenshot (250)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/5ce8cc0d-bd31-4b42-9a3e-dd4930cc46c6)


2. **Configure Instance Details**:
   - Choose instance type, network, subnet, and other settings as per your requirements.

3. **Add Storage**:
   - Allocate storage space according to your needs.

4. **Add Tags**:
   - Optionally, add tags for better organization.
     
5. **Configure Security Group**:
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your IP address
 
6. **Review and Launch**:
   - Review the configuration and launch the instance.
         
7. **Connect to the Instance**:
   - Use windows terminal to connect to the instance via SSH.
8. **SSH to the instance**
 Note: for Ubuntu server, when connecting to it via SSH/Putty or any other tool, we used ubuntu user, but for RedHat you will need to use ec2-user user.
Connection string will look like ec2-user@<Public-IP>
```
ssh -i "steghub.pem" ec2-user@16.170.143.75
```
![Screenshot (251)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/45732530-9902-4b0d-a693-2b8f1f2458bb)

Repeat the same steps as for the Web Server, but instead of appslv create db-lv and mount it to /db directory instead of /var/www/html/.
**". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.**
![Screenshot (253)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/6ae4ed6b-1562-4f67-a401-27a199a03e17)

**Use lsblk command to inspect what block devices are attached to the server.**
```
lsblk
```
![Screenshot (252)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c34b81e8-5995-4931-aac2-397473b0ff9e)

**Use df -h to see all mounts and free space on the server.**
```
df -h
```
![Screenshot (254)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/b35044d8-e325-4fa2-b290-3d1b1d6d866d)

**Use gdisk utility to create a single partition on each of the 3 disks**
```
sudo gdisk /dev/nvme1n1
sudo gdisk /dev/nvme2n1
sudo gdisk /dev/nvme3n1
```
![Screenshot (254)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/b128e625-808c-4952-ab78-7aded0dace71)

**Use lsblk utility to view the newly configured partition on each of the 3 disks.**
```
lsblk
```
![Screenshot (256)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/296ead5c-6888-4054-80be-249d8797bcb4)
 Install lvm2 package using 
```
sudo yum install lvm2
```
![Screenshot (257)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/85bd8ceb-3a2a-4ab3-bf95-dffc09bad31d)

 Run``` sudo lvmdiskscan ``` command to check for available partitions
![Screenshot (258)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/483b1d78-b938-4e53-a3ae-6eef6fc0a6d5)

**Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**
```
sudo pvcreate /dev/nvme1n1p1
sudo pvcreate /dev/nvme2n1p1
sudo pvcreate /dev/nvme3n1p1
```
![Screenshot (236)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/8e450053-6f3e-4d24-bf4c-ce5e515f145a)
**Verify that your Physical volume has been created successfully by running sudo pvs**
```sudo pvs```

![Screenshot (237)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/1143f0ae-7a2d-4f4c-9fc1-ebfcb88b98a1)

**Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg**
```
sudo vgcreate database-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
```
![Screenshot (259)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/31c93d28-73ac-4687-844e-5ca23b0e0a7f)

**Verify that your VG has been created successfully by running ```sudo vgs``**
![Screenshot (260)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e93e8600-4aa6-4146-abd3-8a36bdbd6544)

**Use lvcreate utility to create a logical volume, db-lv (Use 20G of the PV size since it is the only LV to be created). Verify that the logical volumes have been created successfully**
```
sudo lvcreate -n db-lv -L 20G database-vg
sudo lvs
```
![Screenshot (261)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/7f0ae74f-79b8-42f8-847d-a623cd29d973)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem and monut /db on db-lv**
```
sudo mkfs.ext4 /dev/database-vg/db-lv
sudo mount /dev/database-vg/db-lv /db
```
![Screenshot (262)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/05298c14-4fa7-4aef-8d6e-2ee2537711e7)

 Update /etc/fstab file so that the mount configuration will persist after restart of the server Get the UUID of the device
 ```
sudo blkid
```

Update the /etc/fstab file with the format shown inside the file using the UUID. Remember to remove the leading and ending quotes
```
sudo vi /etc/fstab
```
![Screenshot (263)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/b6f8b512-d830-43c7-9d78-626ee15af336)

**Test the configuration and reload daemon. Verify the setup**
```
sudo mount -a   # Test the configuration

sudo systemctl daemon-reload

df -h   # Verifies the setup
```
![Screenshot (264)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/ae0787fe-18cc-4810-bf26-e8954978749f)
## Step 3: Install wordpress on webserver ec2 instance
**1. Update the repository**
```
sudo yum -y update
```
**2. Install wget, Apache and it's dependencies**
```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
![Screenshot (265)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/d76f9ab7-5c87-4769-8931-8145d8a6ef1a)

**3. Start Apache**
```
sudo systemctl enable httpd
sudo systemctl start httpd
```
![Screenshot (266)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/e8da0c51-2307-40d1-ac5d-8c7d65605549)

**4. To install PHP and it's dependencies**
**Enable EPEL Repository**
```
sudo dnf install -y epel-release
```
**Enable Remi Repository:**
```
sudo dnf install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm
```
**Enable the PHP Module using dnf:**
```
sudo dnf module reset php
sudo dnf module enable php:8.2
```
**Install PHP:**
```
sudo dnf install -y php php-cli php-fpm php-json php-common php-mysqlnd php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath php-json
```
**Start and Enable PHP-FPM Service:**
```
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```
**Verify PHP Installation:**
```
php -v
```
![Screenshot (267)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/31fdfbe3-aede-445f-ae54-fedf55313368)
**Restart Apache**
```
sudo systemctl restart httpd
```
**Download wordpress and copy wordpress to /var/www/htmlmkdir wordpress**
```
mkdir wordpress
sudo wget http://wordpress.org/latest.tar.gz sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz cp wordpress/wp-config-sample.php wordpress/wpconfig.php
cp -R wordpress /var/www/html/
```
![Screenshot (268)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/636d53a6-c7f6-4149-9aea-0f2647e09b31)

**Configure SELinux Policies**
```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```

## Step 4 — Install MySQL on your DB Server EC2**
```
sudo yum update
sudo yum install mysql-server
```
Verify that the service is up and running by using ``` sudo systemctl status mysql```,
if it is not running, restart the service and enable it so it will be running even
```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```
![Screenshot (269)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a72c0262-efb1-4a50-ba03-6b48d56e471a)

## Step 5 — Configure DB to work with WordPress
```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER `myuser`@`16.171.12.79` IDENTIFIED BY
'mypass';
GRANT ALL ON wordpress.* TO 'myuser'@'16.171.12.79';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```
![Screenshot (270)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/c913cb0a-80c8-4f7e-830d-e0645d6a802f)
## Step 6 — Configure WordPress to connect to remote database.
Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web
Server's IP address, so in the Inbound Rule configuration specify source as /32
![Screenshot (271)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/a6f49296-ccd7-4401-a305-2b6acd8c2f36)


**. Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client**
```
sudo yum install mysql
mysql -u myuser -p -h 13.60.47.222
```
![Screenshot (273)](https://github.com/highbee2810/STEGHUB-DevOps-cloud-Engineering/assets/155490206/0b9f6925-e5c1-4f54-a044-d4562ccb1fa8)


