# SYS-255 Tech Journal - Networking Fundamentals

## Overview

Concept notes covering TCP vs UDP, Linux fundamentals and history, Windows Server Core, and remote administration concepts.

---

## TCP vs UDP

Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) are foundational pillars of the internet, enabling different types of data transmission from a network source to the destination.

| Feature | TCP | UDP |
|---------|-----|-----|
| **Connection** | Connection-oriented (3-way handshake) | Connectionless |
| **Reliability** | Reliable — guarantees delivery | Unreliable — no delivery guarantee |
| **Ordering** | Maintains packet order | No ordering |
| **Speed** | Slower (overhead from reliability) | Faster (minimal overhead) |
| **Use Cases** | HTTP/S, SSH, FTP, SMTP, DNS (zone transfers) | DNS (queries), DHCP, VoIP, streaming, gaming |
| **Error Checking** | Yes, with retransmission | Basic checksum only |
| **Flow Control** | Yes (windowing) | No |

**Key takeaway:** TCP is more reliable, while UDP prioritizes speed and efficiency.

---

## Linux Fundamentals

### History

* Created by **Linus Torvalds** in 1991 (Linux = "Linus's UNIX")
* Originally contained just the kernel, a compiler (gcc '87), bash ('89), and utility applications
* Reference: Unix genealogy tree at distrowatch.com

### The Linux Kernel

The kernel implements:

* Multitasking and multiuser functionality
* Hardware management
* Memory allocation
* Application execution environment

### Linux Server Roles

**Servers:**
NFS, SSH, HTTP/S, DHCP, Kerberos, DNS

**Network Appliances:**
Firewall, IDS/IPS

### Open Source Licensing Principles

What guides open-source licensing:

* Free redistribution
* Access to source code
* Integrity of the author's source code
* Distribution of license
* License must not be specific to a product
* License must not restrict other software
* License must be technology-neutral

> **Example:** Proxmox is open-source virtualization software

### Why Open Source Matters

* Often a major source of innovation
* Community approach to developing emerging standards

---

## Windows Server Core

### What is Server Core?

A minimal installation option for Windows Server that removes nearly all interactive GUI elements.

### Advantages

* Fewer resources needed
* Less code == fewer vulnerabilities
* Windows CLI for automation
* Stresses remote management
* Can do nearly all the things a full GUI server can do

### Universal Naming Convention (UNC)

UNC is a standard format for naming network resources by specifying their location on a network.

```
\\server\share\folder\file.txt
```

* Begins with two backslashes
* Followed by server name, share name, and optional path
* Allows users to access shared resources across a network without mapping drives

### SMB (Server Message Block)

SMB is a network protocol for sharing files and printers. It enables client computers to access shared resources like files and printers from a server on a local network.

| Concept | Description |
|---------|-------------|
| **UNC Path** | `\\servername\sharename` |
| **Default SMB Port** | 445 (TCP) |
| **Legacy SMB Port** | 139 (TCP) |
| **Use Case** | File shares, printer sharing, remote administration |
