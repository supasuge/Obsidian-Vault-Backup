## Table of Contents

- [Cheatsheet: Managing Services in PowerShell](#cheatsheet:\managing\services\in\powershell)
  - [Basic Service Management](#Basic\Service\Management)
  - [Advanced Service Management](#Advanced\Service\Management)
  - [Querying Specific Services](#Querying\Specific\Services)
  - [Service Dependencies](#Service\Dependencies)

Continuing to the next topic: Managing Services. Here's a concise cheatsheet for managing services using PowerShell.

---

# Cheatsheet: Managing Services in PowerShell

## Basic Service Management
- **List all services**: `Get-Service`
- **Start a service**: `Start-Service -Name "<ServiceName>"`
- **Stop a service**: `Stop-Service -Name "<ServiceName>" -Force`
- **Pause a service**: `Suspend-Service -Name "<ServiceName>"`
- **Resume a service**: `Resume-Service -Name "<ServiceName>"`
- **Restart a service**: `Restart-Service -Name "<ServiceName>"`

## Advanced Service Management
- **Change service startup type**:
  ```powershell
  Set-Service -Name "<ServiceName>" -StartupType Automatic
  ```
  - Startup Types: `Automatic`, `Manual`, `Disabled`
- **Check the status of a service**:
  ```powershell
  Get-Service -Name "<ServiceName>" | Select Status, Name, DisplayName
  ```
- **Create a new service** (Advanced and should be used carefully):
  ```powershell
  New-Service -Name "<ServiceName>" -BinaryPathName "<PathToExecutable>" -DisplayName "<DisplayName>" -StartupType Manual
  ```
- **Delete a service**:
  ```powershell
  Remove-Service -Name "<ServiceName>"
  ```

## Querying Specific Services
- **Find a service by display name**:
  ```powershell
  Get-Service | Where-Object { $_.DisplayName -like "*<part_of_display_name>*" }
  ```
- **List services in a specific state** (e.g., Running, Stopped):
  ```powershell
  Get-Service | Where-Object { $_.Status -eq 'Running' }
  ```

## Service Dependencies
- **List service dependencies**:
  ```powershell
  Get-Service -Name "<ServiceName>" | Select-Object -ExpandProperty DependentServices
  ```
- **List services that a specific service depends on**:
  ```powershell
  Get-Service -Name "<ServiceName>" | Select-Object -ExpandProperty ServicesDependedOn
  ```

---

This cheatsheet provides a range of PowerShell commands for managing Windows services. Managing services is a common task for system administrators, involving starting, stopping, and configuring services as needed. Always ensure to understand the implications of these commands, especially when creating or removing services, as they can have significant effects on system operations.



