## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Hash Protocol Comparison](#Hash\Protocol\Comparison)
  - [LM](#LM)
      - [NTLM Authentication Request:](#NTLM\Authentication\Request:)
      - [V1 Challenge & Response Algorithm](#V1\Challenge\&\Response\Algorithm)
      - [V2 Challenge & Response Algorithm](#V2\Challenge\&\Response\Algorithm)
  - [Domain Cached Credentials (MSCache2)](#Domain\Cached\Credentials\(MSCache2))
  - [Domain Users](#Domain\Users)
  - [User Naming Attributes](#User\Naming\Attributes)
      - [Common User Attributes](#Common\User\Attributes)
  - [Types of Groups](#Types\of\Groups)
  - [Group Type and Scope](#Group\Type\and\Scope)
      - [AD Group Scope Examples](#AD\Group\Scope\Examples)
  - [Important Group Attributes](#Important\Group\Attributes)
- [Built-in AD Groups](#built-in\ad\groups)
      - [Server Operators Group Details](#Server\Operators\Group\Details)
      - [Domain Admins Group Membership](#Domain\Admins\Group\Membership)

## Table of Contents

      - [Hash Protocol Comparison](#Hash\Protocol\Comparison)
  - [LM](#LM)
      - [NTLM Authentication Request:](#NTLM\Authentication\Request:)
      - [V1 Challenge & Response Algorithm](#V1\Challenge\&\Response\Algorithm)
      - [V2 Challenge & Response Algorithm](#V2\Challenge\&\Response\Algorithm)
  - [Domain Cached Credentials (MSCache2)](#Domain\Cached\Credentials\(MSCache2))
  - [Domain Users](#Domain\Users)
  - [User Naming Attributes](#User\Naming\Attributes)
      - [Common User Attributes](#Common\User\Attributes)
  - [Types of Groups](#Types\of\Groups)
  - [Group Type and Scope](#Group\Type\and\Scope)
      - [AD Group Scope Examples](#AD\Group\Scope\Examples)
  - [Important Group Attributes](#Important\Group\Attributes)
- [Built-in AD Groups](#built-in\ad\groups)
      - [Server Operators Group Details](#Server\Operators\Group\Details)
      - [Domain Admins Group Membership](#Domain\Admins\Group\Membership)


Aside from Kerberos and LDAP, Active Directory uses several other authentication methods which can be used (and abused) by applications and services in AD. These include LM, NTLM, NTLMv1, and NTLMv2. LM and NTLM here are the hash names, and NTLMv1 and NTLMv2 are authentication protocols that utilize the LM or NT hash. Below is a quick comparison between these hashes and protocols, which shows us that, while not perfect by any means, Kerberos is often the authentication protocol of choice wherever possible. It is essential to understand the difference between the hash types and the protocols that use them.


#### Hash Protocol Comparison

|**Hash/Protocol**|**Cryptographic technique**|**Mutual Authentication**|**Message Type**|**Trusted Third Party**|
|---|---|---|---|---|
|`NTLM`|Symmetric key cryptography|No|Random number|Domain Controller|
|`NTLMv1`|Symmetric key cryptography|No|MD4 hash, random number|Domain Controller|
|`NTLMv2`|Symmetric key cryptography|No|MD4 hash, random number|Domain Controller|
|`Kerberos`|Symmetric key cryptography & asymmetric cryptography|Yes|Encrypted ticket using DES, MD5|Domain Controller/Key Distribution Center (KDC)|


## LM

`LAN Manager` (LM or LANMAN) hashes are the oldest password storage mechanism used by the Windows operating system. LM debuted in 1987 on the OS/2 operating system. If in use, they are stored in the SAM database on a Windows host and the NTDS.DIT database on a Domain Controller. Due to significant security weaknesses in the hashing algorithm used for LM hashes, it has been turned off by default since Windows Vista/Server 2008. However, it is still common to encounter, especially in large environments where older systems are still used. Passwords using LM are limited to a maximum of `14` characters. Passwords are not case sensitive and are converted to uppercase before generating the hashed value, limiting the keyspace to a total of 69 characters making it relatively easy to crack these hashes using a tool such as Hashcat.

Before hashing, a 14 character password is first split into two seven-character chunks. If the password is less than fourteen characters, it will be padded with NULL characters to reach the correct value. Two DES keys are created from each chunk. These chunks are then encrypted using the string `KGS!@#$%`, creating two 8-byte ciphertext values. These two values are then concatenated together, resulting in an LM hash. This hashing algorithm means that an attacker only needs to brute force seven characters twice instead of the entire fourteen characters, making it fast to crack LM hashes on a system with one or more GPUs. If a password is seven characters or less, the second half of the LM hash will always be the same value and could even be determined visually without even needed tools such as Hashcat. The use of LM hashes can be disallowed using [Group Policy](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/network-security-do-not-store-lan-manager-hash-value-on-next-password-change). An LM hash takes the form of `299bd128c1101fd6`.

`NT LAN Manager` (NTLM) hashes are used on modern Windows systems. It is a challenge-response authentication protocol and uses three messages to authenticate: a client first sends a `NEGOTIATE_MESSAGE` to the server, whose response is a `CHALLENGE_MESSAGE` to verify the client's identity. Lastly, the client responds with an `AUTHENTICATE_MESSAGE`. These hashes are stored locally in the SAM database or the NTDS.DIT database file on a Domain Controller. The protocol has two hashed password values to choose from to perform authentication: the LM hash (as discussed above) and the NT hash, which is the MD4 hash of the little-endian UTF-16 value of the password. The algorithm can be visualized as: `MD4(UTF-16-LE(password))`.

#### NTLM Authentication Request:
Client |(User Auth Request)  NTLM Negotiate message---> | Server(DC)
														NTLM Challenge Message<-- |Server(DC)
Client(User Auth Request) NTLM Authenticate message ---> | Server(DC)


An NT hash takes the form of `b4b9b02e6f09a9bd760f388b67351e2b`, which is the second half of the full NTLM hash. An NTLM hash looks like this:

  
NTLM Authentication Request

```shell-session
Rachel:500:aad3c435b514a4eeaad3b935b51304fe:e46b9e548fa0d122de7f59fb6d48eaa2:::
```

Looking at the hash above, we can break the NTLM hash down into its individual parts:

- `Rachel` is the username
- `500` is the Relative Identifier (RID). 500 is the known RID for the `administrator` account
- `aad3c435b514a4eeaad3b935b51304fe` is the LM hash and, if LM hashes are disabled on the system, can not be used for anything
- `e46b9e548fa0d122de7f59fb6d48eaa2` is the NT hash. This hash can either be cracked offline to reveal the cleartext value (depending on the length/strength of the password) or used for a pass-the-hash attack. Below is an example of a successful pass-the-hash attack using the [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) tool:

  
NTLM Authentication Request

```shell-session
gdxqpardo@htb[/htb]$ crackmapexec smb 10.129.41.19 -u rachel -H e46b9e548fa0d122de7f59fb6d48eaa2
```

Neither LANMAN nor NTLM uses a salt.
#### V1 Challenge & Response Algorithm

  V1 Challenge & Response Algorithm

```shell-session
C = 8-byte server challenge, random
K1 | K2 | K3 = LM/NT-hash | 5-bytes-0
response = DES(K1,C) | DES(K2,C) | DES(K3,C)
```


NTLMv1 Hash Example:

  
NTLMv1 Hash Example

```shell-session
u4-netntlm::kNS:338d08f8e26de93300000000000000000000000000000000:9526fb8c23a90751cdd619b6cea564742e1e4bf33006ba41:cb8086049ec4736c
```


been the default in Windows since Server 2000. It is hardened against certain spoofing attacks that NTLMv1 is susceptible to. NTLMv2 sends two responses to the 8-byte challenge received by the server. These responses contain a 16-byte HMAC-MD5 hash of the challenge, a randomly generated challenge from the client, and an HMAC-MD5 hash of the user's credentials. A second response is sent, using a variable-length client challenge including the current time, an 8-byte random value, and the domain name. The algorithm is as follows:

#### V2 Challenge & Response Algorithm

  V2 Challenge & Response Algorithm

```shell-session
SC = 8-byte server challenge, random
CC = 8-byte client challenge, random
CC* = (X, time, CC2, domain name)
v2-Hash = HMAC-MD5(NT-Hash, user name, domain name)
LMv2 = HMAC-MD5(v2-Hash, SC, CC)
NTv2 = HMAC-MD5(v2-Hash, SC, CC*)
response = LMv2 | CC | NTv2 | CC*
```

```shell-session
admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030
```

NTLMv2 is harder to crack.



## Domain Cached Credentials (MSCache2)

In an AD environment, the authentication methods mentioned in this section and the previous require the host we are trying to access to communicate with the "brains" of the network, the Domain Controller. Microsoft developed the [MS Cache v1 and v2](https://webstersprodigy.net/2014/02/03/mscash-hash-primer-for-pentesters/) algorithm (also known as `Domain Cached Credentials` (DCC) to solve the potential issue of a domain-joined host being unable to communicate with a domain controller (i.e., due to a network outage or other technical issue) and, hence, NTLM/Kerberos authentication not working to access the host in question. Hosts save the last `ten` hashes for any domain users that successfully log into the machine in the `HKEY_LOCAL_MACHINE\SECURITY\Cache` registry key. These hashes cannot be used in pass-the-hash attacks. Furthermore, the hash is very slow to crack with a tool such as Hashcat, even when using an extremely powerful GPU cracking rig, so attempts to crack these hashes typically need to be extremely targeted or rely on a very weak password in use. These hashes can be obtained by an attacker or pentester after gaining local admin access to a host and have the following format: `$DCC2$10240#bjones#e4e938d12fe5974dc42a90120bd9c90f`. It is vital as penetration testers that we understand the varying types of hashes that we may encounter while assessing an AD environment, their strengths, weaknesses, how they can be abused (cracking to cleartext, pass-the-hash, or relayed), and when an attack may be futile (i.e., spending days attempting to crack a set of Domain Cached Credentials).




Local Accounts:
Stored locally on a certain server or workstation. Can be assigned rights on that host either individually or via group membership. Only works on one hose, not across the domain.
	- `Administrator`: this account has the SID `S-1-5-domain-500` and is the first account created with a new Windows installation. It has full control over almost every resource on the system. It cannot be deleted or locked, but it can be disabled or renamed. Windows 10 and Server 2016 hosts disable the built-in administrator account by default and create another local account in the local administrator's group during setup.
	- Gues: This account is diabled by defualt. The purpose of this account it to allow users without an account on the computer to log in temporarily wiht limited access rights. By defualt, it has a blank password and is generally recommended to be left disabled becuase of the securit risk of allowing anonymous access to a host.
	- 

- `SYSTEM`: The SYSTEM (or `NT AUTHORITY\SYSTEM`) account on a Windows host is the default account installed and used by the operating system to perform many of its internal functions. Unlike the Root account on Linux, `SYSTEM` is a service account and does not run entirely in the same context as a regular user. Many of the processes and services running on a host are run under the SYSTEM context. One thing to note with this account is that a profile for it does not exist, but it will have permissions over almost everything on the host. It does not appear in User Manager and cannot be added to any groups. A `SYSTEM` account is the highest permission level one can achieve on a Windows host and, by default, is granted Full Control permissions to all files on a Windows system.
    
- `Network Service`: This is a predefined local account used by the Service Control Manager (SCM) for running Windows services. When a service runs in the context of this particular account, it will present credentials to remote services.
    
- `Local Service`: This is another predefined local account used by the Service Control Manager (SCM) for running Windows services. It is configured with minimal privileges on the computer and presents anonymous credentials to the network.
    

It is worth studying Microsoft's documentation on [local default accounts](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/local-accounts) in-depth to gain a better understanding of how the various accounts work together on an individual Windows system and across a domain network. Take some time to look them over and understand the nuances between them.



## Domain Users

Domain users differ from local users in that they are granted rights from the domain to access resources such as file servers, printers, intranet hosts, and other objects based on the permissions granted to their user account or the group that account is a member of. Domain user accounts can log in to any host in the domain, unlike local users. For more information on the many different Active Directory account types, check out this [link](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-accounts). One account to keep in mind is the `KRBTGT` account, however. This is a type of local account built into the AD infrastructure. This account acts as a service account for the Key Distribution service providing authentication and access for domain resources. This account is a common target of many attackers since gaining control or access will enable an attacker to have unconstrained access to the domain. It can be leveraged for privilege escalation and persistence in a domain through attacks such as the [Golden Ticket](https://attack.mitre.org/techniques/T1558/001/) attack.


## User Naming Attributes

Security in Active Directory can be improved using a set of user naming attributes to help identify user objects like logon name or ID. The following are a few important Naming Attributes in AD:

|||
|---|---|
|`UserPrincipalName` (UPN)|This is the primary logon name for the user. By convention, the UPN uses the email address of the user.|
|`ObjectGUID`|This is a unique identifier of the user. In AD, the ObjectGUID attribute name never changes and remains unique even if the user is removed.|
|`SAMAccountName`|This is a logon name that supports the previous version of Windows clients and servers.|
|`objectSID`|The user's Security Identifier (SID). This attribute identifies a user and its group memberships during security interactions with the server.|
|`sIDHistory`|This contains previous SIDs for the user object if moved from another domain and is typically seen in migration scenarios from domain to domain. After a migration occurs, the last SID will be added to the `sIDHistory` property, and the new SID will become its `objectSID`.|



#### Common User Attributes

  Common User Attributes

```powershell-session
PS C:\htb Get-ADUser -Identity htb-student
```
For a deeper look at user object attributes, check out this [page](https://docs.microsoft.com/en-us/windows/win32/ad/user-object-attributes).


 What default user account has the SID "S-1-5-domain-500" ?
 Administrator
What account has the highest permission level possible on a windows host?
SYSTEM

What user naming attribute is unique to the user and will remain so even if the account is deleted?
ObjectGUID





## Types of Groups

In simpler terms, groups are used to place users, computers, and contact objects into management units that provide ease of administration over permissions and facilitate the assignment of resources such as printers and file share access. For example, if an admin needs to assign 50 members of a department access to a new share drive, it would be time-consuming to add each user's account individually. Granting permissions this way would also make it more difficult to audit who has access to resources and difficult to clean up/revoke permissions. Instead, a sysadmin can either use an existing group or create a new group and grant that specific group permissions over the resource. From here, every user in the group will inherit the permissions based on their membership in the group. If the permissions need to be modified or revoked for one or more users, they could merely be removed from the group, leaving the other users unaffected and their permissions intact.

Groups in Active Directory have two fundamental characteristics: `type` and `scope`. The `group type` defines the group's purpose, while the `group scope` shows how the group can be used within the domain or forest. When creating a new group, we must select a group type. There are two main types: `security` and `distribution` groups.


## Group Type and Scope

**Security Groups:**

- Used for assigning permissions and rights to multiple users.
- Simplifies management and reduces overhead.
- Users inherit permissions from the group.
- Eases adding/removing users without changing group's permissions.

**Distribution Groups:**

- Utilized by email applications like Microsoft Exchange.
- Functions like mailing lists.
- Enables auto-adding emails in "To" field in Outlook.
- Cannot be used for assigning permissions in domain environments.



There are three different `group scopes` that can be assigned when creating a new group.

1. Domain Local Group
2. Global Group
3. Universal Group




**Group Scopes:**

- Three types: Domain Local Group, Global Group, Universal Group.

**Domain Local Group:**

- Manages permissions to domain resources within creation domain.
- Cannot be used in other domains but CAN include users from other domains.
- Can be nested in other local groups, but NOT within global groups.

**Global Group:**

- Grants access to resources in another domain.
- Contains accounts only from the creation domain.
- Can be added to both other global groups and local groups.

**Universal Group:**

- Manages resources across multiple domains.
- Permissions to any object within the same forest.
- Available to all domains and can contain users from any domain.
- Stored in the Global Catalog (GC); changes trigger forest-wide replication.
- Recommended to maintain other groups (e.g., global) within universal groups.
- Global group membership less likely to change, only triggers domain-level replication.
- If individual users/computers are in universal groups, changes trigger forest-wide replication, leading to network overhead and potential issues.
- Critical groups in AD have specific scope settings (e.g., Enterprise and Schema admins vs. Domain admins).






#### AD Group Scope Examples

  AD Group Scope Examples

```powershell-session
PS C:\htb> Get-ADGroup  -Filter * |select samaccountname,groupscope
```

Group scopes can be changed, however:
- A global group can only be converted to a universal group if it is not part of another global gorup
- A domain local group can only be onverted to a universal group if the domain local group does not contain any other domain local groups as members
- A universal Group can be converted to a domain local group without any restrictions
- A universal group can only be converted to a global group if it does not contain any other universal groups as members







## Important Group Attributes
Like users, groups have many attributes. Some of the most important group attributes include:
- cn: the cn or Common-Name is the name of the group in Active Directory Domain services
- member: Which user, group and contact object are members of the group
- groupType: An integer that specifies the group type and scope
- memberOf: A listing of any groups that contain the group as a member (nested group membership)
-  `objectSid`: This is the security identifier or SID of the group, which is the unique value used to identify the group as a security principal.

What group type is best utilized for assigning permissions and rights to users?
Security
Security


True of False: A global group can only contain accounts from the domain where it was created?
True

Can a Universal group be converted to a Domain Local Group?
yes

# Built-in AD Groups
|Group Name|Description|
|---|---|
|`Account Operators`|Members can create and modify most types of accounts, including those of users, local groups, and global groups, and members can log in locally to domain controllers. They cannot manage the Administrator account, administrative user accounts, or members of the Administrators, Server Operators, Account Operators, Backup Operators, or Print Operators groups.|
|`Administrators`|Members have full and unrestricted access to a computer or an entire domain if they are in this group on a Domain Controller.|
|`Backup Operators`|Members can back up and restore all files on a computer, regardless of the permissions set on the files. Backup Operators can also log on to and shut down the computer. Members can log onto DCs locally and should be considered Domain Admins. They can make shadow copies of the SAM/NTDS database, which, if taken, can be used to extract credentials and other juicy info.|
|`DnsAdmins`|Members have access to network DNS information. The group will only be created if the DNS server role is or was at one time installed on a domain controller in the domain.|
|`Domain Admins`|Members have full access to administer the domain and are members of the local administrator's group on all domain-joined machines.|
|`Domain Computers`|Any computers created in the domain (aside from domain controllers) are added to this group.|
|`Domain Controllers`|Contains all DCs within a domain. New DCs are added to this group automatically.|
|`Domain Guests`|This group includes the domain's built-in Guest account. Members of this group have a domain profile created when signing onto a domain-joined computer as a local guest.|
|`Domain Users`|This group contains all user accounts in a domain. A new user account created in the domain is automatically added to this group.|
|`Enterprise Admins`|Membership in this group provides complete configuration access within the domain. The group only exists in the root domain of an AD forest. Members in this group are granted the ability to make forest-wide changes such as adding a child domain or creating a trust. The Administrator account for the forest root domain is the only member of this group by default.|
|`Event Log Readers`|Members can read event logs on local computers. The group is only created when a host is promoted to a domain controller.|
|`Group Policy Creator Owners`|Members create, edit, or delete Group Policy Objects in the domain.|
|`Hyper-V Administrators`|Members have complete and unrestricted access to all the features in Hyper-V. If there are virtual DCs in the domain, any virtualization admins, such as members of Hyper-V Administrators, should be considered Domain Admins.|
|`IIS_IUSRS`|This is a built-in group used by Internet Information Services (IIS), beginning with IIS 7.0.|
|`Pre–Windows 2000 Compatible Access`|This group exists for backward compatibility for computers running Windows NT 4.0 and earlier. Membership in this group is often a leftover legacy configuration. It can lead to flaws where anyone on the network can read information from AD without requiring a valid AD username and password.|
|`Print Operators`|Members can manage, create, share, and delete printers that are connected to domain controllers in the domain along with any printer objects in AD. Members are allowed to log on to DCs locally and may be used to load a malicious printer driver and escalate privileges within the domain.|
|`Protected Users`|Members of this [group](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#protected-users) are provided additional protections against credential theft and tactics such as Kerberos abuse.|
|`Read-only Domain Controllers`|Contains all Read-only domain controllers in the domain.|
|`Remote Desktop Users`|This group is used to grant users and groups permission to connect to a host via Remote Desktop (RDP). This group cannot be renamed, deleted, or moved.|
|`Remote Management Users`|This group can be used to grant users remote access to computers via [Windows Remote Management (WinRM)](https://docs.microsoft.com/en-us/windows/win32/winrm/portal)|
|`Schema Admins`|Members can modify the Active Directory schema, which is the way all objects with AD are defined. This group only exists in the root domain of an AD forest. The Administrator account for the forest root domain is the only member of this group by default.|
|`Server Operators`|This group only exists on domain controllers. Members can mod
ify services, access SMB shares, and backup files on domain controllers. By default, this group has no members.|



#### Server Operators Group Details

  Server Operators Group Details

```powershell-session
PS C:\htb>  Get-ADGroup -Identity "Server Operators" -Properties *
```

#### Domain Admins Group Membership

  Domain Admins Group Membership

```powershell-session
PS C:\htb>  Get-ADGroup -Identity "Domain Admins" -Properties * | select DistinguishedName,GroupCategory,GroupScope,Name,Members
```
https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment

https://github.com/FSecureLABS/SharpGPOAbuse


|**Privilege**|**Description**|
|---|---|
|`SeRemoteInteractiveLogonRight`|This privilege could give our target user the right to log onto a host via Remote Desktop (RDP), which could potentially be used to obtain sensitive data or escalate privileges.|
|`SeBackupPrivilege`|This grants a user the ability to create system backups and could be used to obtain copies of sensitive system files that can be used to retrieve passwords such as the SAM and SYSTEM Registry hives and the NTDS.dit Active Directory database file.|
|`SeDebugPrivilege`|This allows a user to debug and adjust the memory of a process. With this privilege, attackers could utilize a tool such as [Mimikatz](https://github.com/ParrotSec/mimikatz) to read the memory space of the Local System Authority (LSASS) process and obtain any credentials stored in memory.|
|`SeImpersonatePrivilege`|This privilege allows us to impersonate a token of a privileged account such as `NT AUTHORITY\SYSTEM`. This could be leveraged with a tool such as JuicyPotato, RogueWinRM, PrintSpoofer, etc., to escalate privileges on a target system.|
|`SeLoadDriverPrivilege`|A user with this privilege can load and unload device drivers that could potentially be used to escalate privileges or compromise a system.|
|`SeTakeOwnershipPrivilege`|This allows a process to take ownership of an object. At its most basic level, we could use this privilege to gain access to a file share or a file on a share that was otherwise not accessible to us.|
https://blog.palantir.com/windows-privilege-abuse-auditing-detection-and-defense-3078a403d74e
https://book.hacktricks.xyz/windows/windows-local-privilege-escalation/privilege-escalation-abusing-tokens

  
Domain Admin Rights Elevated

```powershell-session
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== ========
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeMachineAccountPrivilege                 Add workstations to domain                                         Disabled
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Disabled
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Disabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Disabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Disabled
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Disabled
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeEnableDelegationPrivilege               Enable computer and user accounts to be trusted for delegation     Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Disabled
SeTimeZonePrivilege                       Change the time zone                                               Disabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Disabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in
```

User rights increase based on the groups they are placed in or their assigned privileges. Below is an example of the rights granted to a `Backup Operators` group member. Users in this group have other rights currently restricted by UAC (additional rights such as the powerful `SeBackupPrivilege` are not enabled by default in a standard console session). Still, we can see from this command that they have the `SeShutdownPrivilege`, which means they can shut down a domain controller. This privilege on its own could not be used to gain access to sensitive data but could cause a massive service interruption should they log onto a domain controller locally (not remotely via RDP or WinRM).


 What built-in group will grant a user full and unrestricted access to a computer?
 Administrator

Waht user right grants a user the ability to make backups of a file system?
SeBackupPrivilege


+ 0  What Windows command can show us all user rights assigned to the current user?
+ whoami /priv


