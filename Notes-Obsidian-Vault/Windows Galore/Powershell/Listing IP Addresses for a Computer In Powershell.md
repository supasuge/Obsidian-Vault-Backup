## Table of Contents

- [Cheatsheet: Listing IP Addresses for a Computer in PowerShell](#cheatsheet:\listing\ip\addresses\for\a\computer\in\powershell)
  - [Displaying IP Configuration](#Displaying\IP\Configuration)
  - [Listing Specific IP Information](#Listing\Specific\IP\Information)
  - [Advanced IP Filtering](#Advanced\IP\Filtering)
  - [IP Address for a Specific Adapter](#IP\Address\for\a\Specific\Adapter)

Great, let's start with a cheatsheet for Listing IP Addresses for a Computer using PowerShell.

---

# Cheatsheet: Listing IP Addresses for a Computer in PowerShell

## Displaying IP Configuration
- **View all IP configuration details**:
  ```powershell
  Get-NetIPConfiguration
  ```
  This command provides comprehensive details, including IP address, subnet mask, default gateway, and DNS servers.

## Listing Specific IP Information
- **List IPv4 addresses**:
  ```powershell
  Get-NetIPAddress -AddressFamily IPv4 | Select-Object InterfaceAlias, IPAddress
  ```
- **List IPv6 addresses**:
  ```powershell
  Get-NetIPAddress -AddressFamily IPv6 | Select-Object InterfaceAlias, IPAddress
  ```

## Advanced IP Filtering
- **Filter IP addresses by specific interface**:
  ```powershell
  Get-NetIPAddress -InterfaceAlias "<InterfaceName>" | Select-Object IPAddress
  ```

## IP Address for a Specific Adapter
- **Display IP address for a specific network adapter**:
  ```powershell
  Get-NetAdapter -Name "<AdapterName>" | Get-NetIPAddress | Select-Object IPAddress
  ```

---
