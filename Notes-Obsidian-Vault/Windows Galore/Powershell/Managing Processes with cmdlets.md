## Table of Contents

- [Cheatsheet: Managing Processes with Process Cmdlets in PowerShell](#cheatsheet:\managing\processes\with\process\cmdlets\in\powershell)
  - [Basic Process Cmdlets](#Basic\Process\Cmdlets)
  - [Advanced Process Management](#Advanced\Process\Management)
  - [Process Information](#Process\Information)
  - [Managing Process Priority](#Managing\Process\Priority)
  - [Working with Process Threads](#Working\with\Process\Threads)

Great, let's proceed to the next topic: Managing Processes with Process Cmdlets. Here's the cheatsheet for that.

---

# Cheatsheet: Managing Processes with Process Cmdlets in PowerShell

## Basic Process Cmdlets
- **Get a list of processes**: `Get-Process`
- **Start a new process**: `Start-Process -FilePath "<Path to Executable>"`
- **Stop a process**: `Stop-Process -Name "<ProcessName>" -Force`
- **Get a specific process by name**: `Get-Process -Name "<ProcessName>"`

## Advanced Process Management
- **Find processes using more resources than usual**:
  ```powershell
  Get-Process | Where-Object { $_.CPU -gt 1000 -or $_.WorkingSet -gt 100MB }
  ```
- **Sort processes by memory usage**:
  ```powershell
  Get-Process | Sort-Object WorkingSet -Descending
  ```
- **Send a process to background**:
  ```powershell
  Start-Process -FilePath "<Path to Executable>" -WindowStyle Hidden
  ```
- **Create a process with administrative privileges**:
  ```powershell
  Start-Process -FilePath "<Path to Executable>" -Verb RunAs
  ```

## Process Information
- **Display detailed information about a process**:
  ```powershell
  Get-Process -Name "<ProcessName>" | Format-List *
  ```
- **Get the main window title of processes**:
  ```powershell
  Get-Process | Where-Object { $_.MainWindowTitle -ne "" } | Select-Object Name, ID, MainWindowTitle
  ```

## Managing Process Priority
- **Change process priority**:
  ```powershell
  (Get-Process -Name "<ProcessName>").PriorityClass = "<PriorityLevel>"
  ```
  - Priority Levels: `Normal`, `Idle`, `High`, `RealTime`, `BelowNormal`, `AboveNormal`

## Working with Process Threads
- **List threads of a process**:
  ```powershell
  (Get-Process -Name "<ProcessName>").Threads
  ```

---

This cheatsheet covers basic to advanced commands for managing processes using PowerShell. It's essential to use these commands with caution, especially when terminating processes or changing process priorities, as these actions can affect system stability. These commands are powerful tools for both routine management and troubleshooting of processes on Windows systems.