## Table of Contents

- [Cheatsheet: WDAC Policy Enforcement in PowerShell](#cheatsheet:\wdac\policy\enforcement\in\powershell)
  - [Enforcing WDAC Policies](#Enforcing\WDAC\Policies)
  - [Switching WDAC Policy Modes](#Switching\WDAC\Policy\Modes)
  - [Updating WDAC Policies](#Updating\WDAC\Policies)
  - [Verifying WDAC Policy Application](#Verifying\WDAC\Policy\Application)

Continuing with WDAC Policy Enforcement using PowerShell:

---

# Cheatsheet: WDAC Policy Enforcement in PowerShell

## Enforcing WDAC Policies
- **Apply a WDAC policy**:
  ```powershell
  Set-CIPolicyIdInfo -FilePath "C:\Path\To\Policy.xml" -PolicyId <GUID>
  ConvertFrom-CIPolicy -XmlFilePath "C:\Path\To\Policy.xml" -BinaryFilePath "C:\Path\To\Policy.bin"
  Copy-Item -Path "C:\Path\To\Policy.bin" -Destination "C:\Windows\System32\CodeIntegrity\CIPolicies\Active\{<GUID>}.cip"
  ```

## Switching WDAC Policy Modes
- **Switch to Enforcement mode** (disallows non-whitelisted apps):
  ```powershell
  Set-RuleOption -FilePath "C:\Path\To\Policy.xml" -Option 3 -Delete # Remove UMCI Audit Mode
  ```
- **Switch to Audit mode** (logs non-whitelisted app execution but doesn't block):
  ```powershell
  Set-RuleOption -FilePath "C:\Path\To\Policy.xml" -Option 6 # Enable Audit Mode
  ```

## Updating WDAC Policies
- **Update a WDAC policy in place**:
  ```powershell
  Update-CIPolicy -FilePath "C:\Path\To\Policy.xml" -ScanPath "C:\Path\To\ExeOrFolder" -UserPEs
  ```

## Verifying WDAC Policy Application
- **Check the status of WDAC policy application**:
  ```powershell
  Get-CIPolicy -FilePath "C:\Path\To\Policy.xml" -ReportFilePath "C:\Path\To\Report.xml"
  ```

---
