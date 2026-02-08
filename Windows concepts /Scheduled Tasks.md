#  Scheduled Tasks 

## Definition

**Task Scheduler** is a built-in Windows component that allows the operating system, administrators, and applications to automatically execute programs, scripts, or commands at predefined times or in response to specific events.

It acts as an **automation engine** that eliminates the need for manual execution by triggering tasks based on configured conditions.

Task Scheduler is implemented as a Windows service called:

```
Schedule
```

Without this service running, scheduled tasks cannot execute.

---

# How Task Scheduler Works (Internal Workflow)

Task Scheduler operates using a structured model consisting of multiple components that define when and how a task executes.

## Core Components

Every Scheduled Task contains the following elements:

### 1. Trigger

A Trigger defines **when the task will execute**.

Examples:

* At system startup
* At user logon
* At a specific time (daily, weekly, etc.)
* When a specific event occurs

Example:

```
Trigger: At system startup
```

---

### 2. Action

An Action defines **what will be executed**.

Examples:

* Run a program
* Execute a PowerShell script
* Execute a command

Example:

```
Action: powershell.exe -File C:\Scripts\backup.ps1
```

---

### 3. Security Context

Defines **which user account runs the task and with what privileges**.

Examples:

```
Run as: SYSTEM
Run as: Administrator
Run as: Standard User
```

This is critical because tasks running as SYSTEM have full control over the system.

---

### 4. Conditions

Optional requirements that must be met before execution.

Examples:

* Only run if computer is idle
* Only run if connected to power
* Only run if network is available

---

### 5. Settings

Control how the task behaves.

Examples:

* Retry if failed
* Stop after a certain duration
* Allow manual execution

---

# Where Scheduled Tasks Are Stored

Scheduled Tasks are stored in two main locations:

## File System Location

```
C:\Windows\System32\Tasks\
```

Tasks are stored as XML files containing configuration details.

---

## Registry Location

```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\
```

Contains task metadata and internal references.

---

# Legitimate Uses of Task Scheduler (Legitimate Usage)

Task Scheduler is widely used for legitimate administrative and system automation purposes.

## Examples:

### 1. System Maintenance

* Disk cleanup
* Temporary file removal
* System optimization

---

### 2. Windows Updates

Windows automatically schedules update checks and installation.

---

### 3. Antivirus Scans

Microsoft Defender uses scheduled tasks to perform periodic scans.

Example:

```
Daily antivirus scan at 02:00 AM
```

---

### 4. Backup Automation

Organizations use scheduled tasks to automate backups.

Example:

```
Run backup script every night
```

---

### 5. Administrative Automation

System administrators use tasks to:

* Run scripts
* Monitor systems
* Collect logs

---

# Malicious Uses of Task Scheduler (Abuse and Attacks)

Attackers frequently abuse Task Scheduler because it is a trusted Windows component.

---

## 1. Persistence

Persistence ensures malware continues running after reboot.

Example attack:

```
Trigger: At system startup
Action: C:\Users\Public\malware.exe
Run as: SYSTEM
```

Result:

Malware runs automatically every time the system starts.

---

## 2. Privilege Escalation

Attackers may configure tasks to run with SYSTEM privileges.

Result:

Full control over the system.

---

## 3. Stealth Execution

Tasks run in the background without user interaction.

This helps attackers remain undetected.

---

## 4. Defense Evasion

Since Task Scheduler is a legitimate Windows feature, malicious tasks may appear normal.

---

# Using Task Scheduler via GUI

The graphical interface allows users to create, modify, and monitor tasks.

## Opening Task Scheduler

Press:

```
Win + R
```

Type:

```
taskschd.msc
```

Press Enter.

---

## Viewing Tasks

Navigate:

```
Task Scheduler Library
```

You will see:

* Task Name
* Status
* Triggers
* Last Run Time
* Author

---

## Viewing Task Details

Click on a task to view:

* Triggers tab
* Actions tab
* Conditions tab
* Settings tab
* Security options

---

# Using Task Scheduler via Command Line

Windows provides the schtasks command.

---

## List all scheduled tasks

```
schtasks /query /fo LIST /v
```

This displays:

* Task name
* User
* Run level
* Last run time
* Action

---

## Create a scheduled task

Example:

```
schtasks /create /sc onlogon /tn TestTask /tr notepad.exe
```

---

## Delete a task

```
schtasks /delete /tn TestTask
```

---

## Run a task manually

```
schtasks /run /tn TestTask
```

---

# Using PowerShell

PowerShell provides advanced control.

## List tasks

```
Get-ScheduledTask
```

---

## View task details

```
Get-ScheduledTaskInfo -TaskName TestTask
```

---

# How to Read and Analyze Scheduled Tasks (Security Perspective)

Security analysts should examine:

## Important Fields

### 1. Task Name

Look for suspicious names:

Example:

```
WindowsUpdateSecurity
SystemMaintenanceService
```

Attackers use legitimate-looking names.

---

### 2. Action Path

Check executable location.

Suspicious example:

```
C:\Users\Public\malware.exe
```

Legitimate example:

```
C:\Windows\System32\svchost.exe
```

---

### 3. User Context

If running as SYSTEM, it has maximum privileges.

---

### 4. Trigger Type

Suspicious triggers:

* At startup
* At logon

---

# Important Event IDs (Task Scheduler Logs)

Location:

```
Event Viewer
→ Applications and Services Logs
→ Microsoft
→ Windows
→ TaskScheduler
→ Operational
```

---

## Event ID 106

Task registered

Meaning:

A new scheduled task was created.

Security relevance:

May indicate persistence.

---

## Event ID 140

Task updated

Meaning:

Task configuration was modified.

---

## Event ID 141

Task deleted

Meaning:

Task removed from system.

---

## Event ID 200

Task executed

Meaning:

Task was started.

---

## Event ID 201

Task completed

Meaning:

Task finished execution.

---

# Security Importance Summary

Task Scheduler is critical for both system functionality and cybersecurity analysis.

Legitimate uses include:

* System maintenance
* Updates
* Backup
* Automation

Malicious uses include:

* Persistence
* Privilege escalation
* Malware execution
* Stealth operations

Security analysts must monitor scheduled tasks because they are commonly abused by attackers to maintain access and execute malicious code.

