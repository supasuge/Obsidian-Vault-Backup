## Table of Contents

- [Getting Remote Services in PowerShell](#getting\remote\services\in\powershell)
  - [Basic Remote Service Retrieval](#Basic\Remote\Service\Retrieval)
  - [Using PowerShell Remoting](#Using\PowerShell\Remoting)
  - [Filtering and Finding Remote Services](#Filtering\and\Finding\Remote\Services)
  - [Advanced Remote Service Management](#Advanced\Remote\Service\Management)
  - [Using Credential](#Using\Credential)

Now, let's move on to the topic of Getting Remote Services. Here's a cheatsheet for retrieving information about services on remote machines using PowerShell.

---

# Getting Remote Services in PowerShell

## Basic Remote Service Retrieval
- **Get services on a remote computer**: 
  ```powershell
  Get-Service -ComputerName <RemoteComputerName>
  ```
  *Note: The `-ComputerName` parameter is deprecated in PowerShell Core (6+). Use remote sessions instead.*

## Using PowerShell Remoting
- **Enter a Remote PowerShell Session**:
  ```powershell
  Enter-PSSession -ComputerName <RemoteComputerName>
  ```
  Then, you can run `Get-Service` as if you were on the remote machine.

- **Run a Command on a Remote Computer**:
  ```powershell
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { Get-Service }
  ```

## Filtering and Finding Remote Services
- **Find specific services on a remote computer**:
  ```powershell
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { Get-Service | Where-Object { $_.Name -like "*<part_of_name>*" } }
  ```

- **Get status of a specific service on remote machines**:
  ```powershell
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { Get-Service -Name "<ServiceName>" }
  ```

## Advanced Remote Service Management
- **Restart a service on a remote computer**:
  ```powershell
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { Restart-Service -Name "<ServiceName>" }
  ```

- **Change startup type of a remote service**:
  ```powershell
  Invoke-Command -ComputerName <RemoteComputerName> -ScriptBlock { Set-Service -Name "<ServiceName>" -StartupType Automatic }
  ```

## Using Credential
- **Run command with specific credentials**:
  ```powershell
  $cred = Get-Credential
  Invoke-Command -ComputerName <RemoteComputerName> -Credential $cred -ScriptBlock { Get-Service }
  ```

---

