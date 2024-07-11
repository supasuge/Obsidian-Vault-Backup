## Table of Contents

- [Linux Reconnaissance and Enumeration Cheat Sheet](#linux\reconnaissance\and\enumeration\cheat\sheet)
  - [1. whois](#1.\whois)
  - [2. dig (Domain Information Groper)](#2.\dig\(Domain\Information\Groper))
  - [3. nslookup](#3.\nslookup)
  - [4. ping](#4.\ping)
  - [5. traceroute](#5.\traceroute)
  - [6. netcat (nc)](#6.\netcat\(nc))
  - [7. nmap](#7.\nmap)
  - [8. theHarvester](#8.\theHarvester)
  - [9. Nikto](#9.\Nikto)
  - [10. SQLmap](#10.\SQLmap)
  - [11. Metasploit Framework](#11.\Metasploit\Framework)
  - [12. Wireshark](#12.\Wireshark)
  - [13. tcpdump](#13.\tcpdump)
  - [14. Hydra](#14.\Hydra)
  - [15. John the Ripper](#15.\John\the\Ripper)
  - [16. Aircrack-ng](#16.\Aircrack-ng)
  - [17. OWASP ZAP](#17.\OWASP\ZAP)
  - [18. Gobuster](#18.\Gobuster)
  - [19. Enum4linux](#19.\Enum4linux)
  - [20. DNSenum](#20.\DNSenum)

# Linux Reconnaissance and Enumeration Cheat Sheet

## 1. whois
Used for querying databases that store registered users or assignees of an Internet resource, such as a domain name, an IP address block, or an autonomous system.

```bash
whois example.com
```

## 2. dig (Domain Information Groper)
Used for querying DNS nameservers for information about host addresses, mail exchanges, nameservers, and related information.

```bash
dig example.com
dig +short example.com MX
dig +short example.com NS
dig @nameserver.example.com example.com AXFR
```

## 3. nslookup
Used to query Internet domain name servers.

```bash
nslookup example.com
nslookup -type=mx example.com
nslookup -type=ns example.com
```

## 4. ping
Used to check the connectivity status to a server.

```bash
ping example.com
```

## 5. traceroute
Traces the route an IP packet follows to an internet host.

```bash
traceroute example.com
```

## 6. netcat (nc)
Used for just about anything involving TCP, UDP, or UNIX-domain sockets.

```bash
nc -nv [IP address] [port]
```

## 7. nmap
Used for network discovery and security auditing.

```bash
nmap -v example.com
nmap -A -T4 example.com
nmap -sS -P0 -p- example.com
nmap -sV -p [port] example.com
```

## 8. theHarvester
Used for open-source intelligence (OSINT) and active reconnaissance.

```bash
theHarvester -d example.com -b google
```

## 9. Nikto
Web server scanner which performs comprehensive tests against web servers for multiple items.

```bash
nikto -h example.com
```

## 10. SQLmap
Automates the process of detecting and exploiting SQL injection flaws.

```bash
sqlmap -u "http://example.com/page.php?id=1"
```

## 11. Metasploit Framework
Used for developing and executing exploit code against a remote target machine.

```bash
msfconsole
use exploit/[module]
set RHOSTS example.com
exploit
```

## 12. Wireshark
Network protocol analyzer.

```bash
wireshark
```

## 13. tcpdump
Command-line packet analyzer.

```bash
tcpdump -i eth0
```

## 14. Hydra
Network logon cracker.

```bash
hydra -l user -P passlist.txt ftp://example.com
```

## 15. John the Ripper
Password cracking tool.

```bash
john --wordlist=wordlist.txt hashfile
```

## 16. Aircrack-ng
Suite of tools for wireless network security.

```bash
aircrack-ng capture.cap
```

## 17. OWASP ZAP
Open-source web application security scanner.

```bash
owasp-zap
```

## 18. Gobuster
Used to brute-force URIs (directories and files) in web sites and DNS subdomains.

```bash
gobuster dir -u http://example.com -w wordlist.txt
```

## 19. Enum4linux
Tool for enumerating information from Windows and Samba systems.

```bash
enum4linux -a target_ip
```

## 20. DNSenum
Script for enumerating DNS information.

```bash
dnsenum example.com
```
