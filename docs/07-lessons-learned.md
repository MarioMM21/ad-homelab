# 07 — Lessons Learned

## Overview
This document covers the challenges encountered during the build of this
Active Directory home lab, how they were resolved, and key takeaways.
This is the most valuable document in any home lab project because it
demonstrates real troubleshooting experience.

## Challenge 1 — Kali Linux Blank Screen After Boot
### Problem
After initial Kali installation the VM would boot to a black or blinking
screen. The desktop environment failed to load.

### Root Cause
Default VirtualBox display settings are incompatible with Kali Linux.
3D acceleration enabled combined with wrong graphics controller caused
the display manager to crash on boot.

### Resolution
Power off the VM and change VirtualBox display settings:
- Video Memory: 128MB
- Graphics Controller: VMSVGA
- 3D Acceleration: Disabled

Then reinstall the desktop environment:
```bash
sudo apt install -y virtualbox-guest-x11
sudo reboot
```

### Lesson
Always configure display settings before first boot on any Linux VM
in VirtualBox. VMSVGA is the most compatible graphics controller.

---

## Challenge 2 — Kali Linux Broken Package Dependencies
### Problem
During the initial apt full-upgrade the Kali VM lost network connectivity
mid-download. This caused cascading broken package dependencies including
libgtk-3, libnettle, and kali-wallpapers-2026 that could not be resolved.

### Root Cause
Interrupted package downloads corrupt the dpkg package state. Once
multiple critical system libraries are in a broken state apt cannot
resolve the dependency chain automatically.

### Resolution Attempts
```bash
sudo apt --fix-broken install -y
sudo dpkg --configure -a
sudo dpkg --remove --force-remove-reinstreq kali-wallpapers-2026
sudo apt full-upgrade -y --allow-downgrades --fix-missing
```

### Final Resolution
Downloaded a fresh Kali ISO and performed a clean reinstall. The broken
state was too deep to recover cleanly. Fresh install took 15 minutes and
resolved all issues immediately.

### Lesson
Always let apt full-upgrade complete fully before rebooting. Never
interrupt a package upgrade. If a Kali install becomes too broken to
recover, a fresh install is faster than fighting cascading dependencies.

---

## Challenge 3 — GPG Public Key Error During apt update
### Problem
After fresh Kali install apt update returned:
NO_PUBKEY ED65462EC8D5E4C5

### Root Cause
The Kali archive signing key was missing from the trusted keyring.
This happens when the ISO is slightly outdated and the key has rotated.

### Resolution
```bash
sudo wget -q -O /etc/apt/trusted.gpg.d/kali-extra.gpg \
https://archive.kali.org/archive-key.asc
sudo apt update && sudo apt full-upgrade -y
```

### Lesson
Always add the Kali archive key immediately after a fresh install before
running apt update. This prevents the GPG error entirely.

---

## Challenge 4 — Windows 10 Home Cannot Join Domain
### Problem
After installing Windows 10 from the Media Creation Tool the domain
field in System Properties was greyed out and could not be clicked.

### Root Cause
The Media Creation Tool installed Windows 10 Home edition. Only Windows
10 Pro, Education, or Enterprise editions support domain join.

### Resolution
Upgraded in place from Home to Pro using a generic upgrade key:
1. Press Windows Key + R → type slui 3 → Enter
2. Enter key: VK7JG-NPHTM-C97JM-9MPGT-3V66T
3. Allow upgrade to complete
4. Verify with winver — should show Windows 10 Pro

### Lesson
When downloading Windows 10 for a domain lab always verify the edition
before installation. The Media Creation Tool defaults to Home on some
configurations. Use slui 3 to upgrade without reinstalling.

---

## Challenge 5 — Static IP Would Not Save on Windows 10
### Problem
When setting the static IP on WKSTN01 using the modern Windows Settings
app the Save button would not apply the changes.

### Root Cause
The modern Windows 10 Settings app has a known bug where IP settings
sometimes fail to save properly especially in VM environments.

### Resolution
Used the classic Control Panel method instead:
1. Press Windows Key + R → type ncpa.cpl → Enter
2. Right click adapter → Properties
3. Double click IPv4 → Properties
4. Enter IP settings and click OK

### Lesson
Always use ncpa.cpl and the classic Control Panel for network settings
in Windows VMs. The modern Settings app is unreliable for static IP
configuration in virtualized environments.

---

## Challenge 6 — Git Push Rejected After Direct GitHub Edit
### Problem
After editing README.md directly on GitHub the local git push was
rejected with:
error: failed to push some refs
hint: Updates were rejected because the remote contains work
hint: that you do not have locally

### Root Cause
Editing files directly on GitHub creates a commit on the remote that
does not exist locally. This causes a divergence between local and
remote branches.

### Resolution
```bash
git pull origin main
git push origin main
```

### Lesson
Always pull before pushing when working across multiple locations or
editing files directly on GitHub. Make it a habit to run git pull
first every time you sit down to work on a project.

---

## Key Takeaways

### Technical Skills Gained
- Active Directory domain setup and administration
- PowerShell scripting for bulk user provisioning
- Group Policy creation and enforcement
- Network configuration in VirtualBox
- Linux troubleshooting and package management
- Git version control and GitHub documentation
- SMB enumeration and AD reconnaissance using CrackMapExec

### Security Concepts Demonstrated
- Defense in depth through layered GPO policies
- Attacker perspective via Kali Linux recon
- Understanding of SMB share risks and admin share exposure
- Importance of least privilege in Active Directory
- How domain enumeration works from an attacker viewpoint

### What I Would Do Differently
- Configure VirtualBox display settings before first boot on every VM
- Verify Windows edition before starting installation
- Use ncpa.cpl instead of modern Settings for network config
- Always pull from GitHub before pushing local changes
- Take snapshots in VirtualBox before major configuration changes