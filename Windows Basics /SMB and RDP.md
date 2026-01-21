#  SMB and RDP


---

## Overview
This section covers two critical Windows network protocols: **Server Message Block (SMB)** and **Remote Desktop Protocol (RDP)**.  
Both are essential for system administration but are **high‑risk attack surfaces** when misconfigured.  
They are also heavily abused by attackers for **Lateral Movement**.

---

# 1. Server Message Block (SMB)

## 1.1 What is SMB?
**Server Message Block (SMB)** is a Microsoft network protocol that allows shared access to network resources such as:
- Files and folders
- Printers
- Administrative shares (ADMIN$, C$)
- Remote command execution (e.g., PsExec)

SMB allows users and applications to interact with remote systems as if the resources were local.

**Default Ports:**
- TCP 445 (modern)
- TCP 139 (legacy)

---

## 1.2 Legitimate Use Cases
- File sharing in corporate environments
- Domain administration
- Remote system management
- Backup operations
- Software deployment tools (SCCM, PsExec)

---

## 1.3 Connecting to an SMB Share

### Using Windows Explorer (GUI)
```

\<IP>\

```
You will be prompted for valid credentials.  
If permissions are insufficient, access will be denied.

---

### Mapping a Network Drive
Used when different credentials are required.

Steps:
1. Right-click the share
2. Select **Map Network Drive**
3. Tick **Connect using different credentials**

Mapped drives appear under:
```

This PC → Network Locations

````

---

### Using Command Line (net use)
Commonly used in security operations and scripting.

Map a share:
```cmd
net use X: \\<IP>\Share /user:john MyPassword
````

Unmap a drive:

```cmd
net use X: /delete
```

Delete all SMB connections:

```cmd
net use * /delete
```

---

## 1.4 Guest Access (High Risk)

Older versions of SMB (SMBv1) allowed authentication without a password using the **Guest** account.

* SMBv1 is disabled by default on modern Windows systems
* Still commonly found on legacy systems

 **Guest access is a critical security risk**

---

## 1.5 SMB Versions

| Version | Features                                  | Security Impact                            |
| ------- | ----------------------------------------- | ------------------------------------------ |
| SMBv1   | Legacy protocol                           | Extremely insecure (WannaCry, EternalBlue) |
| SMBv2   | Performance improvements, message signing | Moderate security                          |
| SMBv3   | Encryption, RDMA, multi-channel           | Most secure                                |

 Presence of **SMBv1** is a major red flag.

---

## 1.6 Security Risks of SMB

If improperly secured, SMB can lead to:

* Pass-the-Hash attacks
* Credential harvesting
* Lateral Movement
* Ransomware propagation
* Remote command execution

---

## 1.7 SMB in Lateral Movement

Attackers commonly use SMB to:

1. Enumerate network shares
2. Authenticate using stolen credentials
3. Copy malicious tools
4. Execute commands remotely
5. Move laterally across systems

Common attacker tools:

* PsExec
* Impacket
* CrackMapExec
* smbclient

---

## 1.8 PsExec

### What is PsExec?

**PsExec** is a Sysinternals tool that allows administrators to execute processes on remote systems using SMB.

It works by:

1. Copying the executable to the ADMIN$ share
2. Creating a temporary Windows service
3. Executing the command
4. Cleaning up after execution

---

### PsExec Example

```cmd
PsExec64.exe -i -accepteula \\10.102.7.57 -u administrator cmd
```

**Parameters:**

* `-i` → Interactive session
* `-accepteula` → Automatically accepts the license
* `-u` → Username (must have administrative privileges)
* `cmd` → Program to execute

---

## 1.9 PsExec Security Risks

* Enables remote command execution without RDP
* Commonly abused by attackers after credential compromise
* Often used in ransomware campaigns

 PsExec usage on non-admin endpoints is a strong indicator of compromise.

---

## 1.10 Active SMB Sessions

Only one SMB session can exist between two systems using a single set of credentials.

If access is denied unexpectedly:

```cmd
net use * /delete
```

---

# 2. Remote Desktop Protocol (RDP)

## 2.1 What is RDP?

**Remote Desktop Protocol (RDP)** is a Microsoft proprietary protocol that provides full graphical remote access to Windows systems.

**Default Port:** TCP 3389

---

## 2.2 Legitimate Use Cases

* Server administration
* IT support
* Remote work environments

---

## 2.3 Connecting via RDP

1. Start → Remote Desktop Connection
2. Enter the target IP address
3. Provide valid credentials

Certificate warnings may appear if the certificate is not trusted.

 Always verify the legitimacy of the target system.

---

## 2.4 Network Level Authentication (NLA)

### What is NLA?

**Network Level Authentication (NLA)** requires users to authenticate **before** a full RDP session is established.

### Why NLA Matters

* Prevents unauthorized access
* Reduces resource exhaustion
* Protects against RDP zero-day vulnerabilities
* Mitigates brute-force attacks

 RDP without NLA is considered high risk.

---

## 2.5 Security Risks of RDP

If exposed or misconfigured:

* Brute-force attacks
* Credential stuffing
* Initial access for attackers
* Lateral Movement
* Ransomware deployment

---

## 2.6 RDP in Lateral Movement

Attackers use RDP to:

1. Access compromised credentials
2. Move between systems
3. Establish persistence
4. Deploy malware or ransomware

---

# 3. Detection and Monitoring

## 3.1 SMB Monitoring

**Relevant Windows Event IDs:**

* 4624 → Successful logon
* 5140 → Network share accessed
* 7045 → New service installed (PsExec indicator)

**Red Flags:**

* Access to ADMIN$ or C$
* PsExec service creation
* SMBv1 traffic
* Excessive `net use` activity

---

## 3.2 RDP Monitoring

**Relevant Windows Event IDs:**

* 4624 (Logon Type 10) → RDP logon
* 4625 → Failed logon attempts
* 4778 / 4779 → RDP session reconnect/disconnect

**Red Flags:**

* RDP logins outside business hours
* Connections from unknown IPs
* RDP access using privileged accounts

---

# 4. Security Summary

| Protocol | High Risk When              | Mitigation                         |
| -------- | --------------------------- | ---------------------------------- |
| SMB      | SMBv1 enabled, Guest access | Disable SMBv1, enforce signing     |
| PsExec   | Unrestricted usage          | Restrict, monitor service creation |
| RDP      | No NLA, exposed to internet | Enable NLA, MFA, firewall rules    |

---

## Key Takeaway

SMB and RDP are **essential administrative tools**, but they are also among the **most abused protocols in enterprise attacks**.
Proper configuration, monitoring, and credential hygiene are critical to preventing lateral movement and full domain compromise.


