## Table of Contents

- [Getting Services Information in PowerShell](#getting\services\information\in\powershell)
  - [Basic Service Retrieval](#Basic\Service\Retrieval)
  - [Filtering and Finding Services](#Filtering\and\Finding\Services)
  - [Detailed Service Information](#Detailed\Service\Information)
  - [Services on Remote Computers](#Services\on\Remote\Computers)

# Getting Services Information in PowerShell

## Basic Service Retrieval
- **List all services**: `Get-Service`
- **Get a specific service by name**: `Get-Service -Name "<ServiceName>"`
- **List services with their status**: `Get-Service | Select-Object Status, Name, DisplayName`

## Filtering and Finding Services
- **Find services by status** (Running, Stopped, etc.):
  ```powershell
  Get-Service | Where-Object { $_.Status -eq 'Running' }
  ```
- **Search services by part of their name**:
  ```powershell
  Get-Service | Where-Object { $_.Name -like "*<part_of_name>*" }
  ```
- **List services based on display name**:
  ```powershell
  Get-Service | Where-Object { $_.DisplayName -like "*<part_of_display_name>*" }
  ```

## Detailed Service Information
- **Display detailed information about a service**:
  ```powershell
  Get-Service -Name "<ServiceName>" | Format-List *
  ```
- **Show service dependencies**:
  ```powershell
  Get-Service -Name "<ServiceName>" | Select-Object -ExpandProperty DependentServices
  ```
- **Show services that depend on a specific service**:
  ```powershell
  Get-Service -Name "<ServiceName>" | Select-Object -ExpandProperty ServicesDependedOn
  ```

## Services on Remote Computers
- **Get services on a remote computer**:
  ```powershell
  Get-Service -ComputerName <RemoteComputerName>
  ```
  Note: The `-ComputerName` parameter is deprecated in PowerShell Core (6+), and using remote PowerShell sessions or cmdlets like `Invoke-Command` is recommended.

---
