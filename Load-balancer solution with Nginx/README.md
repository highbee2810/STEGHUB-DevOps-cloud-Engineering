# This project Aims to demonstrate how Apache can be used as load balancer to spread incoming traffics accross multiple web servers
I have demonstrated earlier how Apache can be used as a load balancer , in this project Nginx will be used instead of apache.
## requirements:
1. Web servers(three RHEL 8 Ec2 instances)
2. NFS server
3. an ubuntu server(the load balancer server)
4. Db server.
## Task
This project consists of two parts:
1. Configure Nginx as a Load Balancer
2. Register a new domain name and configure secured connection using
SSL/TLS certificates
**our Target Architecture**
![Screenshot (388)](https://github.com/user-attachments/assets/18e53721-cf98-41b7-b85d-e2a3c09b48cf)

**Part 1 - Configure Nginx As A Load Balancer**
1. Uninstall apache from the ubuntu server
   ```
   sudo apt remove apache2
   ```
   ![Screenshot (389)](https://github.com/user-attachments/assets/699e1405-9174-4d0e-b029-b4b805048be7)
2. Update /etc/hosts file for local DNS with Web Servers' names (e.g. Web1 and Web2) and their local IP addresses)
```
sudo vi /etc/hosts
```
   ![Screenshot (390)](https://github.com/user-attachments/assets/1bbcfa1b-dec8-4ea4-a370-0d3864c105a1)

3. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers
   ```
   sudo apt update
   sudo apt install nginx
   ```
   ![Screenshot (391)](https://github.com/user-attachments/assets/8f9b8db9-2299-4327-8ae7-6e8e6e26c31a)

Configure Nginx LB using Web Servers' names defined in /etc/hosts
Open the default nginx configuration file
```
sudo vi /etc/nginx/ngin.conf
```

#insert following configuration into http section

```
upstream myproject {
server Web1 weight=5;
server Web2 weight=5;
}
server {
listen 80;
server_name www.domain.com;
location / {
proxy_pass http://myproject;
}
}
#comment out this line
# include /etc/nginx/sites-enabled/*;
```
![Screenshot (392)](https://github.com/user-attachments/assets/9ecd4600-c243-4444-888e-675caa33e53b)

test to see that the configuration is okay 
```
sudo nginx -t
```
![Screenshot (393)](https://github.com/user-attachments/assets/ff1070e5-9501-4687-9693-cba0b1794c29)

Restart Nginx and make sure the service is up and running
```
sudo systemctl restart nginx
sudo systemctl status nginx
```
![Screenshot (394)](https://github.com/user-attachments/assets/47847472-9952-4ed5-9670-184be8514614)

## Part 2 - Register a new domain name and configure secured connection using SSL/TLS certificates
In order to get a valid SSL certificate - you need to register a new domain name, you can do it using any Domain name registrar - a company that manages reservation of domain names. The most popular ones are: Godaddy.com, Domain.com, Bluehost.com.

1. Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)
   freedns.afraid.org is used in this project to get a free domain name
   
   ![Screenshot (395)](https://github.com/user-attachments/assets/12957bba-5dd8-4e17-9898-08d64d0b5958)


2. Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP
   allocate a new ellastic IP address
   ![Screenshot (396)](https://github.com/user-attachments/assets/4af7cb15-18c6-48b0-9cef-f4519be6b0bf)
associate it with the Nginx server
![Screenshot (398)](https://github.com/user-attachments/assets/498294e1-aede-493a-8b95-d93ea94d3b2d)
3. Update A record in your registrar to point to Nginx LB using Elastic IP address
add the elatisc ip address to the domain name
![Screenshot (401)](https://github.com/user-attachments/assets/39f4cc36-8e63-4b26-aac1-71a4270ca90c)

Check that your Web Servers can be reached from your browser using newdomain name using HTTP protocol - http://tooling.moo.com

using DNS checker
![Screenshot (402)](https://github.com/user-attachments/assets/4ac73fff-5620-4a31-840f-283f50104c5c)
it can be recahed
4. Configure Nginx to recognize your new domain name
Update your nginx.conf with server_name www.tooling.moo.com instead of server_name www.domain.com
```
sudo nano /etc/nginx/nginx.conf
```
5. Install certbot and request for an SSL/TLS certificate
Make sure snapd service is active and running
```
sudo systemctl status snapd
```
![Screenshot (404)](https://github.com/user-attachments/assets/45d23ef9-a677-4caf-b2b7-dd54b1a13108)
Install certbot
```
sudo snap install --classic certbot
```
![Screenshot (405)](https://github.com/user-attachments/assets/cecd386b-9bf1-4c0e-b996-21db0e048dfb)

Request your certificate (just follow the certbot instructions - you will need to choose which domain you want your certificate to be issued for, domainname will be looked up from nginx.conf file so make sure you have updated it on step 4)
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
```

