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



