
# Users and Groups

---

##  Core Concepts

### User
- A user is an individual account that can:
  - Log in to the system
  - Execute commands
  - Run applications
- A user does **not** inherently have privileges
- Privileges are inherited from **group memberships**

### Group
- A group is a container for privileges
- Groups cannot log in
- Any user added to a group inherits its permissions

**Security Formula:**
```

User + Group Membership = Access Token = Effective Privileges

````

---

## 2. Local Users

### 2.1 Offensive – User Enumeration
```cmd
net user
````

**Attacker Objective:**

* Identify valid local accounts
* Detect naming patterns
* Build a target list for credential attacks

---

### 2.2 Offensive – User Profiling

```cmd
net user Example
```

**Key Fields for Attackers:**

* Account active
* Password expires
* Last logon
* Local Group Memberships

**Abuse Opportunities:**

* Password never expires → long-term persistence
* Old or inactive accounts → low monitoring

---

### 2.3 Offensive – Malicious User Creation (Persistence)

```cmd
net user backdoor P@ssw0rd /add
net localgroup Administrators backdoor /add
```

**Impact:**

* Local persistence mechanism
* Privileged backdoor account

---

### 2.4 Defensive – User Monitoring

* Monitor for:

  * New local user creation
  * Disabled account reactivation
  * Password policy modifications
* User creation without a change request = suspicious activity

---

## 3. Local Groups

### 3.1 Offensive – Group Enumeration

```cmd
net localgroup
```

**High-Value Target Groups:**

* Administrators
* Remote Desktop Users
* Backup Operators
* Event Log Readers

---

### 3.2 Offensive – Group Membership Analysis

```cmd
net localgroup Administrators
```

**Objective:**

* Identify low-privileged users inside high-privilege groups

---

### 3.3 Offensive – Privilege Escalation via Group Membership

```cmd
net localgroup "Remote Desktop Users" attacker /add
```

**Result:**

* Remote access without full administrator privileges
* Low-noise lateral movement

---

### 3.4 Offensive – Abuse of Non-Admin Groups

| Group                   | Abuse Capability             |
| ----------------------- | ---------------------------- |
| Backup Operators        | Dump SAM and SYSTEM          |
| Event Log Readers       | Read or clear security logs  |
| Power Users             | Execute high-impact binaries |
| Remote Management Users | Remote system management     |

---

### 3.5 Defensive – Group Integrity Monitoring

* Monitor:

  * Group membership changes
  * Creation of new privileged groups
* Unexpected group modification = high-severity alert

---

## 4. Local Administrator

### 4.1 Offensive – Why Local Administrator Matters

If an attacker gains Local Administrator privileges, they can:

* Disable security controls
* Dump credentials
* Create services and scheduled tasks

**Result:** Full system compromise

---

### 4.2 Defensive – Monitoring Local Administrator Changes

* Detect:

  * Additions to the Administrators group
  * Newly created administrator accounts
* Any admin group modification = critical alert

---

## 5. GUI vs Command Prompt (CMD)

### Offensive Perspective

* CMD is:

  * Native to Windows
  * Low-noise
  * Always available

```cmd
net user
net localgroup
net localgroup Administrators
```

---

### Defensive Perspective

* CMD is used for:

  * Rapid system auditing
  * Incident validation
  * Pre/post-incident comparison

---

## 6. CMD Flags and Their Security Impact

| Flag                   | Offensive Use                  | Defensive Interpretation               |
| ---------------------- | ------------------------------ | -------------------------------------- |
| /add                   | Persistence and privilege gain | Unauthorized account or group creation |
| /delete                | Removal of legitimate users    | Destructive or evasive activity        |
| Password never expires | Long-term access               | Policy misconfiguration                |
| Account active         | Enable access                  | Dormant account abuse                  |

---

## 7. Detection and Logging

### Critical Events to Detect

* Local user creation or deletion
* Group membership changes
* Privilege escalation events

### Detection Sources

* Windows Security Event Log
* Local Account Management events

Any change without:

* Approved change request
* Incident context

= **Potential compromise**

---

## Summary

* Users = Entry point
* Groups = Privilege amplification
* Local Administrator = Full control

**Attacker mindset:**
Which group gives maximum access with minimal noise?

**Defender mindset:**
Which account or group change indicates an intrusion?
