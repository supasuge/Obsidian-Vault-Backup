## Table of Contents

    - [LDAP - TCP 389 / 3268](#LDAP\-\TCP\389\/\3268)
    - [SMB](#SMB)
- [extended LDIF](#extended\ldif)
- [LDAPv3](#ldapv3)
- [base <> (default) with scope baseObject](#base\<>\(default)\with\scope\baseobject)
- [filter: (objectclass=*)](#filter:\(objectclass=*))
- [requesting: namingcontexts](#requesting:\namingcontexts)
- [search result](#search\result)
- [numResponses: 2](#numresponses:\2)
- [numEntries: 1](#numentries:\1)
- [extended LDIF](#extended\ldif)
- [LDAPv3](#ldapv3)
- [base <DC=BLACKFIELD,DC=local> with scope subtree](#base\<dc=blackfield,dc=local>\with\scope\subtree)
- [filter: (objectclass=*)](#filter:\(objectclass=*))
- [requesting: ALL](#requesting:\all)
- [search result](#search\result)
- [numResponses: 1](#numresponses:\1)

*After Nmap Scans~~~~*

**Any time You see DNS it's worth trying a zone transfer--**
```bash
dig @10.10.10.192 blackfield.local


#The zone transfer would list all the known subdomains, but it fails:


root@kali# dig axfr @10.10.10.192 blackfield.local


#@nd Zone transer to try and list all the known subdomains:
dig axfr @10.10.10.192 blackfield.local



### LDAP - TCP 389 / 3268

I’ll use `ldapsearch` to see what information I can pull. Even though I have a domain name already, I’ll ask LDAP for the base naming contexts:


root@kali# ldapsearch -h 10.10.10.192 -x -s base namingcontext


DomainDnsZones.blackfield.local and ForestDnsZones.blackfield.local seem like interesting subdomains. Interestingly, they both resolve over `dig` as well (only one shown):



root@kali$ dig @10.10.10.192 ForestDnsZones.BLACKFIELD.local



#Nothing
root@kali ldapsearch -h 10.10.10.192 -x -b "DC=BLACKFIELD,DC=local"






#enumerate subdomains.

### SMB
CrackMapExec gives hostname DC01, which is that DC for blackfield.local

crackmapexec smb MACHINE_TARGET 




#Nulll Connection
smbmap -H 10.10.10.192:445







________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
     SMBMap - Samba Share Enumerator | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB session(s)                                
                                                                                                    
[+] IP: 10.10.10.192:445        Name: 10.10.10.192              Status: Guest session   
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        forensic                                                NO ACCESS       Forensic / Audit share.
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                NO ACCESS       Logon server share 
        profiles$                                               READ ONLY
        SYSVOL                                                  NO ACCESS       Logon server share 
           




Recon
nmap
DNS - TCP/UDP 53
LDAP - TCP 389 / 3268
SMB - TCP 445
Access as support
Access as audit2020
Shell as svc_backup
Priv: svc_backup –> administrator
Beyond Root - EFS
Blackfield
Blackfield was a beautiful Windows Activity directory box where I’ll get to exploit AS-REP-roasting, discover privileges with bloodhound from my remote host using BloodHound.py, and then reset another user’s password over RPC. With access to another share, I’ll find a bunch of process memory dumps, one of which is lsass.exe, which I’ll use to dump hashes with pypykatz. Finally with a hash that gets a WinRM shell, I’ll abuse backup privileges to read the ntds.dit file that contains all the hashes for the domain (as well as a copy of the SYSTEM reg hive). I’ll use those to dump the hashes, and get access as the administrator. In Beyond Root, I’ll look at the EFS that prevented my reading root.txt using backup privs, as well as go down a rabbit hole into Windows sessions and why the cipher command was returning weird results.

Box Info
Name	BlackfieldBlackfield
Play on HackTheBox
Release Date	06 Jun 2020
Retire Date	3 Oct 2020
OS	Windows Windows
Base Points	Hard [40]
Rated Difficulty	Rated difficulty for Blackfield
Radar Graph	Radar chart for Blackfield
First Blood User	00 days, 00 hours, 31 mins, 13 seconds cube0x0
First Blood Root	00 days, 00 hours, 59 mins, 23 seconds cube0x0
Creator	aas
Recon
nmap
nmap found eight open TCP ports and two UDP ports:

root@kali# nmap -p- --min-rate 10000 -oA scans/nmap-alltcp 10.10.10.192
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-07 15:29 EDT
Nmap scan report for 10.10.10.192
Host is up (0.073s latency).
Not shown: 65527 filtered ports
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
389/tcp  open  ldap
445/tcp  open  microsoft-ds
593/tcp  open  http-rpc-epmap
3268/tcp open  globalcatLDAP
5985/tcp open  wsman

Nmap done: 1 IP address (1 host up) scanned in 33.26 seconds
root@kali# nmap -p 53,88,135,389,445,593,3268,5985 -sC -sV -oA scans/nmap-tcpscripts 10.10.10.192
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-07 15:30 EDT
Nmap scan report for 10.10.10.192
Host is up (0.15s latency).

PORT     STATE SERVICE       VERSION
53/tcp   open  domain?
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|_    bind
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2020-06-08 02:33:08Z)
135/tcp  open  msrpc         Microsoft Windows RPC
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=6/7%Time=5EDD4080%P=x86_64-pc-linux-gnu%r(DNSVe
SF:rsionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\x
SF:04bind\0\0\x10\0\x03");
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 7h02m00s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2020-06-08T02:35:25
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 197.46 seconds
root@kali# nmap -sU -p- --min-rate 10000 -oA scans/nmap-alludp 10.10.10.192
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-07 15:46 EDT
Nmap scan report for 10.10.10.192
Host is up (0.015s latency).
Not shown: 65533 open|filtered ports
PORT    STATE SERVICE
53/udp  open  domain
389/udp open  ldap

Nmap done: 1 IP address (1 host up) scanned in 26.54 seconds
Based on that combination, it looks like a Windows Domain controller. No real hint on the OS at this point. There is a domain name from the LDAP output, blackfield.local.

DNS - TCP/UDP 53
Any time I see DNS on TCP it’s worth trying a zone transfer. I can query for blackfield.local:

root@kali# dig @10.10.10.192 blackfield.local

; <<>> DiG 9.16.2-Debian <<>> @10.10.10.192 blackfield.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59954
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;blackfield.local.              IN      A

;; ANSWER SECTION:
blackfield.local.       600     IN      A       10.10.10.192

;; Query time: 36 msec
;; SERVER: 10.10.10.192#53(10.10.10.192)
;; WHEN: Sun Jun 07 20:07:29 EDT 2020
;; MSG SIZE  rcvd: 61
The zone transfer would list all the known subdomains, but it fails:

root@kali# dig axfr @10.10.10.192 blackfield.local

; <<>> DiG 9.16.2-Debian <<>> axfr @10.10.10.192 blackfield.local
; (1 server found)
;; global options: +cmd
; Transfer failed.
LDAP - TCP 389 / 3268
I’ll use ldapsearch to see what information I can pull. Even though I have a domain name already, I’ll ask LDAP for the base naming contexts:

root@kali# ldapsearch -h 10.10.10.192 -x -s base namingcontexts
# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: namingcontexts
#

#
dn:
namingcontexts: DC=BLACKFIELD,DC=local
namingcontexts: CN=Configuration,DC=BLACKFIELD,DC=local
namingcontexts: CN=Schema,CN=Configuration,DC=BLACKFIELD,DC=local
namingcontexts: DC=DomainDnsZones,DC=BLACKFIELD,DC=local
namingcontexts: DC=ForestDnsZones,DC=BLACKFIELD,DC=local

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
DomainDnsZones.blackfield.local and ForestDnsZones.blackfield.local seem like interesting subdomains. Interestingly, they both resolve over dig as well (only one shown):

root@kali# dig @10.10.10.192 ForestDnsZones.BLACKFIELD.local

; <<>> DiG 9.16.2-Debian <<>> @10.10.10.192 ForestDnsZones.BLACKFIELD.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 38214
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;ForestDnsZones.BLACKFIELD.local. IN    A

;; ANSWER SECTION:
ForestDnsZones.BLACKFIELD.local. 600 IN A       10.10.10.192

;; Query time: 16 msec
;; SERVER: 10.10.10.192#53(10.10.10.192)
;; WHEN: Mon Jun 08 07:15:03 EDT 2020
;; MSG SIZE  rcvd: 76
Unfortunately for me, I can’t get LDAP to give me any more information:

root@kali# ldapsearch -h 10.10.10.192 -x -b "DC=BLACKFIELD,DC=local"
# extended LDIF
#
# LDAPv3
# base <DC=BLACKFIELD,DC=local> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# search result
search: 2
result: 1 Operations error
text: 000004DC: LdapErr: DSID-0C090A59, comment: In order to perform this opera
 tion a successful bind must be completed on the connection., data 0, v4563

# numResponses: 1
I tried the subdomains and got the same error. It seems I need creds at this point.

SMB - TCP 445
CrackMapExec
crackmapexec gives a hostname, DC01, which is in line with the thinking that this was a domain controller. It also gives a domain, BLACKFIELD.local.

root@kali# crackmapexec smb 10.10.10.192
SMB         10.10.10.192    445    DC01             [*] Windows 10.0 Build 17763 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:False)
Null Connection
With no creds, I can read the profiles$ share:

root@kali# smbmap -H 10.10.10.192 -u null                                        
[+] Guest session       IP: 10.10.10.192:445    Name: unknown                                           
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        forensic                                                NO ACCESS       Forensic / Audit share.
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                NO ACCESS       Logon server share 
        profiles$                                               READ ONLY
        SYSVOL                                                  NO ACCESS       Logon server share 
I can connect, and there are over 300 directories in the share:

root@kali# smbclient -N //10.10.10.192/profiles$
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Jun  3 12:47:12 2020
  ..                                  D        0  Wed Jun  3 12:47:12 2020
  AAlleni                             D        0  Wed Jun  3 12:47:11 2020
  ABarteski                           D        0  Wed Jun  3 12:47:11 2020
  ABekesz                             D        0  Wed Jun  3 12:47:11 2020
  ABenzies                            D        0  Wed Jun  3 12:47:11 2020
  ABiemiller                          D        0  Wed Jun  3 12:47:11 2020
  AChampken                           D        0  Wed Jun  3 12:47:11 2020
  ACheretei                           D        0  Wed Jun  3 12:47:11 2020
  ACsonaki                            D        0  Wed Jun  3 12:47:11 2020
  AHigchens                           D        0  Wed Jun  3 12:47:11 2020
  AJaquemai                           D        0  Wed Jun  3 12:47:11 2020
...[snip]...
Each one is empty:

smb: \> ls ZTimofeeff
  ZTimofeeff                          D        0  Wed Jun  3 12:47:12 2020

\ZTimofeeff
  .                                   D        0  Wed Jun  3 12:47:12 2020
  ..                                  D        0  Wed Jun  3 12:47:12 2020

                7846143 blocks of size 4096. 3322639 blocks available
Trying to do a recursive get takes a while, and returns no files:

smb: \> recurse on
smb: \> prompt off
smb: \> mget *
smb: \>
While I can’t access any files, this is an opportunity to create a list of usernames. I’ll mount the share on my local box (just hitting enter when prompted for a password):

root@kali# mount -t cifs //10.10.10.192/profiles$ /mnt
Password for root@//10.10.10.192/profiles$: 


Now I’ll use `-1` in `ls` to print only the directories one per line:


root@kali# mv users users.old; ls -1 /mnt/ > users
```