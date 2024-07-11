## Table of Contents

- [Cheatsheet: Advanced Remoting in PowerShell](#cheatsheet:\advanced\remoting\in\powershell)
  - [Configuring Advanced Session Options](#Configuring\Advanced\Session\Options)
  - [Invoking Commands with Advanced Options](#Invoking\Commands\with\Advanced\Options)
  - [Using Sessions for Script Execution](#Using\Sessions\for\Script\Execution)
  - [Handling Errors in Remote Sessions](#Handling\Errors\in\Remote\Sessions)
  - [Working with Disconnected Sessions](#Working\with\Disconnected\Sessions)

# Cheatsheet: Advanced Remoting in PowerShell

## Configuring Advanced Session Options
- **Customize timeout for a session**:
  ```powershell
  $options = New-PSSessionOption -OperationTimeout (60 * 1000) # 60 seconds
  $session = New-PSSession -ComputerName <RemoteComputerName> -SessionOption $options
  ```

- **Create a session with a specific authentication method**:
  ```powershell
  $session = New-PSSession -ComputerName <RemoteComputerName> -Authentication CredSSP
  ```

## Invoking Commands with Advanced Options
- **Run a script block as a specific user**:
  ```powershell
  $cred = Get-Credential
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { <Command> } -Credential $cred
  ```

- **Run long-running scripts with no timeout**:
  ```powershell
  Invoke-Command -Session $session -ScriptBlock { <Command> } -AsJob
  ```

## Using Sessions for Script Execution
- **Execute a local script on a remote computer**:
  ```powershell
  $script = { C:\Path\To\Script.ps1 }
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock $script
  ```

## Handling Errors in Remote Sessions
- **Error handling in remote script blocks**:
  ```powershell
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock {
    try {
      <Command>
    } catch {
      Write-Error $_.Exception.Message
    }
  }
  ```

## Working with Disconnected Sessions
- **Start a session, run commands, and disconnect**:
  ```powershell
  $session = New-PSSession -ComputerName <RemoteComputerName>
  Invoke-Command -Session $session -ScriptBlock { <Command> } -AsJob
  Disconnect-PSSession -Session $session
  ```

- **Reconnect to a session and retrieve job results**:
  ```powershell
  Connect-PSSession -Session $session
  Receive-Job -Session $session
  ```

---

