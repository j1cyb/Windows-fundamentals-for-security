#  Scheduled Tasks 

## Overview

**Scheduled Tasks** are a built‑in Windows feature that allows programs or scripts to run automatically at a specific time or when a particular event occurs.
They are heavily used for **automation and system administration**, but they are also commonly abused by attackers to achieve **persistence**.

From a cybersecurity perspective, Scheduled Tasks are a **high‑value persistence mechanism**.

---

## Why Scheduled Tasks Are Important

### Legitimate Uses

* System maintenance
* Software updates
* Backups
* Log collection
* Administrative automation

### Security Impact

* Tasks can run **without user interaction**
* Tasks can execute as **SYSTEM or Administrator**
* Tasks survive **reboots**
* Tasks can be hidden inside normal‑looking system names

---

## Core Components of a Scheduled Task

### Trigger

Defines **when** the task runs.
Examples:

* At a specific time
* On system startup
* On user logon
* Repeating intervals

### Action

Defines **what** the task does.
Examples:

* Run a program
* Execute a script
* Launch PowerShell or CMD

### Conditions

Define **environment requirements**.
Examples:

* Only when idle
* Only on AC power
* Only when connected to a network

### Settings

Control **task behavior**.
Examples:

* Restart if failed
* Stop after X time
* Delete task if not used

---

## Creating Tasks Using Task Scheduler (GUI)

1. Open **Task Scheduler**
2. Go to **Task Scheduler Library**
3. Select **Create Basic Task**
4. Set:

   * Name and description
   * Trigger
   * Action
   * Conditions and settings
5. Finish and review task properties

---

## Using `schtasks` (Command Line)

`schtasks` allows creating, modifying, querying, and deleting scheduled tasks from the CLI.

### Basic Syntax

```cmd
schtasks /create /tn "Task Name" /tr "Action" /sc "Schedule" /ru "User"
```

---

## Important `schtasks` Flags (Detailed)

### `/tn` – Task Name

Specifies the name of the task.

```cmd
/tn "Windows Update Checker"
```

**Attacker abuse:**

* Uses system‑like names to blend in
  (e.g. `WindowsTelemetry`, `AdobeUpdater`)

**Defender note:**

* Unknown system‑looking names are suspicious

---

### `/tr` – Task Run (Action)

Specifies what command or program is executed.

```cmd
/tr "powershell.exe -enc <Base64>"
```

**Attacker abuse:**

* PowerShell, CMD, rundll32, mshta
* Encoded or obfuscated commands

**Red flags:**

* `powershell.exe -enc`
* Scripts in `AppData`, `Temp`, `Downloads`

---

### `/sc` – Schedule (Trigger)

Defines when the task runs.

| Value   | Description       | Security Risk |
| ------- | ----------------- | ------------- |
| DAILY   | Runs daily        | Medium        |
| HOURLY  | Runs every hour   | High          |
| MINUTE  | Runs every minute | Very High     |
| ONLOGON | On user login     | High          |
| ONSTART | On system startup | Critical      |

```cmd
/sc ONSTART
```

---

### `/ru` – Run As User

Specifies the account used to run the task.

```cmd
/ru SYSTEM
```

**Attacker abuse:**

* Runs malware as SYSTEM

**Defender note:**

* SYSTEM tasks running scripts are high risk

---

### `/rp` – Run Password

Provides the user password.

```cmd
/rp Password123
```

**Security concern:**

* Credential exposure
* Rare but dangerous

---

### `/ri` – Repeat Interval

Repeats task execution at defined intervals.

```cmd
/ri 5
```

**Attacker use:**

* Beaconing
* Repeated C2 connections

---

### `/du` – Duration

Specifies how long repetition lasts.

```cmd
/du 9999:59
```

Used to keep malicious tasks running indefinitely.

---

### `/f` – Force

Forces task creation without confirmation.

```cmd
/f
```

Often used to overwrite existing tasks silently.

---

### `/query` – List Tasks

Lists scheduled tasks.

```cmd
schtasks /query /v /fo LIST
```

**Defensive importance:**

* Primary enumeration command
* Shows actions, users, triggers

---

### `/delete` – Delete Task

Removes a scheduled task.

```cmd
schtasks /delete /tn "Task Name"
```

**Attacker use:**

* Cleanup after execution

---

## Example: Malicious Persistence Task (For Analysis Only)

```cmd
schtasks /create /tn "WindowsTelemetry" ^
/tr "powershell.exe -enc <payload>" ^
/sc ONSTART ^
/ru SYSTEM ^
/f
```

### What This Achieves

* Persistence across reboots
* SYSTEM‑level execution
* No user interaction

---

## Monitoring & Detection

### Event Viewer

Path:

```text
Microsoft-Windows-TaskScheduler/Operational
```

Important Event IDs:

* **106** – Task registered
* **140** – Task updated
* **200** – Task started

---

### Sysinternals Autoruns

* Displays all scheduled tasks
* Reveals hidden and suspicious entries

---

### PowerShell Monitoring

```powershell
Get-ScheduledTask | Get-ScheduledTaskInfo
```

Useful for:

* Enumerating tasks
* Identifying abnormal triggers and actions

---

## Defender Checklist

* Review tasks running as SYSTEM
* Inspect PowerShell or script‑based actions
* Watch for ONSTART / ONLOGON triggers
* Validate file paths and signatures
* Investigate unknown task names

---

## Summary

* Scheduled Tasks are a powerful automation feature
* They are a **common persistence technique**
* `schtasks` flags define execution, timing, and privilege
* Proper monitoring and review are critical for defense
