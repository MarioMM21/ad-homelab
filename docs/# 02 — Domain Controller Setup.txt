# 02 — Domain Controller Setup

## Overview
This document covers the installation and configuration of Windows Server 2022
as the Active Directory Domain Controller for the lab.local domain.

## Environment
- Server Name: DC02
- OS: Windows Server 2022 Standard Evaluation
- IP Address: 10.0.2.10
- Domain: lab.local
- NetBIOS Name: LAB

## Step 1 — Windows Server 2022 Installation
1. Boot DC02 VM from Windows Server 2022 ISO
2. Select Language: English (United States)
3. Click Install Now
4. Select Windows Server 2022 Standard Evaluation (Desktop Experience)
5. Accept license terms
6. Select Custom Install
7. Select unallocated disk → Next
8. Let installation complete — server will reboot automatically
9. Set Administrator password: Lab@dmin123!

## Step 2 — Initial Server Configuration
Open Server Manager → Local Server and configure:

### Rename the Server
1. Click the computer name
2. Click Change
3. Set name to: DC02
4. Click OK and restart later

### Set Static IP Address
1. Click Ethernet in Local Server
2. Right click adapter → Properties
3. Double click IPv4 → Properties
4. Configure:

| Setting | Value |
|---|---|
| IP Address | 10.0.2.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.0.2.1 |
| Preferred DNS | 10.0.2.10 |
| Alternate DNS | 8.8.8.8 |

### Disable IE Enhanced Security
1. Click On next to IE Enhanced Security Configuration
2. Set Administrator to Off
3. Set Users to Off
4. Click OK

## Step 3 — Install AD DS, DNS, and DHCP Roles
1. Open Server Manager → Manage → Add Roles and Features
2. Select Role-based or feature-based installation
3. Select DC02 from the server list
4. Check the following roles:
   - Active Directory Domain Services
   - DHCP Server
   - DNS Server
5. Click Add Features when prompted
6. Click Next through remaining screens
7. Click Install and wait for completion
8. Do not close the window until Installation succeeded appears

## Step 4 — Promote to Domain Controller
1. Click the yellow flag in Se