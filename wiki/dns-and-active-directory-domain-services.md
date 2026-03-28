# SYS-255 Tech Journal - DNS & Active Directory Domain Services

## Overview

Concept notes covering Active Directory architecture, DNS integration, and domain fundamentals.

---

## Active Directory Fundamentals

### What is Active Directory?

Active Directory (AD) is a **database full of objects**. Everything in the object is a **class**, and everything you fill in is unique. Everything that defines a class was built by a developer who created that object schema.

### Core Objects

The two main object types in AD:

* **Users** — represent people or service accounts
* **Computers** — represent machines joined to the domain

### AD Core Services

**AD DS (Active Directory Domain Services)** is the primary service that provides:

* Centralized authentication
* User and computer management
* Group Policy application
* DNS integration

### Supporting Services

| Service | Notes |
|---------|-------|
| **DNS** | If DNS is not available, Microsoft will install it during the AD setup |
| **DHCP** | Can be integrated with AD for dynamic DNS updates |

---

## Domain Concepts

### What is a Domain?

A domain is an **area of jurisdiction** — you own it and are responsible for everything in it.

Example: `cyber.local`

### Domain Types

There are two different kinds of domains, but they mean the same thing — it's jurisdictional. Both represent a boundary of administrative control and authentication.

### Domain Naming

* Internal domains commonly use `.local` TLD (e.g., `thomas.local`)
* Valid public TLDs include `.com`, `.gov`, `.edu`, `.net`
* The naming of internal domains is a subject of many debates among systems administrators
* Using `.local` can cause issues with mDNS (multicast DNS) in some environments

---

## DNS and AD Integration

DNS is critical for Active Directory to function. AD relies on DNS for:

* **Service location** — clients find domain controllers via SRV records
* **Name resolution** — hostname to IP mapping (A records)
* **Reverse lookups** — IP to hostname mapping (PTR records)
* **Domain join** — workstations must resolve the domain name to find a DC

### DNS Record Types in AD

| Record Type | Purpose | Example |
|-------------|---------|---------|
| **A** | Maps hostname to IP | `ad01-thomas` → `10.0.5.5` |
| **PTR** | Maps IP to hostname | `10.0.5.5` → `ad01-thomas.thomas.local` |
| **SRV** | Locates services (auto-created by AD) | `_ldap._tcp.thomas.local` |
| **SOA** | Start of Authority for the zone | Zone metadata |
| **NS** | Name server for the zone | `ad01-thomas.thomas.local` |
