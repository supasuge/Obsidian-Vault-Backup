## Table of Contents

- [Cheatsheet: Using Windows Defender Application Control (WDAC) in PowerShell](#cheatsheet:\using\windows\defender\application\control\(wdac)\in\powershell)
  - [Introduction to WDAC](#Introduction\to\WDAC)
  - [Setting Up WDAC Policies](#Setting\Up\WDAC\Policies)
  - [Deploying WDAC Policies](#Deploying\WDAC\Policies)
  - [Managing WDAC Policies](#Managing\WDAC\Policies)
  - [Auditing WDAC Policies](#Auditing\WDAC\Policies)

# Cheatsheet: Using Windows Defender Application Control (WDAC) in PowerShell

## Introduction to WDAC
Windows Defender Application Control (WDAC) helps you control which applications and drivers can run on a Windows system. It's a security feature that can be managed using PowerShell.

## Setting Up WDAC Policies
- **Create a new WDAC policy**:
  ```powershell
  New-CIPolicy -Level PcaCertificate -FilePath "C:\Path\To\Policy.xml"
  ```
  This creates a new policy file based on the PCA certificate level.

- **Convert XML policy to binary format**:
  ```powershell
  ConvertFrom-CIPolicy -XmlFilePath "C:\Path\To\Policy.xml" -BinaryFilePath "C:\Path\To\Policy.bin"
  ```

## Deploying WDAC Policies
- **Deploy a WDAC policy**:
  ```powershell
  Set-RuleOption -FilePath "C:\Path\To\Policy.xml" -Option 3 # Enable UMCI
  Set-CIPolicyIdInfo -FilePath "C:\Path\To\Policy.xml" -PolicyId <GUID>
  ConvertFrom-CIPolicy -XmlFilePath "C:\Path\To\Policy.xml" -BinaryFilePath "C:\Path\To\Policy.bin"
  Copy-Item -Path "C:\Path\To\Policy.bin" -Destination "C:\Windows\System32\CodeIntegrity\CIPolicies\Active\{<GUID>}.cip"
  ```

## Managing WDAC Policies
- **Merge multiple WDAC policies**:
  ```powershell
  Merge-CIPolicy -OutputFilePath "C:\Path\To\MergedPolicy.xml" -PolicyPaths "C:\Path\To\Policy1.xml", "C:\Path\To\Policy2.xml"
  ```

- **Update and replace an existing WDAC policy**:
  ```powershell
  Set-CIPolicyIdInfo -FilePath "C:\Path\To\UpdatedPolicy.xml" -PolicyId <GUID>
  ConvertFrom-CIPolicy -XmlFilePath "C:\Path\To\UpdatedPolicy.xml" -BinaryFilePath "C:\Path\To\UpdatedPolicy.bin"
  Copy-Item -Path "C:\Path\To\UpdatedPolicy.bin" -Destination "C:\Windows\System32\CodeIntegrity\CIPolicies\Active\{<GUID>}.cip"
  ```

## Auditing WDAC Policies
- **Enable WDAC policy auditing**:
  ```powershell
  Set-RuleOption -FilePath "C:\Path\To\Policy.xml" -Option 6 -Delete # Enable Audit Mode
  ```

---
