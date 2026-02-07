# Windows Security Policies 

## Overview

Windows Security Policies are a set of configuration rules that define how the Windows operating system enforces security. These policies control authentication, authorization, auditing, and system behavior to protect against unauthorized access, privilege abuse, and cyber attacks.

They are enforced internally by the **Local Security Authority Subsystem Service (LSASS)** and configured using tools such as **Local Security Policy** and **Group Policy**.

---

# Local Security Policy

## Definition

Local Security Policy is a set of security settings applied to a single Windows computer. It defines how the local system handles:

* Password requirements
* User authentication
* User privileges
* Security auditing
* Access control

It is primarily used on standalone systems or computers not joined to a domain.

You can open it using:

```
secpol.msc
```

---

## How It Works Internally

When a user attempts to log in:

1. The login request is sent to LSASS.
2. LSASS checks the Local Security Policy database.
3. It verifies:

   * Password requirements
   * Account lockout status
   * User privileges
4. Access is granted or denied based on policy.

---

## Storage Location

Policies are stored locally in:

```
C:\Windows\System32\secedit.sdb
```

and enforced by the Windows Security Subsystem.

---

## Example

If configured:

```
Minimum password length = 12
```

Windows will reject any password shorter than 12 characters.

---

# Group Policy

## Definition

Group Policy is a centralized security management system used in domain environments. It allows administrators to configure and enforce security policies across multiple computers from a central server called the Domain Controller.

---

## How It Works

1. Administrator configures policies on the Domain Controller.
2. Policies are stored in Active Directory and SYSVOL.
3. Client computers retrieve policies automatically.
4. Policies are applied and enforced locally.

---

## Example

Administrator configures:

```
Account lockout threshold = 5 attempts
```

All domain computers enforce this rule automatically.

---

## Key Difference Between Local Security Policy and Group Policy

| Local Security Policy      | Group Policy                    |
| -------------------------- | ------------------------------- |
| Applies to one computer    | Applies to multiple computers   |
| Stored locally             | Stored in Active Directory      |
| Used in standalone systems | Used in enterprise environments |
| Managed locally            | Managed centrally               |

---

# Important Security Settings

## 1. Password Policy

Location:

```
Account Policies → Password Policy
```

Important settings:

### Minimum password length

Defines minimum characters required.

Security impact:
Prevents weak passwords.

---

### Password complexity requirement

Requires:

* Uppercase letters
* Lowercase letters
* Numbers
* Special characters

Security impact:
Prevents password guessing attacks.

---

### Maximum password age

Defines how long passwords remain valid.

Security impact:
Reduces risk of long-term credential compromise.

---

## 2. Account Lockout Policy

Location:

```
Account Policies → Account Lockout Policy
```

Important setting:

### Account lockout threshold

Example:

```
5 failed attempts
```

Security impact:
Prevents brute-force attacks.

---

## 3. Audit Policy

Location:

```
Local Policies → Audit Policy
```

Example:

```
Audit logon events
```

Security impact:
Allows monitoring and detection of suspicious activity.

---

## 4. User Rights Assignment

Location:

```
Local Policies → User Rights Assignment
```

Controls:

* Who can log in
* Who can shut down the system
* Who can debug processes

Security impact:
Prevents privilege escalation.

---

# Important Event IDs and Their Meaning

Windows records security activity in Event Viewer.

Location:

```
Event Viewer → Windows Logs → Security
```

---

## Event ID 4624 – Successful Logon

Meaning:
User successfully logged in.

Security impact:
Normal event, but suspicious if unexpected account or time.

---

## Event ID 4625 – Failed Logon

Meaning:
Login attempt failed.

Security impact:
Multiple failures may indicate brute-force attack.

---

## Event ID 4672 – Special Privileges Assigned

Meaning:
High-level privileges assigned to a logon session.

Security impact:
Normal for SYSTEM, suspicious for regular users.

---

## Event ID 4688 – Process Created

Meaning:
A new process was started.

Security impact:
Used to detect malware execution.

---

## Event ID 4720 – User Account Created

Meaning:
New user account created.

Security impact:
Potential attacker persistence.

---

## Event ID 1102 – Audit Log Cleared

Meaning:
Security logs were deleted.

Security impact:
Highly suspicious. Attackers clear logs to hide activity.

---

# Security Impact of Windows Security Policies

Proper configuration protects against:

---

## Brute Force Attacks

Account lockout policy prevents repeated login attempts.

---

## Password Attacks

Password complexity prevents weak passwords.

---

## Privilege Escalation

User rights assignment limits privilege abuse.

---

## Malware Execution

Audit policies help detect malicious processes.

---

## Unauthorized Access

Authentication policies ensure only authorized users can log in.

---

# Real-World Example

Without security policies:

* Attacker can attempt unlimited passwords
* Gain access to system
* Escalate privileges

With proper policies:

* Account gets locked
* Event logs record attack
* Security team detects intrusion

---

# Conclusion

Local Security Policy and Group Policy are fundamental components of Windows security.

They control:

* Authentication
* Authorization
* Auditing
* System behavior

Proper configuration significantly improves system security and helps detect and prevent cyber attacks.

---
