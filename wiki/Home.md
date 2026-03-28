# SYS-255 Tech Journal

Welcome to the SYS-255 Wiki! This repository documents all labs, configurations, and notes from **SYS-255: Sysadmin & Net Services I** at Champlain College.

## Course Overview

SYS-255 covers the deployment, administration, and troubleshooting of common operating system environments. Topics include user access and privileges, DHCP, DNS, remote access, file services, update and patch management, security, and remote management across Windows Server and Linux platforms.

## Environment Architecture

The lab environment consists of:

* **FW01**: pfSense router/firewall/gateway
* **WKS01**: Windows 11 workstation
* **AD01**: Windows Server 2022 (Active Directory Domain Controller)
* **DHCP01**: Rocky Linux DHCP server
* **FS01**: Windows Server Core (File Services)
* **Blog02**: Rocky Linux web server (Joomla/LAMP)

Network configuration:

* WAN Interface: 10.0.17.0/24
* LAN Interface: 10.0.5.0/24
* Domain: `thomas.local`

## Labs

* [Lab 01 - Environment Setup](lab-01-environment-setup)
* [Lab 02 - Active Directory & DNS Setup](lab-02-active-directory-and-dns-setup)
* [Lab 03 - Linux Setup & Basic Commands](lab-03-linux-setup-and-basic-commands)
* [Lab 04 - DHCP Configuration](lab-04-dhcp-configuration)
* [Lab 05 - ADDS and Group Policy](lab-05-adds-and-group-policy)
* [Lab 07 - Server Core, File Services & GPO Drive Mapping](lab-07-server-core-file-services-and-gpo-drive-mapping)

## Additional Labs & Guides

* [Apache HTTPD & Linux AD Join](apache-httpd-and-linux-ad-join)
* [Blog02 - Joomla LAMP Stack Setup](blog02-joomla-lamp-stack-setup)

## Concept Notes

* [DHCP, SSH & Linux Permissions](dhcp-ssh-and-linux-permissions)
* [DNS & Active Directory Domain Services](dns-and-active-directory-domain-services)
* [Networking Fundamentals](networking-fundamentals)

## Reference

* [Passwords & Credentials](passwords-and-credentials)
