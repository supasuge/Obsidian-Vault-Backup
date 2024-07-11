## Table of Contents

- [Cheatsheet: Establishing a Persistent Connection with PowerShell Remoting](#cheatsheet:\establishing\a\persistent\connection\with\powershell\remoting)
  - [Creating Persistent Remote Sessions](#Creating\Persistent\Remote\Sessions)
  - [Using the Persistent Session](#Using\the\Persistent\Session)
  - [Managing the Session](#Managing\the\Session)
  - [Closing the Session](#Closing\the\Session)

Proceeding to the next topic, let's create a cheatsheet for Establishing a Persistent Connection using PowerShell Remoting.

---

# Cheatsheet: Establishing a Persistent Connection with PowerShell Remoting

## Creating Persistent Remote Sessions
- **Establish a persistent session**:
  ```powershell
  $session = New-PSSession -ComputerName <RemoteComputerName>
  ```
  This creates a session object that you can use to run multiple commands over time.

## Using the Persistent Session
- **Run a command in a persistent session**:
  ```powershell
  Invoke-Command -Session $session -ScriptBlock { <Command> }
  ```

- **Run multiple commands in a session**:
  ```powershell
  Invoke-Command -Session $session -ScriptBlock {
    <Command1>
    <Command2>
    # Add as many commands as needed
  }
  ```

## Managing the Session
- **Check the state of the session**:
  ```powershell
  $session | Get-PSSession
  ```

- **Disconnect from the session without ending it**:
  ```powershell
  Disconnect-PSSession -Session $session
  ```
  This allows the session to be reconnected later.

- **Reconnect to a disconnected session**:
  ```powershell
  Connect-PSSession -Session $session
  ```

## Closing the Session
- **End a persistent session**:
  ```powershell
  Remove-PSSession -Session $session
  ```
  Always remember to close sessions when they are no longer needed.

---
