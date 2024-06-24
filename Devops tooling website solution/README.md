# DevOps Tooling Website Solution- Documentation.
This project introduces a set of DevOps tools that will help  in day to day activities in managing, developing, testing, deploying and monitoring different projects.

## in this project you will implement a solution that consists of following components:
1.**Infrastrucure**: AWS
2.**Web server**: : Red Hat Enterprise Linux 8
3.**Database server**: Ubuntu 24.04 + MySQL
4.**Storage server**: Red Hat Enterprise Linux 8 + NFS Server
5.**Programming Language**: PHP
6.**Code Repository:** Github

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
