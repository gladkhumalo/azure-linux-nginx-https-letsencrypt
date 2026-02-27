# Azure Linux VM – Nginx with HTTPS (Let's Encrypt)

## 📌 Project Overview

This lab demonstrates how to deploy a Linux Virtual Machine in Microsoft Azure, install Nginx, configure DNS using Azure's default public domain, and secure the web server with a free SSL certificate from Let's Encrypt.

The goal of this project was to simulate a real-world cloud administrator task: provisioning infrastructure, configuring a web server, and implementing HTTPS securely.

---


## 🏗 Architecture

- Microsoft Azure Virtual Machine (Ubuntu Linux)
- Public IP with Azure DNS label:
  - nginx-web.eastus.cloudapp.azure.com
- Network Security Group (NSG)
  - Port 80 (HTTP)
  - Port 443 (HTTPS)
- Nginx Web Server
- Let's Encrypt SSL Certificate via Certbot

---

## 🚀 Deployment Steps

### 1️⃣ Create Azure VM

- OS: Ubuntu Server 24.04 LTS
- Size: Standard_D2s_v3 (lab environment)
- Disk: 30GB, Premium SSD (locally-redundant storage)
- VNET and IP Address: Default
- Public IP: Yes (left to Default values)
- Open inbound ports:
  - 22 (SSH)
  - 80 (HTTP)
  - 443 (HTTPS)
    
<p align="left">
  <img src="assets/Deployment Complete.jpg" width="700">
</p>

---
#### Assign a public DNS name:
Click on the public IP > **Configuration.**
I labelled mine: **mywebserver.eastus.cloudapp.azure.com**
<p align="left">
  <img src="assets/dns-name-label.jpg" width="700">
</p>
<p align="left">
  <img src="assets/dns-name-label-marked.jpg" width="700">
</p>

### 1️⃣ Login 
<p align="left">
  <img src="assets/dns-name-label.jpg" width="700">
</p>

