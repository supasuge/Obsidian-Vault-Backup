## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Checking the Status of Defender with Get-MpComputerStatus](#Checking\the\Status\of\Defender\with\Get-MpComputerStatus)
      - [Using Get-AppLockerPolicy cmdlet](#Using\Get-AppLockerPolicy\cmdlet)
  - [PowerShell Constrained Language Mode](#PowerShell\Constrained\Language\Mode)
      - [Enumerating Language Mode](#Enumerating\Language\Mode)
      - [Using Find-LAPSDelegatedGroups](#Using\Find-LAPSDelegatedGroups)
      - [Using Find-LAPSDelegatedGroups](#Using\Find-LAPSDelegatedGroups)
      - [Using Find-AdmPwdExtendedRights](#Using\Find-AdmPwdExtendedRights)
      - [Using Get-LAPSComputers](#Using\Get-LAPSComputers)
- [Linux Credentialed Enumeration](#linux\credentialed\enumeration)
  - [CrackMapExec](#CrackMapExec)
      - [CME Options (SMB)](#CME\Options\(SMB))
      - [CME - Domain User Enumeration](#CME\-\Domain\User\Enumeration)
      - [CME - Domain Group Enumeration](#CME\-\Domain\Group\Enumeration)

## Table of Contents

      - [Checking the Status of Defender with Get-MpComputerStatus](#Checking\the\Status\of\Defender\with\Get-MpComputerStatus)
      - [Using Get-AppLockerPolicy cmdlet](#Using\Get-AppLockerPolicy\cmdlet)
  - [PowerShell Constrained Language Mode](#PowerShell\Constrained\Language\Mode)
      - [Enumerating Language Mode](#Enumerating\Language\Mode)
      - [Using Find-LAPSDelegatedGroups](#Using\Find-LAPSDelegatedGroups)
      - [Using Find-LAPSDelegatedGroups](#Using\Find-LAPSDelegatedGroups)
      - [Using Find-AdmPwdExtendedRights](#Using\Find-AdmPwdExtendedRights)
      - [Using Get-LAPSComputers](#Using\Get-LAPSComputers)
- [Linux Credentialed Enumeration](#linux\credentialed\enumeration)
  - [CrackMapExec](#CrackMapExec)
      - [CME Options (SMB)](#CME\Options\(SMB))
      - [CME - Domain User Enumeration](#CME\-\Domain\User\Enumeration)
      - [CME - Domain Group Enumeration](#CME\-\Domain\Group\Enumeration)



cmdlet [Get-MpComputerStatus](https://docs.microsoft.com/en-us/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) to get the current Defender status. Here, we can see that the `RealTimeProtectionEnabled` parameter is set to `True`, which means Defender is enabled on the system.

#### Checking the Status of Defender with Get-MpComputerStatus

  Checking the Status of Defender with Get-MpComputerStatus

```powershell
PS C:\htb> Get-MpComputerStatus
```


 Organizations also often focus on blocking the `PowerShell.exe` executable, but forget about the other [PowerShell executable locations](https://www.powershelladmin.com/wiki/PowerShell_Executables_File_System_Locations) such as `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe` or `PowerShell_ISE.exe`. We can see that this is the case in the `AppLocker` rules shown below. All Domain Users are disallowed from running the 64-bit PowerShell executable located at:

`%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe`

#### Using Get-AppLockerPolicy cmdlet

  Using Get-AppLockerPolicy cmdlet

```powershell
PS C:\htb> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

 We can use the built-in PowerShell cmdlet [Get-MpComputerStatus](https://docs.microsoft.com/en-us/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) to get the current Defender status. Here, we can see that the `RealTimeProtectionEnabled` parameter is set to `True`, which means Defender is enabled on the system.
  
Checking the Status of Defender with Get-MpComputerStatus

```powershell
PS C:\htb> Get-MpComputerStatus
```

## PowerShell Constrained Language Mode

PowerShell [Constrained Language Mode](https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/) locks down many of the features needed to use PowerShell effectively, such as blocking COM objects, only allowing approved .NET types, XAML-based workflows, PowerShell classes, and more. We can quickly enumerate whether we are in Full Language Mode or Constrained Language Mode.

#### Enumerating Language Mode

  Enumerating Language Mode

```powershell
PS C:\htb> $ExecutionContext.SessionState.LanguageMode
```

The Microsoft [Local Administrator Password Solution (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) is used to randomize and rotate local administrator passwords on Windows hosts and prevent lateral movement. We can enumerate what domain users can read the LAPS password set for machines with LAPS installed and what machines do not have LAPS installed. The [LAPSToolkit](https://github.com/leoloobeek/LAPSToolkit) greatly facilitates this with several functions. One is parsing `ExtendedRights` for all computers with LAPS enabled. This will show groups specifically delegated to read LAPS passwords, which are often users in protected groups. An account that has joined a computer to a domain receives `All Extended Rights` over that host, and this right gives the account the ability to read passwords. Enumeration may show a user account that can read the LAPS password on a host. This can help us target specific AD users who can read LAPS passwords.

#### Using Find-LAPSDelegatedGroups
#### Using Find-LAPSDelegatedGroups

  Using Find-LAPSDelegatedGroups

```powershell
PS C:\htb> Find-LAPSDelegatedGroups
```

The `Find-AdmPwdExtendedRights` checks the rights on each computer with LAPS enabled for any groups with read access and users with "All Extended Rights." Users with "All Extended Rights" can read LAPS passwords and may be less protected than users in delegated groups, so this is worth checking for.

#### Using Find-AdmPwdExtendedRights

  Using Find-AdmPwdExtendedRights

```powershell
PS C:\htb> Find-AdmPwdExtendedRights
```





We can use the `Get-LAPSComputers` function to search for computers that have LAPS enabled when passwords expire, and even the randomized passwords in cleartext if our user has access.

#### Using Get-LAPSComputers

  Using Get-LAPSComputers

```powershell-session
PS C:\htb> Get-LAPSComputers
```




# Linux Credentialed Enumeration

## CrackMapExec

```shell
optional arguments:
  -h, --help            show this help message and exit
  -t THREADS            set how many concurrent threads to use (default: 100)
  --timeout TIMEOUT     max timeout in seconds of each thread (default: None)
  --jitter INTERVAL     sets a random delay between each connection (default: None)
  --darrell             give Darrell a hand
  --verbose             enable verbose output

protocols:
  available protocols

  {mssql,smb,ssh,winrm}
    mssql               own stuff using MSSQL
    smb                 own stuff using SMB
    ssh                 own stuff using SSH
    winrm               own stuff using WINRM

Ya feelin' a bit buggy all of a sudden?
```



#### CME Options (SMB)

  CME Options (SMB)

```shell
gdxqpardo@htb[/htb]$ crackmapexec smb -h

positional arguments:
  target                the target IP(s), range(s), CIDR(s), hostname(s), FQDN(s), file(s) containing a list of targets, NMap XML or
                        .Nessus file(s)

optional arguments:
  -h, --help            show this help message and exit
  -id CRED_ID [CRED_ID ...]
                        database credential ID(s) to use for authentication
  -u USERNAME [USERNAME ...]
                        username(s) or file(s) containing usernames
  -p PASSWORD [PASSWORD ...]
                        password(s) or file(s) containing passwords
  -k, --kerberos        Use Kerberos authentication from ccache file (KRB5CCNAME)

```

CME offers a help menu for each protocol (i.e., `crackmapexec winrm -h`, etc.). Be sure to review the entire help menu and all possible options. For now, the flags we are interested in are:

- -u Username `The user whose credentials we will use to authenticate`
- -p Password `User's password`
- Target (IP or FQDN) `Target host to enumerate` (in our case, the Domain Controller)
- --users `Specifies to enumerate Domain Users`
- --groups `Specifies to enumerate domain groups`
- --loggedon-users `Attempts to enumerate what users are logged on to a target, if any`



#### CME - Domain User Enumeration

We start by pointing CME at the Domain Controller and using the credentials for the `forend` user to retrieve a list of all domain users. Notice when it provides us the user information, it includes data points such as the [badPwdCount](https://docs.microsoft.com/en-us/windows/win32/adschema/a-badpwdcount) attribute. This is helpful when performing actions like targeted password spraying. We could build a target user list filtering out any users with their `badPwdCount` attribute above 0 to be extra careful not to lock any accounts out.

  CME - Domain User Enumeration

```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```

We can also obtain a complete listing of domain groups. We should save all of our output to files to easily access it again later for reporting or use with other tools.

#### CME - Domain Group Enumeration

  CME - Domain Group Enumeration

10.129.164.166```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```


The above snippet lists the groups within the domain and the number of users in each. The output also shows the built-in groups on the Domain Controller, such as `Backup Operators`. We can begin to note down groups of interest. Take note of key groups like `Administrators`, `Domain Admins`, `Executives`, any groups that may contain privileged IT admins, etc. These groups will likely contain users with elevated privileges worth targeting during our assessment.












