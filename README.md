# Moodle Installation on AWS EC2 (Manual Setup)

This guide explains how to manually install **Moodle** on an **AWS EC2 instance** running Amazon Linux 2023, Ubuntu, or Debian.

## **1. Connect to Your EC2 Instance**
Use SSH to connect to your instance:
```bash
ssh -i "moodle-install.pem" ec2-user@your-ec2-public-ip
```

## **2. Update System Packages
```bash
sudo apt update && sudo apt upgrade -y  # For Ubuntu/Debian
sudo dnf update -y                     # For Amazon Linux 2023
```

### **3. Install Apache Web Server
``` bash
sudo apt install apache2 -y  # Ubuntu/Debian
sudo systemctl start apache2  
sudo systemctl enable apache2

sudo dnf install httpd -y    # Amazon Linux 2023
sudo systemctl start httpd   
sudo systemctl enable httpd
```
## **4. Install PHP and Required Extensions


