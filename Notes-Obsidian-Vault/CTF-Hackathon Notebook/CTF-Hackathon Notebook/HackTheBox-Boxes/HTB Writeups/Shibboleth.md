## Table of Contents


Nmap _----_
```
0/tcp open  http    Apache httpd 2.4.41
|_http-title: Did not follow redirect to http://shibboleth.htb/
|_http-server-header: Apache/2.4.41 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: phone|general purpose|proxy server|WAP|webcam
Running (JUST GUESSING): Google Android 4.4.X (92%), Linux 4.X|5.X|2.6.X|3.X (91%), WebSense embedded (91%), Linksys embedded (90%), AXIS embedded (89%)
OS CPE: cpe:/o:google:android:4.4.0 cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:linux:linux_kernel cpe:/o:linux:linux_kernel:2.6.32 cpe:/o:linux:linux_kernel:3 cpe:/h:linksys:ea3500
Aggressive OS guesses: Android 4.4.0 (92%), Linux 4.15 - 5.8 (91%), Linux 5.3 - 5.4 (91%), Websense Content Gateway (91%), Linux 2.6.32 (91%), Linux 5.0 - 5.5 (90%), Linux 3.6 - 3.10 (90%), Linksys EA3500 WAP (90%), Axis M3006-V network camera (89%), Cisco CP-DX80 collaboration endpoint (Android) (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: shibboleth.htb
```
https://book.hacktricks.xyz/network-services-pentesting/623-udp-ipmi

`IPMI`) is a set of standardized specifications for hardware-based host management systems used for system management and monitoring. It acts as an autonomous subsystem and works independently of the host's BIOS, CPU, firmware, and underlying operating system. IPMI provides sysadmins with the ability to manage and monitor systems even if they are powered off or in an unresponsive state. It operates using a direct network connection to the system's hardware and does not require access to the operating system via a login shell. IPMI can also be used for remote upgrades to systems without requiring physical access to the target host. IPMI is typically used in three ways:

- Before the OS has booted to modify BIOS settings
    

- When the host is fully powered down
    

- Access to a host after a system failure