# Moodle Installation on AWS EC2 (Manual Setup)

This guide explains how to manually install **Moodle** on an **AWS EC2 instance** running Amazon Linux 2023, Ubuntu, or Debian.

## **1. Connect to Your EC2 Instance**
Use SSH to connect to your instance:
```bash
ssh -i "moodle-install.pem" ec2-user@your-ec2-public-ip
