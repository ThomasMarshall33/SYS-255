# SYS-255 Tech Journal - DHCP, SSH & Linux Permissions

## Overview

This page consolidates concept notes and hands-on configuration for DHCP fundamentals, securing SSH, and managing Linux file permissions and user accounts.

---

## DHCP Concepts

### Life Without DHCP

* Manual assignment of IP addresses for all servers and printers
* Some workstations don't need a static/permanent address
* Accounting and IP reconciliation (big spreadsheet)
* Less auto config == more manual config
* Layer 2 and 3 troubleshooting becomes harder

### Key Terms

| Term | Definition |
|------|-----------|
| **Scope** | Range of IPs a DHCP server can lease out |
| **Lease** | Temporary assignment of IP for a DHCP client, managed by the DHCP server |
| **T1** | 50% of lease duration (first renewal attempt) |
| **T2** | 87.5% of lease duration (second renewal attempt) |
| **BOOTP** | Older protocol which DHCP replaces with extra features (lease support, network settings/options) |

### DHCP Server Setup via Windows Server Manager

1. Open Server Manager → Add Roles and Features Wizard
2. Select the server and add the **DHCP Server** role
3. Click through and install
4. Click the flag notification → Complete DHCP configuration
5. Click Next on everything and close
6. Open the DHCP Management Console
7. Right-click → Open DHCP Manager → Add a Scope

Scope configuration:

```
Start/End IP: [your range]
Subnet Mask: 255.255.255.0 (/24)
Exclusions: None
Lease Duration: [set as needed]
Default Gateway: 10.0.5.2
```

Activate the scope, then enable DHCP on the target server.

> **Reference:** https://www.youtube.com/watch?v=TM4Mnv0RMiQ

---

## Securing SSH

### Disable Root Login

#### 1. Open SSH Config

```bash
sudo vi /etc/ssh/sshd_config
```

#### 2. Find the Authentication Section (around line 714)

Add or modify:

```
PermitRootLogin no
```

#### 3. Save and Exit

* Press `Esc` to exit insert mode
* Type `:wq` and press Enter

#### 4. Restart SSH Service

```bash
sudo systemctl restart sshd
```

---

## Linux File Permissions & User Management

### Creating Users and Setting Passwords

```bash
sudo useradd bob
sudo useradd alice
sudo useradd charlie

sudo passwd bob        # bobinamob
sudo passwd alice      # aliceismatalic
sudo passwd charlie    # charliethecat
```

### Creating Groups and Adding Users

```bash
sudo groupadd mygroup
sudo usermod -aG mygroup bob
sudo usermod -aG mygroup alice
sudo usermod -aG mygroup charlie
```

### Changing File Ownership

To change the owner of a file, use `chown`:

```bash
sudo chown username /path/to/file
```

To change both owner and group:

```bash
sudo chown username:groupname /path/to/file
```

### File Permission Basics

Linux permissions use the format: `rwxrwxrwx` (owner/group/others)

| Symbol | Permission | Numeric |
|--------|-----------|---------|
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |

Example:

```bash
chmod 755 /path/to/file    # Owner: rwx, Group: r-x, Others: r-x
chmod 644 /path/to/file    # Owner: rw-, Group: r--, Others: r--
```
