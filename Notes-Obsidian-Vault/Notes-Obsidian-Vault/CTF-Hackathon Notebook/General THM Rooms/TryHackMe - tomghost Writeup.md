## Table of Contents

  - [Recon/Enumeration](#Recon/Enumeration)
      - [Services](#Services)
  - [Shell as User](#Shell\as\User)

## Recon/Enumeration
```bash
nmap -p- --open --min-rate 10000 --open 10.10.92.91              
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-02 10:31 EST
Nmap scan report for 10.10.92.91
Host is up (0.13s latency).
Not shown: 65454 filtered tcp ports (no-response), 78 closed tcp ports (conn-refused)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE
22/tcp   open  ssh
53/tcp   open  domain
8080/tcp open  http-proxy
```

```bash
nmap -p22,53,8080 -sCV 10.10.92.91
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-01-02 10:32 EST
Nmap scan report for 10.10.92.91
Host is up (0.13s latency).

PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f3:c8:9f:0b:6a:c5:fe:95:54:0b:e9:e3:ba:93:db:7c (RSA)
|   256 dd:1a:09:f5:99:63:a3:43:0d:2d:90:d8:e3:e1:1f:b9 (ECDSA)
|_  256 48:d1:30:1b:38:6c:c6:53:ea:30:81:80:5d:0c:f1:05 (ED25519)
53/tcp   open  tcpwrapped
8080/tcp open  http       Apache Tomcat 9.0.30
|_http-title: Apache Tomcat/9.0.30
|_http-favicon: Apache Tomcat
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.73 seconds
```
#### Services
- `OpenSSH 7.2p2 Ubuntu 4ubuntu2.8`
- `Apache Tomcat 9.0.30`

I went ahead and starting searching for Apache Tomcat vulnerabilities at this point. and in the mean time ran a quick `gobuster` directory enumeration scan. 
## Shell as User

















































