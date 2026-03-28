# SYS-255 Tech Journal - Lab 02: Active Directory & DNS Setup

## Lab Overview

This lab covers the installation and configuration of Active Directory Domain Services (AD DS) on Windows Server 2022, including DNS configuration with forward and reverse lookup zones, domain user creation, and joining a workstation to the domain.

## Environment Architecture

* **AD01**: Windows Server 2022 (Domain Controller) — `10.0.5.5`
* **WKS01**: Windows 11 workstation — `10.0.5.108`
* **FW01**: pfSense firewall — `10.0.5.2`

Domain: `thomas.local`

## Configuration Steps

### AD01 - Initial Server Configuration

#### 1. Network Configuration via sconfig

```
IP Address: 10.0.5.5
Netmask: 255.255.255.0
Gateway: 10.0.5.2
DNS: 10.0.5.2
Time Zone: UTC-5:00 Eastern Time (US & Canada)
Computer Name: ad01-thomasmarshall
```

> **Important:** Make sure you get the hostname right before promoting to a domain controller.

#### 2. Verify Setup

```powershell
whoami
ping google.com
```

### Active Directory Domain Services Installation

#### 1. Install AD DS Role

* Open Server Manager → Manage → Add Roles and Features
* Select **Active Directory Domain Services** → Add Features
* Click Next until Install

#### 2. Promote to Domain Controller

* Click the flag notification in Server Manager
* Click **Promote this server to a domain controller**
* Select **Add a new forest**
* Domain name: `thomas.local`

```
DSRM Password: 9676gopd3h6M
```

> **Note:** Because we used a `.local` top-level domain (TLD), an error will appear during installation. Valid TLDs are `.com`, `.gov`, `.edu`, `.net`, etc. Since this is an internal domain, we leave it as is. The naming of local domains is a common debate among sysadmins.

#### 3. Post-Reboot Verification

After reboot, verify DNS now points to `127.0.0.1`:

* Open Server Manager → Local Server → Click Ethernet

### DNS Configuration

#### 1. Open DNS Manager

* Server Manager → DNS → Right-click AD01 → DNS Manager

#### 2. Create Reverse Lookup Zone

* Right-click Reverse Lookup Zones → New Zone
* Follow defaults
* Network ID: `10.0.5`

#### 3. Create DNS Records

In the `thomas.local` forward lookup zone:

* Click on `fw01-thomas` → Properties → Check PTR → Apply → Recheck → Apply
* Do the same for `ad01-thomasmarshall`

Verify: The reverse DNS entries for fw01 and ad01 should now appear in the `5.0.10` reverse lookup zone (refresh the view if needed).

### Active Directory User Creation

#### 1. Open AD Users and Computers

* Server Manager → AD DS → Right-click AD01 → Active Directory Users and Computers

#### 2. Create Admin Account

Under the Domain's Users folder:

```
Username: thomas.marshall-adm
Password: OETcq33
Group Membership: Domain Admins (uppercase D and A)
Uncheck: User must change password at next login
```

#### 3. Create Standard Account

```
Username: thomas.marshall
Password: 33qctEO
Uncheck: User must change password at next login
```

> **From this point forward**, log in using your AD `thomas.marshall` or `thomas.marshall-adm` accounts depending on the privileges needed, not the local accounts.

### WKS01 - Domain Join

#### 1. Update DNS

Set WKS01's DNS to `10.0.5.5` (AD01's address).

#### 2. Verify DNS Resolution

```powershell
nslookup 10.0.5.2
nslookup fw1-thomas.thomas.local
ping fw1-thomas.thomas.local
```

#### 3. Join Domain

* Control Panel → System and Security → System → Change Settings → Change
* Check **Domain** and type: `thomas`
* Enter credentials:

```
User: thomas.marshall-adm
Password: OETcq33
```

## PowerShell Commands Used

### Verify Hostname and Connectivity

```powershell
whoami
hostname
ping 10.0.5.2
ping fw01-thomas
```

### DNS Lookups

```powershell
nslookup 10.0.5.2
nslookup fw1-thomas.thomas.local
```

## Troubleshooting Notes

### ping fw01-thomas Fails After AD Install

* This is expected before creating the DNS A record for fw01
* After creating the A record and PTR record in DNS Manager, it should resolve

### Domain Join Fails on WKS01

* Make sure WKS01 DNS is pointing to `10.0.5.5` (AD01), not `10.0.5.2` (pfSense)
* Verify with `nslookup thomas.local` — it should resolve to AD01

### DSRM Password

* The DSRM (Directory Services Restore Mode) password is used to recover the directory in case of error
* In production, this would be used if things went terribly wrong
