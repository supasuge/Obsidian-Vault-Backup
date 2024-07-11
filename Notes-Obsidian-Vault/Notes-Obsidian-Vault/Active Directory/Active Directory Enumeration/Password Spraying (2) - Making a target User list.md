## Table of Contents

  - [Table of Contents](#Table\of\Contents)
        - [Detailed User Enumeration](#Detailed\User\Enumeration)
      - [Using enum4linux](#Using\enum4linux)
- [SMB NULL Session to Pull User List](#smb\null\session\to\pull\user\list)
  - [Tools for SMB NULL Sessions and LDAP Anonymous Binds](#Tools\for\SMB\NULL\Sessions\and\LDAP\Anonymous\Binds)
      - [Using rpcclient](#Using\rpcclient)
  - [Gathering Users with LDAP Anonymous](#Gathering\Users\with\LDAP\Anonymous)
      - [Using ldapsearch](#Using\ldapsearch)

## Table of Contents

        - [Detailed User Enumeration](#Detailed\User\Enumeration)
      - [Using enum4linux](#Using\enum4linux)
- [SMB NULL Session to Pull User List](#smb\null\session\to\pull\user\list)
  - [Tools for SMB NULL Sessions and LDAP Anonymous Binds](#Tools\for\SMB\NULL\Sessions\and\LDAP\Anonymous\Binds)
      - [Using rpcclient](#Using\rpcclient)
  - [Gathering Users with LDAP Anonymous](#Gathering\Users\with\LDAP\Anonymous)
      - [Using ldapsearch](#Using\ldapsearch)


##### Detailed User Enumeration
To mount a good pass spray attack, need a list of valid doomain users to attempt to auth with. 
- Leveraging an SMB NULL session to retrieve a complete list of domain users from the domain controller
- Utilizing an LDAP anonymous bind to query LDAP anonymously and pull down the domain user list
- Using a tool such as kerbrute to validate users utilizing a word list from a source such as - [statistically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames) GitHub repo, or gathered by using a tool such as [linkedin2username](https://github.com/initstring/linkedin2username) to create a list of potentially valid users
- Using a set of credentials from a Linux or Windows attack system either provided by our client or obtained through another means such as LLMNR/NBT-NS response poisoning using `Responder` or even a successful password spray using a smaller wordlist

#### Using enum4linux

  Using enum4linux

```shell
gdxqpardo@htb[/htb]$ enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```

# SMB NULL Session to Pull User List

If you are on an internal machine but don’t have valid domain credentials, you can look for **SMB NULL sessions** or **LDAP anonymous binds** on Domain Controllers. Either of these will allow you to obtain an accurate list of all users within Active Directory and the password policy.

If you already have credentials for a domain user or SYSTEM access on a Windows host, then you can easily query Active Directory for this information.

> **Note:** It’s possible to do this using the SYSTEM account because it can impersonate the computer. A computer object is treated as a domain user account (with some differences, such as authenticating across forest trusts). If you don’t have a valid domain account, and SMB NULL sessions and LDAP anonymous binds are not possible, you can create a user list using external resources such as email harvesting and LinkedIn. This user list will not be as complete, but it may be enough to provide you with access to Active Directory.

## Tools for SMB NULL Sessions and LDAP Anonymous Binds

Some tools that can leverage SMB NULL sessions and LDAP anonymous binds include:

- **enum4linux**
- **rpcclient**
- **CrackMapExec**

Regardless of the tool, we'll have to do a bit of filtering to clean up the output and obtain a list of only usernames, one on each line. We can do this with `enum4linux` with the `-U` flag.
We can use the `enumdomusers` command after connecting anonymously using `rpcclient`.

#### Using rpcclient

  Using rpcclient

```shell
gdxqpardo@htb[/htb]$ rpcclient -U "" -N 172.16.5.5
```
dfsdf

## Gathering Users with LDAP Anonymous

We can use various tools to gather users when we find an LDAP anonymous bind. Some examples include [windapsearch](https://github.com/ropnop/windapsearch) and [ldapsearch](https://linux.die.net/man/1/ldapsearch). If we choose to use `ldapsearch` we will need to specify a valid LDAP search filter. We can learn more about these search filters in the [Active Directory LDAP](https://academy.hackthebox.com/course/preview/active-directory-ldap) module.

#### Using ldapsearch

  Using ldapsearch

```shell-session
gdxqpardo@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```





















