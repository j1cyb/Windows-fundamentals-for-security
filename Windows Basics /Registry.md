 Registry


--------------------------------------------------
What is the Windows Registry
--------------------------------------------------

The Windows Registry is a collection of databases that store configuration settings for the operating system, users, applications, and hardware.
It contains values stored inside keys, which exist within registry hives.

Registry values hold instructions that Windows and applications rely on to function.
Registry keys are similar to folders: they contain values and can also contain subkeys.

The registry follows a hierarchical structure like this:

Hive\Key\Subkey\Subkey

Example:
HKEY_LOCAL_MACHINE\SOFTWARE\Adobe

In this path:
- HKEY_LOCAL_MACHINE is the Hive
- SOFTWARE is a Subkey
- Adobe is a Subkey under SOFTWARE

--------------------------------------------------
Registry Editor (GUI)
--------------------------------------------------

Viewing the Registry
The registry can be accessed using the Registry Editor (regedit), which is built into Windows.
The editor is not the registry itself, but a graphical interface to interact with it.

Ways to open:
- Type regedit in the Start menu
- Or press Win + R and type regedit

Once opened, you will see the main registry hives.

--------------------------------------------------
Main Registry Hives
--------------------------------------------------

HKEY_USERS (HKU)
Contains all currently loaded user profiles on the system.

HKEY_CURRENT_USER (HKCU)
Contains configuration for the currently logged-in user.
It is a subkey of HKU.

HKEY_LOCAL_MACHINE (HKLM)
Stores system-wide configuration that applies to all users.

HKEY_CLASSES_ROOT (HKCR)
Controls file associations and which applications open specific file types.
It is a subkey of HKLM\Software.

HKEY_CURRENT_CONFIG
Contains hardware profile information used during system startup.

--------------------------------------------------
Registry Value Types
--------------------------------------------------

Common registry value types include:

REG_BINARY
Raw binary data displayed in hexadecimal.

REG_DWORD
32-bit numeric value, commonly used by services and drivers.

REG_QWORD
64-bit numeric value.

REG_SZ
Fixed-length text string.

REG_EXPAND_SZ
Expandable string containing variables resolved at runtime.

REG_MULTI_SZ
Multiple strings stored in a single value.

REG_NONE
Data with no defined type.

REG_RESOURCE_*
Used for hardware resource configuration and device drivers.

--------------------------------------------------
Modifying the Registry (GUI)
--------------------------------------------------

Modifying a Value
- Double-click the value
- Or Right-click → Modify

Depending on the value type:
- REG_DWORD accepts numeric data only
- REG_SZ accepts text data only

Creating Keys and Values
- Right-click inside a key
- Select New → Key or Value

Every new key contains a default (Default) value.

Right-clicking a key allows:
- Rename
- Delete
- Export
- Permissions

--------------------------------------------------
Registry via Command Prompt (REG)
--------------------------------------------------

General syntax:
REG [Operation] [Parameters]

--------------------------------------------------
REG QUERY – Querying the Registry
--------------------------------------------------

Query a key:
REG QUERY HKLM\Software\Microsoft\Windows\CurrentVersion\WSMAN

Query all subkeys recursively:
REG QUERY HKLM\SOFTWARE\Microsoft /s

Query a specific value:
REG QUERY HKLM\...\WSMAN /v ServiceStackVersion

Query the default value:
REG QUERY HKLM\...\CurrentVersion /ve

--------------------------------------------------
Searching the Registry
--------------------------------------------------

Search using /f:
REG QUERY HKLM\SOFTWARE\Microsoft /s /f token

Search key names only:
/k

Search data only:
/d

Case-sensitive search:
/c

Exact match only:
/e

Filter by value type:
/t REG_SZ

--------------------------------------------------
REG ADD – Adding Keys and Values
--------------------------------------------------

Add a key:
REG ADD HKLM\SOFTWARE\Microsoft\ActiveSync\NewKey2

Add a value:
REG ADD HKLM\...\NewKey2 /v Data /t REG_SZ /d example

Force overwrite without confirmation:
/f

--------------------------------------------------
REG DELETE – Deleting Keys and Values
--------------------------------------------------

Delete a key (including subkeys):
REG DELETE HKLM\...\NewKey2

Delete a value:
REG DELETE HKLM\...\NewKey2 /v Data

Delete without confirmation:
/f

--------------------------------------------------
REG COPY – Copying Registry Data
--------------------------------------------------

Copy keys and values:
REG COPY Key1 Key2 /s /f

--------------------------------------------------
REG SAVE / LOAD / UNLOAD
--------------------------------------------------

Save a hive or key:
REG SAVE HKLM\SOFTWARE backup.hiv /y

Load a saved hive:
REG LOAD HKLM\TempHive backup.hiv

Unload a loaded hive:
REG UNLOAD HKLM\TempHive

--------------------------------------------------
REG RESTORE
--------------------------------------------------

Restore a previously saved hive:
REG RESTORE HKLM\SOFTWARE backup.hiv

--------------------------------------------------
REG COMPARE
--------------------------------------------------

Compare registry keys:
REG COMPARE Key1 Key2 /v ValueName /s

/oa  Show all results
/od  Show differences only
/os  Show matches only
/on  No output

--------------------------------------------------
REG EXPORT / IMPORT
--------------------------------------------------

Export a key:
REG EXPORT HKLM\...\Key file.reg /y

Import a registry file:
REG IMPORT file.reg

--------------------------------------------------
Attacker Perspective
--------------------------------------------------

Attackers abuse the registry for:
- Persistence
- Disabling security controls
- Modifying system behavior
- Executing payloads at startup

Common abuse locations:
- Run / RunOnce keys
- Services
- Policies
- Windows Defender keys

--------------------------------------------------
Defender / SOC Perspective
--------------------------------------------------

Defenders monitor:
- REG ADD / REG DELETE activity
- Startup and persistence keys
- Changes in HKLM and HKCU
- Unexpected reg.exe executions

Analysis focus:
- Process creation context
- Parent-child process relationships
- Before/after registry state comparison

--------------------------------------------------
Security Summary
--------------------------------------------------

The Windows Registry is the core of the operating system.
A single malicious or incorrect change can fully compromise a system.
Understanding the registry is essential for both attackers and defenders.

