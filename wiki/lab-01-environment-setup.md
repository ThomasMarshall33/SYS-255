# SYS-255 Tech Journal - Lab 01: Environment Setup

## Lab Overview

This lab focused on building the foundational server environment with routed networking (LAN and WAN) using pfSense as the firewall/router and configuring a Windows 11 workstation. The architecture establishes the base network that all future labs build upon.

## Environment Architecture

* **FW01**: pfSense router/firewall/gateway
* **WKS01**: Windows 11 workstation

Network configuration:

```
WAN: 10.0.17.0/24 (upstream gateway: 10.0.17.2)
LAN: 10.0.5.0/24
```

## Configuration Steps

### FW01 - pfSense Configuration

#### 1. Hardware Setup

Open the hardware page on the FW01 VM and change the network device (net1) to LAN.

```
Network Device net0 MAC: BC:24:11:1E:60:65
Network Device net1 MAC: BC:24:11:73:66:c8
```

#### 2. Interface Assignment

* Press 1 to configure WAN and LAN
* Match MAC addresses to the correct interfaces
* vtnet0 = WAN, vtnet1 = LAN
* **Say NO to setting up VLANs first**

> **Tip:** If stuck with the console, use `Ctrl+H`

#### 3. WAN Interface Configuration

```
Select 2 → Set Interface IP Address
Select 1 → Pick WAN interface
DHCP: No
IP Address: [Assigned WAN IP from Canvas]/24
Upstream Gateway: 10.0.17.2
IPv4 Name Server: 10.0.17.2
IPv6: No
DHCP Server on WAN: No
HTTP for GUI: No (use HTTPS)
```

#### 4. LAN Interface Configuration

```
Select 2 → Set Interface IP Address
Select 2 → Pick LAN interface
DHCP: No
IP Address: 10.0.5.2/24
Upstream Gateway: [Leave blank - you ARE the gateway]
DHCPv6: No
IPv6: [Press Enter to bypass]
DHCP Server on LAN: No
HTTP: No
```

### WKS01 - Windows 11 Workstation

#### 1. Local Admin Account Setup

* Open `lusrmgr.msc`
* Go to Users → Add new user
* Username: `thomas.-loc`
* Double-click the user → Member Of → Add `Administrators`
* Check the box so password never expires
* Log out and log back in

#### 2. Verify Setup

```cmd
whoami
hostname
```

#### 3. Network Configuration

Go to Internet and Network Settings → IPv4 Settings:

```
IP Address: 10.0.5.108
Subnet Mask: 255.255.255.0
Default Gateway: 10.0.5.2
Preferred DNS: 10.0.5.2
```

### pfSense Web Interface Configuration

Navigate to `https://10.0.5.2` in the browser on WKS01.

* Skip over the wizard
* Leave the setting checked to override the DNS server on PPP/WAN

```
Hostname: fw1-thomas
Domain: thomas.local
Primary DNS: 8.8.8.8
RFC1918 Networks: Uncheck "Block private networks from entering via WAN"
```

## Network Diagram

```
                    Internet
                       |
                  10.0.17.2 (Gateway)
                       |
    [WAN: Assigned IP] |
                     FW01
      [LAN: 10.0.5.2] |
                       |
                  10.0.5.108
                     WKS01
                   (Win 11)
```

## Troubleshooting Notes

### No Internet Access from WKS01

* Verify WAN gateway IP (10.0.17.2) is correct on pfSense
* Verify RFC1918 blocking is disabled in pfSense web interface
* Check that WKS01 default gateway points to 10.0.5.2

### pfSense Web Interface Not Loading

* Make sure you are using HTTPS (`https://10.0.5.2`)
* Verify WKS01 has correct IP and can ping 10.0.5.2
