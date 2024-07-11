## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Password Spray Visualization](#Password\Spray\Visualization)
- [Enumerating & Retrieving Password Policies](#enumerating\&\retrieving\password\policies)
  - [Enumerating the Password Policy - from Linux - Credentialed](#Enumerating\the\Password\Policy\-\from\Linux\-\Credentialed)
      - [Using rpcclient](#Using\rpcclient)
      - [Using enum4linux](#Using\enum4linux)
      - [Using enum4linux-ng](#Using\enum4linux-ng)
  - [Enumerating Null Session - from Windows](#Enumerating\Null\Session\-\from\Windows)
      - [Establish a null session from windows](#Establish\a\null\session\from\windows)
      - [Error: Account is Disabled](#Error:\Account\is\Disabled)
      - [Error: Password is Incorrect](#Error:\Password\is\Incorrect)
      - [Error: Account is locked out (Password Policy)](#Error:\Account\is\locked\out\(Password\Policy))
  - [Enumerating the Password Policy - from Linux - LDAP Anonymous Bind](#Enumerating\the\Password\Policy\-\from\Linux\-\LDAP\Anonymous\Bind)
      - [Using ldapsearch](#Using\ldapsearch)
  - [Enumerating the Password Policy - from Windows](#Enumerating\the\Password\Policy\-\from\Windows)
      - [Using net.exe](#Using\net.exe)
      - [Using PowerView](#Using\PowerView)

## Table of Contents

      - [Password Spray Visualization](#Password\Spray\Visualization)
- [Enumerating & Retrieving Password Policies](#enumerating\&\retrieving\password\policies)
  - [Enumerating the Password Policy - from Linux - Credentialed](#Enumerating\the\Password\Policy\-\from\Linux\-\Credentialed)
      - [Using rpcclient](#Using\rpcclient)
      - [Using enum4linux](#Using\enum4linux)
      - [Using enum4linux-ng](#Using\enum4linux-ng)
  - [Enumerating Null Session - from Windows](#Enumerating\Null\Session\-\from\Windows)
      - [Establish a null session from windows](#Establish\a\null\session\from\windows)
      - [Error: Account is Disabled](#Error:\Account\is\Disabled)
      - [Error: Password is Incorrect](#Error:\Password\is\Incorrect)
      - [Error: Account is locked out (Password Policy)](#Error:\Account\is\locked\out\(Password\Policy))
  - [Enumerating the Password Policy - from Linux - LDAP Anonymous Bind](#Enumerating\the\Password\Policy\-\from\Linux\-\LDAP\Anonymous\Bind)
      - [Using ldapsearch](#Using\ldapsearch)
  - [Enumerating the Password Policy - from Windows](#Enumerating\the\Password\Policy\-\from\Windows)
      - [Using net.exe](#Using\net.exe)
      - [Using PowerView](#Using\PowerView)


#### Password Spray Visualization

|**Attack**|**Username**|**Password**|
|---|---|---|
|1|bob.smith@inlanefreight.local|Welcome1|
|1|john.doe@inlanefreight.local|Welcome1|
|1|jane.doe@inlanefreight.local|Welcome1|
|DELAY|||
|2|bob.smith@inlanefreight.local|Passw0rd|
|2|john.doe@inlanefreight.local|Passw0rd|
|2|jane.doe@inlanefreight.local|Passw0rd|
|DELAY|||
|3|bob.smith@inlanefreight.local|Winter2022|
|3|john.doe@inlanefreight.local|Winter2022|
|3|jane.doe@inlanefreight.local|Winter2022|

# Enumerating & Retrieving Password Policies

---

## Enumerating the Password Policy - from Linux - Credentialed

As stated in the previous section, we can pull the domain password policy in several ways, depending on how the domain is configured and whether or not we have valid domain credentials. With valid domain credentials, the password policy can also be obtained remotely using tools such as [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) or `rpcclient`.

```shell
gdxqpardo@htb[/htb]$ crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```

When creating a domain in earlier versions of Windows Server, anonymous access was granted to certain shares, which allowed for domain enumeration. An SMB NULL session can be enumerated easily. For enumeration, we can use tools such as `enum4linux`, `CrackMapExec`, `rpcclient`, etc.

We can use [rpcclient](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) to check a Domain Controller for SMB NULL session access.

Once connected, we can issue an RPC command such as `querydominfo` to obtain information about the domain and confirm NULL session access.


#### Using rpcclient

  Using rpcclient

```shell
gdxqpardo@htb[/htb]$ rpcclient -U "" -N 172.16.5.5
```

```shell
rpcclient $> querydominfo
```

Let's try this using [enum4linux](https://labs.portcullis.co.uk/tools/enum4linux). `enum4linux` is a tool built around the [Samba suite of tools](https://www.samba.org/samba/docs/current/man-html/samba.7.html) `nmblookup`, `net`, `rpcclient` and `smbclient` to use for enumeration of windows hosts and domains. It can be found pre-installed on many different penetration testing distros, including Parrot Security Linux. Below we have an example output displaying information that can be provided by `enum4linux`. Here are some common enumeration tools and the ports they use:

|Tool|Ports|
|---|---|
|nmblookup|137/UDP|
|nbtstat|137/UDP|
|net|139/TCP, 135/TCP, TCP and UDP 135 and 49152-65535|
|rpcclient|135/TCP|
|smbclient|445/TCP|


#### Using enum4linux

  Using enum4linux

```shell
gdxqpardo@htb[/htb]$ enum4linux -P 172.16.5.5
```

#### Using enum4linux-ng

  Using enum4linux-ng

```shell
gdxqpardo@htb[/htb]$ enum4linux-ng -P 172.16.5.5 -oA ilfreight
```

## Enumerating Null Session - from Windows

It is less common to do this type of null session attack from Windows, but we could use the command `net use \\host\ipc$ "" /u:""` to establish a null session from a windows machine and confirm if we can perform more of this type of attack.

#### Establish a null session from windows

  Establish a null session from windows

```cmd
C:\htb> net use \\DC01\ipc$ "" /u:""
The command completed successfully.
```

We can also use a username/password combination to attempt to connect. Let's see some common errors when trying to authenticate:

#### Error: Account is Disabled

  Error: Account is Disabled

```cmd
C:\htb> net use \\DC01\ipc$ "" /u:guest
System error 1331 has occurred.

This user can't sign in because this account is currently disabled.
```


#### Error: Password is Incorrect

  Error: Password is Incorrect

```cmd-session
C:\htb> net use \\DC01\ipc$ "password" /u:guest
System error 1326 has occurred.

The user name or password is incorrect.
```

#### Error: Account is locked out (Password Policy)

  Error: Account is locked out (Password Policy)

```cmd-session
C:\htb> net use \\DC01\ipc$ "password" /u:guest
System error 1909 has occurred.

The referenced account is currently locked out and may not be logged on to.
```

## Enumerating the Password Policy - from Linux - LDAP Anonymous Bind

[LDAP anonymous binds](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/anonymous-ldap-operations-active-directory-disabled) allow unauthenticated attackers to retrieve information from the domain, such as a complete listing of users, groups, computers, user account attributes, and the domain password policy. This is a legacy configuration, and as of Windows Server 2003, only authenticated users are permitted to initiate LDAP requests. We still see this configuration from time to time as an admin may have needed to set up a particular application to allow anonymous binds and given out more than the intended amount of access, thereby giving unauthenticated users access to all objects in AD.


With an LDAP anonymous bind, we can use LDAP-specific enumeration tools such as `windapsearch.py`, `ldapsearch`, `ad-ldapdomaindump.py`, etc., to pull the password policy. With [ldapsearch](https://linux.die.net/man/1/ldapsearch), it can be a bit cumbersome but doable. One example command to get the password policy is as follows:


#### Using ldapsearch

  Using ldapsearch

```shell
gdxqpardo@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```


## Enumerating the Password Policy - from Windows

If we can authenticate to the domain from a Windows host, we can use built-in Windows binaries such as `net.exe` to retrieve the password policy. We can also use various tools such as PowerView, CrackMapExec ported to Windows, SharpMapExec, SharpView, etc.

Using built-in commands is helpful if we land on a Windows system and cannot transfer tools to it, or we are positioned on a Windows system by the client, but have no way of getting tools onto it. One example using the built-in net.exe binary is:



#### Using net.exe

  Using net.exe

```cmd
C:\htb> net accounts
```


Here we can glean the following information:

- Passwords never expire (Maximum password age set to Unlimited)
- The minimum password length is 8 so weak passwords are likely in use
- The lockout threshold is 5 wrong passwords
- Accounts remained locked out for 30 minutes

PowerView is also quite handy for this:

#### Using PowerView

  Using PowerView

```powershell
PS C:\htb> import-module .\PowerView.ps1
PS C:\htb> Get-DomainPolicy
```


The default password policy when a new domain is created is as follows, and there have been plenty of organizations that never changed this policy:

|Policy|Default Value|
|---|---|
|Enforce password history|24 days|
|Maximum password age|42 days|
|Minimum password age|1 day|
|Minimum password length|7|
|Password must meet complexity requirements|Enabled|
|Store passwords using reversible encryption|Disabled|
|Account lockout duration|Not set|
|Account lockout threshold|0|
|Reset account lockout counter after|Not set|




















































































































