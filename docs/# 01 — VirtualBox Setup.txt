# 01 — VirtualBox Setup

## Overview
This document covers the initial VirtualBox configuration required to build
the Active Directory home lab environment including NAT Network creation and
VM hardware settings.

## Environment
- Hypervisor: Oracle VirtualBox
- Host OS: Windows
- Network: NAT Network (ADLab) — 10.0.2.0/24

## Step 1 — Create the NAT Network
1. Open VirtualBox
2. Click File → Tools → Network Manager
3. Click the NAT Networks tab
4. Click Create and configure:
   - Name: ADLab
   - IPv4 Prefix: 10.0.2.0/24
   - Enable DHCP: checked
5. Click Apply

## Step 2 — DC02 VM Settings
- Name: DC02
- Type: Microsoft Windows
- Version: Windows 2022 (64-bit)
- RAM: 4096MB
- CPUs: 2
- Disk: 50GB

### Display Settings
- Video Memory: 128MB
- Graphics Controller: VMSVGA
- 3D Acceleration: Disabled

### Network Settings
- Adapter 1: NAT Network → ADLab

### Advanced Settings
- Shared Clipboard: Bidirectional
- Drag and Drop: Bidirectional

## Step 3 — WKSTN01 VM Settings
- Name: WKSTN01
- Type: Microsoft Windows
- Version: Windows 10 (64-bit)
- RAM: 2048MB
- CPUs: 2
- Disk: 40GB

### Display Settings
- Video Memory: 128MB
- Graphics Controller: VMSVGA
- 3D Acceleration: Disabled

### Network Settings
- Adapter 1: NAT Network → ADLab

### Advanced Settings
- Shared Clipboard: Bidirectional
- Drag and Drop: Bidirectional

## Step 4 — Kali Linux VM Settings
- Name: Kali
- Type: Linux
- Version: Debian (64-bit)
- RAM: 2048MB
- CPUs: 2
- Disk: 40GB

### Network Settings
- Adapter 1: NAT Network → ADLab

## IP Address Scheme
| VM | Role | IP Address |
|---|---|---|
| DC02 | Domain Controller | 10.0.2.10 |
| WKSTN01 | Workstation | 10.0.2.20 |
| Kali | Attacker VM | 10.0.2.30 |

## Screenshots
![NAT Network](../screenshots/164552-nat-network.png)
![DC02 Network Settings](../screenshots/23-wkstn01-networ