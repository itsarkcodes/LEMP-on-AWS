# Technical Documentation: Deployment of WordPress with LEMP Stack and Let's Encrypt SSL

## Introduction
This technical documentation provides a step-by-step guide on setting up a deployment process for a WordPress website using Nginx as the web server, LEMP (Linux, Nginx, MySQL, PHP) stack, and Let's Encrypt SSL for secure communication. The deployment process follows security best practices and aims to ensure optimal performance of the website.

## Youtube Video
## **Here is the complete review of this assignment in a Youtube Video:** https://youtu.be/PcG7tPwmaQ4

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

