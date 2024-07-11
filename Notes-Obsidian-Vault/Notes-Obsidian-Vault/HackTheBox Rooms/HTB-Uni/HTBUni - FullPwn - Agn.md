## Table of Contents

          - [banner grabbing from Port 3000](#banner\grabbing\from\Port\3000)
          - [Ladies and Gentlemen... We Got em!](#Ladies\and\Gentlemen...\We\Got\em!)

```zsh
┌──(yaditydo@dumbpad)-[~/Desktop/CTFs/HTBUniversity2023/Full-Pwn]
└─$nmap -p- --min-rate 10000 --open 10.129.241.86 -oN ag-ports.sc
Starting Nmap 7.80 ( https://nmap.org ) at 2023-12-08 14:27 EST
Nmap scan report for 10.129.241.86
Host is up (0.042s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
3000/tcp open  ppp
````

**Ports Open**
- 22
- 80
- 3000

###### banner grabbing from Port 3000
- Jetty looks interesting
```bash
nc -nv 10.129.241.86 3000
Connection to 10.129.241.86 3000 port [tcp/*] succeeded!
GET / HTTP1.1
HTTP/1.1 505 HTTP Version Not Supported
Content-Type: text/html;charset=iso-8859-1
Content-Length: 58
Connection: close
Server: Jetty(11.0.14)
```



```zsh
nmap -p3000 -sCV 10.129.241.86 -oN a-p3k.scan
Starting Nmap 7.80 ( https://nmap.org ) at 2023-12-08 14:36 EST
Nmap scan report for 10.129.241.86
Host is up (0.041s latency).

PORT     STATE SERVICE VERSION
3000/tcp open  http    Jetty 11.0.14
|_http-server-header: Jetty(11.0.14)
|_http-title: Metabase

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.89 seconds
```
###### Ladies and Gentlemen... We Got em!
`'unsafe-eval'`

![[Pasted image 20231208144115.png]]
