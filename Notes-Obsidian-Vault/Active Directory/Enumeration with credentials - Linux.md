## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [CME - Domain User Enumeration](#CME\-\Domain\User\Enumeration)
      - [CME - Domain Group Enumeration](#CME\-\Domain\Group\Enumeration)
      - [CME - Logged On Users](#CME\-\Logged\On\Users)
      - [CME Share Searching](#CME\Share\Searching)
      - [Share Enumeration - Domain Controller](#Share\Enumeration\-\Domain\Controller)
  - [SMBMap](#SMBMap)
      - [SMBMap To Check Access](#SMBMap\To\Check\Access)
      - [Recursive List Of All Directories](#Recursive\List\Of\All\Directories)
  - [rpcclient](#rpcclient)
      - [rpcclient Enumeration](#rpcclient\Enumeration)
      - [RPCClient User Enumeration By RID](#RPCClient\User\Enumeration\By\RID)
      - [Enumdomusers](#Enumdomusers)
  - [Impacket Toolkit](#Impacket\Toolkit)
      - [Psexec.py](#Psexec.py)
      - [Using psexec.py](#Using\psexec.py)
      - [wmiexec.py](#wmiexec.py)
      - [Using wmiexec.py](#Using\wmiexec.py)
      - [Windapsearch Help](#Windapsearch\Help)
      - [Windapsearch - Domain Admins](#Windapsearch\-\Domain\Admins)
      - [Windapsearch - Privileged Users](#Windapsearch\-\Privileged\Users)

## Table of Contents

      - [CME - Domain User Enumeration](#CME\-\Domain\User\Enumeration)
      - [CME - Domain Group Enumeration](#CME\-\Domain\Group\Enumeration)
      - [CME - Logged On Users](#CME\-\Logged\On\Users)
      - [CME Share Searching](#CME\Share\Searching)
      - [Share Enumeration - Domain Controller](#Share\Enumeration\-\Domain\Controller)
  - [SMBMap](#SMBMap)
      - [SMBMap To Check Access](#SMBMap\To\Check\Access)
      - [Recursive List Of All Directories](#Recursive\List\Of\All\Directories)
  - [rpcclient](#rpcclient)
      - [rpcclient Enumeration](#rpcclient\Enumeration)
      - [RPCClient User Enumeration By RID](#RPCClient\User\Enumeration\By\RID)
      - [Enumdomusers](#Enumdomusers)
  - [Impacket Toolkit](#Impacket\Toolkit)
      - [Psexec.py](#Psexec.py)
      - [Using psexec.py](#Using\psexec.py)
      - [wmiexec.py](#wmiexec.py)
      - [Using wmiexec.py](#Using\wmiexec.py)
      - [Windapsearch Help](#Windapsearch\Help)
      - [Windapsearch - Domain Admins](#Windapsearch\-\Domain\Admins)
      - [Windapsearch - Privileged Users](#Windapsearch\-\Privileged\Users)

CME offers a help menu for each protocol (i.e., `crackmapexec winrm -h`, etc.). Be sure to review the entire help menu and all possible options. For now, the flags we are interested in are:

- -u Username `The user whose credentials we will use to authenticate`
- -p Password `User's password`
- Target (IP or FQDN) `Target host to enumerate` (in our case, the Domain Controller)
- --users `Specifies to enumerate Domain Users`
- --groups `Specifies to enumerate domain groups`
- --loggedon-users `Attempts to enumerate what users are logged on to a target, if any`



We'll start by using the SMB protocol to enumerate users and groups. We will target the Domain Controller (whose address we uncovered earlier) because it holds all data in the domain database that we are interested in. Make sure you preface all commands with `sudo`.

#### CME - Domain User Enumeration

We start by pointing CME at the Domain Controller and using the credentials for the `forend` user to retrieve a list of all domain users. Notice when it provides us the user information, it includes data points such as the [badPwdCount](https://docs.microsoft.com/en-us/windows/win32/adschema/a-badpwdcount) attribute. This is helpful when performing actions like targeted password spraying. We could build a target user list filtering out any users with their `badPwdCount` attribute above 0 to be extra careful not to lock any accounts out.

  CME - Domain User Enumeration

```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users

```

#### CME - Domain Group Enumeration

  CME - Domain Group Enumeration

```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```



The above snippet lists the groups within the domain and the number of users in each. The output also shows the built-in groups on the Domain Controller, such as `Backup Operators`. We can begin to note down groups of interest. Take note of key groups like `Administrators`, `Domain Admins`, `Executives`, any groups that may contain privileged IT admins, etc. These groups will likely contain users with elevated privileges worth targeting during our assessment.

#### CME - Logged On Users

We can also use CME to target other hosts. Let's check out what appears to be a file server to see what users are logged in currently.

  CME - Logged On Users

```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```

#### CME Share Searching

We can use the `--shares` flag to enumerate available shares on the remote host and the level of access our user account has to each share (READ or WRITE access). Let's run this against the INLANEFREIGHT.LOCAL Domain Controller.

#### Share Enumeration - Domain Controller

  Share Enumeration - Domain Controller

```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```


```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'

```

## SMBMap

SMBMap is great for enumerating SMB shares from a Linux attack host. It can be used to gather a listing of shares, permissions, and share contents if accessible. Once access is obtained, it can be used to download and upload files and execute remote commands.
SMBMap is great for enumerating SMB shares from a Linux attack host. It can be used to gather a listing of shares, permissions, and share contents if accessible. Once access is obtained, it can be used to download and upload files and execute remote commands.

Like CME, we can use SMBMap and a set of domain user credentials to check for accessible shares on remote systems. As with other tools, we can type the command `smbmap` `-h` to view the tool usage menu. Aside from listing shares, we can use SMBMap to recursively list directories, list the contents of a directory, search file contents, and more. This can be especially useful when pillaging shares for useful information.

#### SMBMap To Check Access

  SMBMap To Check Access

```shell-session
gdxqpardo@htb[/htb]$ smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```

Let's do a recursive listing of the directories in the `Department Shares` share. We can see, as expected, subdirectories for each department in the company.

#### Recursive List Of All Directories

  Recursive List Of All Directories

```shell-session
gdxqpardo@htb[/htb]$ smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```

As the recursive listing dives deeper, it will show you the output of all subdirectories within the higher-level directories. The use of `--dir-only` provided only the output of all directories and did not list all files. Try this against other shares on the Domain Controller and see what you can find.

Now that we've covered shares, let's look at `RPCClient`.

---

## rpcclient

[rpcclient](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) is a handy tool created for use with the Samba protocol and to provide extra functionality via MS-RPC. It can enumerate, add, change, and even remove objects from AD. It is highly versatile; we just have to find the correct command to issue for what we want to accomplish. The man page for rpcclient is very helpful for this; just type `man rpcclient` into your attack host's shell and review the options available. Let's cover a few rpcclient functions that can be helpful during a penetration test.

Due to SMB NULL sessions (covered in-depth in the password spraying sections) on some of our hosts, we can perform authenticated or unauthenticated enumeration using rpcclient in the INLANEFREIGHT.LOCAL domain. An example of using rpcclient from an unauthenticated standpoint (if this configuration exists in our target domain) would be:

Code: bash

```bash
rpcclient -U "" -N 172.16.5.5
```
The above will provide us with a bound connection, and we should be greeted with a new prompt to start unleashing the power of rpcclient.
#### rpcclient Enumeration

While looking at users in rpcclient, you may notice a field called `rid:` beside each user. A [Relative Identifier (RID)](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) is a unique identifier (represented in hexadecimal format) utilized by Windows to track and identify objects. To explain how this fits in, let's look at the examples below:

- The [SID](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) for the INLANEFREIGHT.LOCAL domain is: `S-1-5-21-3842939050-3880317879-2865463114`.
- When an object is created within a domain, the number above (SID) will be combined with a RID to make a unique value used to represent the object.
- So the domain user `htb-student` with a RID:[0x457] Hex 0x457 would = decimal `1111`, will have a full user SID of: `S-1-5-21-3842939050-3880317879-2865463114-1111`.
- This is unique to the `htb-student` object in the INLANEFREIGHT.LOCAL domain and you will never see this paired value tied to another object in this domain or any other.



However, there are accounts that you will notice that have the same RID regardless of what host you are on. Accounts like the built-in Administrator for a domain will have a RID [administrator] rid:[0x1f4], which, when converted to a decimal value, equals `500`. The built-in Administrator account will always have the RID value `Hex 0x1f4`, or 500. This will always be the case. Since this value is unique to an object, we can use it to enumerate further information about it from the domain. Let's give it a try again with rpcclient. We will dig a bit targeting the `htb-student` user.

#### RPCClient User Enumeration By RID

  RPCClient User Enumeration By RID

```shell-session
rpcclient $> queryuser 0x457
```

When we searched for information using the `queryuser` command against the RID `0x457`, RPC returned the user information for `htb-student` as expected. This wasn't hard since we already knew the RID for `htb-student`. If we wished to enumerate all users to gather the RIDs for more than just one, we would use the `enumdomusers` command.

#### Enumdomusers

  Enumdomusers

```shell-session
rpcclient $> enumdomusers
```

## Impacket Toolkit

Impacket is a versatile toolkit that provides us with many different ways to enumerate, interact, and exploit Windows protocols and find the information we need using Python. The tool is actively maintained and has many contributors, especially when new attack techniques arise. We could perform many other actions with Impacket, but we will only highlight a few in this section; [wmiexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/wmiexec.py) and [psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py). Earlier in the poisoning section, we grabbed a hash for the user `wley` with `Responder` and cracked it to obtain the password `transporter@4`. We will see in the next section that this user is a local admin on the `ACADEMY-EA-FILE` host. We will utilize the credentials for the next few actions.

#### Psexec.py

One of the most useful tools in the Impacket suite is `psexec.py`. Psexec.py is a clone of the Sysinternals psexec executable, but works slightly differently from the original. The tool creates a remote service by uploading a randomly-named executable to the `ADMIN$` share on the target host. It then registers the service via `RPC` and the `Windows Service Control Manager`. Once established, communication happens over a named pipe, providing an interactive remote shell as `SYSTEM` on the victim host.

#### Using psexec.py

To connect to a host with psexec.py, we need credentials for a user with local administrator privileges.

Code: bash

```bash
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125  
```


#### wmiexec.py

Wmiexec.py utilizes a semi-interactive shell where commands are executed through [Windows Management Instrumentation](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page). It does not drop any files or executables on the target host and generates fewer logs than other modules. After connecting, it runs as the local admin user we connected with (this can be less obvious to someone hunting for an intrusion than seeing SYSTEM executing many commands). This is a more stealthy approach to execution on hosts than other tools, but would still likely be caught by most modern anti-virus and EDR systems. We will use the same account as with psexec.py to access the host.

#### Using wmiexec.py

Code: bash

```bash
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5  
```


#### Windapsearch Help

  Windapsearch Help

```shell-session
usage: windapsearch.py [-h] [-d DOMAIN] [--dc-ip DC_IP] [-u USER]
                       [-p PASSWORD] [--functionality] [-G] [-U] [-C]
                       [-m GROUP_NAME] [--da] [--admin-objects] [--user-spns]
                       [--unconstrained-users] [--unconstrained-computers]
                       [--gpos] [-s SEARCH_TERM] [-l DN]
                       [--custom CUSTOM_FILTER] [-r] [--attrs ATTRS] [--full]
                       [-o output_dir]

Script to perform Windows domain enumeration through LDAP queries to a Domain
Controller

optional arguments:
  -h, --help            show this help message and exit

Domain Options:
  -d DOMAIN, --domain DOMAIN
                        The FQDN of the domain (e.g. 'lab.example.com'). Only
                        needed if DC-IP not provided
  --dc-ip DC_IP         The IP address of a domain controller

Bind Options:
  Specify bind account. If not specified, anonymous bind will be attempted

  -u USER, --user USER  The full username with domain to bind with (e.g.
                        'ropnop@lab.example.com' or 'LAB\ropnop'
  -p PASSWORD, --password PASSWORD
                        Password to use. If not specified, will be prompted
                        for

Enumeration Options:
  Data to enumerate from LDAP

  --functionality       Enumerate Domain Functionality level. Possible through
                        anonymous bind
  -G, --groups          Enumerate all AD Groups
  -U, --users           Enumerate all AD Users
  -PU, --privileged-users
                        Enumerate All privileged AD Users. Performs recursive
                        lookups for nested members.
  -C, --computers       Enumerate all AD Computers
```

#### Windapsearch - Domain Admins

  Windapsearch - Domain Admins

```shell-session
gdxqpardo@htb[/htb]$ python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```


From the results in the shell above, we can see that it enumerated 28 users from the Domain Admins group. Take note of a few users we have already seen before and may even have a hash or cleartext password like `wley`, `svc_qualys`, and `lab_adm`.

To identify more potential users, we can run the tool with the `-PU` flag and check for users with elevated privileges that may have gone unnoticed. This is a great check for reporting since it will most likely inform the customer of users with excess privileges from nested group membership.

#### Windapsearch - Privileged Users

  Windapsearch - Privileged Users

```shell-session
gdxqpardo@htb[/htb]$ python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```