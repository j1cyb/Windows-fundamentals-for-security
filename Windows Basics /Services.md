# Windows Services 

## Introduction

Windows Services are executable applications designed to run in their own session and perform long-running tasks in the background. Unlike normal applications, services do not provide a graphical user interface (GUI) and are typically invisible to end users.

Services can be configured to:
- Start automatically when the system boots
- Be started manually by a user or administrator
- Be paused, stopped, or restarted as needed

They are commonly used on servers and critical systems where their execution must not interfere with users performing their daily tasks.

From a cybersecurity perspective, services are extremely important because they often run with high privileges and persist across system reboots.

---

## Accessing and Managing Services (GUI)

### Opening the Services Console

You can access the Services management console in two ways:

1. Search for **Services** in the Start Menu and press Enter.
2. Press `Win + R`, type:
```

services.msc

```
then press Enter.

---

### Interacting with a Service

- Right-click a service to:
- Start
- Stop
- Pause
- Restart

- Double-click a service or select **Properties** to modify its configuration.

---

## Service Properties Tabs (Detailed)

### 1. General Tab

The General tab contains core information about the service:

- **Service Name**
- Used internally by Windows to identify the service.
- **Display Name**
- A user-friendly name shown in the Services console.
- **Description**
- A short explanation of what the service does.
- **Path to Executable**
- The exact file that is launched when the service starts.
- **Startup Type**
- Automatic
- Automatic (Delayed Start)
- Manual
- Disabled
- **Service Status**
- Running, Stopped, or Paused
- **Start Parameters**
- Optional arguments passed to the executable.

---

### 2. Log On Tab

Defines which account the service runs under.

Common service accounts include:
- `SYSTEM`
- `Local Service`
- `Network Service`
- `DefaultAppPool`

Custom user accounts can also be used, but they must have the appropriate permissions.

---

### 3. Recovery Tab

Controls what happens when the service fails:
- Restart the service
- Run a program
- Take no action

This is useful for maintaining availability but can also be abused by attackers.

---

### 4. Dependencies Tab

Shows:
- Services that the current service depends on
- Services that depend on the current service

Understanding dependencies is critical when troubleshooting failures or analyzing malicious behavior.

---

## Limitations of the GUI

While the GUI provides useful controls, some service parameters cannot be modified after installation. For advanced configuration and analysis, command-line tools are required.

---

## Managing Services with sc.exe

`sc.exe` is a powerful command-line tool for creating, configuring, querying, and deleting Windows services.

### Display Help
```

sc /?

```

---

### Query Service Status
```

sc query <service_name>

```

---

### Query Detailed Configuration
```

sc qc <service_name>

```

This returns:
- Binary path
- Startup type
- Dependencies
- Service account

---

### Modify Service Configuration
```

sc config <service_name> /?

```

---

### Change the Executable Path
```

sc config <service_name> binpath= C:\Windows\MyProgram.exe

```

---

### Configure Service Dependencies
```

sc config <service_name> depend= ServiceA/ServiceB

```

Dependencies must be separated by a forward slash (`/`).

---

## Service Startup Types

Services can start in several ways:

- **Manual**
  - Requires a user or application to start it.
- **Automatic**
  - Starts during system boot.
- **Automatic (Delayed Start)**
  - Starts shortly after boot to reduce system load or wait for dependencies.
- **Disabled**
  - Cannot be started unless re-enabled.

---

## Service Accounts and Privileges

Most services run under high-privilege accounts such as `SYSTEM`.

This means:
- Full control over the system
- Access to files, registry, memory, and network resources

Improperly secured services represent a serious security risk.

---

## Why Attackers Target Services

Services are highly attractive attack targets because:

1. They run silently in the background.
2. They provide persistence across reboots.
3. They often run with SYSTEM-level privileges.
4. They appear legitimate and trusted.
5. They can be remotely controlled.

Attackers frequently use services to maintain long-term access to compromised systems.

---

## Examples of Malicious Services

### Example 1: Fake System Service

Service Name:
```

WindowsUpdateHelper

```

Executable Path:
```

C:\Users\Public\svchost.exe

```

This is malicious because the legitimate `svchost.exe` should only run from:
```

C:\Windows\System32\svchost.exe

```

---

### Example 2: Backdoor Service

- Service launches a command-and-control (C2) agent.
- Automatically starts on boot.
- Communicates with an external server.

---

### Example 3: Service Hijacking

- Attacker modifies an existing service.
- Changes the `binPath` to point to a malicious executable.
- Service appears legitimate but executes attacker-controlled code.

---

## How to Review Services (Security Methodology)

### Step 1: List All Services
- Focus on:
  - Running services
  - Automatic startup services

---

### Step 2: Review Service Names
Check for:
- Misspellings
- Generic names
- Names mimicking legitimate Windows services

---

### Step 3: Inspect Executable Paths (Critical)

Red flags:
- Executables located in:
  - `C:\Users\`
  - `AppData`
  - `Temp`
- Unknown or suspicious filenames

---

### Step 4: Check Service Accounts
Red flags:
- Unnecessary SYSTEM privileges
- Custom user accounts without justification

---

### Step 5: Analyze Dependencies
- Unusual or unnecessary dependencies
- Circular or illogical service relationships

---

### Step 6: Use sc.exe for Verification
```

sc query <service_name>
sc qc <service_name>

```

---

## Indicators of Compromise (IOCs)

### Common Red Flags

- Services running from non-system directories
- Automatic startup services with unknown purpose
- Services without descriptions
- Services that frequently crash and restart
- Services executing:
  - `cmd.exe`
  - `powershell.exe`
- Unsigned service binaries

---

## Defensive Strategies (Blue Team)

- Maintain a baseline of approved services
- Monitor for:
  - New service creation
  - Changes to `binPath`
- Enable logging:
  - Windows Event ID 7045 (Service Installation)
- Use tools like:
  - Sysmon
  - Sigcheck
- Regularly audit service configurations

---

## Security Summary

> Any service represents a potential attack vector.  
> Any service running as SYSTEM represents high risk.  
> Any unknown service must be investigated immediately.

Windows services are powerful, necessary, and dangerous if misconfigured or abused.
