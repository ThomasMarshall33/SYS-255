# SYS-255 Tech Journal - Lab 04: DHCP Configuration

## Lab Overview

This lab covers configuring a DHCP server on Rocky Linux using `dhcpd`, including editing the configuration file, managing the firewall to allow DHCP requests, configuring lease times, and verifying DHCP functionality from a Windows workstation. PuTTY is also introduced as a remote management tool.

## Environment Architecture

* **DHCP01**: Rocky Linux DHCP server — `10.0.5.3`
* **WKS01**: Windows 11 workstation (DHCP client)
* **AD01**: Windows Server 2022 (DC) — `10.0.5.5`
* **FW01**: pfSense — `10.0.5.2`

## Configuration Steps

### Installing PuTTY on WKS01

PuTTY is a free SSH and Telnet client for Windows used for remote server management.

#### 1. Open Internet Explorer

* File Explorer → This PC → Local Disk C → Program Files (x86) → Internet Explorer

#### 2. Download PuTTY

* Navigate to the PuTTY download page (chiark.greenend.org.uk)
* Download `putty-64bit-0.83-installer.msi`
* Follow default installation steps

#### 3. Launch PuTTY

* File Explorer → This PC → Local Disk C → Program Files → PuTTY
* Double-click PuTTY
* Enter hostname: `dhcp01-thomas`

### DHCP Server Configuration on DHCP01

#### 1. Elevate to Root

```bash
sudo -i
```

#### 2. Backup Original Config

```bash
cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak
```

#### 3. Edit DHCP Configuration

```bash
vi /etc/dhcp/dhcpd.conf
```

Enter the DHCP configuration (subnet, range, options, etc.) then save:

* Press `Esc` → type `:wq`

#### 4. Verify Configuration

```bash
cat /etc/dhcp/dhcpd.conf
```

#### 5. Start and Enable DHCP Service

```bash
systemctl start dhcpd
systemctl enable dhcpd
```

### Firewall Configuration

Allow incoming DHCP requests through the firewall:

```bash
sudo firewall-cmd --permanent --add-service=dhcp
sudo firewall-cmd --reload
```

Verify:

```bash
sudo firewall-cmd --list-all
```

### Configuring WKS01 as DHCP Client

On WKS01, change the Ethernet adapter settings:

* Set IPv4 to **Obtain an IP address automatically**
* Set DNS to **Obtain DNS server address automatically**

### Changing DHCP Lease Time

#### 1. Edit the Config

```bash
sudo vi /etc/dhcp/dhcpd.conf
```

* Press `i` to enter insert mode
* Add/modify lease time directives
* Press `Esc` → type `:wq`

#### 2. Restart DHCP Service

```bash
sudo systemctl restart dhcpd
```

## What is PuTTY?

PuTTY is a free SSH and Telnet client for Windows. It's a terminal emulator that allows you to:

* Connect remotely to servers and network devices
* Access command-line interfaces on Linux/Unix systems
* Manage routers, switches, and other network equipment
* Execute commands on remote systems securely

It's commonly used by network administrators and IT professionals for remote system management and configuration.

## Troubleshooting Notes

### DHCP Service Won't Start

* Check config syntax: `dhcpd -t`
* Check logs: `journalctl -u dhcpd -n 50`
* Make sure the backup was made before editing

### WKS01 Not Getting an IP

* Verify DHCP service is running: `systemctl status dhcpd`
* Verify firewall allows DHCP: `firewall-cmd --list-all`
* On WKS01, release and renew: `ipconfig /release` then `ipconfig /renew`

### PuTTY Can't Connect

* Verify DHCP01 is reachable: `ping 10.0.5.3` from WKS01
* Make sure SSH service is running on DHCP01: `systemctl status sshd`
