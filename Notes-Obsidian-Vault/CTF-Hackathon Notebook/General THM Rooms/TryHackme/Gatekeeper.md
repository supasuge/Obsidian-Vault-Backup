## Table of Contents

    - [!1. Nmap --](#!1.\Nmap\--)
      - [SMB Enumeration](#SMB\Enumeration)
      - [Transfer gatekeeper.exe to Windows Host](#Transfer\gatekeeper.exe\to\Windows\Host)

### !1. Nmap --
```bash
└─$ nmap -p- --min-rate 10000 10.10.226.167   

135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 94.91 seconds
                                                                                
┌──(kali㉿kali)-[~]
└─$ nmap -p 135,139,445,3389 -sCV -Pn 10.10.226.167
135/tcp  open  msrpc              Microsoft Windows RPC
139/tcp  open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp  open                     Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)







3389/tcp open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: GATEKEEPER
|   NetBIOS_Domain_Name: GATEKEEPER
|   NetBIOS_Computer_Name: GATEKEEPER
|   DNS_Domain_Name: gatekeeper
|   DNS_Computer_Name: gatekeeper
|   Product_Version: 6.1.7601
|_  System_Time: 2023-10-29T23:15:48+00:00
|_ssl-date: 2023-10-29T23:15:53+00:00; +3s from scanner time.
| ssl-cert: Subject: commonName=gatekeeper
| Not valid before: 2023-10-28T23:03:47
|_Not valid after:  2024-04-28T23:03:47
Service Info: Host: GATEKEEPER; OS: Windows; CPE: cpe:/o:microsoft:windows




Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2023-10-29T23:15:48
|_  start_date: 2023-10-29T23:03:30
|_clock-skew: mean: 48m03s, deviation: 1h47m19s, median: 3s
|_nbstat: NetBIOS name: GATEKEEPER, NetBIOS user: <unknown>, NetBIOS MAC: 02:d4:11:e9:4f:39 (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: gatekeeper
|   NetBIOS computer name: GATEKEEPER\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-10-29T19:15:48-04:00
| smb2-security-mode: 
|   2:1:0: 
|_    Message signing enabled but not required
```

#### SMB Enumeration
```bash
smbclient -L \\MACHINE_IP

smbclient \\\\MACHINE_IP\Users

get Share/gatekeeper.exe
```


#### Transfer gatekeeper.exe to Windows Host
- A) use shared folder between host macchien and VM.
- B) Below
```bash
python3 -m http.server 80    #1.1.1.1:80 for example purposes
```
	From Windows
```powershell
certutil -urlcache -f http://HOME_INET/filename filename.output
```