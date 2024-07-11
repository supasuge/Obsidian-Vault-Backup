## Table of Contents

    - [LAN Manager Hash](#LAN\Manager\Hash)
      - [SMB Signing](#SMB\Signing)
      - [LDAP Signing](#LDAP\Signing)
- [Notesheet: SMB and LDAP Signing for Enhanced Security](#notesheet:\smb\and\ldap\signing\for\enhanced\security)
  - [1. SMB (Server Message Block) Signing](#1.\SMB\(Server\Message\Block)\Signing)
    - [Purpose:](#Purpose:)
    - [Enabling SMB Signing via Group Policy:](#Enabling\SMB\Signing\via\Group\Policy:)
    - [Command Line Alternative:](#Command\Line\Alternative:)
    - [Importance:](#Importance:)
  - [2. LDAP (Lightweight Directory Access Protocol) Signing](#2.\LDAP\(Lightweight\Directory\Access\Protocol)\Signing)
    - [Purpose:](#Purpose:)
    - [Enabling LDAP Signing via Group Policy:](#Enabling\LDAP\Signing\via\Group\Policy:)
    - [Command Line Alternative:](#Command\Line\Alternative:)
    - [Importance:](#Importance:)
  - [General Advice for Command Line Enthusiasts:](#General\Advice\for\Command\Line\Enthusiasts:)



Topics:
Secure Communications
Data Integrity


`GroupPolicyMangementEditor` is a tool for configuring various security policies.

### LAN Manager Hash
The user account password for Windows ins't stored in clear text, its sstored with two types of hashes.
**If the password for any user account is changed or set with fewer than 15 characters, both LM hash (LAN Manager hash) and NT Hash are used and stored in AD**


#### SMB Signing
	Server Message Block
- Utilized for file and print communication. 
- Allows secure transmission over the network. 
- SMB Signing is crucial to prevent MITM attacks
```
Group Policy Management Editor >
Computer Configuration > 
Policies > 
Windows Settings > 
Security Settings > 
Local Policies > 
Security Options > 
#double click Microsoft network server:
Digitally sign communication (always) >
select Enable Digitally Sign Communications
```

#### LDAP Signing
LDAP Enables locating and authenticating resources on the network. Exploited via replay attacks, authentication, MITM, or custom LDAP requests. LDAP signing is a simple Authentication and Security Layer (SASL) propery that only accepts signed LDAP requests and ignores other requests.
```powershell

Group Policy Management Editor > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options > Domain controller: LDAP server signing requirements > select Require signing from the dropdown
```


# Notesheet: SMB and LDAP Signing for Enhanced Security

## 1. SMB (Server Message Block) Signing

### Purpose:

- **Secure File and Print Communication:** Prevents unauthorized tampering of data during transit and defends against MITM attacks.

### Enabling SMB Signing via Group Policy:
- **Path:** `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options`
- **Setting:** `Microsoft network server: Digitally sign communication (always)`
- **Action:** Set to `Enable Digitally Sign Communications`
___
### Command Line Alternative:
- **PowerShell Cmdlet:** `Set-GPRegistryValue`
- **Example:**
```powershell
Set-GPRegistryValue -Name "YourGPOName" -Key "HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters" -ValueName "RequireSecuritySignature" -Type DWORD -Value 2
```
___
### Importance:

- **Data Integrity:** Ensures data is authenticated and unaltered in transit.
- **Attack Mitigation:** Protects against data tampering and malicious code injection during file transfers.

---

## 2. LDAP (Lightweight Directory Access Protocol) Signing

### Purpose:

- **Secure Authentication:** Protects against replay attacks, MITM attacks, and unauthorized LDAP requests.

### Enabling LDAP Signing via Group Policy:

- **Path:** `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options`
- **Setting:** `Domain controller: LDAP server signing requirements`
- **Action:** Set to `Require signing`

### Command Line Alternative:

- **PowerShell Cmdlet:** `Set-GPRegistryValue`
- **Example:**
    
    powershell
    

- ```
```powershell
Set-GPRegistryValue -Name "YourGPOName" -Key "HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters" -ValueName "LDAPServerIntegrity" -Type DWORD -Value 2
```
### Importance:
- **Authentication Security:** Ensures the authenticity and integrity of LDAP requests.
- **Prevent Unauthorized Access:** Thwarts LDAP-based attacks and unauthorized access attempts.

---

## General Advice for Command Line Enthusiasts:

- **Regularly Update Group Policies:** Use scripts or scheduled tasks to keep policies in sync.
- **Monitor Security Logs:** Utilize tools like `Event Viewer` or `Get-EventLog` in PowerShell to track security events related to SMB and LDAP activities.
- **Automation:** Consider automating security checks and policy updates using batch scripts or PowerShell scripts.

---