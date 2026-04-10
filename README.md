# Active Directory Home Lab

A fully functional Active Directory environment built in Oracle VirtualBox
to simulate enterprise identity management, group policy enforcement, 
and domain operations.

## Lab Overview

| Component | Details |
|---|---|
| **Domain** | lab.local |
| **Domain Controller** | Windows Server 2022 (DC02) |
| **Workstation** | Windows 10 Pro (WKSTN01) |
| **Attacker VM** | Kali Linux (coming soon) |
| **Network** | VirtualBox NAT Network — 10.0.2.0/24 |
| **DC IP** | 10.0.2.10 |
| **Workstation IP** | 10.0.2.20 |

## What This Lab Demonstrates

- Active Directory DS, DNS, and DHCP installation and configuration
- Promoting a Windows Server 2022 machine to a Domain Controller
- Bulk user provisioning via PowerShell (100+ accounts across 5 departments)
- Organizational Unit (OU) structure and user organization
- Group Policy Object creation and enforcement
- Domain join and workstation management
- GPO verification via gpresult and gpupdate

## Skills Demonstrated

`Active Directory` `PowerShell` `Group Policy` `DNS` `DHCP`
`Windows Server 2022` `Windows 10 Pro` `VirtualBox` `Network Configuration`

## Tools Used

- Oracle VirtualBox
- Windows Server 2022
- Windows 10 Pro
- Kali Linux (in progress)
- PowerShell / RSAT
- Group Policy Management Console (GPMC)

## Documentation

| Step | Document |
|---|---|
| 1 | [VirtualBox Setup](docs/01-virtualbox-setup.md) |
| 2 | [DC Setup](docs/02-dc-setup.md) |
| 3 | [Bulk Users](docs/03-bulk-users.md) |
| 4 | [Domain Join](docs/04-domain-join.md) |
| 5 | [Group Policy](docs/05-group-policy.md) |
| 6 | [Kali Recon](docs/06-kali-recon.md) |
| 7 | [Lessons Learned](docs/07-lessons-learned.md) |

## Scripts

- [`bulk_users.ps1`](scripts/bulk_users.ps1) — Creates 100 AD users across 5 departments

## Group Policy Objects Created

| GPO | Purpose |
|---|---|
| Password Policy | Minimum 12 chars, complexity enabled, 90 day expiration |
| Disable USB Storage | Blocks all removable storage access |
| Account Lockout Policy | Locks account after 5 failed attempts for 30 minutes |

## Lab Status

| Task | Status |
|---|---|
| NAT Network | ✅ Complete |
| Domain Controller | ✅ Complete |
| Bulk Users (100+) | ✅ Complete |
| Domain Join | ✅ Complete |
| Group Policy | ✅ Complete |
| Kali Recon | ✅ Complete |
| Documentation | ✅ Complete |
