# Windows Concepts – Security & Internals

This section documents core Windows operating system concepts from a **security and internal architecture perspective**.

The focus is on understanding **how Windows works internally**, how native components are **legitimately used**, how they are **abused by attackers**, and how **security analysts investigate and detect such abuse**.

This content serves as a theoretical foundation for Windows security analysis, threat hunting, and digital forensics.

---

## Covered Topics

### 1. NTFS (New Technology File System)
Covers how Windows stores and manages files, including:
- Master File Table (MFT)
- Disk clusters
- Access Control Lists (DACL & SACL)
- File permissions and auditing
- Security implications of NTFS design

---

### 2. Environment Variables
Explains how environment variables define the runtime environment for processes:
- PATH variable behavior
- Process inheritance and the Process Environment Block (PEB)
- Interaction with executables
- Security risks such as PATH hijacking and privilege escalation

---

### 3. Windows Scheduled Tasks
Documents Task Scheduler internals and security relevance:
- Triggers, actions, security context, and conditions
- Storage locations (XML files and Registry)
- Legitimate administrative use
- Abuse for persistence and privilege escalation
- Relevant Event IDs for detection and investigation

---

### 4. Windows Security Policies
Explains how Windows enforces security controls:
- Local Security Policy vs Group Policy
- Authentication and authorization mechanisms
- Password, lockout, and audit policies
- User rights assignment
- Security-related Event IDs used in monitoring and incident response

---

### 5. Windows Registry
Provides an in-depth explanation of the Windows Registry:
- Registry structure (Hives, Keys, Values)
- Important registry paths
- Startup and persistence mechanisms
- Registry abuse by malware
- Forensic and threat hunting relevance

---

### 6. Volume Shadow Copy Service (VSS)
Explains how VSS works and its importance in security:
- Copy-on-Write mechanism
- Backup and recovery usage
- Abuse for accessing locked system files
- Ransomware behavior
- Digital forensic value

---

### 7. CertUtil
Documents the CertUtil utility from a security perspective:
- Certificate and cryptographic operations
- File hashing, encoding, and decoding
- File download capabilities
- Abuse as a Living Off The Land Binary (LOLBIN)
- Detection strategies and MITRE ATT&CK mapping

---

### 8. Background Intelligent Transfer Service (BITS)
Explains BITS architecture and attacker abuse:
- Job-based transfer model
- Legitimate system usage
- Abuse for stealthy malware delivery
- Event logs and forensic artifacts
- Detection and investigation techniques

---

### 9. Alternate Data Streams (ADS)
Explains NTFS Alternate Data Streams:
- Internal stream structure
- Legitimate use cases
- Malware hiding and defense evasion
- Detection using native Windows tools
- Security risks and forensic relevance

---

## Purpose of This Section

The purpose of this documentation is to:
- Build a deep understanding of Windows internals
- Identify how native Windows features are abused by attackers
- Support security analysis, threat hunting, and incident response
- Act as a technical reference for Windows security investigations

---

## Disclaimer

This content is intended for **educational and defensive security purposes only**, including blue team analysis, digital forensics, and incident response.

