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
``` bash
sudo apt-get install php php-cli php-fpm php-mysql php-xml php-zip php-curl php-mbstring php-intl php-soap php-json php-xmlrpc php-gd
```
## 5. Install MariaDB (or MySQL)

``` bash
sudo dnf install mariadb105-server -y  # Amazon Linux 2023
sudo apt install mariadb-server -y     # Ubuntu/Debian
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

## 6. Configure the Database

``` bash
sudo mysql -u root -p
```

## 6. Configure the Database

``` bash
sudo mysql -u root -p
```

### Inside MySQL, run:

``` bash
CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodleuser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```


## 7. Download and Install Moodle

``` bash
cd /var/www/html
git clone -b MOODLE_405_STABLE git://git.moodle.org/moodle.git  
sudo mv moodle /var/www/html/
sudo mkdir /var/www/html/moodledata
sudo chown -R www-data:www-data /var/www/html/moodle /var/www/html/moodledata  # Ubuntu/Debian
sudo chown -R apache:apache /var/www/html/moodle /var/www/html/moodledata      # Amazon Linux
sudo chmod -R 777 /var/www/html/moodledata
```

## 8. Configure Moodle

``` bash
cd /var/www/html
git clone -b MOODLE_405_STABLE git://git.moodle.org/moodle.git  
sudo mv moodle /var/www/html/
sudo mkdir /var/www/html/moodledata
sudo chown -R www-data:www-data /var/www/html/moodle /var/www/html/moodledata  # Ubuntu/Debian
sudo chown -R apache:apache /var/www/html/moodle /var/www/html/moodledata      # Amazon Linux
sudo chmod -R 777 /var/www/html/moodledata
```

Modify these lines: 
 ``` php
$CFG->dbtype    = 'mariadb';
$CFG->dblibrary = 'native';
$CFG->dbhost    = 'localhost';
$CFG->dbname    = 'moodle';
$CFG->dbuser    = 'moodleuser';
$CFG->dbpass    = 'your_password';
$CFG->wwwroot   = 'http://your-public-ip/moodle';
$CFG->dataroot  = '/var/www/html/moodledata';

```


## 9. Set Correct Permissions for Moodle
``` bash
sudo chmod -R 750 /var/www/html/moodledata
```
## 12. Configure PHP
``` bash
sudo nano /etc/php.ini               # Amazon Linux
sudo nano /etc/php/8.3/apache2/php.ini  # Ubuntu/Debian (Adjust PHP version if needed)
```
Search for this line :
``` bash
;max_input_vars = 1000
```
Replace it with this : 
``` bash
max_input_vars = 5000
```

Save the file and restart apache : 

``` bash
sudo systemctl restart httpd    # Amazon Linux
sudo systemctl restart apache2  # Ubuntu/Debian
```

Now, open http://your-public-ip/moodle in a browser and follow the instructions











