# Table of Contents
- [Windows Authorization Process](#Windows\Authorization\Process)
- [Rights and Privileges in Windows](#Rights\and\Privileges\in\Windows)
- [Local Admin User Rights - Elevated](#Local\Admin\User\Rights\-\Elevated)
###### Resources
[Windows Privileges Documentation](https://learn.microsoft.com/en-us/windows/win32/secauthz/privileges)
[Security Identifiers(SIDs)](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-identifiers)
[Windows privilege abuse auditing detection and defense](https://blog.palantir.com/windows-privilege-abuse-auditing-detection-and-defense-3078a403d74e)
###### Documentation notes
A system administrator assigns privileges to user and group accounts, whereas the system grants or denies access to a securable object based on the access rights granted in the ACEs in the object's DACL.

- Rights that can be granted to perform tasks such as managing services, loading drivers, shutting down the system, debugging, and more
- different from access rights which a system used to gran or deny access to securable objects
- Stored in db, access is granted via access token when a user logs on to a system. 
- An account can have local privileges on a specific computer and different privileges on different systems if the account belongs to an Active Directory domain

## Windows Authorization Process
**Security Principals** are anything that can be authenticated by Windows OS (User and computer accounts, and processes that run in the security context or another user/account, or security groups that these accounts belong to).
- Primary way of controlling access to resources on windows hosts.
- Identified by a unique [Security Identifier (SID)](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/security-identifiers-in-windows).

The Windows authorization process compares a user's access token, which includes their SID, group SIDs, and privileges, against the Access Control Entries (ACEs) in the object's security descriptor to determine access rights. This comparison decides whether to grant or deny access to securable objects like folders, occurring almost instantaneously during every access attempt. Privilege escalation activities aim to manipulate this process to gain unauthorized access.
![[auth_process 1.png]]

## Rights and Privileges in Windows
- Contains many groups that grant members powerful rights/privileges. These can be abused to escalate privileges on both a standalone windows host and within an active directory domain environment.
- these may be used to gain Domain Admin, local administrator, or SYSTEM privileges on a Windows workstation, server, or Domain Controller (DC). Some of these groups are listed below.

| **Group**                       | **Description**                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Default Administrators**      | Domain Admins and Enterprise Admins are "super" groups.                                                                                                                                                                                                                                                                                                            |
| **Server Operators**            | Members can modify services, access SMB shares, and backup files.                                                                                                                                                                                                                                                                                                  |
| **Backup Operators**            | Members are allowed to log onto DCs locally and should be considered Domain Admins. They can make shadow copies of the SAM/NTDS database, read the registry remotely, and access the file system on the DC via SMB. This group is sometimes added to the local Backup Operators group on non-DCs.                                                                  |
| **Print Operators**             | Members can log on to DCs locally and "trick" Windows into loading a malicious driver.                                                                                                                                                                                                                                                                             |
| **Hyper-V Administrators**      | If there are virtual DCs, any virtualization admins, such as members of Hyper-V Administrators, should be considered Domain Admins.                                                                                                                                                                                                                                |
| **Account Operators**           | Members can modify non-protected accounts and groups in the domain.                                                                                                                                                                                                                                                                                                |
| **Remote Desktop Users**        | Members are not given any useful permissions by default but are often granted additional rights such as `Allow Login Through Remote Desktop Services` and can move laterally using the RDP protocol.                                                                                                                                                               |
| **Remote Management Users**     | Members can log on to DCs with PSRemoting (This group is sometimes added to the local remote management group on non-DCs).                                                                                                                                                                                                                                         |
| **Group Policy Creator Owners** | Members can create new GPOs but would need to be delegated additional permissions to link GPOs to a container such as a domain or OU.                                                                                                                                                                                                                              |
| **Schema Admins**               | Members can modify the Active Directory schema structure and backdoor any to-be-created Group/GPO by adding a compromised account to the default object ACL.                                                                                                                                                                                                       |
| **DNS Admins**                  | Members can load a DLL on a DC, but do not have the necessary permissions to restart the DNS server. They can load a malicious DLL and wait for a reboot as a persistence mechanism. Loading a DLL will often result in the service crashing. A more reliable way to exploit this group is to [create a WPAD record](https://cube0x0.github.io/Pocing-Beyond-DA/). |


### User Rights Assignment
- [User rights assignment documentation](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/security-policy-settings/user-rights-assignment)

| Setting Constant              | Setting Name                                                                                                                                                                              | Standard Assignment                                     | Description                                                                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| SeNetworkLogonRight           | [Access this computer from the network](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/access-this-computer-from-the-network)               | Administrators, Authenticated Users                     | Determines which users can connect to the device from the network. Required by network protocols such as SMB, NetBIOS.        |
| SeRemoteInteractiveLogonRight | [Allow log on through Remote Desktop Services](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/allow-log-on-through-remote-desktop-services) | Administrators, Remote Desktop Users                    | Determines which users or groups can access the login screen of a remote device through a Remote Desktop Services connection. |
| SeBackupPrivilege             | [Back up files and directories](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/back-up-files-and-directories)                               | Administrators                                          | Determines which users can bypass file and directory permissions for backing up the system.                                   |
| SeSecurityPrivilege           | [Manage auditing and security log](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/manage-auditing-and-security-log)                         | Administrators                                          | Determines which users can specify object access audit options and view/clear the Security log in Event Viewer.               |
| SeTakeOwnershipPrivilege      | [Take ownership of files or other objects](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/take-ownership-of-files-or-other-objects)         | Administrators                                          | Determines which users can take ownership of any securable object.                                                            |
| SeDebugPrivilege              | [Debug programs](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/debug-programs)                                                             | Administrators                                          | Determines which users can attach to or open any process, even if they do not own it.                                         |
| SeImpersonatePrivilege        | [Impersonate a client after authentication](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/impersonate-a-client-after-authentication)       | Administrators, Local Service, Network Service, Service | Determines which programs can impersonate a user or another specified account.                                                |
| SeLoadDriverPrivilege         | [Load and unload device drivers](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/load-and-unload-device-drivers)                             | Administrators                                          | Determines which users can dynamically load and unload device drivers.                                                        |
| SeRestorePrivilege            | [Restore files and directories](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/restore-files-and-directories)                               | Administrators                                          | Determines which users can bypass file and directory permissions when restoring files and directories.                        |
- [User rights in windows server](https://4sysops.com/archives/user-rights-assignment-in-windows-server-2016/)
`whoami /priv` - listing of all user rights for the current user
- Some rights are only available to administrative users and can only be listed/leveraged when running an elevated cmd or PowerShell session
- These concepts of elevated rights and [User Account Control (UAC)](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works) are security features introduced with Windows Vista to default to restricting applications from running with full permissions unless necessary. If we compare and contrast the rights available to us as an admin in a non-elevated console vs. an elevated console, we will see that they differ drastically.


#### Local Admin User Rights - Elevated
If we run an elevated command window, we can see the complete listing of rights available to us.
- When a privilege is listed for our account in the `Disabled` state, it means that our account has the specific privilege assigned. Still, it cannot be used in an access token to perform the associated actions until it is enabled
- Windows doesn't provide a built-in command or PowerShell cmdlet to enable privileges, so we need to script it.

[This](https://www.powershellgallery.com/packages/PoshPrivilege/0.3.0.0/Content/Scripts%5CEnable-Privilege.ps1) powershell script can be used to enable certain privileges.
- [This](https://www.leeholmes.com/adjusting-token-privileges-in-powershell/) script can be used to adjust token privileges



