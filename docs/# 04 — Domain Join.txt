# 04 — Domain Join

## Overview
This document covers joining the Windows 10 Pro workstation (WKSTN01)
to the lab.local domain and verifying successful domain membership.

## Environment
- Workstation Name: WKSTN01
- OS: Windows 10 Pro
- IP Address: 10.0.2.20
- Domain: lab.local
- Domain Controller: DC02 (10.0.2.10)

## Prerequisites
- DC02 must be running and lab.local domain must be active
- WKSTN01 must be on the ADLab NAT Network
- Windows 10 Pro edition required — Home edition cannot join a domain

## Step 1 — Install Guest Additions on WKSTN01
1. With WKSTN01 running click Devices in VirtualBox menu
2. Click Insert Guest Additions CD Image
3. Open File Explorer → This PC
4. Double click CD Drive VirtualBox Guest Additions
5. Double click VBoxWindowsAdditions.exe
6. Click Next → Next → Install
7. Select Reboot Now when finished

## Step 2 — Set Static IP Address
1. Press Windows Key + R → type ncpa.cpl → Enter
2. Right click Ethernet → Properties
3. Double click Internet Protocol Version 4 (TCP/IPv4)
4. Select Use the following IP address and enter:

| Setting | Value |
|---|---|
| IP Address | 10.0.2.20 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.0.2.1 |
| Preferred DNS | 10.0.2.10 |
| Alternate DNS | 8.8.8.8 |

5. Click OK → OK → Close

## Step 3 — Verify Connectivity to DC02
Open CMD and run:

```cmd
ping 10.0.2.10
```

Expected output: 4 packets sent, 4 received

Then verify DNS resolution:

```cmd
nslookup lab.local
```

Expected output: Server resolves to 10.0.2.10

## Step 4 — Upgrade Windows 10 Home to Pro
If Windows 10 Home is installed domain join will be greyed out.
Upgrade to Pro using:

1. Press Windows Key + R → type slui 3 → Enter
2. Enter generic upgrade key: VK7JG-NPHTM-C97JM-9MPGT-3V66T
3. Click Next and allow upgrade to complete
4. Verify edition with Windows Key + R → winver

## Step 5 — Join the Domain
1. Press Windows Key + R → type sysdm.cpl → Enter
2. Click Computer Name tab
3. Click Change
4. Under Member of select Domain
5. Type: lab.local
6. Click OK
7. Enter credentials when prompted:
   - Username: Administrator
   - Password: Lab@dmin123!
8. Click OK
9. Welcome to the lab.local domain popup will appear
10. Click OK → OK → Restart Now

## Step 6 — Log in as Domain User
At the login screen:
1. Click Other user
2. Enter:
   - Username: LAB\Administrator
   - Password: Lab@dmin123!

## Step 7 — Verify Domain Membership
Open CMD as Administrator and run:

```cmd
whoami
```

Expected output:

Then run:

```cmd
systeminfo | findstr /B /C:"Domain"
```

Expected output:

## Step 8 — Verify WKSTN01 in Active Directory
On DC02 open Active Directory Users and Computers:
1. Expand lab.local
2. Click Computers container
3. WKSTN01 should appear in the list

## Screenshots
![WKSTN01 Static IP](../screenshots/181623-wkstn01-static-ip.png)
![WKSTN01 Network Settings](../screenshots/23-wkstn01-network-settings.png)
![WKSTN01 Desktop](../screenshots/24-wkstn01-desktop.png)
![AD Users Computers](../screenshots/170924-ad-users-computers.png)