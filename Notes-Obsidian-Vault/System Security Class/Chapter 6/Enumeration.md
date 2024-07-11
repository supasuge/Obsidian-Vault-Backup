## Table of Contents

      - [Countermeasures](#Countermeasures)
  - [6.2 Remote Procedure Calls](#6.2\Remote\Procedure\Calls)
      - [SunRPC](#SunRPC)
    - [Remote Method Invocation](#Remote\Method\Invocation)
    - [6.3 Server Message Block](#6.3\Server\Message\Block)
      - [Nmap Scripts](#Nmap\Scripts)
      - [Countermeasures](#Countermeasures)
    - [6.4 Simple Network Management Protocol](#6.4\Simple\Network\Management\Protocol)
          - [Countermeasures](#Countermeasures)
    - [6.5 Simple Mail Transfer Protocol](#6.5\Simple\Mail\Transfer\Protocol)
      - [Countermeasures](#Countermeasures)
    - [6.6 Web-based Enumeration](#6.6\Web-based\Enumeration)
      - [Countermeasures](#Countermeasures)
        - [Important Terms](#Important\Terms)
      - [Metasploit](#Metasploit)
      - [RMI](#RMI)
      - [RPC](#RPC)
      - [SNMP](#SNMP)
      - [SMTP](#SMTP)
      - [port scan](#port\scan)
      - [MIB](#MIB)
      - [NFS](#NFS)
      - [NetBIOS](#NetBIOS)
      - [dirb](#dirb)

- Port scanning
- determining what services are running and then extracting information from those services




**nmap Version Scan**  

` $ sudo nmap -sV 192.168.1.15`


**SSH2 Algorithm Enumeration**  

` $ sudo nmap --script=ssl-enum-ciphers 192.168.1.15`

#### Countermeasures

Each service is going to have different configuration settings that will prevent attackers from enumeration, but at a high level, the following countermeasures should be considered for every service:  

- **Firewalls:** Both network and host‐based firewalls can be used to protect services by preventing systems that should not be communicating to send requests.
- **Authentication:** Strong authentication can help to protect some services, since authentication may happen before requests can be made to the service.
- **Reduce Information Provided:** Some services will allow you to configure how much information is provided in the banner—the information the service greets the user with when the application receives a connection request.

## 6.2 Remote Procedure Calls
A remote procedure call (RPC) is a service that allows remote systems to consume procedures external to the application calling them. A program on system A can call a function or procedure on another system across the network. It does this using the RPC protocol.


#### SunRPC

The idea of interprocess communication has been around for decades. There have been several implementations of request‐response protocols over the decades. **Java's remote method invocation (RMI)** is a recent example of this. Before that, there was the **Common Object Request Broker Architecture (CORBA)**

The command used, `rpcinfo ‐p`, has the `rpcinfo` probe the host provided. In this case, the host is an IP address rather than a hostname.

`$ rpcinfo -p 192.168.86.52`

```
 program vers proto   port  service  
    100000    4   tcp    111  portmapper  
    100000    3   tcp    111  portmapper  
    100000    2   tcp    111  portmapper  
    100000    4   udp    111  portmapper  
    100000    3   udp    111  portmapper  
    100000    2   udp    111  portmapper  
    100005    1   udp  43939  mountd  
    100005    1   tcp  58801  mountd  
    100005    2   udp  46384  mountd  
    100005    2   tcp  50405  mountd  
    100005    3   udp  49030  mountd  
    100005    3   tcp  50553  mountd  
    100003    3   tcp   2049  nfs  
    100003    4   tcp   2049  nfs  
    100227    3   tcp   2049  nfs_acl  
    100003    3   udp   2049  nfs  
    100227    3   udp   2049  nfs_acl  
    100021    1   udp  34578  nlockmgr  
    100021    3   udp  34578  nlockmgr  
    100021    4   udp  34578  nlockmgr  
    100021    1   tcp  39297  nlockmgr  
    100021    3   tcp  39297  nlockmgr  
    100021    4   tcp  39297  nlockmgr
```
**Metasploit sunrpc Scanner**  

` msf> use auxiliary/scanner/misc/sunrpc_portmapper`


### Remote Method Invocation
Java programs are very popular, especially when it comes to web applications. Java provides a lot of assistance to programmers, especially when it comes to libraries and interfaces that can be implemented with your own functionality. Java includes its own capability for remote procedure calls, though in Java it's called _remote method invocation_. To have a program that uses RMI, the system needs a version of the portmapper for Java called the `rmiregistry`

The program uses `RMI` registers itself with the `remiregistry` program. This means that anyone can check with the `rmiregistry` to see what services are offered. The `rmiregistry` program will respond in a similar way to what we saw when we checked with the portmapper.


**Running RMI Scanner in Metasploit**  
  

```
msf> use auxiliary/gather/java_rmi_registry  
msf auxiliary(gather/java_rmi_registry)> show options
```

Note: The program `javac` is used to compile Java source code to an intermediate code file. The program `java` is used to execute an intermediate code file

**Using BaRMIe**  
```bash
root@quiche:~# java -jar BaRMIe_v1.01.jar  192.168.86.62
```
- Get nameserver 
- Localhost
- Inheritance tree.

![[Pasted image 20240205133505.png]]


### 6.3 Server Message Block
- Most common implementation of remote procedue calls is used in Windows networks.



SMB is an Application layer protocol that can operate over different protocols at lower layers. First, it can operate directly over TCP without any other Session layer protocols. If a system were running SMB directly over TCP, you would find TCP port 445 to be open. SMB can also operate over session protocols like NetBIOS, which is an application programming interface (API) developed by IBM to extend the input/output (I/O) capabilities away from the local system and onto the network. If you see UDP ports 137 and 138 open, you will know that you have found SMB running on top of NetBIOS. However, if you find TCP ports 137 and 139 open, you will have found SMB running on NetBIOS over TCP. Keep in mind that NetBIOS is used for name services in this case.


**nbtstat Output**  
  

```
C:\Users\kilroy> nbtstat -a billthecat  
  
Local Area Connection:  
Node IpAddress: [192.168.86.50] Scope Id: []  
  
          NetBIOS Remote Machine Name Table  
  
       Name               Type         Status  
    ---------------------------------------------  
    BILLTHECAT     <00>  UNIQUE      Registered  
    BILLTHECAT     <20>  UNIQUE      Registered  
    WORKGROUP      <00>  GROUP       Registered  
  
    MAC Address = AC-87-A3-36-D6-AA

```


  
What we've seen so far simply asks for all the functions (names) associated with a hostname on the network. That's one individual system. If we want to see all the hostnames that are talking SMB/NetBIOS, we need to ask for something else. We can still use `nbtstat`, we just pass a different parameter in on the command line. We are looking for resolved names. This list can come from broadcast messages when there is no centralized database for name lookups—systems announce their names and their presence when they come online and then periodically after that. It can also come from the Windows Internet Name Server (WINS), which is a central repository of names of systems on an enterprise network. Windows servers will have WINS functionality, so systems register with the WINS and all names can be resolved.  
  
In the following code listing, you can see a list of names on the network. Since there is no Windows server and, as a result, no WINS on the network, these are all names that have been identified through broadcast messages. These systems are all macOS, but they are sharing files on the network using SMB. To do that, they need to behave like any other system that communicates using SMB.


**Listing Resolved Names with nbstat**
```
C:\Users\kilroy> nbstat -r
NetBIOS Names Resolution and Registration Statistics  
        ----------------------------------------------------  
  
        Resolved By Broadcast     = 47  
        Resolved By Name Server   = 0  
  
        Registered By Broadcast   = 8  
        Registered By Name Server = 0  
  
        NetBIOS Names Resolved By Broadcast  
        ---------------------------------------------  
                YAZPISTACHIO   <00>  
                BILLTHECAT     <00>  
                YAZPISTACHIO   <00>  
                YAZPISTACHIO   <00>  
                LOLAGRANOLA    <00>  
                LOLAGRANOLA    <00>  
                YAZPISTACHIO   <00>  
                YAZPISTACHIO   <00>
```

Linux SMB: **Samba**

**nmblookup for Enumeration**  
```bash
kilroy@savagewood$ nmblookup -S -B 192.168.86.255 billthecat  
Can't load /etc/samba/smb.conf - run testparm to debug it  
querying billthecat on 192.168.86.255  
192.168.86.32 billthecat<00>  
Looking up status of 192.168.86.32  
     BILLTHECAT      <00> -         H <ACTIVE>  
     BILLTHECAT      <20> -         H <ACTIVE>  
     WORKGROUP       <00> - <GROUP> H <ACTIVE>  
  
     MAC Address = AC-87-A3-36-D6-AA
```

Using `‐B` tells `nmblookup` to use the broadcast address that is supplied


The `net` command allows you to query the network using SMB messages. As an example, the output that follows shows statistics from the workstation service on a Windows server. This shows information about network communication primarily, including bytes transferred. It can also show you the number of sessions that have been started and the sessions that have failed.
```
PS C:\Users\kilroy> net statistics workstation  
Workstation Statistics for \\SERVER2020  
  
  
Statistics since 1/1/2021 6:08:25 PM  
  
  
  Bytes received                               2994818  
  Server Message Blocks (SMBs) received        8  
  Bytes transmitted                            5615081  
  Server Message Blocks (SMBs) transmitted     0  
  Read operations                              1180  
  Write operations                             0  
  Raw reads denied                             0  
  Raw writes denied                            0  
  
  Network errors                               0  
  Connections made                             0  
  Reconnections made                           0  
  Server disconnects                           0  
  
  Sessions started                             0  
  Hung sessions                                0  
  Failed sessions                              0  
  Failed operations                            0  
  Use count                                    687  
  Failed use count                             0  
  
The command completed successfully.
```


#### Nmap Scripts
  
**smb‐os‐discovery Scan Output**  
`$ sudo nmap --script smb-os-discovery 192.168.1.10`


**Enumerating Shares with Nmap**  
  

`$ sudo nmap --script smb-enum-shares 192.168.1.10`


**SMB Version Scan with Metasploit**  
  
`msf auxiliary(scanner/smb/smb_version)> run`

  
**smb_login Module Options**  
  We can also use Metasploit to enumerate users. To do that, you would use the `smb_enumusers_domain` module. If you know one, you can use a username and password. This would allow the module to authenticate against the system to obtain additional users.

`Module options (auxiliary/scanner/smb/smb_login):`


  
Another type of enumeration that is useful when it comes to SMB is to look at shares. While shares are common on servers in enterprise environments, users may also enable shares on their desktops, unless it is prohibited administratively. They may do this to easily get files from one user to another. These local shares may have weaker permissions set on them, which may make them easier to get files or other information from. Metasploit has a module for this, as you can see here:  
  

```
msf6> use auxiliary/scanner/smb/smb_enumshares  
msf6 auxiliary(scanner/smb/smb_enumshares)> show options
```

**Scanning a Network with nbtscan**  
  
`root@quiche:~# nbtscan 192.168.86.0/24`

You may start to see a bit of a pattern when it comes to enumeration and SMB. While there are a lot of tools available, they all perform the same functions, and the easiest thing to do when it comes to enumeration is to identify systems that use SMB, including the name they advertise on the network. One note about that name: it may not resolve to an IP address. A name announced using NetBIOS is intended to be used and resolved on the local network. This means it won't resolve using DNS unless DNS is configured to use the same names and IP addresses

- Possible to have one name for your windows sharing and another one for your DNS, assuming the system even has a DNS address. If your enterprise networks uses WINS, they will resolve to be the same because of how the local system register to WINS.


  
Another tool, and we'll see the same capabilities with this one, is `enum4linux`. The following example is being run from a Kali Linux system where it is installed by default, but it's easy enough to get a copy of it. It's just a Perl script, so to run it, you need a Perl interpreter. The following example enumerates the shares on a specific host, identified by IP address. The target system is a Linux system running Samba to provide Windows networking functionality over SMB. In the output, you will find a lot of information related to how Samba is configured. As an example, we can see that the workgroup WORKGROUP is configured, which is a way of organizing systems on a local network that are all using Windows.

  
**enum4linux Share Enumeration**    
`root@quiche:~# enum4linux  -S 192.168.86.52`
```
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sun Jan 3 12:18:25 2021  
  
==========================  
|    Target Information    |  
==========================  
Target ……….. 192.168.86.52  
RID Range …….. 500-550,1000-1050  
Username ……… ''  
Password ……… ''  
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none  
  
  
=====================================================  
|    Enumerating Workgroup/Domain on 192.168.86.52    |  
=====================================================  
[+] Got domain/workgroup name: WORKGROUP  
  
======================================  
|    Session Check on 192.168.86.52    |  
======================================  
[+] Server 192.168.86.52 allows sessions using username '', password ''  
  
============================================  
|    Getting domain SID for 192.168.86.52    |  
============================================  
Domain Name: WASHERE  
Domain Sid: (NULL SID)  
[+] Can't determine if host is part of domain or part of a workgroup  
  
==========================================  
|    Share Enumeration on 192.168.86.52    |  
==========================================  
WARNING: The "syslog" option is deprecated  
  
    Sharename       Type      Comment  
    ---------       ----      -------  
    homes           Disk      Home Directories  
    print$          Disk      Printer Drivers  
    IPC$            IPC       Service (bobbie server (Samba, Ubuntu))  
Reconnecting with SMB1 for workgroup listing.  
  
    Server               Comment  
    ---------            -------  
  
    Workgroup            Master  
    ---------            -------  
    WORKGROUP            STEVEDALLAS  
  
[+] Attempting to map shares on 192.168.86.52
```


#### Countermeasures

Because SMB is such an essential foundation of Windows networks, you may think it would be difficult to enact countermeasures to protect against enumeration. However, there are still some things you can do to protect against attackers. Some of this is related to configuration and hardening, while some relates to additional controls you can put into place.
- **Disable SMBv1:** SMBv1 is a much older version of the protocol and is not necessary except in very unusual situations. It is vulnerable to attack in different ways and can be prone to information leakage. This is something you can easily disable. In fact, newer versions of Windows have it disabled by default.
- **Enable Host-Based Firewall**: Using zones will help protect individual systems if they are on untrusted networks. A host-based firewall can also have rules that restrict who can talk to individual systems using SMB. In most cases, you shouldn't need to allow SMB conversations originating to desktops except for certain infrastructure systems.
- **Network Firewalls:** Host‐based firewalls will protect individual systems, but often you can catch a lot of this before the host by enabling network firewalls between network segments, only allowing SMB from expected infrastructure systems.
- **Disable Sharing:** In enterprise networks, it is generally going to be a bad idea to allow sharing from individual workstations. It's better to put shares on servers where you can control them centrally and let users share files from the file servers. If you disable sharing, you are automatically reducing the information that may be available from outside the host.
- **Disable NetBIOS over TCP/IP:** This is not a protocol that should be necessary for most Windows networking functions. This can be disabled, protecting probes of NetBIOS using the TCP/IP stack.

![[Pasted image 20240205140041.png]]
#SNMP
### 6.4 Simple Network Management Protocol
SNMP has been around since the late 1980's. 

- SNMPv1 also includes a number of flaws that make it problematic from a security perspective. It is a binary protocol, but there is no encryption supported with it. This means anyone who can obtain the packets being transmitted can decode it easily enough
- Secondly, there is very weak authentication. SNMPv1 uses community strings to gain access. You either get read-only access or get read-write access. There is no granularity beyond that. There is also no concept of users. You provide the appropriate string and you get that level of access. The string "public" is used for read-only, while the string "private" is used for read-write. This is not hard-coded, but it's common.

**snmpwalk of Linux System**  

```bash
root@quiche:~# snmpwalk -v 2c -c public 192.168.86.52  
iso.3.6.1.2.1.1.1.0 = STRING: "Linux bobbie 4.15.0-30-generic #32-Ubuntu SMP Sun Jan 4 17:42:43 UTC 2021 x86_64"  
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10  
iso.3.6.1.2.1.1.3.0 = Timeticks: (7606508) 21:07:45.08  
iso.3.6.1.2.1.1.4.0 = STRING: "Foo <foo@wubble.com>"  
iso.3.6.1.2.1.1.5.0 = STRING: "bobbie"  
iso.3.6.1.2.1.1.6.0 = STRING: "Erie, CO"  
iso.3.6.1.2.1.1.7.0 = INTEGER: 72  
iso.3.6.1.2.1.1.8.0 = Timeticks: (1) 0:00:00.01  
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.11.3.1.1  
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.15.2.1.1  
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.10.3.1.1  
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1  
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1  
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49  
iso.3.6.1.2.1.1.9.1.2.7 = OID: iso.3.6.1.2.1.4  
iso.3.6.1.2.1.1.9.1.2.8 = OID: iso.3.6.1.2.1.50  
iso.3.6.1.2.1.1.9.1.2.9 = OID: iso.3.6.1.6.3.13.3.1.3  
iso.3.6.1.2.1.1.9.1.2.10 = OID: iso.3.6.1.2.1.92
```


###### Countermeasures
The simplest countermeasure for SNMP is to just disable it. This may not be possible, though, depending on how your network is being managed. Beyond that, always upgrade to SNMPv3 since SNMPv1 has been deprecated for many years and it is not only vulnerable to attack, but also has no possibility to encrypt data, so everything is in plain text

- **Upgrade to SNMPv3**: get rid of v1, and v2
- **Require Authentication**: Disable the use of just community strings for authentication.
- **Require Encryption**: Be sure all SNMP communications are encrypted
- **Use Firewalls**: Use a combination of network-based firewalls to guarantee only the right hosts communicate with one another. SNMP should not be allowed without restriction across the network.

#SMTP
### 6.5 Simple Mail Transfer Protocol
SMTP operates using series of verbs to interact with the server. The client sends a verb and any other necessary parameters to the SMTP server. Based on the verb, the server knows how to handle the parameters received. 
**SMTP Conversation**  
```bash
root@quiche:~# nc 192.168.86.52 25  
220 bobbie.lan ESMTP Postfix (Ubuntu)  
EHLO blah.com  
250-bobbie.lan  
250-PIPELINING  
250-SIZE 10240000  
250-VRFY  
250-ETRN  
250-STARTTLS  
250-ENHANCEDSTATUSCODES  
250-8BITMIME  
250-DSN  
250 SMTPUTF8  
MAIL From: foo@foo.com  
250 2.1.0 Ok  
RCPT To: wubble@wubble.com  
250 2.1.5 Ok  
DATA  
354 End data with <CR><LF>.<CR><LF>  
From: Goober  
To: Someone  
Date: today  
Subject: Hi  
  
Nothing really to say.  
  
.  
250 2.0.0 Ok: queued as 33471301389
```

```bash
$ nc 192.168.86.52 25  
220 bobbie.lan ESMTP Postfix (Ubuntu)  
EHLO blah.com  
250-bobbie.lan  
250-PIPELINING  
250-SIZE 10240000  
250-VRFY  
250-ETRN  
250-STARTTLS  
250-ENHANCEDSTATUSCODES  
250-8BITMIME  
250-DSN  
250 SMTPUTF8  
MAIL From: foo@foo.com  
250 2.1.0 Ok  
RCPT To: wubble@wubble.com  
250 2.1.5 Ok  
DATA  
354 End data with <CR><LF>.<CR><LF>  
From: Goober  
To: Someone  
Date: today  
Subject: Hi  
  
Nothing really to say.  
  
.  
250 2.0.0 Ok: queued as 33471301389
```

  
Once you initiate the conversation using either `HELO` or `EHLO`, you will get a list of capabilities offered by the server. There are a couple of capabilities in SMTP that we can use to enumerate users or at least email addresses. One of them you can see is `VRFY`, which can be used to verify users. Not all mail servers will have this feature enabled, since it can be used to identify legitimate users or email addresses. That means it can be used by attackers as well as spammers. Here you can see an example of the use of `VRFY` against a local mail server running Postfix, which has `VRFY` enabled by default.  
  
**Testing VRFY**  
  
```bash
root@quiche:~# nc 192.168.86.52 25  
220 bobbie.lan ESMTP Postfix (Ubuntu)  
EHLO blah.com  
250-bobbie.lan  
250-PIPELINING  
250-SIZE 10240000  
250-VRFY  
250-ETRN  
250-STARTTLS  
250-ENHANCEDSTATUSCODES  
250-8BITMIME  
250-DSN  
250 SMTPUTF8  
VRFY root@localhost  
252 2.0.0 root@localhost  
VRFY kilroy@localhost  
252 2.0.0 kilroy@localhost  
VRFY root  
252 2.0.0 root
```
  
We could do it manually. Metasploit again to the rescue, though. The module `smtp_enum` will take a word list and do the same thing automatically that you saw done manually earlier. It will run through all the users in the word list, checking to see whether each user exists. There are two ways to test whether users exist—either the `VRFY` command or the `MAIL TO` command. In the following listing, you can see the results of a run against the same server. This is using the default word list that comes with Metasploit that has a list of common Unix usernames (`unix_users.txt`).  
#smtpenumeration  
**smtp_enum Run**  
```bash
msf auxiliary(scanner/smtp/smtp_enum)> use auxiliary/scanner/smtp/smtp_enum  
msf auxiliary(scanner/smtp/smtp_enum)> show options
```


#### Countermeasures

When it comes to countermeasures for SMTP enumeration, we are mostly looking to try to protect against identifying users by probing the mail server. Given the prevalence of username lists, this protection may be of limited use, but it's worth adding in any protection that may make things a little harder.  

- **Disable VRFY:** Some SMTP servers will allow you to disable this command. Removing it from your mail server should not impact any functionality required for the delivery of mail.
- **Ignore Unknown Addresses:** Have your SMTP server ignore usernames/email addresses that are not known. Generating an error provides the attacker with information about the existence or nonexistence of the user.
- **Restrict Information in Headers:** Be sure you are not showing information about your internal email chain to the extent you can. This will provide information to the attacker about internal systems, as well as other SMTP servers within your environment.
- **Disable Open Relays:** This is far less common than it used to be but ensuring all systems in your environment are not allowing relaying to other domains will help protect your systems from being used to enumerate other domains, since a lot of end‐user networks, such as those belonging to cable networks, restrict outbound SMTP to everything other than the cable provider's email address.
- **Implement Email Security:** While these practices may not be perfect at protecting against enumeration, the more enterprises that implement practices like the Sender Policy Framework (SPF) and DomainKeys Identified Mail (DKIM), the better we will be able to restrict who can and can't send email, which includes probing servers for email addresses.
![[Pasted image 20240205141159.png]]

### 6.6 Web-based Enumeration
1. identify web directories
	- Wordlist enumeration using `dirb`, `gobuster` etc.

What we've done so far is to check known or expected directories on a web server. Using a word list doesn't guarantee that you are going to identify all directories that are available on the server. If a directory isn't in the word list, it won't be identified. We can turn to another tool to help with fuzzing directory names, meaning generating names dynamically based on a set of rules. You may expect at this point that we would turn to Metasploit because it's so useful. You'd be correct. We can use the `brute_dirs` module. Using this module, you set a format for what a directory name could or should look like and the module will run through all possible names that match the format. Here you can see the options available for the module, followed by a format set. We're going to be testing against all words with lowercase characters whose lengths are between one and eight characters.

  
**brute_dirs Metasploit Module**  
```
msf> use auxiliary/scanner/http/brute_dirs  
msf auxiliary(scanner/http/brute_dirs)> info
```

**Enumerating Usernames in WordPress**  
```
msf auxiliary(scanner/http/wordpress_login_enum)> set BLANK_PASSWORDS true  
BLANK_PASSWORDS => true  
msf auxiliary(scanner/http/wordpress_login_enum)> set RHOSTS 192.168.86.52  
RHOSTS => 192.168.86.52  
msf auxiliary(scanner/http/wordpress_login_enum)> run
```

**Listing loot in msfconsole**  
`msf auxiliary(scanner/http/wordpress_login_enum)> loot`

- `wpscan` can be used from kali linux to enumerate plugins in wordpress

  
**Enumerating Plugins in WordPress**
`$ wpscan --url http://192.168.1.15`


#### Countermeasures

There is a lot of information that can be leaked from web servers. There are some techniques you can use to protect against web‐based enumeration, though.  

- **Restrict Information Provided:** Headers and error messages should not provide information about the server and especially version numbers, including information about the underlying operating system or application stack.
- **Use Appropriate Access Control:** Ensure all web applications have strong authentication enabled and appropriate access controls to protect resources from unauthorized users.
- **Disable Directory Listings:** Disable open directory listings from web servers to protect potentially sensitive files from open access.

##### Important Terms
#### Metasploit
An exploit framework that can be used for fast exploit development using building blocks. It can also be used for attacks using pre-developed exploits.

---
#### RMI
Remote method invocation. A Java application programming interface (API) that allows users to create distributed applications and communicate with them remotely.

---
#### RPC
Remote procedure call. A service that allows remote systems to consume procedures external to the application calling them.

---
#### SNMP
Simple Network Management Protocol. An Application layer protocol that facilitates the exchange of management information between network devices.

---
#### SMTP
Simple Mail Transfer Protocol. A set of communication guidelines that allow the software to transmit electronic mail over the Internet.

---
#### port scan
A technique for determining which network ports are open. It is useful for determining network security and firewall strength.

---
#### MIB
Management information base. A database used for managing the entities in a communication network.

---
#### NFS
Network File System. A distributed file system protocol originally developed by Sun Microsystems allowing a user on a client computer to access files over a computer network.

---
#### NetBIOS
An application programming interface (API) developed by IBM to extend the input/output capabilities away from the local system and onto the network.

---
#### dirb
A web content scanner that works by launching a dictionary-based attack against a web server and analyzing the response.


- Which tools allow you to create your own enumeration function based on ports being identified as open?
`nmap`

- What tool does a Java program need to use to implement remote process communication?
`rmic`

- What is the process Java programs identify themselves to if they are sharing procedures over the network?
`RMI registry`


- What are RPCs primarily used for?
`Interprocess communications`

- Which Metasploit module would you use to take advantage of potentially weaker permissions on an end user's workstation?
`auxiliary/scanner/smb/smb_enumshares`

- Which of these is **not** a way to protect against enumeration with SMB?
`Implement the latest NetBIOS patches`

- What underlying functionality does SMB need to enable Windows file sharing?
`RPC`


- Which of these is a built-in program on Windows for gathering information using SMB?
`nbstat`

- What would you be trying to enumerate if you were to use enum4linux?
`Shares and/or users`

- Which version of SNMP should network administrators be running?
`V3`

- What version of SNMP introduced encryption and user-based authentication?
`Version 3`

- Which SMTP command would be easiest to disable to prevent attackers from misusing it for enumeration?
`VRFY`

- If you try to use the VRFY command against an SMTP server and it fails, what status code will you get?
`550`

- You are working with a colleague and you see them interacting with an email server using the VRFY command. What is your colleague doing in the given scenario?
`Verifying email addresses`

- The utility dirb is used for what purpose?
`Directory Enumeration`


In this lesson, enumeration of different protocols was discussed using different tools. Including enumeration of SNMP(Simple Network Management Protocol), SMTP(Simple Mail Transfer Protocol), SMB(Server Message Block), and general web Application/Service enumeration such as directory brute forcing. The use of tools such as nmap was dicussed as well as the different arguments used for diferent purposes as well as scripts, dirbuster, dirb, snmpwalk, enum4linux, smbmap, or gobuster. The use of different useful metasploit modules was also introduced as well as different tools for windows/linux.





