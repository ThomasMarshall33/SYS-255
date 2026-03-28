# SYS-255 Tech Journal - Lab 03: Linux Setup & Basic Commands

## Lab Overview

This lab introduces Rocky Linux into the environment, covering initial network configuration via `nmtui`, hostname setup, user management, and adding the Linux host to the Active Directory DNS. This is the first Linux system added to the `thomas.local` domain infrastructure.

## Environment Architecture

* **DHCP01**: Rocky Linux — `10.0.5.3`
* **AD01**: Windows Server 2022 (DC) — `10.0.5.5`
* **FW01**: pfSense — `10.0.5.2`

## Configuration Steps

### DHCP01 - Rocky Linux Initial Setup

#### 1. Login

```
User: champuser
Password: Ch@mpl@1n!25
```

#### 2. Network Configuration via nmtui

```bash
nmtui
```

* Click **Edit Connection**
* Change IPv4 from **Automatic** to **Manual**

```
IP Address: 10.0.5.3/24    (MUST include /24)
Gateway: 10.0.5.2
DNS: 10.0.5.5
Search Domain: thomas.local
```

* Make sure **Automatically Connect** has an `X`

> **Note:** If you get an error because of no name, put `sudo` before `nmtui`

#### 3. Set Hostname

```
Hostname: dhcp01-thomas
```

#### 4. Create User Account

```bash
sudo useradd thomas
sudo passwd thomas
# Password: jack
```

#### 5. Add User to Wheel (Admin) Group

```bash
sudo usermod -aG wheel thomas
```

### Adding DHCP01 to AD01 DNS

#### 1. Open DNS Manager on AD01

* Server Manager → DNS → Right-click AD01 → DNS Manager

#### 2. Create A Record

* Drop down the Forward Lookup Zone menu
* Right-click `thomas.local` → New Host

```
Name: dhcp01-thomas
IP Address: 10.0.5.3
☑ Create associated pointer (PTR) record
```

## Essential Linux Commands

### Navigation & File System

| Command | Description |
|---------|-------------|
| `pwd` | Shows the current directory |
| `cd` | Changes directory |
| `cd /home` | Go to the home directory |
| `cd ..` | Go to the parent directory |
| `ls -l` | List files with details in current directory |
| `mkdir` | Create a new directory |
| `clear` | Clear the terminal screen |

### User Management

| Command | Description |
|---------|-------------|
| `useradd name` | Create a new user account |
| `passwd user` | Set/change a user's password |
| `usermod -aG wheel username` | Add user to the admin (wheel) group |
| `whoami` | Display the current user |
| `groups` | Show groups the current user belongs to |
| `exit` | Logout of the current user |

### System & Networking

| Command | Description |
|---------|-------------|
| `ip a` | Show network interface information |
| `ssh thomas@dhcp01-thomas` | SSH into a remote host |
| `sudo -i` | Act as root user for extended time |
| `sudo` | Run a single command with elevated privileges |
| `yum install` | Install a package |

## Network Diagram

```
                     FW01
          [LAN: 10.0.5.2]
                      |
        ---------------------------------
        |              |                |
   10.0.5.108      10.0.5.5        10.0.5.3
      WKS01          AD01          DHCP01
   (Win 11)    (Server 2022)   (Rocky Linux)
                    (DC)
```

## Troubleshooting Notes

### nmtui Permission Error

* Use `sudo nmtui` if you get permission errors

### Cannot Reach Gateway

* Verify the `/24` is included in the IP address field in nmtui
* Check that the Automatically Connect checkbox is enabled
* Run `ip a` to confirm the interface has the correct IP assigned
