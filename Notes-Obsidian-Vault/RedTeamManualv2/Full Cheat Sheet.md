## Table of Contents

  - [Common TCP and UDP Ports](#Common\TCP\and\UDP\Ports)
    - [TCP Ports](#TCP\Ports)
    - [UDP Ports](#UDP\Ports)
  - [Active Directory (AD) Services Ports](#Active\Directory\(AD)\Services\Ports)
    - [Domain Controller Indicators](#Domain\Controller\Indicators)
  - [Network Scanning Commands](#Network\Scanning\Commands)
    - [ARP Scan](#ARP\Scan)
    - [Service Scan](#Service\Scan)
  - [File Transfer Protocols](#File\Transfer\Protocols)
    - [FTP Commands](#FTP\Commands)
    - [SSH Commands](#SSH\Commands)
    - [SMTP Commands](#SMTP\Commands)
    - [SNMP Commands](#SNMP\Commands)
    - [SMB Commands](#SMB\Commands)
    - [Windows Shares](#Windows\Shares)
    - [LDAP Commands](#LDAP\Commands)
    - [Kerberos Commands](#Kerberos\Commands)
    - [RPC Commands](#RPC\Commands)
    - [SQL Commands](#SQL\Commands)
    - [NFS Commands](#NFS\Commands)
    - [RDP Commands](#RDP\Commands)
    - [VNC Commands](#VNC\Commands)

## Common TCP and UDP Ports
#commonports #tcpports
### TCP Ports
- **21**: FTP (File Transfer Protocol)
- **22**: SSH (Secure Shell)
- **23**: Telnet
- **25**: SMTP (Simple Mail Transfer Protocol)
- **80**: HTTP (Hypertext Transfer Protocol)
- **110**: POP3 (Post Office Protocol 3)
- **111**: ONC RPC (Open Network Computing Remote Procedure Call)
- **143**: IMAP (Internet Message Access Protocol)
- **443**: HTTPS (HTTP Secure)
- **139 & 445**: SMB (Server Message Block)
- **1433**: MSSQL (Microsoft SQL Server)
- **1978**: WiFi Mouse
- **2049**: NFS (Network File System)
- **3306**: MySQL
- **3389**: Windows Remote Desktop (RDP)
- **5900**: VNC (Virtual Network Computing)
- **5985**: WinRM HTTP (Windows Remote Management)
- **5986**: WinRM HTTPS

### UDP Ports
#udpports
- **53**: DNS (Domain Name System)
- **67 & 68**: DHCP (Dynamic Host Configuration Protocol)
- **69**: TFTP (Trivial File Transfer Protocol)
- **161**: SNMP (Simple Network Management Protocol)

## Active Directory (AD) Services Ports
#AD #activedirectory 
- **53**: DNS
- **88**: Kerberos Authentication
- **135**: WMI RPC (Windows Management Instrumentation)
- **138, 139, 445**: SMB
- **389**: LDAP (Lightweight Directory Access Protocol)
- **636**: LDAPS (LDAP over SSL)
- **5355**: LLMNR (Link-Local Multicast Name Resolution)

### Domain Controller Indicators
#domaincontrollers
- Ports: **53**, **88**, **389** (**LDAP**), **636** (**LDAPS**)
- File: `%SYSTEMROOT%\NTDS\NTDS.dit` (contains user password hashes)

## Network Scanning Commands
### ARP Scan
- `arp-scan -l [range]`
- `netdiscover -r [range]`

### Service Scan
- `autorecon [targets] -v`
- `nmap -p- -T4 -sC -sV -vv [targets]`

## File Transfer Protocols

### FTP Commands
- `wget -m ftp://[username]:[password]@[host]` → Download all files
- `ftp [host]` or `ftp [username]@[host]` → Connect to FTP
- Common Commands: `ls`, `binary`, `ascii`, `put [file]`, `get [file]`, `mget *`, `close`

### SSH Commands
- `ssh [domain]\\[username]@[host] -p [port]` → Connect via SSH
- `hydra -l [username] -P [wordlist] -s [port] ssh://[host]` → SSH brute force

### SMTP Commands
- `ismtp -h [host]:25 -e [wordlist] -l 3` → SMTP enumeration
- `sendemail -s [host] -xu [username] -xp [password] -f [from] -t [to] -u [subject] -m [message] -a [attachment]`
- `swaks --server [host] -au [username] -ap [password] -f [from] -t [to] --h-Subject [subject] --body [message] --attach @[attachment] -n`

### SNMP Commands
- `hydra -P [wordlist] -v [host] snmp`
- `snmp-check -c [community] [ip]`
- `snmpwalk -c [community] -v [version] [host] NET-SNMP-EXTEND-MIB::nsExtendOutputFull`

### SMB Commands
- `nbtscan -r [range]`
- `enum4linux -v -a [host]`
- `crackmapexec smb [host] -u [username] -p [password] --rid-brute`

### Windows Shares
- `dir \\[domain or ip]\[share] /user:[username] [password]`
- `net use [drive letter]: \\[domain]\[share] /user:[username] [password] /persistent:yes`

### LDAP Commands
- `nmap --script=ldap* [host]`
- `ldapdomaindump ldap://[host] -u '[domain]\[user]' -p [password] -o [dir]`
- `ldapsearch -x -H ldap://[host] -b base namingcontexts`

### Kerberos Commands
- `kerbrute userenum --dc [DC] -d [domain] [userlist]`
- `kerbrute passwordspray --dc [DC] -d [domain] [userlist] [password]`

### RPC Commands
- `rpcclient -N -U "" [host]`
- `rpcclient -U [domain]/[user]%[password] [host]`

### SQL Commands
- **MySQL**: `mysql -h [host] -P [port] -u [username] -p'[password]'`
- **MSSQL**: `impacket-mssqlclient [domain]/[username]:[password]@[host] -port [port] -windows-auth`

### NFS Commands
- `rpcinfo -p [host]`
- `showmount -e [host]`
- `mount [host]:[share] /mnt/[dir]`
- `unmount /mnt/[dir]`

### RDP Commands
- `xfreerdp /u:[domain]\\[username] /p:[password] /v:[host] +clipboard /drive:[Windows share name],[kali folder]`
- `hydra -l [username] -P [wordlist] -s [port] rdp://[host]`

### VNC Commands
- `vncviewer [host]:[port] -passwd [password file]`
- `hydra -s [port] -P [wordlist] -t 4 [host] vnc`

