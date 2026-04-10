# 05 — Group Policy

## Overview
This document covers the creation and enforcement of Group Policy Objects
(GPOs) on the lab.local domain. Three GPOs were created and applied to
the domain and verified on WKSTN01.

## Environment
- Domain: lab.local
- Domain Controller: DC02
- Workstation: WKSTN01
- Tool: Group Policy Management Console (GPMC)

## GPOs Created
| GPO Name | Purpose | Scope |
|---|---|---|
| Password Policy | Enforce strong password requirements | lab.local |
| Disable USB Storage | Block all removable storage access | lab.local |
| Account Lockout Policy | Lock accounts after failed attempts | lab.local |

## Step 1 — Open Group Policy Management Console
On DC02 open Server Manager:
1. Click Tools in the top right
2. Click Group Policy Management
3. Expand Forest: lab.local → Domains → lab.local

## Step 2 — Create Password Policy GPO
1. Right click lab.local
2. Click Create a GPO in this domain and Link it here
3. Name: Password Policy
4. Click OK
5. Right click Password Policy → Edit

Navigate to:Computer Configuration → Policies → Windows Settings →
Security Settings → Account Policies → Password Policy

Configure the following settings:

| Setting | Value |
|---|---|
| Minimum password length | 12 characters |
| Password must meet complexity requirements | Enabled |
| Maximum password age | 90 days |
| Minimum password age | 1 day |
| Enforce password history | 10 passwords remembered |

For each setting:
1. Double click the setting
2. Check Define this policy setting
3. Enter the value
4. Click OK

## Step 3 — Create Disable USB Storage GPO
1. Right click lab.local
2. Click Create a GPO in this domain and Link it here
3. Name: Disable USB Storage
4. Click OK
5. Right click Disable USB Storage → Edit

Navigate to:Computer Configuration → Policies → Administrative Templates →
System → Removable Storage Access

Enable all of the following settings:
- All Removable Storage classes: Deny all access
- CD and DVD: Deny read access
- CD and DVD: Deny write access
- Removable Disks: Deny read access
- Removable Disks: Deny write access
- Tape Drives: Deny read access
- Tape Drives: Deny write access
- WPD Devices: Deny read access
- WPD Devices: Deny write access

For each setting:
1. Double click the setting
2. Select Enabled
3. Click OK

## Step 4 — Create Account Lockout Policy GPO
1. Right click lab.local
2. Click Create a GPO in this domain and Link it here
3. Name: Account Lockout Policy
4. Click OK
5. Right click Account Lockout Policy → Edit

Navigate to:Computer Configuration → Policies → Windows Settings →
Security Settings → Account Policies → Account Lockout Policy

Configure the following settings:

| Setting | Value |
|---|---|
| Account lockout threshold | 5 invalid attempts |
| Account lockout duration | 30 minutes |
| Reset account lockout counter after | 30 minutes |

## Step 5 — Apply GPOs to WKSTN01
Switch to WKSTN01 and open CMD as Administrator:

```cmd
gpupdate /force
```

Expected output:
Updating Computer Policy...
Updating User Policy...
Computer Policy update has completed successfully.
User Policy update has completed successfully.

## Step 6 — Verify GPOs Are Applied
Still in CMD on WKSTN01 run:

```cmd
gpresult /scope computer /r
```

Under Applied Group Policy Objects you should see:
Account Lockout Policy
Disable USB Storage
Password Policy
Default Domain Policy

## Security Impact
| GPO | Security Benefit |
|---|---|
| Password Policy | Prevents weak passwords and credential attacks |
| Disable USB Storage | Prevents data exfiltration via removable media |
| Account Lockout Policy | Mitigates brute force and password spray attacks |

## Screenshots
![GPO Console](../screenshots/32-gpo-console.png)
![Password Policy Settings](../screenshots/34-gpo-password-policy-settings.png)
![USB Storage Disabled](../screenshots/35-gpo-disable-usb.png)
![Account Lockout](../screenshots/36-gpo-account-lockout.png)
![GPUpdate Force](../screenshots/37-gpupdate-force.png)
![GPResult](../screenshots/38-gpresult.png)

