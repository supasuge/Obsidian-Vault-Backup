#windowsprivilegeescalation #windowsenumeration
###### Objectives During Enumeration
- System information
- Environment Variables
- Configuration Information
- Patches and Updates
- Installed Programs
- Running Services
- Running Processes
- User & Group Information
- User/Group Privileges
- Password Policy and Other account information
- [Windows Command Reference](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)

| **Sysem Configuration**                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The highly privileged `NT AUTHORITY\SYSTEM` account, or [LocalSystem](https://docs.microsoft.com/en-us/windows/win32/services/localsystem-account) account which is a highly privileged account with more privileges than a local administrator account and is used to run most Windows services. |
| The built-in local `administrator` account. Some organizations disable this account, but many do not. It is not uncommon to see this account reused across multiple systems in a client environment.                                                                                              |
| Another local account that is a member of the local `Administrators` group. Any account in this group will have the same privileges as the built-in `administrator` account.                                                                                                                      |
| A standard (non-privileged) domain user who is part of the local `Administrators` group.                                                                                                                                                                                                          |
| A domain admin (highly privileged in the Active Directory environment) that is part of the local `Administrators` group.                                                                                                                                                                          |

Enumeration is the key to privilege escalation. When we have initial access, it's vital to gain situational awareness and uncover details relating to the OS version, patch level, installed software, current privileges, group memberships, and more. 


### Key Data Points
- `OS Version` - Knowing the type of Windows OS will give us an idea of the types of tools that may be available. Such as the powershell version, and lack thereof on legacy systems.
- `Version` - As with the OS Version, there may be publicy available exploits that can cause system instability or even a complete crash. 
- `Running Services` - Knowing what services are on the host is important, especially those running under `NT AUTHORITY\SYSTEM` or an administrator-level account. A misconfigured or vulnerable services runnin in the context of a privileged account can lead to futher privilege escalation.

## System Information

Looking at the system itself will give us a better idea of the exact `operating system version`, `hardware in use`, `installed programs`, and `security updates`. This will help us narrow down our hunt for any missing patches and associated CVEs that we may be able to leverage to escalate privileges. Using the [tasklist](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tasklist) command to look at running processes will give us a better idea of what applications are currently running on the system.

Knowing what OS version, hardware, sec updates etc will help us narrow down potential paths of attack. 
### Tasklist
- Look at running process to see what applications are currently running on the system
```cmd-session
C:\htb> tasklist /svc
```

It is essential to become familiar with standard Windows processes such as [Session Manager Subsystem (smss.exe)](https://en.wikipedia.org/wiki/Session_Manager_Subsystem), [Client Server Runtime Subsystem (csrss.exe)](https://en.wikipedia.org/wiki/Client/Server_Runtime_Subsystem), [WinLogon (winlogon.exe)](https://en.wikipedia.org/wiki/Winlogon), [Local Security Authority Subsystem Service (LSASS)](https://en.wikipedia.org/wiki/Local_Security_Authority_Subsystem_Service), and [Service Host (svchost.exe)](https://en.wikipedia.org/wiki/Svchost.exe), among others and the services associated with them. Being able to spot standard processes/services quickly will help speed up our enumeration and enable us to hone in on non-standard processes/services, which may open up a privilege escalation path.

Other processes such as `MsMpEng.exe`, Windows Defender, are interesting because they can help us map out what protections are in place on the target host that we may have to evade/bypass.

### Display All Environment Variables
- Explains a lot about the host configuration.
`set` command is used to printout all environment variables. If the folder placed in the PATH is writable by your user, it may be possible to perform DLL injections against other applications. When running a program, windows looks for that program in the Current Working Directory first, then from the PATH going left to right. This means if the custom path is placed on the left (before C:\Windows\System32) its much more dangerous.

`set` can also give helpful information including the HOME DRIVE. Often enough, this will be a file share. Navigating to the file share itself may reveal other directories that can be accessed. 

Additionally, shares are utilized for home directories so the user can log on to other computers and have the same experience/files/desktop/etc. ([Roaming Profiles](https://docs.microsoft.com/en-us/windows-server/storage/folder-redirection/folder-redirection-rup-overview)). This may also mean the user takes malicious items with them. If a file is placed in `USERPROFILE\AppData\Microsoft\Windows\Start Menu\Programs\Startup`, when the user logs into a different machine, this file will execute.

```cmd
C:\htb> set # This does not work as is in powershell
```


### Detailed Configuration Information
the `systeminfo` command will show if the box has been patched recently and if it is a VM. If the box hasn't been getting patched, getting admin access may be as simple as running a known exploit. Google the KBs installed under [HotFixes](https://www.catalog.update.microsoft.com/Search.aspx?q=hotfix) to get an idea of when the box has been patched. This information isn't always present, as it is possible to hide hotfixes software from non-administrators. The `System Boot Time` and `OS Version` can also be checked to get an idea of the patch level. If the box has not been restarted in over six months, chances are it is also not being patched.

```cmd
C:\htb> systeminfo
```

#### Patches and Updates
if `sysinfo` doesn't displaay hotfixes, they may be queriable with `WMI` using the `WMI`-Command binary with [QFE (Quick Fix Engineering)](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-quickfixengineering) to display patches.

```cmd-session
C:\htb> wmic qfe
```

We can do this with PowerShell as well using the [Get-Hotfix](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-hotfix?view=powershell-7.1) cmdlet.

```powershell-session
PS C:\htb> Get-HotFix | ft -AutoSize
```

#### Installed Programs
```powershell
wmic product get name
```

Of course, this can also be done using the `Get-WmiObject` cmdlet.

```powershell
Get-WmiObject -Class Win32_Product | select name, Version
```


### Display Running Processes.
`netstat` command will display active TCP/UDP connections and will give us a better idea of what services are listening on which port(s) both locally and accessible to the outside. Vulnerable services can be used to exploit privileges.

#### Netstat

```cmd-session
PS C:\htb> netstat -ano
```
- View running process, network ports in use/addresses etc.

#### Find the name of a process
```powershell
Get-Process -Id <ID_HERE> | Select-Object -ExpandProperty ProcessName
```

### User & Group Information
Users are often the weakest link in an organization, especially when systems are configured and patched well. It's essential to gain an understanding of the users and groups on the system, members of specific groups that can provide us with admin level access, the privileges the current user has, password policy info, and any logged on users that we may be able to target.

#### Logged-In Users
It's always important to determine what users are logged into a system, Are they idle? Active? Can we determine what they are working on? 

While more challenging to pull off, we can sometimes attack users directly to escalate privileges or gain further access. During an evasive engagement, we would need to tread lightly on a host with other user(s) actively working on it to avoid detection.

```cmd-session
C:\htb> query user
```

#### Current User
Always check what user context our account is running under first. Sometimes, we are already SYSTEM or equivalent. 
Suppose we gain access as a service account. In that case, we may have privileges such as `SeImpersonatePrivilege`, which can often be easily abused to escalate privileges using a tool such as [Juicy Potato](https://github.com/ohpe/juicy-potato).

```cmd-session
C:\htb> echo %USERNAME%
```
#### Current User Privileges

As mentioned prior, knowing what privileges our user has can greatly help in escalating privileges. We will look at individual user privileges and escalation paths later in this module.

```cmd-session
C:\htb> whoami /priv
```

#### Current User Group Information

Has our user inherited any rights through their group membership? Are they privileged in the Active Directory domain environment, which could be leveraged to gain access to more systems?

```cmd-session
C:\htb> whoami /groups
```

#### Get All Users

Knowing what other users are on the system is important as well. If we gained RDP access to a host using credentials we captured for a user `bob`, and see a `bob_adm` user in the local administrators group, it is worth checking for credential re-use. Can we access the user profile directory for any important users? We may find valuable files such as scripts with passwords or SSH keys in a user's Desktop, Documents, or Downloads folder.

```cmd-session
C:\htb> net user
```

#### Get All Groups

Knowing what non-standard groups are present on the host can help us determine what the host is used for, how heavily accessed it is, or may even lead to discovering a misconfiguration such as all Domain Users in the Remote Desktop or local administrators groups.

```cmd-session
C:\htb> net localgroup
```

#### Details About a Group

It is worth checking out the details for any non-standard groups. Though unlikely, we may find a password or other interesting information stored in the group's description. During our enumeration, we may discover credentials of another non-admin user who is a member of a local group that can be leveraged to escalate privileges.

```cmd-session
C:\htb> net localgroup administrators
```

#### Get Password Policy & Other Account Information
```powershell
net accounts
```

What non-default privilege does the htb-student user have?
> `whoami /priv`
> `SeTakeOwnershipPrivilege`

Who is a member of the Backup Operators group?
> `sarah`
> `net localgroup "Backup Operators"`


What service is listening on port 8080? 
> `tomcat8`
> `netstat -ano | findstr :8080 //get pid`
> `Get-Process -Id | Select-Object | -ExpandProperty ProcessName`


What user is logged in to the target host?
> `sccm_svc`
> `query user`


What type of session does this user have?
> `console`