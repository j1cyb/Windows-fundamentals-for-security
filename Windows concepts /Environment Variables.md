### Environment Variables 

#### Definition

Environment Variables are dynamic key-value pairs used by the Windows operating system and applications to define configuration settings and runtime environment information. Each variable consists of a **Name** and a **Value**, providing important data such as system paths, user directories, and configuration locations. Environment variables allow Windows and applications to function correctly without hardcoding fixed values.

**Format example:**

```
NAME = VALUE
```

**Example:**

```
TEMP = C:\Users\Administrator\AppData\Local\Temp
```

---

### How the System Uses Environment Variables

#### 1. Locating Executable Programs

When a user runs a command such as:

```
notepad
```

Windows uses the **PATH** environment variable to search for the executable file. It checks each directory listed in PATH in order until it finds `notepad.exe`, then executes it.

---

#### 2. Defining File and Directory Locations

Environment variables allow Windows and applications to determine the locations of important files and directories.

**Examples:**

* `TEMP` → Specifies the location for temporary files
* `USERPROFILE` → Specifies the user’s home directory

Applications rely on these variables to store data, retrieve resources, and operate consistently across different systems.

---

#### 3. Providing Runtime Configuration to Processes

When Windows creates a new process, it copies system and user environment variables into that process’s memory. These variables define the process runtime environment, such as where to locate executables, where to store temporary data, and how the process should behave.

---

### Relationship Between Environment Variables and Processes

Environment Variables are part of a process’s runtime environment. When a process is created, Windows copies environment variables into the **Process Environment Block (PEB)**.

Key points:

* Each process has its own independent copy of environment variables
* Changes do **not** affect already running processes
* Only newly created processes receive updated variables
* Child processes inherit environment variables from their parent

**Example inheritance chain:**

```
explorer.exe
 ├─ cmd.exe
 └─ powershell.exe
```

Each child process inherits the environment variables from `explorer.exe`.

---

### Common Environment Variable Examples

#### PATH

```
PATH = C:\Windows\System32;C:\Windows;
```

**Function:**
Used by Windows to locate executable files when commands are executed.

---

#### TEMP

```
TEMP = C:\Users\Administrator\AppData\Local\Temp
```

**Function:**
Specifies the directory used to store temporary files such as:

* Cache files
* Installation files
* Intermediate processing data

---

### Potential Security Risks

#### PATH Hijacking

Attackers can modify the PATH variable and place a malicious executable in a directory listed earlier in the PATH order.

**Example:**

```
PATH = C:\Malware;C:\Windows\System32
```

Windows may execute:

```
C:\Malware\notepad.exe
```

instead of the legitimate system binary.

---

#### Privilege Escalation

If a privileged process relies on environment variables such as PATH, attackers may manipulate those variables to execute malicious code with elevated privileges.

---

#### Execution Hijacking

Manipulating environment variables can redirect applications to execute malicious binaries instead of legitimate ones.

---

#### Temporary File Manipulation

If attackers gain access to the directory specified by the TEMP variable, they may read, modify, or replace temporary files, potentially leading to data leakage or code execution.

---

### Conclusion

Environment Variables are essential components of Windows that define the runtime environment for processes. They enable applications and the operating system to locate executables, manage file locations, and configure execution behavior. Since each process maintains its own copy of environment variables, improper configuration or manipulation can introduce serious security risks such as PATH hijacking, execution hijacking, and privilege escalation.
