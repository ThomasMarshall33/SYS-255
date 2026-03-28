# SYS-255 Tech Journal - Apache HTTPD & Linux AD Join

## Lab Overview

This lab covers two key topics: joining a Rocky Linux system to a Windows Active Directory domain using REALMD, and installing/configuring the Apache HTTP Server (httpd) with firewall rules and a custom web page.

## Key Concepts

### Apache HTTPD

* Apache is a large open-source umbrella project; the httpd server is a single but major component
* It's free and runs on Linux (can be coerced to run on Windows)
* Web default root directory: `/var/www/html`
* Default port: 80 (TCP)

### REALMD

* Allows us to leverage the centralized authentication of AD for Linux systems
* Each Linux system has its own local `/etc/passwd` and `/etc/shadow`
* REALMD allows us to join Linux servers to Windows AD domains
* Provides a single point of administration over all accounts for the entire AD domain

## Configuration Steps

### Joining Linux to Active Directory

#### 1. Install REALMD

```bash
sudo dnf install realmd
```

#### 2. Join the Domain

```bash
sudo realm join THOMAS.LOCAL
```

> **Important:** The domain name must be typed in ALL UPPERCASE (`THOMAS.LOCAL`)

### Apache HTTP Server Installation

#### 1. Install Apache

```bash
sudo dnf install httpd
```

#### 2. Start and Enable the Service

```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```

* `enable` — Configures Apache to start automatically on boot
* `start` — Starts the Apache service immediately

#### 3. Verify Apache is Running

```bash
sudo systemctl status httpd
```

You should see "active (running)" in green.

### Firewall Configuration

#### 4. Add HTTP Service

```bash
sudo firewall-cmd --permanent --add-service=http
```

#### 5. Add HTTPS Service

```bash
sudo firewall-cmd --permanent --add-service=https
```

#### 6. Reload Firewall

```bash
sudo firewall-cmd --reload
```

#### 7. Verify Firewall Rules

```bash
sudo firewall-cmd --list-all
```

Expected output should include:

```
services: ssh http https
ports: 80/tcp 443/tcp
```

### Testing Apache

#### 8. Access from Browser

From a web browser on another machine, navigate to:

```
http://your-server-hostname
```

or

```
http://your-server-ip
```

You should see the default Apache test page.

### Creating a Custom Web Page

#### 9. Remove Default Welcome Page

```bash
sudo rm -f /etc/httpd/conf.d/welcome.conf
```

#### 10. Create Custom index.html

```bash
sudo vi /var/www/html/index.html
```

Add your content:

```html
<h1>Welcome to web01-yourname</h1>
<p>Apache HTTP Server is running!</p>
```

Save and exit (`:wq` in vi).

#### 11. Test Custom Page

Refresh your browser — you should now see your custom page.

## Useful firewall-cmd Commands

| Command | Description |
|---------|-------------|
| `sudo firewall-cmd --state` | Check firewall status |
| `sudo firewall-cmd --get-services` | List all available services |
| `sudo firewall-cmd --permanent --remove-service=http` | Remove a service |
| `sudo firewall-cmd --permanent --add-port=8080/tcp` | Add a specific port |
| `sudo firewall-cmd --query-service=http` | Check if a service is enabled |
| `sudo firewall-cmd --reload` | Reload after changes |

## Troubleshooting Notes

### Apache Won't Start

```bash
sudo journalctl -u httpd -n 50
```

### Can't Access Website

* Verify Apache is running: `sudo systemctl status httpd`
* Check firewall: `sudo firewall-cmd --list-all`
* Verify network connectivity: `ping server-ip`

### Firewall Changes Not Working

Make sure to reload after making changes:

```bash
sudo firewall-cmd --reload
```
