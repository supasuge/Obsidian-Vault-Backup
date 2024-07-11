## Table of Contents

- [Host & Network Reconnaissance](#host\&\network\reconnaissance)
  - [Advanced Security & Audit Commands](#Advanced\Security\&\Audit\Commands)
    - [Security Descriptors and Auditing](#Security\Descriptors\and\Auditing)
    - [Managing Credentials](#Managing\Credentials)
    - [Event Log Management](#Event\Log\Management)
  - [Deeper Network Insights](#Deeper\Network\Insights)
    - [Advanced IP Configuration](#Advanced\IP\Configuration)
    - [Advanced Port and Connection Analysis](#Advanced\Port\and\Connection\Analysis)
  - [Scripting and Automation](#Scripting\and\Automation)
    - [PowerShell Script Execution](#PowerShell\Script\Execution)
    - [Batch File Commands](#Batch\File\Commands)
  - [Remote Administration and Management](#Remote\Administration\and\Management)
    - [Remote System Interaction](#Remote\System\Interaction)
    - [Active Directory Management](#Active\Directory\Management)
  - [System Monitoring and Performance Analysis](#System\Monitoring\and\Performance\Analysis)
    - [System Performance and Health](#System\Performance\and\Health)
    - [Resource Usage and Analysis](#Resource\Usage\and\Analysis)

# Host & Network Reconnaissance 

## Advanced Security & Audit Commands

### Security Descriptors and Auditing
- `Get-Acl -Path C:\ | Format-List` - Retrieves the Access Control List (ACL) for the C: drive.
- `Auditpol /get /category:*` - Displays the current audit policy.

### Managing Credentials
- `Get-Credential` - Prompts for user credentials and creates a credential object.
- `ConvertTo-SecureString -String "PlainTextPassword" -AsPlainText -Force` - Converts a plain text string to a secure string.

### Event Log Management
- `Get-EventLog -LogName System -Newest 50` - Retrieves the 50 most recent entries from the System event log.
- `Clear-EventLog -LogName Application` - Clears the Application event log.

## Deeper Network Insights

### Advanced IP Configuration
- `netsh interface ipv4 show subinterfaces` - Shows the MTU, speed, and other properties of network interfaces.
- `netsh wlan show profiles` - Lists all wireless network profiles stored on the computer.

### Advanced Port and Connection Analysis
- `Test-Connection -ComputerName <hostname> -Count 5 -Delay 2` - Pings a host 5 times with a 2-second delay between each ping.
- `tnc <hostname> -port <portnumber>` - Tests TCP connectivity to a specific port.

## Scripting and Automation

### PowerShell Script Execution
- `.\script.ps1` - Executes a PowerShell script in the current directory.
- `$variable = Get-Service` - Assigns the output of `Get-Service` to a variable.

### Batch File Commands
- `@echo off` - Hides the command being executed in a batch file.
- `start <program>` - Starts a program or command in a separate window.

## Remote Administration and Management

### Remote System Interaction
- `Enter-PSSession -ComputerName <hostname> -Credential (Get-Credential)` - Starts an interactive session with a remote computer with credentials.
- `Invoke-Command -ComputerName <hostname> -ScriptBlock { Get-EventLog -LogName Application } -Credential (Get-Credential)` - Executes a command on a remote computer using credentials.

### Active Directory Management
- `Get-ADComputer -Filter 'Name -like "*Server*"'` - Finds AD computer objects with "Server" in their names.
- `Set-ADUser -Identity <username> -PasswordNeverExpires $true` - Sets a user's password to never expire.

## System Monitoring and Performance Analysis

### System Performance and Health
- `Get-Counter -Counter "\Processor(_Total)\% Processor Time"` - Measures the total CPU usage.
- `Get-Process | Where-Object { $_.CPU -gt 1000 }` - Lists processes using more than 1000 milliseconds of CPU time.

### Resource Usage and Analysis
- `Get-Process | Sort-Object -Property WS -Descending | Select-Object -First 10` - Lists the top 10 processes by memory usage.
- `Get-WmiObject Win32_PerfFormattedData_PerfOS_Memory | Select-Object AvailableMBytes` - Shows available memory in MB.
