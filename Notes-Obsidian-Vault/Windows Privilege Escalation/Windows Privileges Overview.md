[Privileges](https://docs.microsoft.com/en-us/windows/win32/secauthz/privileges) in Windows are rights that an account can be granted to perform a variety of operations on the local system such as managing services, loading drivers, shutting down the system, debugging an application, and more. Privileges are different from access rights, which a system uses to grant or deny access to securable objects. User and group privileges are stored in a database and granted via an access token when a user logs on to a system. An account can have local privileges on a specific computer and different privileges on different systems if the account belongs to an Active Directory domain. Each time a user attempts to perform a privileged action, the system reviews the user's access token to see if the account has the required privileges, and if so, checks to see if they are enabled. Most privileges are disabled by default. Some can be enabled by opening an administrative cmd.exe or PowerShell console, while others can be enabled manually.


Goal is always to gain admin access to a system or multiple systems. 


## Windows Authorization Process

**Security principals** are anything that can be authenticated by the Windows operating system, including user and computer accounts, processes that run in the security context or another user/computer account, or the security groups that these accounts belong to. 

**Security principals** are the primary way of controlling access to resources on Windows hosts. Every single security principal is identified by a unique [Security Identifier (SID)](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/security-identifiers-in-windows). When a security principal is created, it is assigned a `SID` which remains assigned to that principal for its lifetime.


The below diagram walks through the Windows authorization and access control process at a high level, showing, for example, the process started when a user attempts to access a securable object such as a folder on a file share. During this process, the user's access token (including their user SID, SIDs for any groups they are members of, privilege list, and other access information) is compared against [Access Control Entries (ACEs)](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-entries) within the object's [security descriptor](https://docs.microsoft.com/en-us/windows/win32/secauthz/security-descriptors) (which contains security information about a securable object such as access rights (discussed below) granted to users or groups).

Once complete, a decision is mate to either grant or deny access. This entire process happens almost instantaneously whenever a user tries to access a resource on a Windows host. As part of our enumeration and privilege escalation activies, we may attemtpt o use and abuse access rights and leverage or insert ourselves into this authorization process to further our access towards our goal.

![[auth_process.png]]

## Rights and Privileges in Windows

Windows contains many groups that grant their members powerful rights and privileges. Many of these can be abused to escalate privileges on both a standalone Windows host and within an Active Directory domain environment. Ultimately, these may be used to gain Domain Admin, local administrator, or SYSTEM privileges on a Windows workstation, server, or Domain Controller (DC). Some of these groups are listed below.

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


## User Rights Assignment

Depending on group membership, and other factors such as privileges assigned via domain and local Group Policy, users can have various rights assigned to their account. This Microsoft article on [User Rights Assignment](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment) provides a detailed explanation of each of the user rights that can be set in Windows as well as security considerations applicable to each right. Below are some of the key user rights assignments, which are settings applied to the localhost. These rights allow users to perform tasks on the system such as logon locally or remotely, access the host from the network, shut down the server, etc.

| Setting [Constant](https://docs.microsoft.com/en-us/windows/win32/secauthz/privilege-constants) | Setting Name                                                                                                                                                                              | Standard Assignment                                     | Description                                                                                                                                                                                                                                                                                                                                                |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **SeNetworkLogonRight**                                                                         | [Access this computer from the network](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/access-this-computer-from-the-network)               | Administrators, Authenticated Users                     | Determines which users can connect to the device from the network. This is required by network protocols such as SMB, NetBIOS, CIFS, and COM+.                                                                                                                                                                                                             |
| **SeRemoteInteractiveLogonRight**                                                               | [Allow log on through Remote Desktop Services](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/allow-log-on-through-remote-desktop-services) | Administrators, Remote Desktop Users                    | This policy setting determines which users or groups can access the login screen of a remote device through a Remote Desktop Services connection. A user can establish a Remote Desktop Services connection to a particular server but not be able to log on to the console of that same server.                                                           |
| **SeBackupPrivilege**                                                                           | [Back up files and directories](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/back-up-files-and-directories)                               | Administrators                                          | This user right determines which users can bypass file and directory, registry, and other persistent object permissions for the purposes of backing up the system.                                                                                                                                                                                         |
| **SeSecurityPrivilege**                                                                         | [Manage auditing and security log](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/manage-auditing-and-security-log)                         | Administrators                                          | This policy setting determines which users can specify object access audit options for individual resources such as files, Active Directory objects, and registry keys. These objects specify their system access control lists (SACL). A user assigned this user right can also view and clear the Security log in Event Viewer.                          |
| **SeTakeOwnershipPrivilege**                                                                    | [Take ownership of files or other objects](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/take-ownership-of-files-or-other-objects)         | Administrators                                          | This policy setting determines which users can take ownership of any securable object in the device, including Active Directory objects, NTFS files and folders, printers, registry keys, services, processes, and threads.                                                                                                                                |
| **SeDebugPrivilege**                                                                            | [Debug programs](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/debug-programs)                                                             | Administrators                                          | This policy setting determines which users can attach to or open any process, even a process they do not own. Developers who are debugging their applications do not need this user right. Developers who are debugging new system components need this user right. This user right provides access to sensitive and critical operating system components. |
| **SeImpersonatePrivilege**                                                                      | [Impersonate a client after authentication](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/impersonate-a-client-after-authentication)       | Administrators, Local Service, Network Service, Service | This policy setting determines which programs are allowed to impersonate a user or another specified account and act on behalf of the user.                                                                                                                                                                                                                |
| **SeLoadDriverPrivilege**                                                                       | [Load and unload device drivers](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/load-and-unload-device-drivers)                             | Administrators                                          | This policy setting determines which users can dynamically load and unload device drivers. This user right is not required if a signed driver for the new hardware already exists in the driver.cab file on the device. Device drivers run as highly privileged code.                                                                                      |
| **SeRestorePrivilege**                                                                          | [Restore files and directories](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/restore-files-and-directories)                               | Administrators                                          | This security setting determines which users can bypass file, directory, registry, and other persistent object permissions when they restore backed up files and directories. It determines which users can set valid security principals as the owner of an object.                                                                                       |

### Windows Server Security Measures

- **Group policy objects (GPO)** – Used in Active Directory domains to configure and regularly reapply security settings to multiple computers.
- **Local security policy (secpol.msc)** – Used to configure a single (local) computer. Note that this is a one-time action. If another administrator changes these settings, you will need to manually change them back to the required state.
	- Enable stricter baselines for each OS version 

It is a best practice to configure security policies using only built-in local security principals and groups, and add needed members to these entities. This gives you much better visibility and flexibility

- GPO provides more options to manage local group members, than to manage security policy members

- **Administrators** – Members of this group have full, unrestricted access to the computer. Even if you remove some privileges from the Administrators group, a skilled administrator can still bypass those settings and gain control of the system. Only add highly trusted people to this group.
- **Authenticated Users** – A special security principal that applies to any session that was authenticated using some account, such as a local or domain account.
- **Local account and member of Administrators group** – A pseudogroup available since Windows Server 2012 R2. It applies to any local account in the Administrators group and is used to mitigate pass-the-hash attacks (lateral movement).
- **Remote Desktop Users** – Members of this group can access the computer via Remote Desktop services (RDP).
- **Guests** – By default, this group has no permissions. I don't think there is any need to use the Guest account and group today.
	- [CIS(Controls)](https://www.cisecurity.org/controls)
	- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/)


**User rights assignments are settings applied to the local device.**
- Allows user's to perform various system tasks (logon, remote logon, accessing the server from network, etc)

For each setting, the following format is used:

- **Name of the setting:** Recommended value, or values
- **Access Credential Manager as a trusted caller:** No one (empty value)

Access to the Credential Manager is granted during Winlogon only to the user who is logging on. Saved user credentials might be compromised if someone else has this privilege.

- **Access this computer from the network:** Administrators, Authenticated Users

Required for users to connect to the computer and its resources, such as an SMB share, shared printers, COM+, etc. If you remove this user right on the DC, no one will be able to log on to the domain.

- **_Note_**_: On DCs, you should also add the “ENTERPRISE DOMAIN CONTROLLERS“ group._
- **Allow log on locally:** Administrators

Default configuration includes the Users group, which allows a standard user to log on to the sever console. Limit this privilege only to administrators

- **Allow log on through Remote Desktop Services:** Administrators, Remote Desktop Users

It's common practice that some applications are used via RDP sessions by standard users. This privilege is also frequently required for remote assistance offered by an organization's helpdesk. If a server is running Remote Desktop Services with the Connection Broker role, the Authenticated Users group must also be added to this privilege.

- **Back up files and directories:** Administrators

This is a sensitive privilege that allows a user to bypass NTFS permissions (only via an NTFS API interface, such as NTBACKUP). A malicious user could backup and restore data on a different computer, thereby gaining access to it.

- **Deny access to this computer from the network/Deny log on through Terminal Services:** Local account and member of Administrators group, Guests

The default value is only Guests. You should add the second group to prevent pass-the-hash attacks, so if a local elevated user is compromised, it cannot be used to elevate privileges on any other network resource, or access it via RDP.

The default value is only Guests. You should add the second group to prevent pass-the-hash attacks, so if a local elevated user is compromised, it cannot be used to elevate privileges on any other network resource, or access it via RDP.

- **Force shutdown from a remote system/Shut down the system:** Administrators

Only administrators should be able to shut down any server, to prevent denial-of-service (DoS) attacks.

- **Manage auditing and security log:** Administrators

This is a sensitive privilege, as anyone with these rights can erase important evidence of unauthorized activity.

- **_Note:_** _If you are running MS Exchange, the “Exchange Servers” group must be added to DCs._

- **Restore files and directories:** Administrators

Attackers with this privilege can overwrite data, or even executable files used by legitimate administrators, with versions that include malicious code.

- **Take ownership of files or other objects:** Administrators

User having this privilege can take control (ownership) of any object, such as a file or folder, and expose sensitive data.

- **Deny log on as a batch job/Deny log on as a service/Deny log on locally:** Guests

To increase security, you should include the Guests group in these three settings.

- **Debug programs/Profile single process/Profile system performance:** Administrators

This setting allows a user to attach a debugger to a system or process, thereby accessing critical, sensitive data. It can be used by attackers to collect information about running critical processes, or which users are logged on.

- **Change the system time:** Administrators, Local Service

Changes in system time might lead to DoS issues, such as unavailability to authenticate to the domain. The Local Service role is required for the Windows Time service, VMware Tools service, and others to synchronize system time with the DC or ESXi host.

- **Create a token object:** No one (empty value)

Users with the ability to create or modify access tokens can elevate any currently logged on account, including their own.

- **Impersonate a client after authentication:** Administrators, Local Service, Network Service, Service

An attacker with this privilege can create a service, trick a client into connecting to that service, and then impersonate that account.

- **_Note:_** _For servers running Internet Information Services (IIS), the "IIS_IUSRS" account must also be added._

- **Load and unload device drivers:** Administrators

Malicious code can be installed that pretends to be a device driver. Administrators should only install drivers with a valid signature.





