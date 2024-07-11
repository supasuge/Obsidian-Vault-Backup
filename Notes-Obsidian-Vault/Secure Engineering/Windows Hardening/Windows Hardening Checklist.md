## Table of Contents

- [Windows Hardening Checklist](#windows\hardening\checklist)
  - [System Updates](#System\Updates)
  - [Data Encryption](#Data\Encryption)
  - [Application Security](#Application\Security)
  - [Malware Protection](#Malware\Protection)
  - [Office Software Security](#Office\Software\Security)
  - [Application Whitelisting](#Application\Whitelisting)
  - [Browser Security](#Browser\Security)
  - [Network Security](#Network\Security)
  - [DNS Security](#DNS\Security)
  - [ARP Security](#ARP\Security)
  - [Remote Access](#Remote\Access)
  - [Account Management](#Account\Management)
  - [User Account Control (UAC)](#User\Account\Control\(UAC))
  - [System Configuration and Management](#System\Configuration\and\Management)

# Windows Hardening Checklist

Enhancing the security of your Windows systems involves implementing a series of best practices to mitigate vulnerabilities and protect against threats. Below is a concise, easy-to-understand checklist to help ensure a more secure Windows environment for both enterprise and personal use.

## System Updates

- [ ] **Enable Windows Auto-Updates**: Navigate to `Start > Settings > Update & Security > Windows Updates` to ensure all critical security updates are automatically installed.

## Data Encryption

- [ ] **Utilize BitLocker for Drive Encryption**: Accessible via `Start > Control Panel > System and Security > BitLocker Drive Encryption`, BitLocker provides robust encryption to protect your data from unauthorized access. Ensure you store the recovery key securely.

## Application Security

- [ ] **Enable Windows Sandbox**: For safely running untrusted applications. Enable it through `Windows Features`, allowing software to run in an isolated environment. *Note: Requires virtualization enabled in BIOS/UEFI.*

- [ ] **Download Apps from Trusted Sources**: Only install applications from the Microsoft Store to avoid malicious software.
We can access Microsoft Application Store by typing `ms-windows-store:` in the Run dialogue.

![Image for Application Store](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/55413d1781c530e53fa175b3e2fadb84.png)
- [ ] **Restrict App Installation Source**: Set your system to allow installations only from the Microsoft Store via `Settings > Apps and Features > Choose where to get apps` then select `The Microsoft Store only`

## Malware Protection

- [ ] **Use Windows Defender Anti Virus**: Ensure real-time protection is enabled for continuous monitoring and protection against malware.
- **Real-time protection** - Enables periodic scanning of the computer.
- **Browser integration** - Enables safe browsing by scanning all downloaded files, etc.
- **Application Guard** - Allows complete web session sandboxing to block malicious websites or sessions to make changes in the computer.
- **Controlled Folder Access** - Protect memory areas and folders from unwanted applications.
## Office Software Security

- [ ] **Harden Microsoft Office**: Disable macros or use Microsoft's Attack Surface Reduction Rules to prevent exploitation via Office applications.

## Application Whitelisting

- [ ] **Configure AppLocker**: Use AppLocker to create rules that allow or deny applications from running, enhancing control over executable files on your system.

## Browser Security

- [ ] **Enable Microsoft SmartScreen in Edge**: Turn on SmartScreen in `Settings > Privacy, Search, and Services` for protection against phishing and malware.

- [ ] **Set Tracking Prevention to Strict**: In Microsoft Edge, adjust tracking prevention settings to 'Strict' to minimize tracking and enhance privacy.

## Network Security

- [ ] **Disable Unused Network Devices**: In `Device Manager`, disable network interfaces that are not in use to reduce attack surfaces.

- [ ] **Disable SMBv1 Protocol**: Use PowerShell to disable SMBv1 to protect against vulnerabilities associated with this file-sharing protocol.
```powershell
Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```
## DNS Security

- [ ] **Secure Local DNS**: Ensure the hosts file is protected and monitor for unauthorized changes that could redirect traffic to malicious sites.
The hosts file is located at `C:\Windows\System32\Drivers\etc\hosts`
## ARP Security

- [ ] **Monitor and Manage ARP Cache**: Regularly check the ARP table for anomalies and clear the cache when necessary to prevent ARP poisoning attacks.

## Remote Access

- [ ] **Disable Unnecessary Remote Access**: Turn off Remote Desktop Protocol (RDP) if it's not required, reducing the risk of remote exploits.

## Account Management

- [ ] **Use Standard Accounts for Daily Activities**: Limit administrative privileges to tasks that require them, reducing the risk of malware execution with elevated permissions.

- [ ] **Implement Strong Authentication**: Use Windows Hello or multi-factor authentication for secure access to your system.

## User Account Control (UAC)

- [ ] **Configure UAC to Always Notify**: Adjust UAC settings to always notify when applications try to make changes to your system, enhancing control over system changes.

## System Configuration and Management

- [ ] **Regularly Review Installed Services**: Use `services.msc` to manage and disable unnecessary services, limiting potential vectors for attack.

- [ ] **Secure Windows Registry and Event Logs**: Limit access to the Windows Registry and regularly review Event Viewer logs for signs of unauthorized access or anomalies.

Implementing these measures will significantly enhance the security posture of your Windows environment, protecting against a wide range of threats and vulnerabilities.