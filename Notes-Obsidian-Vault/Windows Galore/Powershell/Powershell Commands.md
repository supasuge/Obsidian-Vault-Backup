## Table of Contents

- [Computer Hardware and Environment Information](#computer\hardware\and\environment\information)
  - [System Overview](#System\Overview)
  - [CPU Information](#CPU\Information)
  - [Memory Information](#Memory\Information)
  - [Disk Information](#Disk\Information)
  - [Network Information](#Network\Information)
  - [Operating System Information](#Operating\System\Information)
  - [User Information](#User\Information)
  - [System Services](#System\Services)
  - [Hardware Components](#Hardware\Components)
  - [BIOS Information](#BIOS\Information)

#  Computer Hardware and Environment Information
## System Overview
- **Get General System Information**
  ```powershell
  Get-ComputerInfo
  ```

## CPU Information
- **Get Processor Details**
  ```powershell
  Get-WmiObject Win32_Processor
  ```

## Memory Information
- **Get Physical Memory (RAM) Details**
  ```powershell
  Get-WmiObject Win32_PhysicalMemory
  ```
- **Get Virtual Memory Information**
  ```powershell
  Systeminfo | findstr /C:"Virtual Memory"
  ```
## Disk Information
- **Get Hard Disk Details**
  ```powershell
  Get-WmiObject Win32_DiskDrive
  ```

- **Get Disk Partition Information**
  ```powershell
  Get-WmiObject Win32_DiskPartition
  ```

- **Get Logical Disk Information**
  ```powershell
  Get-WmiObject Win32_LogicalDisk
  ```

## Network Information

- **Get Network Adapter Details**
  ```powershell
  Get-WmiObject Win32_NetworkAdapter
  ```

- **Get Network Configuration**
  ```powershell
  Get-WmiObject Win32_NetworkAdapterConfiguration | Where-Object {$_.IPEnabled -eq "TRUE"}
  ```

## Operating System Information

- **Get OS Details**
  ```powershell
  Get-WmiObject Win32_OperatingSystem
  ```

- **Get OS Environment Variables**
  ```powershell
  Get-ChildItem Env:
  ```

## User Information

- **Get Current User Information**
  ```powershell
  [System.Security.Principal.WindowsIdentity]::GetCurrent()
  ```

- **Get All User Accounts**
  ```powershell
  Get-WmiObject Win32_UserAccount
  ```

## System Services

- **Get Running Services**
  ```powershell
  Get-Service | Where-Object {$_.Status -eq "Running"}
  ```

- **Get Specific Service Status**
  ```powershell
  Get-Service -Name "ServiceName"
  ```

## Hardware Components

- **Get Video Controller Information**
  ```powershell
  Get-WmiObject Win32_VideoController
  ```

- **Get Sound Device Information**
  ```powershell
  Get-WmiObject Win32_SoundDevice
  ```

- **Get Motherboard Information**
  ```powershell
  Get-WmiObject Win32_BaseBoard
  ```

## BIOS Information

- **Get BIOS Details**
  ```powershell
  Get-WmiObject Win32_BIOS
  ```

---

