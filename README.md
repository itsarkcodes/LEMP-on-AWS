# Technical Documentation: Deployment of WordPress with LEMP Stack and Let's Encrypt SSL

## Introduction
This technical documentation provides a step-by-step guide on setting up a deployment process for a WordPress website using Nginx as the web server, LEMP (Linux, Nginx, MySQL, PHP) stack, and Let's Encrypt SSL for secure communication. The deployment process follows security best practices and aims to ensure optimal performance of the website.

## Youtube Video
## **Here is the complete review of this assignment in a Youtube Video:** https://youtu.be/PcG7tPwmaQ4
## Hosted Website: https://arkcodes.tech

## Prerequisites
Before proceeding with the automated deployment process, ensure the following prerequisites are met:
* A EC2 Instance on AWS ,running a secure Linux distribution (e.g., Ubuntu 20.04).
* A registered domain name (e.g., arkcodes.tech)
* Access to the VPS with sudo privileges.
* Basic familiarity with Linux command-line and server administration.

## Step 1: Initial Server Setup

1. Sign in to the AWS Management Console at https://aws.amazon.com/.
2. Navigate to EC2 in the AWS Management Console.
3. Click on "Launch Instance."
4. Choose an Amazon Machine Image (AMI) for your instance.
5. Select the instance type based on your requirements.
6. Configure instance details, network settings, and storage.
7. Add tags for better organization (optional).
8. Configure the security group to control inbound and outbound traffic. Make sure to add port 80, 22 and 443 in the Inbound rules
9. Review your instance configuration and click on "Launch."
10. Select a key pair or Create and download the private key (.pem)

## Step 2: Install WordPress with NGINX on Ubuntu
1. Access your EC2 instance using SSH and the downloaded .pem key. Here we are using EC2 Instance Connect  
![image](https://github.com/itsarkcodes/devops-assignment/assets/87442305/b521988d-f3b1-449b-8e97-f80797d36228)
2. Update the package list and install nginx packages:
```nginx
sudo apt update
sudo apt install nginx 
```
3. Start the nginx service
```nginx
sudo systemctl start nginx
```
Check status with ```sudo systemctl status nginx```
![image](https://github.com/itsarkcodes/devops-assignment/assets/87442305/6477c62c-bc40-4dfc-9269-211b8d3a7ff9)

## Step 2: Install MYSQL
1. Install using the following command:
```nginx
apt install mariadb-server -y
```
2. Now we will start mariadb
```
sudo systemctl start mariadb
```
3. Now configure the DB
```
sudo mysql_secure_installation
```
4. Now it will ask for current password, Just hit enter as we dont have any
5. Now it will ask you for setting a root password, Enter 'Y' and proceed by entering the password
6. Select Yes for all the options asked
The installation of MariaDB is complete in your Ubuntu 22.04 system. Now proceed with installing PHP in the next step.

## Step 3: Install PHP
The latest version of PHP (8.1) is available in the repositories of Ubuntu 22.04 and is the default candidate for installation so simply run the following command in terminal to install it.
```
sudo apt install php
```
We will also install some extensions for php
```
sudo apt install php-mysql php-gd php-common php-mbstring php-curl php-cli php-fpm -y
```
The above apt-get command also installs few other packages as well like MySQL, XML, Curl and GD packages and makes sure that your WordPress site can interact with the database, support for XMLRPC, and also to crop and resize images automatically. Further, the php-fpm (Fast process manager) package is needed by NGINX to process PHP pages of your WordPress installation. Remember that FPM service will run automatically once the installation of PHP is over.

## Step 4: Download Wordpress
In this step, download the archived WordPress file using wget and unzip it. To accomplish it run the following commands from the terminal.
```
wget https://wordpress.org/latest.zip
unzip latest.zip
sudo mv wordpress/* /var/www/html
```
Now change directory to /var/www/html and delete 2 files
```
cd /var/www/html
sudo rm -rf index.html index.nginx-debian.html
```
## Step 5: Configure NGINX to host WordPress
Let us now proceed with configuring NGINX server blocks to serve your WordPress domain. To start with navigate to the ```/etc/nginx/sites-enabled```
Here you will find a default file. We will copy the file and name it as wordpress and remove the default site
```
sudo cp default wordpress.conf
sudo rm -rf default
sudo rm -rf /etc/nginx/sites-available/default
```
Now open wordpress.conf and paste the below configuration
```nginx
server {
            listen 80;
            server_name domain.com  www.domain.com
            client_max_body_size 30M
            root /var/www/html/;
            index index.php 
	         
            location / {
                         try_files $uri $uri/ /index.php?$args;
            }

            location ~ \.php$ {
                         fastcgi_pass unix:/run/php/php8.1-fpm.sock;
            }
            
            location ~ /\.ht {
                         deny all;
            }
}
```
Now press ctrl + x and hit Enter to save
Restart nginx
```
sudo systemctl restart nginx
```
**Now open domain name and you will get  wordpress creating page**
![image](https://github.com/itsarkcodes/devops-assignment/assets/87442305/30850e23-b4ea-4fb8-85a2-c8662b00910c)

## Step 6: Create WordPress Database
We will now create a user and a database especially for WordPress installation. To do that, log in to the MariaDB server using ```sudo mysql -u root -p``` command and complete the steps as described below.

```
$ mysql -u root -p
Enter password: (enter any password for database)

MariaDB [mysql]> CREATE DATABASE wordpress;
Query OK, 1 row affected (0.00 sec)

MariaDB [mysql]> GRANT ALL ON wordpress_db.* TO 'wpuser'@'localhost' IDENTIFIED BY 'Passw0rd!' WITH GRANT OPTION;
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> exit
```

## Step 7: Install WordPress
To complete the installation of WordPress, point your favorite web browser to domain.com and follow the steps as described below.  

![image](https://github.com/itsarkcodes/devops-assignment/assets/87442305/23a18e5d-8f99-40c6-80dc-0d65aa3affaf)

Now provide the site information like site title, username, password, email and click on ‘Install WordPress’ button.

![image](https://github.com/itsarkcodes/devops-assignment/assets/87442305/87b06c26-8aa3-48ab-9eed-df7bb4130842)

Provide the user name and password that we have entered previously to login for the first time.
![image](https://github.com/itsarkcodes/devops-assignment/assets/87442305/e51184c2-31b5-4cb4-af10-9416c3591bd4)

## Step 8: Install SSL using LetsEncrypt
Just follow the commands
```
sudo certbot --nginx
```
- Enter Email Address
- It will ask for domain name, just hit enter
- and boom! it will be deployed to both domain names i.e domain.com and www.domain.com


# Conclusion
Congratulations! You have successfully set up deployment process for a WordPress website using Nginx as the web server, LEMP stack, and Let's Encrypt SSL. Your website is now deployed with security best practices and optimized for performance.
