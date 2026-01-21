# Registry 

---

## What is the Windows Registry

The Windows Registry is a collection of databases that store configuration settings for the operating system, users, applications, and hardware.

It contains values stored inside keys, which exist within registry hives.

Registry values hold instructions that Windows and applications rely on to function.  
Registry keys are similar to folders: they contain values and can also contain subkeys.

The registry follows a hierarchical structure:

```

Hive\Key\Subkey\Subkey

```

### Example
```

HKEY_LOCAL_MACHINE\SOFTWARE\Adobe

```

In this path:
- **HKEY_LOCAL_MACHINE** is the Hive
- **SOFTWARE** is a Subkey
- **Adobe** is a Subkey under SOFTWARE

---

## Registry Editor (GUI)

### Viewing the Registry

The registry can be accessed using the Registry Editor (`regedit`), which is built into Windows.

The Registry Editor is **not the registry itself**, but a graphical interface used to interact with it.

### Ways to Open
- Type `regedit` in the Start menu
- Press `Win + R`, type `regedit`, and press Enter

Once opened, the main registry hives are displayed.

---

## Main Registry Hives

### HKEY_USERS (HKU)
Contains all currently loaded user profiles on the system.

---

### HKEY_CURRENT_USER (HKCU)
Contains configuration settings for the currently logged-in user.  
It is a subkey of **HKU**.

---

### HKEY_LOCAL_MACHINE (HKLM)
Stores system-wide configuration settings that apply to all users.

---

### HKEY_CLASSES_ROOT (HKCR)
Controls file associations and determines which applications open specific file types.  
It is a subkey of **HKLM\Software**.

---

### HKEY_CURRENT_CONFIG
Contains hardware profile information used during system startup.

---

## Registry Value Types

Common registry value types include:

### REG_BINARY
Raw binary data displayed in hexadecimal format.

### REG_DWORD
32-bit numeric value, commonly used by services and drivers.

### REG_QWORD
64-bit numeric value.

### REG_SZ
Fixed-length text string.

### REG_EXPAND_SZ
Expandable string that contains variables resolved at runtime.

### REG_MULTI_SZ
Multiple strings stored within a single value.

### REG_NONE
Data with no defined type.

### REG_RESOURCE_*
Used for hardware resource configuration and device drivers.

---

## Modifying the Registry (GUI)

### Modifying a Value
- Double-click the value  
- Or right-click → **Modify**

Behavior depends on value type:
- **REG_DWORD** accepts numeric data only
- **REG_SZ** accepts text data only

---

### Creating Keys and Values
- Right-click inside a key
- Select **New → Key** or **New → Value**

Every new key contains a default `(Default)` value.

---

### Key Context Menu Options
Right-clicking a key allows you to:
- Rename
- Delete
- Export
- Modify Permissions

---

## Registry via Command Prompt (REG)

General syntax:
```

REG [Operation] [Parameters]

```

---

## REG QUERY – Querying the Registry

### Query a Key
```

REG QUERY HKLM\Software\Microsoft\Windows\CurrentVersion\WSMAN

```

---

### Query All Subkeys Recursively
```

REG QUERY HKLM\SOFTWARE\Microsoft /s

```

---

### Query a Specific Value
```

REG QUERY HKLM...\WSMAN /v ServiceStackVersion

```

---

### Query the Default Value
```

REG QUERY HKLM...\CurrentVersion /ve

```

---

## Searching the Registry

### Search Using /f
```

REG QUERY HKLM\SOFTWARE\Microsoft /s /f token

```

Search options:
- `/k` → Search key names only
- `/d` → Search data only
- `/c` → Case-sensitive search
- `/e` → Exact match only
- `/t REG_SZ` → Filter by value type

---

## REG ADD – Adding Keys and Values

### Add a Key
```

REG ADD HKLM\SOFTWARE\Microsoft\ActiveSync\NewKey2

```

---

### Add a Value
```

REG ADD HKLM...\NewKey2 /v Data /t REG_SZ /d example

```

---

### Force Overwrite Without Confirmation
```

/f

```

---

## REG DELETE – Deleting Keys and Values

### Delete a Key (Including Subkeys)
```

REG DELETE HKLM...\NewKey2

```

---

### Delete a Value
```

REG DELETE HKLM...\NewKey2 /v Data

```

---

### Delete Without Confirmation
```

/f

```

---

## REG COPY – Copying Registry Data

### Copy Keys and Values
```

REG COPY Key1 Key2 /s /f

```

---

## REG SAVE / LOAD / UNLOAD

### Save a Hive or Key
```

REG SAVE HKLM\SOFTWARE backup.hiv /y

```

---

### Load a Saved Hive
```

REG LOAD HKLM\TempHive backup.hiv

```

---

### Unload a Loaded Hive
```

REG UNLOAD HKLM\TempHive

```

---

## REG RESTORE

### Restore a Previously Saved Hive
```

REG RESTORE HKLM\SOFTWARE backup.hiv

```

---

## REG COMPARE

### Compare Registry Keys
```

REG COMPARE Key1 Key2 /v ValueName /s

```

Comparison options:
- `/oa` → Show all results
- `/od` → Show differences only
- `/os` → Show matches only
- `/on` → No output

---

## REG EXPORT / IMPORT

### Export a Key
```

REG EXPORT HKLM...\Key file.reg /y

```

---

### Import a Registry File
```

REG IMPORT file.reg

```

---

## Attacker Perspective

Attackers abuse the registry for:
- Persistence
- Disabling security controls
- Modifying system behavior
- Executing payloads at startup

Common abuse locations include:
- Run / RunOnce keys
- Services
- Policies
- Windows Defender configuration keys

---

## Defender / SOC Perspective

Defenders monitor:
- `REG ADD` and `REG DELETE` activity
- Startup and persistence-related keys
- Changes in **HKLM** and **HKCU**
- Unexpected executions of `reg.exe`

Analysis focuses on:
- Process creation context
- Parent-child process relationships
- Before-and-after registry state comparisons

---

## Security Summary

The Windows Registry is the core of the operating system.

A single malicious or incorrect registry change can fully compromise a system.

Understanding the registry is essential for both attackers and defenders.

