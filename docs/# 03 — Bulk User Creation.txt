# 03 — Bulk User Creation

## Overview
This document covers the automated creation of 100+ Active Directory user
accounts using PowerShell scripting. Users are organized across 5 departments
within a dedicated Organizational Unit (OU).

## Environment
- Domain: lab.local
- OU: LabUsers
- Departments: IT, HR, Finance, Sales, Operations
- Total Users: 100
- Default Password: Password1!

## Step 1 — Create the LabUsers OU
1. Open Server Manager → Tools → Active Directory Users and Computers
2. Expand lab.local in the left panel
3. Right click lab.local → New → Organizational Unit
4. Name: LabUsers
5. Leave Protect container from accidental deletion checked
6. Click OK

## Step 2 — Create the Bulk User Script
1. Open Notepad as Administrator
2. Paste the script below
3. Save as C:\Scripts\bulk_users.ps1
4. Set Save as type to All Files

## Script — bulk_users.ps1
```powershell
# bulk_users.ps1 — creates 100 users in lab.local
Import-Module ActiveDirectory

$password = ConvertTo-SecureString "Password1!" -AsPlainText -Force
$ou = "OU=LabUsers,DC=lab,DC=local"

$departments = @("IT","HR","Finance","Sales","Operations")

1..100 | ForEach-Object {
    $num      = $_.ToString("000")
    $first    = "User"
    $last     = "Lab$num"
    $username = "user.lab$num"
    $dept     = $departments[($_ % 5)]

    New-ADUser `
        -GivenName       $first `
        -Surname         $last `
        -Name            "$first $last" `
        -SamAccountName  $username `
        -UserPrincipalName "$username@lab.local" `
        -Path            $ou `
        -Department      $dept `
        -AccountPassword $password `
        -Enabled         $true `
        -PasswordNeverExpires $true
}

Write-Host "Done. 100 users created in OU=LabUsers"
```

## Step 3 — Run the Script
Open PowerShell as Administrator and run:

```powershell
powershell.exe -ExecutionPolicy Bypass -File C:\Scripts\bulk_users.ps1
```

When complete you should see: