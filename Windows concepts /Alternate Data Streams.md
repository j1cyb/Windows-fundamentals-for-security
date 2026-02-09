#  Alternate Data Streams (ADS)

##  Definition

Alternate Data Streams (ADS) is a feature of the NTFS file system that allows a single file to contain multiple separate data streams.

Normally, a file contains one main stream called the **Primary Data Stream**, which holds the visible content of the file.

However, NTFS allows additional hidden streams called **Alternate Data Streams**, which can store extra data without affecting the main file content or size displayed in File Explorer.

---

## How ADS Works Internally

Every file in NTFS has at least one stream:

```
filename.txt:$DATA
```

This is the primary stream.

NTFS allows additional streams:

```
filename.txt:hidden
filename.txt:secret
filename.txt:payload.exe
```

Structure example:

```
filename.txt
│
├── Primary Stream ($DATA)
│   Visible file content
│
├── Alternate Stream (hidden)
│   Hidden data
│
└── Alternate Stream (payload.exe)
    Hidden executable or data
```

These alternate streams are not visible in File Explorer.

---

##  Why ADS Exists

ADS was originally designed for compatibility and extended functionality.

Main purposes:

### Compatibility

To support file systems like Macintosh HFS, which use multiple forks.

### Metadata Storage

To store additional file information such as:

* Security metadata
* File origin information
* Download zone identifier

Example:

When downloading a file from the internet, Windows creates:

```
file.exe:Zone.Identifier
```

This tells Windows the file came from the Internet.

---

## Key Characteristics of ADS

* Only supported on NTFS file system
* Hidden from normal file browsing
* Can store any type of data
* Does not change visible file size
* Can store executable code
* Can be larger than the main file

---

## Creating Alternate Data Streams

Using Command Prompt:

Create file:

```
echo Hello > file.txt
```

Create ADS:

```
echo SecretData > file.txt:hidden
```

Now file.txt contains hidden data.

---

##  Viewing Alternate Data Streams

List ADS:

```
dir /R
```

Output example:

```
file.txt
file.txt:hidden:$DATA
```

View ADS content:

```
more < file.txt:hidden
```

Or:

```
notepad file.txt:hidden
```

Using PowerShell:

```
Get-Content file.txt -Stream hidden
```

---

## Deleting Alternate Data Streams

Delete entire file:

```
del file.txt
```

This removes all ADS automatically.

Delete specific ADS using PowerShell:

```
Remove-Item file.txt -Stream hidden
```

---

## Legitimate Uses of ADS

ADS has legitimate uses in Windows.

Examples:

### Zone Identifier

Windows stores download origin:

```
file.exe:Zone.Identifier
```

### Metadata Storage

Applications store metadata without modifying file content.

### File tagging

Some applications store tags or extra info.

---

## Security Risks of ADS

ADS is dangerous because it allows attackers to hide data inside files.

Main risks:

* Hidden malware storage
* Hidden scripts
* Hidden tools
* Data hiding
* Evasion of detection

ADS is commonly used in:

* Malware
* Rootkits
* Advanced Persistent Threats (APT)

---

##  Attack Example: Hiding Malware in ADS

Attacker hides executable:

```
malware.exe:hidden
```

Or inside legitimate file:

```
report.txt:payload.exe
```

The file appears harmless:

```
report.txt
```

But contains hidden malware.

---

##  Attack Example: Hiding Commands

Attacker stores commands:

```
echo malicious_command > file.txt:cmd
```

This hides malicious instructions.

---

## Attack Example: Persistence

Attacker stores script in ADS and executes later.

This allows stealth persistence.

---

##  Why ADS Is Dangerous for Security

Because:

* Invisible in File Explorer
* Invisible to normal users
* Often missed by basic antivirus
* Allows stealth data hiding
* Enables defense evasion

ADS is a known technique in MITRE ATT&CK under:

```
Defense Evasion
Hide Artifacts
```

---

##  Detection Techniques (Defense)

Security analysts detect ADS using:

Command Prompt:

```
dir /R
```

PowerShell:

```
Get-Item -Path file.txt -Stream *
```

For scanning entire system:

```
dir /R /S
```

Security tools:

* Sysinternals Streams tool
* PowerShell
* Digital Forensics tools

---

##  Prevention and Defense

Best defensive measures:

### Monitor NTFS streams

Use security monitoring tools.

### Use antivirus with ADS scanning

Modern antivirus detects ADS.

### Use PowerShell detection

```
Get-ChildItem -Recurse | Get-Item -Stream *
```

### Monitor suspicious behavior

Watch for:

* Hidden executable streams
* Suspicious scripts in ADS

---

## Summary

Alternate Data Streams is a powerful NTFS feature that allows files to contain hidden data streams.

While designed for legitimate purposes, ADS is frequently abused by attackers to hide malware and evade detection.

Understanding ADS is critical for:

* Cybersecurity analysts
* Threat hunters
* Digital forensics investigators
* Incident responders

ADS represents both a legitimate file system feature and a potential security risk.
