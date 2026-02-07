# NTFS (New Technology File System) 

## What is NTFS?

NTFS stands for **New Technology File System**. It is the main file system used by modern Windows operating systems such as Windows 10, Windows 11, and Windows Server.

A file system is responsible for:

* Storing files on the disk
* Organizing files and folders
* Tracking file locations
* Managing access permissions
* Protecting files from unauthorized access

NTFS provides **better security, reliability, and performance** compared to older file systems like FAT and HPFS.

---

## How NTFS stores files

NTFS stores files using a special structure called the **Master File Table (MFT)**.

The MFT is a table that contains a record for every file and folder on the disk.

Each file record in the MFT contains:

* File name
* File size
* File location on disk (clusters)
* Owner information
* Permissions (ACL)
* Timestamps (creation, modification, access)
* Other metadata

The actual file data is stored in small storage units called **clusters**.

### How it works step-by-step:

1. When a file is created, NTFS creates a record in the MFT
2. NTFS stores the file data in disk clusters
3. NTFS saves the cluster locations inside the MFT record
4. NTFS attaches security information (ACL) to the file
5. When the file is opened, Windows reads the MFT to locate and access the file

The MFT acts like an index that tells Windows where everything is stored.

---

## What is ACL (Access Control List)?

ACL stands for **Access Control List**.

It is a security feature used by NTFS to control:

* Who can access a file or folder
* What actions they can perform

Examples of permissions:

* Read
* Write
* Execute
* Delete
* Full Control

Each file and folder has an ACL that defines these permissions.

---

## Types of ACL

### 1. DACL (Discretionary Access Control List)

DACL controls access permissions.

It defines:

* Which users or groups can access the file
* What actions they are allowed or denied

Example:

* Administrator → Full Control
* User → Read Only
* Guest → No Access

Windows checks the DACL before allowing access.

---

### 2. SACL (System Access Control List)

SACL controls auditing and logging.

It defines:

* Which access attempts should be logged
* Successful access attempts
* Failed access attempts

This helps monitor sensitive files and detect suspicious activity.

---

## Why ACL is important for security

ACL is critical for protecting the system and data.

It helps prevent unauthorized users from:

* Reading sensitive files
* Modifying system files
* Deleting important data
* Installing malware
* Escalating privileges

ACL ensures only authorized users can access or modify files.

---

## Security use examples

### Example 1: Protecting system files

System files are protected so normal users cannot modify them. Only administrators have permission.

### Example 2: Protecting sensitive company files

Financial or confidential files can only be accessed by authorized employees.

### Example 3: Preventing unauthorized access

If an attacker gains access to a system, ACL prevents them from accessing restricted files.

### Example 4: Auditing and monitoring access

SACL logs access attempts, helping detect attacks or suspicious behavior.

---

## Final summary

NTFS is a secure and advanced file system used by Windows.

It stores files using the Master File Table (MFT) and disk clusters.

NTFS uses ACL to control and monitor access to files.

ACL improves security by restricting access and logging file activity.

This makes NTFS essential for protecting system integrity and sensitive data.

