# SYS-255 Tech Journal - Blog02: Joomla LAMP Stack Setup

## Lab Overview

This lab covers the complete installation of Joomla CMS on a Rocky Linux 9 server, including network configuration, Active Directory domain join, LAMP stack installation (Linux, Apache, MariaDB, PHP), and Joomla deployment with Apache virtual hosts.

## Environment Architecture

* **Blog02**: Rocky Linux 9 web server — `10.0.5.11`
* **AD01**: Windows Server 2022 (DC) — `10.0.5.5`
* **FW01**: pfSense — `10.0.5.2`

Domain: `thomas.local`

## Phase 1: Network Configuration

### Configure Static IP

```bash
# Use nmtui (with sudo)
sudo nmtui
```

```
IP Address: 10.0.5.11/24
Gateway: 10.0.5.2
DNS: 10.0.5.5
Search Domain: thomas.local
☑ Automatically Connect
```

### Set Hostname

```bash
sudo hostnamectl set-hostname blog02.thomas.local
```

### Verify Connectivity

```bash
ping -c 4 8.8.8.8
ping -c 4 ad01-thomas.thomas.local
```

## Phase 2: Join Active Directory Domain

### Install Required Packages

```bash
sudo dnf install -y realmd sssd oddjob oddjob-mkhomedir adcli samba-common-tools
```

### Discover and Join Domain

```bash
sudo realm discover thomas.local
sudo realm join --user=Administrator thomas.local
```

### Verify Domain Join

```bash
sudo realm list
id administrator@thomas.local
```

### Configure Sudoers for AD Users (Optional)

```bash
sudo visudo
# Add line:
# %domain\ admins@thomas.local ALL=(ALL) ALL
```

### Test AD Login

```bash
su - youradusername@thomas.local
```

## Phase 3: Install LAMP Stack

### Update System

```bash
sudo dnf update -y
```

### Install Apache

```bash
sudo dnf install -y httpd
sudo systemctl enable httpd
sudo systemctl start httpd
```

### Install MariaDB

```bash
sudo dnf install -y mariadb-server mariadb
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

### Secure MariaDB

```bash
sudo mysql_secure_installation
```

* Press Enter for no root password
* Set new root password
* Answer Y to all remaining prompts

### Install PHP and Extensions

```bash
sudo dnf install -y php php-mysqlnd php-gd php-xml php-mbstring php-json php-intl php-zip php-curl
sudo systemctl restart httpd
```

### Configure Firewall

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

## Phase 4: Create Joomla Database

### Log into MariaDB

```bash
sudo mysql -u root -p
```

### Create Database and User

```sql
CREATE DATABASE joomla_db;
CREATE USER 'joomla_user'@'localhost' IDENTIFIED BY 'YourStrongPassword123!';
GRANT ALL PRIVILEGES ON joomla_db.* TO 'joomla_user'@'localhost';
FLUSH PRIVILEGES;
QUIT;
```

## Phase 5: Download and Install Joomla

### Install Tools and Download

```bash
sudo dnf install -y wget unzip
cd /var/www/html
sudo mkdir joomla
cd joomla
sudo wget https://downloads.joomla.org/cms/joomla4/4-1-5/Joomla_4-1-5-Stable-Full_Package.zip?format=zip -O joomla.zip
```

### Extract and Set Permissions

```bash
sudo unzip joomla.zip
sudo rm joomla.zip
sudo chown -R apache:apache /var/www/html/joomla
sudo chmod -R 755 /var/www/html/joomla
sudo chcon -R -t httpd_sys_content_rw_t /var/www/html/joomla
```

## Phase 6: Configure Apache Virtual Host

### Create Config File

```bash
sudo vi /etc/httpd/conf.d/blog02.thomas.local.conf
```

### Add Configuration

```apache
<VirtualHost *:80>
    ServerName blog02.thomas.local
    DocumentRoot /var/www/html/joomla

    <Directory /var/www/html/joomla/>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>

    ErrorLog /var/log/httpd/blog02-error.log
    CustomLog /var/log/httpd/blog02-access.log combined
</VirtualHost>
```

Press `Esc`, then `:wq!` to save and exit.

### Restart Apache

```bash
sudo systemctl restart httpd
sudo systemctl status httpd
```

## Phase 7: Complete Joomla Web Installation

### Access Joomla Installer

Open browser: `http://blog02.thomas.local` (or use IP if DNS not configured)

### Step 1 — Site Configuration

* Site Name: Your site name
* Email: Your email
* Username: admin
* Password: Strong password

### Step 2 — Database Configuration

```
Database Type: MySQLi
Host Name: localhost
Username: joomla_user
Password: YourStrongPassword123!
Database Name: joomla_db
```

### Step 3 — Finalization

* Review settings → Click Install
* **Remove the installation folder** when prompted

### Access Admin Panel

```
http://blog02.thomas.local/administrator
```

## Verification Checklist

- [ ] Network is configured and accessible
- [ ] Can ping domain controller
- [ ] Successfully joined AD domain
- [ ] Can login with AD credentials
- [ ] Apache is running (`sudo systemctl status httpd`)
- [ ] MariaDB is running (`sudo systemctl status mariadb`)
- [ ] Joomla site loads in browser
- [ ] Can access Joomla admin panel

## Troubleshooting Notes

### Apache Won't Start

```bash
sudo systemctl status httpd
sudo journalctl -xe
```

### Database Connection Fails

```bash
sudo systemctl status mariadb
sudo mysql -u joomla_user -p joomla_db
```

### SELinux Permission Issues

```bash
sudo getenforce
sudo setenforce 0    # Temporary, for testing only
```

### Optional: Install Let's Encrypt SSL

```bash
sudo dnf install -y certbot python3-certbot-apache
sudo certbot --apache -d blog02.thomas.local
```
