## Table of Contents

- [Ultimate Windows Command Line (CLI) Cheatsheet](#ultimate\windows\command\line\(cli)\cheatsheet)
  - [Overview](#Overview)
  - [Basic Command Comparison](#Basic\Command\Comparison)
  - [File System Navigation](#File\System\Navigation)
  - [Advanced File Operations](#Advanced\File\Operations)
  - [System Information and Management](#System\Information\and\Management)
  - [Scripting and Automation](#Scripting\and\Automation)
  - [Network Operations](#Network\Operations)
  - [Miscellaneous Commands](#Miscellaneous\Commands)

# Ultimate Windows Command Line (CLI) Cheatsheet

**Title:** Windows CLI for Bash Users  
**Date:** 2024-01-31  
**Categories:** Command Line, Windows, Bash Transition  
**Tags:** Windows CLI, CMD, PowerShell, Bash Comparison

---

## Overview

Transitioning from Bash (Linux) to the Windows Command Line (including CMD and PowerShell) can be challenging. This cheatsheet is designed to help you navigate the Windows Command Line Interface (CLI), drawing parallels to Bash when possible.

---

## Basic Command Comparison

| Task | Bash (Linux) | CMD (Windows) | PowerShell (Windows) |
| ---- | ------------ | ------------- | -------------------- |
| List files | `ls` | `dir` | `Get-ChildItem` |
| Change directory | `cd` | `cd` | `Set-Location` |
| Copy files | `cp` | `copy` | `Copy-Item` |
| Move/Rename files | `mv` | `move` | `Move-Item` |
| Delete files | `rm` | `del` | `Remove-Item` |
| View file content | `cat` | `type` | `Get-Content` |
| Edit file in CLI | `nano`, `vi` | `edit` (Notepad) | `notepad`, `ise` (PowerShell ISE) |
| Download file | `wget`, `curl` | - | `Invoke-WebRequest` |
| Print working directory | `pwd` | `cd` | `Get-Location` |
| Display manual/help | `man`, `--help` | `/?` | `Get-Help` |
| File search | `find`, `grep` | `find`, `findstr` | `Select-String` |
| Display environment variables | `echo $VAR` | `echo %VAR%` | `echo $env:VAR` |
| Set environment variable | `export VAR=value` | `set VAR=value` | `$env:VAR = "value"` |
| Execute a script | `./script.sh` | `script.bat` or `script.cmd` | `.\script.ps1` |

---

## File System Navigation

- **Listing Drives:** 
  - CMD: `wmic logicaldisk get name`
  - PowerShell: `Get-PSDrive`

- **Changing Drives:** 
  - CMD & PowerShell: Simply type the drive letter followed by `:` (e.g., `D:`)

- **Current Directory:** 
  - CMD: `echo %cd%`
  - PowerShell: `Write-Output $PWD`

- **Parent Directory:** 
  - CMD & PowerShell: `cd ..`

---

## Advanced File Operations

- **Recursive Directory Listing:**
  - CMD: `dir /s`
  - PowerShell: `Get-ChildItem -Recurse`

- **Search for Files:**
  - CMD: `dir /s filename`
  - PowerShell: `Get-ChildItem -Recurse | Where-Object { $_.Name -like "*filename*" }`

- **Creating Symbolic Links:**
  - CMD: `mklink Link Target`
  - PowerShell: `New-Item -ItemType SymbolicLink -Path "Link" -Target "Target"`

---

## System Information and Management

- **Display System Info:**
  - CMD: `systeminfo`
  - PowerShell: `Get-ComputerInfo`

- **List Running Processes:**
  - CMD: `tasklist`
  - PowerShell: `Get-Process`

- **Terminate a Process:**
  - CMD: `taskkill /IM processname.exe`
  - PowerShell: `Stop-Process -Name "processname"`

- **View Network Configuration:**
  - CMD: `ipconfig /all`
  - PowerShell: `Get-NetIPConfiguration`

- **Disk Usage:**
  - CMD: `fsutil volume diskfree c:`
  - PowerShell: `Get-PSDrive C | Select-Object Used,Free`

---

## Scripting and Automation

- **Batch Scripting (.bat/.cmd):**
  - Basic scripting similar to Bash but with a different syntax.
  - Loops, conditionals, and variable handling are available but more limited.

- **PowerShell Scripting (.ps1):**
  - Advanced scripting language, more akin to programming than shell scripting.
  - Access to .NET Framework, complex data structures, and robust error handling.

- **Running Scripts:**
  - CMD: `call script.bat`
  - PowerShell: `.\script.ps1` (after setting execution policy if needed)

---

## Network Operations

- **Ping Host:**
  - CMD & PowerShell: `ping hostname`

- **Traceroute:**
  - CMD: `tracert hostname`
  - PowerShell: `Test-NetConnection -ComputerName hostname -TraceRoute`

- **Display Open Network Connections:**
  - CMD: `netstat -an`
  - PowerShell: `Get-NetTCPConnection`

---

## Miscellaneous Commands

- **Environment Variables:**
  - List all: CMD: `set`, PowerShell: `Get-ChildItem Env:`

- **Clipboard Operations:**
  - CMD: No native support (use clip.exe for output redirection)
  - PowerShell: `Get-Clipboard`, `Set-Clipboard`

- **Searching Command History:**
  - CMD: Press F7
  - PowerShell: `Get-History`, Up arrow for history navigation

---
