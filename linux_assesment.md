default gate way:10.0.5.2
ip for blog: 10.0.5.11
dns: thomas.local 
https://wiki.crowncloud.net/?How_to_Install_Joomla_with_LAMP_Stack_on_Rocky_Linux_9#How+to+Install+Joomla+with+LAMP+Stack+on+Rocky+Linux+9
https://www.howtoforge.com/how-to-install-strapi-cms-on-rocky-linux-9/




# Complete Joomla Installation Guide for blog02 - Rocky Linux 9

## Overview
This guide covers the complete installation process for Joomla on Rocky Linux 9, including:
1. Network Configuration
2. Active Directory Domain Join
3. LAMP Stack Installation
4. Joomla Installation

---

## Phase 1: Network Configuration

### Configure Static IP (if needed)
```bash
# Check current network interface
nmcli device status


#NMTUI 
# Configure static IP (adjust interface name, typically ens33 or ens192)
sudo nmcli con mod ens33 ipv4.addresses 10.0.5.11/24
sudo nmcli con mod ens33 ipv4.gateway 10.0.5.2
sudo nmcli con mod ens33 ipv4.dns "192.168.1.Y"  # Your AD DC IP
sudo nmcli con mod ens33 ipv4.method manual
sudo nmcli con up ens33
```

### Verify Network Connectivity
```bash
ping -c 4 8.8.8.8
ping -c 4 your-domain-controller.yourdomain.com
```

### Set Hostname
```bash
sudo hostnamectl set-hostname blog02.yourdomain.com
```

---

## Phase 2: Join Active Directory Domain

### Install Required Packages
```bash
sudo dnf install -y realmd sssd oddjob oddjob-mkhomedir adcli samba-common-tools
```

### Discover Domain
```bash
sudo realm discover yourdomain.com
```

### Join the Domain
```bash
sudo realm join --user=Administrator yourdomain.com
# Enter AD admin password when prompted
```

### Verify Domain Join
```bash
sudo realm list
id administrator@yourdomain.com
```

### Configure Sudoers for AD Users/Groups (Optional)
```bash
sudo visudo
# Add line:
# %domain\ admins@yourdomain.com ALL=(ALL) ALL
```

### Test AD Login
```bash
su - youradusername@yourdomain.com
```

---

## Phase 3: Install LAMP Stack

### Update System
```bash
sudo dnf update -y
```

### Install Apache Web Server
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
- Press Enter for no root password
- Set new root password
- Answer Y to all remaining prompts

### Install PHP and Required Extensions
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

---

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

---

## Phase 5: Download and Install Joomla

### Install Required Tools
```bash
sudo dnf install -y wget unzip
```

### Download Joomla
```bash
cd /var/www/html
sudo mkdir joomla
cd joomla
sudo wget https://downloads.joomla.org/cms/joomla4/4-1-5/Joomla_4-1-5-Stable-Full_Package.zip?format=zip -O joomla.zip
```

### Extract Joomla
```bash
sudo unzip joomla.zip
sudo rm joomla.zip
```

### Set Permissions
```bash
sudo chown -R apache:apache /var/www/html/joomla
sudo chmod -R 755 /var/www/html/joomla
sudo chcon -R -t httpd_sys_content_rw_t /var/www/html/joomla
```

---

## Phase 6: Configure Apache Virtual Host

### Create Virtual Host Configuration
```bash
sudo vi /etc/httpd/conf.d/blog02.yourdomain.com.conf
```

### Add Configuration (press `i` to insert)
```apache
<VirtualHost *:80>
    ServerName blog02.yourdomain.com
    DocumentRoot /var/www/html/joomla
    
    <Directory /var/www/html/joomla/>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>
    
    ErrorLog /var/log/httpd/blog02-error.log
    CustomLog /var/log/httpd/blog02-access.log combined
</VirtualHost>
```

Press `Esc`, then type `:wq!` and press `Enter` to save and exit

### Restart Apache
```bash
sudo systemctl restart httpd
sudo systemctl status httpd
```

---

## Phase 7: Complete Joomla Web Installation

### Access Joomla Installer
Open browser: `http://blog02.yourdomain.com` (or use IP if DNS not configured)

### Configuration Steps

**Step 1 - Site Configuration:**
- Site Name: Your site name
- Email: Your email
- Username: admin (or your AD username)
- Password: Strong password
- Click "Setup Database Connection"

**Step 2 - Database Configuration:**
- Database Type: MySQLi
- Host Name: localhost
- Username: joomla_user
- Password: YourStrongPassword123!
- Database Name: joomla_db
- Click "Install Joomla"

**Step 3 - Finalization:**
- Review settings
- Click "Install"
- **Remove installation folder** when prompted

### Access Admin Panel
Navigate to: `http://blog02.yourdomain.com/administrator`
Login with credentials created above

---

## Verification Checklist

- [ ] Network is configured and accessible
- [ ] Can ping domain controller
- [ ] Successfully joined AD domain
- [ ] Can login with AD credentials
- [ ] Apache is running (`sudo systemctl status httpd`)
- [ ] MariaDB is running (`sudo systemctl status mariadb`)
- [ ] Joomla site loads in browser
- [ ] Can access Joomla admin panel

---

## Important Notes

- Replace `yourdomain.com` with your actual domain
- Replace IP addresses with your network's configuration
- Replace `YourStrongPassword123!` with actual strong passwords
- Make sure firewall allows HTTP/HTTPS traffic
- Document all passwords securely!

---

## Troubleshooting

### If Apache won't start:
```bash
sudo systemctl status httpd
sudo journalctl -xe
```

### If database connection fails:
```bash
sudo systemctl status mariadb
sudo mysql -u joomla_user -p joomla_db
```

### Check SELinux if permission issues:
```bash
sudo getenforce
sudo setenforce 0  # Temporary, for testing only
```

---

## Optional: Install Let's Encrypt SSL

After basic setup is working, secure your site with SSL:
```bash
sudo dnf install -y certbot python3-certbot-apache
sudo certbot --apache -d blog02.yourdomain.com
```

---

**Created for System Administration Class - blog02 Installation**
