# Managing Processes 

##  What is a Process?
A **process** is an instance of a program that is currently running on a system.  
In Windows, every running application or system component operates as a process.

Key points:
- Each process has its own **Process ID (PID)**.
- Each process consumes system resources such as **CPU, memory (RAM), disk, and network**.
- Some applications run as a **single process**, while others (e.g., web browsers) spawn **multiple child processes**.

Types of processes:
- **System Processes**: Essential for Windows to function (e.g., `csrss.exe`, `wininit.exe`).
- **User Processes**: Applications launched by users (e.g., `notepad.exe`, `edge.exe`).
- **Background Processes**: Applications or services running without user interaction.

---

##  Process vs Service

| Feature | Process | Service |
|------|--------|--------|
| Definition | A running program | A background program managed by Windows |
| Startup | Manual or automatic | Usually automatic at boot |
| User Interaction | Often has a GUI | Usually no GUI |
| Account | User or system account | SYSTEM / LocalService / NetworkService |
| Security Risk | Malware can hide as a process | Malware often persists as a service |

**Security note:**  
Attackers often disguise malicious programs as legitimate services to maintain persistence.

---

##  Why Process Monitoring is Important for Security
Monitoring processes helps with:
- **Early malware detection**
- **Identifying unauthorized programs**
- **Detecting privilege escalation attempts**
- **Investigating suspicious system behavior**

### Examples of Suspicious Processes
- `svchost.exe` running from a non-standard directory
- Unknown processes consuming high CPU or RAM
- System processes running under a normal user account
- Processes with unusual command-line arguments

---

##  Process Monitoring Tools

### Task Manager (GUI Tool)

#### Opening Task Manager
- `Ctrl + Shift + Esc`
- `Ctrl + Alt + Delete → Task Manager`
- Right-click **Start Menu → Task Manager**

---

#### Basic View
Shows currently running applications.

Actions:
- **End Task** – Force close an application
- **Switch to** – Bring app to foreground
- **Open file location** – Inspect executable path
- **Search online** – Check legitimacy
- **Properties** – Review file details

---

#### Detailed View (More Details)

**Processes Tab**
- Applications
- Background processes
- Windows processes

Key features:
- CPU, Memory, Disk, Network usage
- Heat-map visualization
- Sorting by resource usage
- Expanding parent/child processes

**Performance Tab**
- Real-time monitoring of CPU, RAM, Disk, Network
- Graphs for the last 60 seconds
- Details about disks, network adapters, and IP addresses

**Users Tab**
- Displays logged-in users
- Shows resources and processes per user

**Details Tab**
- Advanced process info: Name, PID, User, Architecture, CPU & Memory usage
- Options:
  - **End Process Tree**
  - **Set Priority**
  - **Set Affinity**
  - **Analyze Wait Chain**
  - **Go to Service(s)**

**Services Tab**
- Lists all system services (Running/Stopped)
- Displays associated PID and service group
- Actions: Start / Stop / Restart (use with caution)

---

###  Command Prompt

#### tasklist
Displays running processes:

```cmd
tasklist
````

Output includes:

* Image Name
* PID
* Session Name
* Memory Usage

---

#### tasklist Filters (/FI)

Filter processes by specific criteria:

```cmd
tasklist /FI "STATUS eq running"
tasklist /FI "PID eq 5240"
tasklist /FI "USERNAME eq Taylor"
tasklist /FI "SERVICES eq LSM"
```

* `/FI` allows filtering by:

  * `STATUS` → running, not responding
  * `PID` → specific process ID
  * `USERNAME` → specific user
  * `SERVICES` → processes hosting specific services

---

#### Verbose Output (/V)

```cmd
tasklist /V
```

Provides additional details:

* CPU Time
* Window Title
* Process Status
* Useful for detecting suspicious or hidden activity

---

#### Services per Process (/SVC)

```cmd
tasklist /SVC
```

Maps processes to their hosted services.
Filter by service:

```cmd
tasklist /FI "SERVICES eq LSM"
```
### taskkill – Terminating Processes

#### Common Flags

| Flag   | Description                                            |
| ------ | ------------------------------------------------------ |
| `/PID` | Kill a process by Process ID                           |
| `/IM`  | Kill a process by Image Name                           |
| `/F`   | Forcefully terminate the process                       |
| `/T`   | Kill the process and all its child processes           |
| `/FI`  | Filter processes for termination (similar to tasklist) |

#### Examples

```cmd
taskkill /PID 1234 /F
taskkill /IM notepad.exe /F
taskkill /PID 3572 /T /F
taskkill /FI "USERNAME eq Taylor" /F
```

**Notes:**

* `/T` kills parent + child processes.
* `/F` forces termination but should be used cautiously.
* `/IM` is useful when the PID is unknown or the process has multiple instances.

---

## Security Best Practices

* Verify the **file path** of system processes.
* Investigate unknown or suspicious processes immediately.
* Monitor **startup services** for persistence mechanisms.
* Combine **Task Manager GUI** and **Command-Line tools** for full visibility.
* Avoid terminating critical system processes unless certain.
* Look for anomalies:

  * High CPU/RAM usage by unknown processes
  * Processes running under unusual accounts
  * Unexpected services hosting multiple processes

---

##  Summary

Understanding and monitoring Windows processes is a **core defensive security skill**.

Key takeaways:

* Processes = running programs; Services = background tasks managed by Windows.
* Monitoring helps detect malware, unauthorized activity, and performance issues.
* Task Manager provides GUI monitoring; `tasklist` and `taskkill` are essential CLI tools.
* Filtering, verbose output, and process-to-service mapping improve analysis efficiency.
* Careful observation and verification distinguish legitimate from suspicious activity.
