 Users and Groups

Introduction
Users and Groups are core components of Windows access control.
Every action on a Windows system is tied to a user account and the groups it belongs to.
Misconfigurations in users or group memberships are one of the most common causes of privilege escalation.
Users and groups can be managed using both the GUI and CMD.

--------------------------------------------------
USERS (GUI)
--------------------------------------------------

Viewing Users
Users can be viewed via:
- lusrmgr.msc
- Computer Management

This interface displays all local user accounts.
Disabled accounts appear with a down-arrow icon.
System accounts (e.g. WDAGUtilityAccount) are often disabled by default.

User Properties
Each user account has:
- Password configuration
- Account status (enabled / disabled)
- Group memberships

Creating a New User
Actions → New User
You can configure:
- Username and password
- User must change password at next logon
- User cannot change password
- Password never expires

Modifying or Deleting Users
Right-click a user:
- Reset password
- Enable / Disable account
- Rename account
- Delete account

--------------------------------------------------
GROUPS (GUI)
--------------------------------------------------

Viewing Groups
Located under:
Local Users and Groups → Groups

Groups define permissions, not identities.
Many groups are built-in or created by installed software.

Important Groups
Administrators → Full system control
Users → Standard access
Remote Desktop Users → RDP access

Managing Group Membership
Users can be added or removed via:
- Group properties (Members tab)
- User properties (Member Of tab)

--------------------------------------------------
USERS VIA CMD
--------------------------------------------------

List all users
net user

View specific user
net user Username

Create user
net user User1 Password123 /add

Delete user
net user User1 /delete

Change password
net user User1 NewPassword

Disable user
net user User1 /active:no

Enable user
net user User1 /active:yes

--------------------------------------------------
NET USER FLAGS
--------------------------------------------------

/add
Creates a new user account.

/delete
Deletes a user account.

/active:yes | no
Enables or disables the account.

/expires:date
Sets account expiration.

/passwordchg:yes | no
Controls password change permission.

/passwordreq:yes | no
Requires password or not.

/logonpasswordchg:yes | no
Force password change at next login.

--------------------------------------------------
GROUPS VIA CMD
--------------------------------------------------

List groups
net localgroup

View group members
net localgroup GroupName

Create group
net localgroup Group1 /add

Add user to group
net localgroup Group1 User1 /add

Remove user from group
net localgroup Group1 User1 /delete

Delete group
net localgroup Group1 /delete

--------------------------------------------------
NET LOCALGROUP FLAGS
--------------------------------------------------

/add
Creates a group or adds a user.

/delete
Deletes a group or removes a user.

--------------------------------------------------
ATTACKER PERSPECTIVE
--------------------------------------------------

Attackers enumerate users and groups to identify privileged accounts.
Common attacker actions:
- Create new users
- Add users to Administrators group
- Abuse net user and net localgroup
Group membership enumeration is a key step in privilege escalation.

--------------------------------------------------
DEFENDER / SOC PERSPECTIVE
--------------------------------------------------

Monitor for:
- New local user creation
- Group membership changes
- Execution of net user and net localgroup
- Accounts enabled or disabled unexpectedly

Audit privileged groups regularly.
Correlate user changes with process creation and authentication logs.

--------------------------------------------------
SECURITY SUMMARY
--------------------------------------------------

Users define identity.
Groups define permissions.
Misconfigured groups lead to full system compromise.
CMD usage is common in attacks and automation.
Continuous monitoring of users and groups is essential.

