## Table of Contents

- [Running Remote Commands in PowerShell](#running\remote\commands\in\powershell)
  - [Using Invoke-Command for Remote Execution](#Using\Invoke-Command\for\Remote\Execution)
  - [Running Commands with Different Credentials](#Running\Commands\with\Different\Credentials)
  - [Script Blocks and Variables](#Script\Blocks\and\Variables)
  - [Using Sessions for Persistent Connections](#Using\Sessions\for\Persistent\Connections)


---
# Running Remote Commands in PowerShell

## Using Invoke-Command for Remote Execution
- **Run a command on a remote computer**:
  ```powershell
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { <Command> }
  ```
  *Example: `Invoke-Command -ComputerName "Server01" -ScriptBlock { Get-Process }`*

- **Run a command on multiple remote computers**:
  ```powershell
  $computers = "Computer01", "Computer02"
  Invoke-Command -ComputerName $computers -ScriptBlock { <Command> }
  ```

## Running Commands with Different Credentials
- **Run a command with specific credentials**:
  ```powershell
  $cred = Get-Credential
  Invoke-Command -ComputerName <RemoteComputerName> -Credential $cred -ScriptBlock { <Command> }
  ```

## Script Blocks and Variables
- **Passing local variables to a remote script block**:
  ```powershell
  $var = "Value"
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { Param($var) <Command> } -ArgumentList $var
  ```

## Using Sessions for Persistent Connections
- **Create a remote session**:
  ```powershell
  $session = New-PSSession -ComputerName <RemoteComputerName>
  ```
- **Execute a command in the remote session**:
  ```powershell
  Invoke-Command -Session $session -ScriptBlock { <Command> }
  ```
- **Close the remote session**:
  ```powershell
  Remove-PSSession -Session $session
  ```

---
