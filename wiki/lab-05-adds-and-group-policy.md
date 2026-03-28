# SYS-255 Tech Journal - Lab 05: ADDS and Group Policy

## Lab Overview

This lab covers creating an Organizational Unit (OU) structure in Active Directory, managing users and groups, and authoring Group Policy Objects (GPOs) with security filtering. The goal is to control the user experience on domain workstations using targeted group policies.

## Environment Architecture

* **AD01**: Windows Server 2022 (DC) — `10.0.5.5`
* **WKS01**: Windows 11 workstation (domain-joined)

Domain: `thomas.local`

## Configuration Steps

### OU Structure Creation

#### 1. Open Active Directory Users and Computers

* Server Manager → Tools → Active Directory Users and Computers

#### 2. Create Parent OU

* Right-click on `thomas.local` → New → Organizational Unit
* Name: `SYS255`

#### 3. Create Child OUs

Right-click on `SYS255` → New → Organizational Unit for each:

* `Accounts`
* `Computers`
* `Groups`

### User Account Creation

Create three users inside the `SYS255\Accounts` OU:

```
User: alice
Password: alicethefatcat123!!

User: bob
Password: same as alice

User: charlie
Password: same as alice
```

### Computer Object Management

* Drag `WKS01` from `thomas.local\Computers` to the `SYS255\Computers` OU
* This allows us to treat SYS255 OU computers differently than others

### Security Group Creation

#### 1. Create Group

* Right-click `SYS255\Groups` → New → Group
* Name: `custom-desktop`
* Type: Global Security Group (leave defaults)

#### 2. Add Members

* Add **alice** and **bob** to `custom-desktop`
* Do **NOT** add charlie

> **Tip:** When adding members, just type their names into the bottom text box — nothing else is required.

### Group Policy Configuration

#### 1. Create the GPO

* Open Group Policy Management (Server Manager → Tools)
* Right-click the `SYS255` OU → Create a GPO in this domain, and Link it here
* Name: `SYS255`

#### 2. Configure Security Filtering

The GPO should only apply to users in the OU who are members of the `custom-desktop` security group.

**Step 1:** Add `custom-desktop` group to the Security Filter

**Step 2:** Remove `Authenticated Users` from the Security Filter

**Step 3:** Add `Domain Computers` to the Security Filter

> **Tip:** If you can't find something, try typing the first part of the name. For example, typing "domain" will show everything with "domain" in the name.

**Step 4:** Delegation tab → Advanced → Uncheck **Apply Group Policy** → Select **Deny**

#### 3. Edit the GPO

Right-click the `SYS255` GPO → Edit

**Key concepts:**

* **Computer settings** are applied when workstations turn on
* **User settings** apply after users login

Common GPO use cases:

* Desktop backgrounds
* Browser settings
* Password policies
* Network shares and printers
* Redirected folders
* Microsoft BitLocker
* Application allowed list policies
* Logon scripts

#### 4. Disable Previous Logon Display

The default display of previously logged-on users is widely considered a security vulnerability, particularly in shared systems. Configure the policy to turn off the default display of previously logged-in users.

* Navigate to: Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options
* Enable: **Interactive logon: Don't display last signed-in**

## Troubleshooting Notes

### GPO Not Applying to Users

* Verify the user is a member of the `custom-desktop` group
* Verify `Authenticated Users` has been removed from Security Filtering
* Verify `Domain Computers` has been added
* On WKS01, force a policy update: `gpupdate /force`

### Can't Find Domain Computers in Security Filter

* Click **Object Types** and make sure **Computers** is checked
* Type "Domain Computers" and click Check Names

### GPO Applies to Everyone Instead of Filtered Group

* Double-check the Delegation tab → Advanced settings
* Make sure **Apply Group Policy** is set to **Deny** for `Authenticated Users`
