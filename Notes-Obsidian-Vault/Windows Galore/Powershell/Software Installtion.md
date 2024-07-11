## Table of Contents

- [Cheatsheet: Working with Software Installations in PowerShell](#cheatsheet:\working\with\software\installations\in\powershell)
  - [Basic Software Installation and Removal](#Basic\Software\Installation\and\Removal)
  - [Using MSI (Microsoft Installer)](#Using\MSI\(Microsoft\Installer))
  - [Advanced Software Management](#Advanced\Software\Management)

Continuing with the next topic, we'll focus on Working with Software Installations using PowerShell.

---

# Cheatsheet: Working with Software Installations in PowerShell

## Basic Software Installation and Removal
- **Install a software package**:
  ```powershell
  Start-Process -FilePath "InstallerFilePath.exe" -ArgumentList "/S" -Wait -PassThru
  ```
  *`/S` is a common silent install switch; check the specific installer for correct parameters.*

- **Uninstall a software package**:
  ```powershell
  Start-Process -FilePath "UninstallerFilePath.exe" -ArgumentList "/S" -Wait -PassThru
  ```

## Using MSI (Microsoft Installer)
- **Install an MSI package**:
  ```powershell
  msiexec /i "PathToMSIFile.msi" /qn
  ```
  `/qn` denotes a silent install.

- **Uninstall an MSI package**:
  ```powershell
  msiexec /x "PathToMSIFile.msi" /qn
  ```

## Advanced Software Management
- **List installed programs**:
  ```powershell
  Get-WmiObject -Class Win32_Product | Select-Object Name, Version
  ```
  *Note: This method is often slow and can trigger a reconfiguration of installed software.*

- **Querying specific software**:
  ```powershell
  Get-WmiObject -Class Win32_Product | Where-Object { $_.Name -like "*<SoftwareName>*" }
  ```

---
