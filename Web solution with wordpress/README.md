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
