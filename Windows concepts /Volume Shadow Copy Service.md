# Windows Concepts: Volume Shadow Copy Service (VSS)

## Overview

The **Volume Shadow Copy Service (VSS)** is a built-in Windows service that allows the operating system to create **read-only snapshots of entire volumes** (such as the `C:\` drive) at a specific point in time.

These snapshots are called:

> **Shadow Copies**

A Shadow Copy represents the exact state of files, folders, and system data at the moment the snapshot was created.

---

## How VSS Works

VSS uses a mechanism called:

> **Copy-on-Write**

Instead of copying the entire volume, VSS only stores the **original data blocks before they are modified**.

### Example

Time: 10:00
File content:

```
C:\Users\Admin\file.txt
hello
```

VSS creates a snapshot.

Time: 11:00
File content changed to:

```
modified
```

VSS preserves the original version (`hello`) in the shadow copy.

This allows recovery of previous versions without duplicating the entire disk.

---

## Use of VSS in Backup Operations

VSS plays a critical role in backup and recovery.

### Why VSS is important for backups:

* Allows backup of files that are currently in use
* Ensures data consistency during backup
* Enables system restore functionality
* Used by Windows Backup and third-party backup software

### Backup without VSS:

```
Backup fails due to locked files
```

### Backup with VSS:

```
Backup succeeds using shadow copy snapshot
```

---

## Use of VSS in Offensive Security (Attack Perspective)

VSS is commonly used by attackers to access sensitive files that are normally locked by the operating system.

### Example: Accessing the SAM database

Normal location:

```
C:\Windows\System32\config\SAM
```

This file is locked while Windows is running.

Using VSS, attackers can access:

```
\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopyX\Windows\System32\config\SAM
```

This allows extraction of password hashes.

---

### Example: Accessing NTDS.dit (Domain Controllers)

Location:

```
C:\Windows\NTDS\NTDS.dit
```

This file contains Active Directory password hashes.

Attackers use VSS to extract it from a shadow copy.

---

## Why Ransomware Deletes Shadow Copies

Ransomware often deletes shadow copies to prevent recovery:

```cmd
vssadmin delete shadows /all /quiet
```

This prevents victims from restoring encrypted files.

---

## How to List Shadow Copies (Two Methods)

---

### Method 1: Command Line (Recommended)

Open Command Prompt as Administrator:

```cmd
vssadmin list shadows
```

Example output:

```cmd
Shadow Copy Volume: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy14

Original Volume: (C:)

Creation Time: 2/8/2026 10:00:00 AM
```

---

### Explanation of Fields

**Shadow Copy Volume**

```
\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy14
```

The actual system path of the shadow copy.

**Original Volume**

```
(C:)
```

Indicates which volume was snapshotted.

**Creation Time**

Shows when the shadow copy was created.

This is important for forensic investigations.

---

### Method 2: GUI Method

1. Open File Explorer
2. Right-click any folder
3. Click Properties
4. Go to:

```
Previous Versions
```

This shows available shadow copies.

---

## Important VSS Commands

---

### List Shadow Copies

```cmd
vssadmin list shadows
```

Displays all shadow copies.

---

### List Volumes

```cmd
vssadmin list volumes
```

Displays volumes that support VSS.

---

### List Shadow Storage

```cmd
vssadmin list shadowstorage
```

Example output:

```
Used Shadow Copy Storage space
Allocated Shadow Copy Storage space
Maximum Shadow Copy Storage space
```

**Used** = current usage
**Allocated** = reserved space
**Maximum** = maximum allowed space

---

### List VSS Writers

```cmd
vssadmin list writers
```

Example output:

```
Writer name: System Writer
State: Stable
```

Writers ensure data consistency during snapshot creation.

Examples:

* System Writer
* Registry Writer
* SQL Writer

---

## Important Flags Explained

---

### /all

Targets all shadow copies.

Example:

```cmd
vssadmin delete shadows /all
```

---

### /quiet

Suppresses confirmation prompts.

Example:

```cmd
vssadmin delete shadows /all /quiet
```

Commonly used by ransomware.

---

### /for=C:

Targets a specific volume.

Example:

```cmd
vssadmin list shadows /for=C:
```

---

## How to Access a Shadow Copy

Example path:

```
\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy14
```

This allows access to previous versions of files.

Example sensitive files:

```
SAM
SYSTEM
SECURITY
NTDS.dit
```

---

## Security Importance of VSS

### Defensive Security (Blue Team)

* Recover deleted files
* Restore encrypted files
* Investigate incidents
* Recover system logs

---

### Offensive Security (Red Team)

* Extract password hashes
* Access locked system files
* Extract Active Directory database

---

### Digital Forensics

* Recover attacker-deleted evidence
* Analyze previous system states
* Timeline reconstruction

---

## Summary

The Volume Shadow Copy Service (VSS) is a critical Windows feature that creates point-in-time snapshots of volumes using Copy-on-Write. It is essential for backup, recovery, and system restore operations. From a security perspective, VSS is valuable for both defenders and attackers, as it allows recovery of deleted data and extraction of sensitive system files.

---

