## Table of Contents

- [PowerShell Superuser Commands Cheatsheet](#powershell\superuser\commands\cheatsheet)
  - [System Administration](#System\Administration)
  - [User and Group Management](#User\and\Group\Management)
  - [Process Management](#Process\Management)
  - [Network Commands](#Network\Commands)
  - [Disk Management](#Disk\Management)
  - [Registry Operations](#Registry\Operations)
  - [Event Logs](#Event\Logs)

Absolutely! Here's a continuation of the cheatsheet focusing on important Windows superuser commands in PowerShell. This section includes system administration, process management, network commands, and more.

---

# PowerShell Superuser Commands Cheatsheet

## System Administration
- **View system information**: `Get-SystemInfo`
- **List installed programs**: `Get-WmiObject -Class Win32_Product`
- **Start a service**: `Start-Service <service_name>`
- **Stop a service**: `Stop-Service <service_name>`
- **Restart a service**: `Restart-Service <service_name>`
- **Get service status**: `Get-Service <service_name>`

## User and Group Management
- **List users**: `Get-LocalUser`
- **Create a new user**: `New-LocalUser -Name <user_name>`
- **Remove a user**: `Remove-LocalUser -Name <user_name>`
- **Add user to a group**: `Add-LocalGroupMember -Group <group_name> -Member <user_name>`
- **List groups**: `Get-LocalGroup`
- **Create a new group**: `New-LocalGroup -Name <group_name>`
- **Remove a group**: `Remove-LocalGroup -Name <group_name>`

## Process Management
- **List running processes**: `Get-Process`
- **Start a process**: `Start-Process <process_name>`
- **Stop a process**: `Stop-Process -Name <process_name>`
- **Stop a process by ID**: `Stop-Process -Id <process_id>`

## Network Commands
- **Display network configuration**: `Get-NetIPConfiguration`
- **List network adapters**: `Get-NetAdapter`
- **Ping a host**: `Test-Connection <host_name>`
- **Trace route**: `Test-NetConnection <host_name> -TraceRoute`
- **Resolve DNS name**: `Resolve-DnsName <host_name>`

## Disk Management
- **List disks**: `Get-Disk`
- **Initialize a disk**: `Initialize-Disk -Number <disk_number>`
- **Format a drive**: `Format-Volume -DriveLetter <drive_letter>`
- **Check disk for errors**: `Repair-Volume -DriveLetter <drive_letter>`

## Registry Operations
- **Read a registry value**: `Get-ItemPropertyValue -Path 'Registry::<path>' -Name <name>`
- **Set a registry value**: `Set-ItemProperty -Path 'Registry::<path>' -Name <name> -Value <value>`
- **Create a registry key**: `New-Item -Path 'Registry::<path>'`
- **Remove a registry key**: `Remove-Item -Path 'Registry::<path>'`

## Event Logs
- **List event logs**: `Get-EventLog -List`
- **Read an event log**: `Get-EventLog -LogName <log_name>`
- **Clear an event log**: `Clear-EventLog -LogName <log_name>`

---

