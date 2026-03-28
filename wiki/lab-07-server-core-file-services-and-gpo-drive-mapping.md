# SYS-255 Tech Journal - Lab 07: Server Core, File Services & GPO Drive Mapping

## Lab Overview

This lab covers deploying a Windows Server Core instance (FS01) for file services, configuring it via `sconfig`, joining it to the domain, installing RSAT tools, creating file shares with proper permissions, and mapping network drives via Group Policy.

## Environment Architecture

* **FS01**: Windows Server Core (File Server) — `10.0.5.x`
* **AD01**: Windows Server 2022 (DC) — `10.0.5.5`
* **WKS01**: Windows 11 workstation
* **FW01**: pfSense — `10.0.5.2`

Domain: `thomas.local`

## Credentials

```
FS01 Local Admin: thomas-adm / jack
User bob: b0bthefatcat123!!
User alice: fortnitecat123!!
```

## Configuration Steps

### FS01 - Server Core Initial Setup

#### 1. Create Local Admin Account

You will be prompted to set up a username and password. This is the **Local Administrator** for this server (not the AD Domain Admin, since it is not joined to the domain yet).

> **Important:** Document the userid and password you create.

#### 2. Network Configuration via sconfig

```
IP Address: [assigned IP]
Subnet Mask: 255.255.255.0
Gateway: 10.0.5.2
DNS: 10.0.5.5
```

> **Critical:** MUST have everything ON to change IP settings and everything OFF to join the domain.

#### 3. Join Domain

* Use `sconfig` to join `thomas.local`
* Log in with your `-adm` account

### Installing RSAT Tools on AD01

Log on to AD01 as your AD Domain named `-adm` user.

#### 1. Add Features

* Server Manager → Manage → Add Roles and Features
* Under **Remote Server Administration Tools (RSAT)** Feature:
  * Add **File Service Tools**
  * Add **File Server Resource Manager Tools**

### File Share Configuration

#### 1. Create Users and Groups

Create the necessary users and security groups in Active Directory Users and Computers (refer to Lab 05 if needed for the steps).

#### 2. Create the File Share

* Open Server Manager on AD01
* Add FS01 to managed servers
* Configure file shares with appropriate NTFS and Share permissions

#### 3. Set Permissions

> **Important:** Change permissions BEFORE you complete the share setup. If you don't, you could run into an issue where the share group keeps reverting back to "Everyone."

#### 4. Verify UNC Path

Access the share via UNC path from WKS01:

```
\\fs01-thomas\sharename
```

### GPO for Mapping Network Drives

#### 1. Create New GPO

* Open Group Policy Management
* Create a new GPO within the `SYS-255` OU

#### 2. Configure Drive Mapping

* Edit the GPO
* Navigate to: User Configuration → Preferences → Windows Settings → Drive Maps
* Right-click → New → Mapped Drive

Configure the drive mapping:

```
Action: Create
Location: \\fs01-thomas\sharename
Drive Letter: [Choose letter]
Reconnect: ✓
```

#### 3. Apply and Verify

* On WKS01, run `gpupdate /force`
* Log off and log back in
* Verify the mapped drive appears in File Explorer

## Troubleshooting Notes

### Can't Join Domain from Server Core

* Make sure all network services are stopped before attempting domain join via sconfig
* Verify DNS points to AD01 (`10.0.5.5`)
* Try `ping thomas.local` to confirm DNS resolution

### Share Permissions Keep Reverting to Everyone

* Set the correct permissions BEFORE completing the share wizard
* If already created, remove the share and recreate with correct permissions from the start

### Mapped Drive Not Appearing After GPO

* Run `gpupdate /force` on WKS01
* Log off and log back in (User policies apply at login)
* Verify the GPO is linked to the correct OU
* Check security filtering on the GPO
