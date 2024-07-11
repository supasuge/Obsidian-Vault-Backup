## Table of Contents

- [Advanced Cheatsheet for Malware Investigation](#advanced\cheatsheet\for\malware\investigation)
  - [Initial System Analysis](#Initial\System\Analysis)
  - [File System Analysis](#File\System\Analysis)
  - [Registry Analysis](#Registry\Analysis)
  - [Log Analysis](#Log\Analysis)
  - [Network Analysis](#Network\Analysis)
  - [Malware Signature Scanning](#Malware\Signature\Scanning)
  - [Memory Analysis](#Memory\Analysis)
  - [Advanced Forensic Tools](#Advanced\Forensic\Tools)
  - [Sandbox Analysis](#Sandbox\Analysis)
  - [Threat Intelligence](#Threat\Intelligence)
  - [Documentation](#Documentation)

As a Blue Team forensics analyst, your focus is on defending against and investigating cyber attacks, including identifying and analyzing malware. Below is a highly advanced cheatsheet for investigating systems for malware using various tools and techniques. This is intended for legal and authorized forensic analysis only.

# Advanced Cheatsheet for Malware Investigation

## Initial System Analysis
- **Check running processes**:
  ```powershell
  Get-Process
  ```
- **Look for suspicious network activity**:
  ```powershell
  Get-NetTCPConnection
  ```

## File System Analysis
- **Scan for recently modified files**:
  ```powershell
  Get-ChildItem -Path C:\ -Recurse | Where-Object { $_.LastWriteTime -gt (Get-Date).AddDays(-10) }
  ```
- **Identify hidden files**:
  ```powershell
  Get-ChildItem -Path C:\ -Recurse -Force | Where-Object { $_.Attributes -Match 'Hidden' }
  ```

## Registry Analysis
- **Check for unusual startup items**:
  ```powershell
  Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Run\*
  Get-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Run\*
  ```

## Log Analysis
- **Review Windows event logs for signs of intrusion**:
  ```powershell
  Get-WinEvent -LogName System -MaxEvents 1000
  Get-WinEvent -LogName Security -MaxEvents 1000
  ```

## Network Analysis
- **Capture and analyze network traffic** (requires additional tools like Wireshark):
  ```powershell
  netsh trace start capture=yes
  # Stop with `netsh trace stop` and analyze the resulting .etl file
  ```

## Malware Signature Scanning
- **Use antivirus software for scanning and identifying known malware**.

## Memory Analysis
- **Capture and analyze memory dump** (using tools like Volatility or Rekall):
  1. Capture memory dump.
  2. Analyze for anomalies, rogue processes, and suspicious memory patterns.

## Advanced Forensic Tools
- **Use specialized forensic tools**:
  - EnCase
  - FTK (Forensic Toolkit)
  - Autopsy

## Sandbox Analysis
- **Analyze suspicious files in a sandbox environment** (using services like VirusTotal, Any.Run).

## Threat Intelligence
- **Utilize threat intelligence platforms** for IOC (Indicators of Compromise) matching.

## Documentation
- **Maintain detailed logs of your findings** for legal and audit purposes.

---
