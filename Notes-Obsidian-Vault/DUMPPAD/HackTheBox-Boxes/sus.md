## Table of Contents

- [DNS Enumeration](#dns\enumeration)
- [SMB Enumeration](#smb\enumeration)
- [Stocker HTB](#stocker\htb)

domain:         iskrasistemi.si
registrar:      Telekom Slovenije, d.d.
registrar-url:  https://domene.telekom.si
nameserver:     uranus.iskrasistemi.si (193.201.45.52)
nameserver:     jupiter.iskrasistemi.si (193.201.45.51)
registrant:     G38304
status:         ok
created:        1996-09-16
expire:         2024-06-06


Server:         10.0.2.3
Address:        10.0.2.3#53

Non-authoritative answer:
Name:   fwupdate.iskrasistemi.si
Address: 13.81.106.17









# DNS Enumeration
```bash
nslookup <domain>
dig axfr @<IP>
```


# SMB Enumeration
```bash
smbmap -u "" -p "" -P 445 -H 10.10.11.222
smbmap -g "guest" -p "" -P 445 -H 10.10.11.222

sudo mount -t cifs -o username=cifs_share_user //10.10.11.222/Development /home/kali/Documents/Machines/HTB/Authority/mount/
```





















# Stocker HTB

```bash
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-14 20:58 EDT
Nmap scan report for 10.10.11.196
Host is up (0.15s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3d:12:97:1d:86:bc:16:16:83:60:8f:4f:06:e6:d5:4e (RSA)
|   256 7c:4d:1a:78:68:ce:12:00:df:49:10:37:f9:ad:17:4f (ECDSA)
|_  256 dd:97:80:50:a5:ba:cd:7d:55:e8:27:ed:28:fd:aa:3b (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://stocker.htb
|_http-server-header: nginx/1.18.0 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.8 (96%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   88.12 ms  10.10.16.1
2   205.36 ms 10.10.11.196

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.49 seconds
```



`authority.htb.corp`

pWm_@dm!N_!23

svc_ldap
lDaP_1n_th3_cle4r!