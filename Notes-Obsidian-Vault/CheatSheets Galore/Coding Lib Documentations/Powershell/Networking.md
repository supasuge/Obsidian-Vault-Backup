## Table of Contents

- [Windows CLI Networking Configuration and System Enumeration Cheatsheet](#windows\cli\networking\configuration\and\system\enumeration\cheatsheet)
  - [Networking Configuration and Information Commands](#Networking\Configuration\and\Information\Commands)
    - [Basic Networking Commands](#Basic\Networking\Commands)
      - [Viewing Network Configuration](#Viewing\Network\Configuration)
      - [Changing IP Address and DNS Settings](#Changing\IP\Address\and\DNS\Settings)
      - [Displaying Route Table](#Displaying\Route\Table)
    - [Advanced Networking](#Advanced\Networking)
      - [Creating a Virtual Network Adapter](#Creating\a\Virtual\Network\Adapter)
      - [Monitoring Network Traffic](#Monitoring\Network\Traffic)
      - [Configuring Firewall](#Configuring\Firewall)
  - [Full System Enumeration](#Full\System\Enumeration)
    - [Basic System Information](#Basic\System\Information)
      - [Checking OS Version](#Checking\OS\Version)
      - [CPU and Memory Information](#CPU\and\Memory\Information)
    - [User and Group Enumeration](#User\and\Group\Enumeration)
      - [Listing Users](#Listing\Users)
      - [Listing Groups and Group Members](#Listing\Groups\and\Group\Members)
    - [Process and Service Enumeration](#Process\and\Service\Enumeration)
      - [Listing Running Processes](#Listing\Running\Processes)
      - [Listing Services and Their Status](#Listing\Services\and\Their\Status)
    - [Registry and Startup Items](#Registry\and\Startup\Items)
      - [Viewing Registry Keys](#Viewing\Registry\Keys)
      - [Checking Startup Programs](#Checking\Startup\Programs)
    - [Disk and Storage Enumeration](#Disk\and\Storage\Enumeration)
      - [Listing Disk Partitions](#Listing\Disk\Partitions)
      - [Viewing Mounted Drives](#Viewing\Mounted\Drives)
  - [Environment Variables and Their Meanings](#Environment\Variables\and\Their\Meanings)
- [Ultimate Windows Command Line (CLI) Cheatsheet](#ultimate\windows\command\line\(cli)\cheatsheet)
  - [Overview](#Overview)
  - [Basic Command Comparison](#Basic\Command\Comparison)
  - [File System Navigation](#File\System\Navigation)
  - [Advanced File Operations](#Advanced\File\Operations)
  - [System Information and Management](#System\Information\and\Management)
  - [Scripting and Automation](#Scripting\and\Automation)
  - [Network Operations](#Network\Operations)
  - [Miscellaneous Commands](#Miscellaneous\Commands)
  - [Environment Variables and Their Meanings](#Environment\Variables\and\Their\Meanings)

# Windows CLI Networking Configuration and System Enumeration Cheatsheet

---

## Networking Configuration and Information Commands

### Basic Networking Commands

#### Viewing Network Configuration
- **CMD:**
  ```cmd
  ipconfig /all
  ```
- **PowerShell:**
  ```powershell
  Get-NetIPConfiguration
  ```

#### Changing IP Address and DNS Settings
- **CMD:** (Static IP)
  ```cmd
  netsh interface ip set address "Local Area Connection" static 192.168.1.2 255.255.255.0 192.168.1.1 1
  ```
- **PowerShell:** (DHCP)
  ```powershell
  Set-NetIPInterface -InterfaceAlias "Ethernet0" -Dhcp Enabled
  ```

#### Displaying Route Table
- **CMD:**
  ```cmd
  route print
  ```
- **PowerShell:**
  ```powershell
  Get-NetRoute
  ```

### Advanced Networking

#### Creating a Virtual Network Adapter
- **PowerShell:**
  ```powershell
  New-NetSwitchTeam -Name "VirtualSwitch" -TeamMembers "Ethernet0", "Ethernet1"
  ```

#### Monitoring Network Traffic
- **PowerShell:**
  ```powershell
  Get-NetTCPConnection | Where-Object { $_.State -eq "Established" }
  ```

#### Configuring Firewall
- **PowerShell:**
  ```powershell
  New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow
  ```

---

## Full System Enumeration

### Basic System Information

#### Checking OS Version
- **CMD:**
  ```cmd
  ver
  ```
- **PowerShell:**
  ```powershell
  Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsHardwareAbstractionLayer
  ```

#### CPU and Memory Information
- **PowerShell:**
  ```powershell
  Get-WmiObject -Class Win32_Processor
  Get-WmiObject -Class Win32_PhysicalMemory
  ```

### User and Group Enumeration

#### Listing Users
- **CMD:**
  ```cmd
  net users
  ```
- **PowerShell:**
  ```powershell
  Get-LocalUser
  ```

#### Listing Groups and Group Members
- **CMD:**
  ```cmd
  net localgroup
  ```
- **PowerShell:**
  ```powershell
  Get-LocalGroup | ForEach-Object { Write-Output $_.Name; Get-LocalGroupMember -Group $_.Name }
  ```

### Process and Service Enumeration

#### Listing Running Processes
- **CMD:**
  ```cmd
  tasklist
  ```
- **PowerShell:**
  ```powershell
  Get-Process
  ```

#### Listing Services and Their Status
- **PowerShell:**
  ```powershell
  Get-Service
  ```

### Registry and Startup Items

#### Viewing Registry Keys
- **PowerShell:**
  ```powershell
  Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run'
  ```

#### Checking Startup Programs
- **PowerShell:**
  ```powershell
  Get-CimInstance Win32_StartupCommand | Select-Object Name, command, Location, User
  ```

### Disk and Storage Enumeration

#### Listing Disk Partitions
- **PowerShell:**
  ```powershell
  Get-Disk | Get-Partition
  ```

#### Viewing Mounted Drives
- **CMD:**
  ```cmd
  wmic logicaldisk get name
  ```
- **PowerShell:**
  ```powershell
  Get-PSDrive -PSProvider FileSystem
  ```

---
## Environment Variables and Their Meanings

- **%APPDATA%**: Path to the Application Data folder for the current user.
- **%COMSPEC%**: Path to the command interpreter (CMD.EXE).
- **%HOMEDRIVE%**: Primary local drive (usually `C:`).
- **%HOMEPATH%**: Path to the current user's home directory.
- **%LOCALAPPDATA%**: Path to local (non-roaming) application data.
- **%PATH%**: Directories included in the system's executable search path.
- **%PROGRAMFILES%**: Path to the Program Files directory.
- **%TEMP%** and **%TMP%**: Path to the temporary files directory.
- **%USERPROFILE%**: Path to the profile of the currently logged-in user.
- **%WINDIR%**: Path to the Windows directory.


## Basic Command Comparison

```plaintext
| Task                          | Bash (Linux)       | CMD (Windows)                | PowerShell (Windows)       |
| ----------------------------- | ------------------ | ---------------------------- | -------------------------- |
| List files                    | ls                 | dir                          | Get-ChildItem              |
| Change directory              | cd                 | cd                           | Set-Location               |
| Copy files                    | cp                 | copy                         | Copy-Item                  |
| Move/Rename files             | mv                 | move                         | Move-Item                  |
| Delete files                  | rm                 | del                          | Remove-Item                |
| View file content             | cat                | type                         | Get-Content                |
| Edit file in CLI              | nano, vi           | edit (Notepad)               | notepad, ise (PowerShell ISE) |
| Download file                 | wget, curl         | -                            | Invoke-WebRequest          |
| Print working directory       | pwd                | cd                           | Get-Location               |
| Display manual/help           | man, --help        | /?                           | Get-Help                   |
| File search                   | find, grep         | find, findstr                | Select-String              |
| Display environment variables | echo $VAR          | echo %VAR%                   | echo $env:VAR              |
| Set environment variable      | export VAR=value   | set VAR=value                | $env:VAR = "value"         |
| Execute a script              | ./script.sh        | script.bat or script.cmd     | .\script.ps1               |
```

---

## File System Navigation

- **Listing Drives:**
  - CMD: `wmic logicaldisk get name`
  - PowerShell: `Get-PSDrive`

- **Changing Drives:**
  - CMD & PowerShell: Type the drive letter followed by `:` (e.g., `D:`)

- **Current Directory:**
  - CMD: `echo %cd%`
  - PowerShell: `Write-Output $PWD`

- **Parent Directory:**
  - CMD & PowerShell: `cd ..`

---

## Advanced File Operations

- **Recursive Directory Listing:**
  - CMD: `dir /s`
  - PowerShell: `Get-ChildItem -Recurse`

- **Search for Files:**
  - CMD: `dir /s filename`
  - PowerShell: `Get-ChildItem -Recurse | Where-Object { $_.Name -like "*filename*" }`

- **Creating Symbolic Links:**
  - CMD: `mklink Link Target`
  - PowerShell: `New-Item -ItemType SymbolicLink -Path "Link" -Target "Target"`

---

## System Information and Management

- **Display System Info:**
  - CMD: `systeminfo`
  - PowerShell: `Get-ComputerInfo`

- **List Running Processes:**
  - CMD: `tasklist`
  - PowerShell: `Get-Process`

- **Terminate a Process:**
  - CMD: `taskkill /IM processname.exe`
  - PowerShell: `Stop-Process -Name "processname"`

- **View Network Configuration:**
  - CMD: `ipconfig /all`
  - PowerShell: `Get-NetIPConfiguration`

- **Disk Usage:**
  - CMD: `fsutil volume diskfree c:`
  - PowerShell: `Get-PSDrive C | Select-Object Used,Free`

---

## Scripting and Automation

- **Batch Scripting (.bat/.cmd):**
  - Basic scripting similar to Bash but with different syntax.
  - Supports loops, conditionals, and variable handling.

- **PowerShell Scripting (.ps1):**
  - Advanced scripting language akin to programming.
  - Access to .NET Framework, complex data structures, and robust error handling.

- **Running Scripts:**
  - CMD: `call script.bat`
  - PowerShell: `.\script.ps1` (after setting execution policy if needed)

---

## Network Operations

- **Ping Host:**
  - CMD & PowerShell: `ping hostname`

- **Traceroute:**
  - CMD: `tracert hostname`
  - PowerShell: `Test-NetConnection -ComputerName hostname -TraceRoute`

- **Display Open Network Connections:**
  - CMD: `netstat -an`
  - PowerShell: `Get-NetTCPConnection`

---

## Miscellaneous Commands

- **Environment Variables:**
  - List all: CMD: `set`, PowerShell: `Get-ChildItem Env:`

- **Clipboard Operations:**
  - CMD: Use `clip.exe` for output redirection
  - PowerShell: `Get-Clipboard`, `Set-Clipboard`

- **Searching Command History:**
  - CMD: Press F7
  - PowerShell: `Get-History`, Up arrow for history navigation

---

