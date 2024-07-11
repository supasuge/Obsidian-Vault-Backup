## Table of Contents

- [Cheatsheet: Windows PowerShell Remoting](#cheatsheet:\windows\powershell\remoting)
  - [Enabling PowerShell Remoting](#Enabling\PowerShell\Remoting)
  - [Creating Remote Sessions](#Creating\Remote\Sessions)
  - [Managing Remote Sessions](#Managing\Remote\Sessions)
  - [Using Sessions for Multiple Computers](#Using\Sessions\for\Multiple\Computers)
  - [Configuring Session Options](#Configuring\Session\Options)

Continuing with the next topic, let's create a cheatsheet for Windows PowerShell Remoting.

---

# Cheatsheet: Windows PowerShell Remoting

## Enabling PowerShell Remoting
- **Enable PowerShell Remoting on a local computer**:
  ```powershell
  Enable-PSRemoting -Force
  ```
  This command configures the computer to receive PowerShell remote commands.

## Creating Remote Sessions
- **Start a remote session**:
  ```powershell
  Enter-PSSession -ComputerName <RemoteComputerName>
  ```
  This opens an interactive session with the remote computer.

- **Start a remote session with specific credentials**:
  ```powershell
  $cred = Get-Credential
  Enter-PSSession -ComputerName <RemoteComputerName> -Credential $cred
  ```

## Managing Remote Sessions
- **Create a persistent remote session**:
  ```powershell
  $session = New-PSSession -ComputerName <RemoteComputerName>
  ```
- **Run commands in a persistent session**:
  ```powershell
  Invoke-Command -Session $session -ScriptBlock { <Command> }
  ```

- **End a remote session**:
  ```powershell
  Remove-PSSession -Session $session
  ```

## Using Sessions for Multiple Computers
- **Run a command on multiple computers using a session**:
  ```powershell
  $sessions = New-PSSession -ComputerName "Computer01", "Computer02"
  Invoke-Command -Session $sessions -ScriptBlock { <Command> }
  Remove-PSSession -Session $sessions
  ```

## Configuring Session Options
- **Create a session with customized options**:
  ```powershell
  $options = New-PSSessionOption -NoMachineProfile
  $session = New-PSSession -ComputerName <RemoteComputerName> -SessionOption $options
  ```

---
