## Table of Contents

- [Cheatsheet: Querying the Uninstall Registry Key in PowerShell](#cheatsheet:\querying\the\uninstall\registry\key\in\powershell)
  - [Accessing the Uninstall Registry](#Accessing\the\Uninstall\Registry)
  - [Filtering and Searching Installed Software](#Filtering\and\Searching\Installed\Software)
  - [Advanced Queries](#Advanced\Queries)

Next, let's cover Querying the Uninstall Registry Key to Find Installed Software using PowerShell.

---

# Cheatsheet: Querying the Uninstall Registry Key in PowerShell

## Accessing the Uninstall Registry
- **List all installed programs from the registry**:
  ```powershell
  Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate
  ```
  This is for 64-bit systems. For 32-bit systems, use `HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*`.

## Filtering and Searching Installed Software
- **Find a specific installed program**:
  ```powershell
  Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Where-Object { $_.DisplayName -like "*<SoftwareName>*" }
  ```

- **List software installed after a specific date**:
  ```powershell
  $date = "2024-01-01"
  Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Where-Object { $_.InstallDate -gt $date }
  ```

## Advanced Queries
- **List software by a specific publisher**:
  ```powershell
  Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Where-Object { $_.Publisher -eq "<PublisherName>" }
  ```

- **Export list to a CSV file**:
  ```powershell
  Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, Publisher, InstallDate | Export-Csv -Path "installed_programs.csv"
  ```

---
