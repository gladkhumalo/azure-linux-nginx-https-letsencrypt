# Azure Linux VM – Nginx with HTTPS (Let's Encrypt)

## 📌 Project Overview

This lab demonstrates how to deploy a Linux Virtual Machine in Microsoft Azure, install Nginx, configure DNS using Azure's default public domain, and secure the web server with a free SSL certificate from Let's Encrypt.

The goal of this project was to simulate a real-world cloud administrator task: provisioning infrastructure, configuring a web server, and implementing HTTPS securely.


## 🏗 Architecture

- Microsoft Azure Virtual Machine (Ubuntu Linux)
- Public IP with an Azure DNS label, for example:
  - `<dns-label>.eastus.cloudapp.azure.com`
- Network Security Group (NSG)
  - Port 80 (HTTP)
  - Port 443 (HTTPS)
- Nginx Web Server
- Let's Encrypt SSL Certificate via Certbot


## 🚀 Deployment Steps

###  Create Azure VM

- OS: Ubuntu Server 24.04 LTS
- Size: Standard_D2s_v3 (lab environment)
- Disk: 30GB, Premium SSD (locally-redundant storage)
- VNET and IP Address: Default
- Public IP: Yes (left to Default values)
- Open inbound ports:
  - 22 (SSH, restricted to your trusted source IP)
  - 80 (HTTP)
  - 443 (HTTPS)
    
<p align="left">
  <img src="assets/Deployment Complete.jpg" width="700">
</p>

<br>

#### Assign a public DNS name:
Click on the public IP > **Configuration.**
Choose a unique label, for example: **mywebserver.eastus.cloudapp.azure.com**
<p align="left">
  <img src="assets/dns-name-label.jpg" width="700">
</p>
<p align="left">
  <img src="assets/dns-name-label-marked.jpg" width="700">
</p>

###  Login 
<p align="left">
  <img src="assets/login.jpg" width="700">
</p>

#### Update packages
```bash
sudo apt update && sudo apt upgrade -y
```

#### Install Nginx
```bash
sudo apt install nginx -y
```
<p align="left">
  <img src="assets/install-nginx.jpg" width="800">
</p>


#### Verify Nginx is serving your domain
```bash
sudo nginx -t
sudo systemctl status nginx
```

##### From your browser:
    mywebserver.eastus.cloudapp.azure.com
<p align="left">
  <img src="assets/test-web-page.jpg" width="800">
</p>

#### Install Certbot
```bash
sudo snap install --classic certbot
```

#### Enable certbot command
```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
<br>

### Get HTTPS certificate (Nginx plugin – easiest)
```bash
sudo certbot --nginx
```

You’ll be asked:
1. **Email address** → enter a real one
2. Agree to terms → Y
3. Choose domain →
   Select:
     mywebserver.eastus.cloudapp.azure.com <your-domain>

4. Redirect HTTP to HTTPS → choose **Redirect**
Certbot will:
  * Get the certificate
  * Modify your Nginx config
  * Reload Nginx automatically
---

### Test HTTPS
Open browser:
  For example:
  ```
    https://nginx-web.eastus.cloudapp.azure.com
  ```

You should see:
🔒 Secure lock
Valid Let’s Encrypt certificate

<p align="left">
  <img src="assets/https-webtest.jpg" width="800">
</p>
<hr>

### 📚 Key Skills Demonstrated
* Azure VM deployment
* Azure Public IP & DNS configuration
* Network Security Group configuration
* Linux system administration
* Nginx configuration
* SSL/TLS implementation
* Certificate lifecycle management


### 🎯 The things I've Learned through this
* Port 80 must remain open for Let's Encrypt validation.
* DNS must resolve publicly before certificate issuance.
* Certbot can automatically modify Nginx configuration.
* Azure NSG misconfiguration can block SSL validation.

## 🔐 Security considerations

- Restrict SSH (port 22) to your trusted public IP; do not expose it to the entire internet.
- Use SSH keys instead of password authentication.
- Keep Ubuntu and Nginx patched.
- Remove unused NSG rules and disable direct SSH access when the lab is complete.
- For production workloads, consider Azure Bastion, Defender for Cloud, managed identities, monitoring, and a dedicated domain.
- Never commit private keys, passwords, Azure credentials, or Certbot account data.

## ✅ Validate the deployment

```bash
sudo nginx -t
sudo systemctl is-active nginx
sudo certbot renew --dry-run
curl -I https://<dns-label>.eastus.cloudapp.azure.com
```

Expected results: Nginx is active, the renewal dry run succeeds, and the HTTPS request returns a successful HTTP response.

## 🧹 Clean up the lab

Azure resources can continue generating charges after you finish. Delete the lab resource group in the Azure portal, or use Azure CLI:

```bash
az group delete --name <resource-group-name> --yes --no-wait
```

Confirm that the VM, managed disk, public IP, and related networking resources have been removed.

## 🛠 Troubleshooting

- **DNS does not resolve:** confirm the DNS label on the VM's public IP and allow time for propagation.
- **HTTP/HTTPS times out:** inspect the NSG inbound rules and the VM firewall.
- **Certbot validation fails:** confirm DNS points to this VM and port 80 is publicly reachable during validation.
- **Nginx configuration fails:** run `sudo nginx -t` and correct the reported file and line before reloading.

## 📄 License

Licensed under the [MIT License](LICENSE).
