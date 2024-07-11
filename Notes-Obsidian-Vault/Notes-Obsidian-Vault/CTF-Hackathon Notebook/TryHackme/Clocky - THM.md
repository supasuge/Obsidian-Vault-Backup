
### Enumeration
**Nmap**
`nmap -p- -sCV --min-rate 10000 -Pn $IP`
Results: 
```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-04 02:35 EDT
Warning: 10.10.167.196 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.167.196
Host is up (0.11s latency).
Not shown: 49091 closed tcp ports (conn-refused), 16440 filtered tcp ports (no-response)
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 d9:42:e0:c0:d0:a9:8a:c3:82:65:ab:1e:5c:9c:0d:ef (RSA)
|   256 ff:b6:27:d5:8f:80:2a:87:67:25:ef:93:a0:6b:5b:59 (ECDSA)
|_  256 e1:2f:4a:f5:6d:f1:c4:bc:89:78:29:72:0c:ec:32:d2 (ED25519)
80/tcp   open  http       Apache httpd 2.4.41
|_http-title: 403 Forbidden
|_http-server-header: Apache/2.4.41 (Ubuntu)
8000/tcp open  http       nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-robots.txt: 3 disallowed entries 
|_/*.sql$ /*.zip$ /*.bak$
|_http-title: 403 Forbidden
8080/tcp open  http-proxy Werkzeug/2.2.3 Python/3.8.10
|_http-title: Clocky
|_http-server-header: Werkzeug/2.2.3 Python/3.8.10
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/2.2.3 Python/3.8.10
|     Date: Thu, 04 Apr 2024 02:36:40 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 6206
|     Connection: close
|     <head>
|     <style>
|     body {
|     background-color:#fff;
|     background-position:center;
|     background-repeat:repeat-y;
|     font-family: "Lucida Grande","Lucida Sans Unicode",geneva,verdana,sans-serif;
|     font-size:10px;
|     color:#777;
|     #container {
|     width:500px;
|     margin:0 auto;
|     #header {
|     background-color:#eeeeee;
|     margin:10px;
|     padding:30px 10px 30px 10px;
|     border-top:2px solid #ccc;
|     #header h1 {
|     text-align:center;
|     font-family:Trebuchet MS, Geneva, Arial, Helvetica, sans-serif;
|     font-size:30px;
|     color:#333;
|     margin:0;
|     font-weight:normal;
|     #header h1 strong {
|     color:#A85BA6;
|     #header h1 a {
|     color:#333;
|     text-decoration:none;
|     #header h2 {
|     font-size:11px;
|     font-weight:normal;
|     text-align:center;
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/2.2.3 Python/3.8.10
|     Date: Thu, 04 Apr 2024 02:36:40 GMT
|     Content-Type: text/html; charset=utf-8
|     Allow: GET, OPTIONS, HEAD
|     Content-Length: 0
|     Connection: close
|   RTSPRequest: 
|     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
|     "http://www.w3.org/TR/html4/strict.dtd">
|     <html>
|     <head>
|     <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: HTTPStatus.BAD_REQUEST - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.94SVN%I=7%D=4/4%Time=660E4A78%P=x86_64-pc-linux-gnu%r(
SF:GetRequest,18ED,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/2\.2\.3\
SF:x20Python/3\.8\.10\r\nDate:\x20Thu,\x2004\x20Apr\x202024\x2002:36:40\x2
SF:0GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:
SF:\x206206\r\nConnection:\x20close\r\n\r\n<head>\n<style>\n\nbody\x20{\n\
SF:x20\x20background-color:#fff;\n\x20\x20\n\x20\x20background-position:ce
SF:nter;\n\x20\x20background-repeat:repeat-y;\n\x20\x20font-family:\x20\"L
SF:ucida\x20Grande\",\"Lucida\x20Sans\x20Unicode\",geneva,verdana,sans-ser
SF:if;\n\x20\x20font-size:10px;\n\x20\x20color:#777;\n}\n#container\x20{\n
SF:\x20\x20width:500px;\n\x20\x20margin:0\x20auto;\n}\n\n#header\x20{\n\x2
SF:0\x20background-color:#eeeeee;\n\x20\x20margin:10px;\n\x20\x20padding:3
SF:0px\x2010px\x2030px\x2010px;\n\x20\x20border-top:2px\x20solid\x20#ccc;\
SF:n}\n\n#header\x20h1\x20{\n\x20\x20text-align:center;\n\x20\x20font-fami
SF:ly:Trebuchet\x20MS,\x20Geneva,\x20Arial,\x20Helvetica,\x20sans-serif;\n
SF:\x20\x20font-size:30px;\n\x20\x20color:#333;\n\x20\x20margin:0;\n\x20\x
SF:20font-weight:normal;\n}\n#header\x20h1\x20strong\x20{\n\x20\x20color:#
SF:A85BA6;\n}\n#header\x20h1\x20a\x20{\n\x20\x20color:#333;\n\x20\x20text-
SF:decoration:none;\n}\n#header\x20h2\x20{\n\x20\x20font-size:11px;\n\x20\
SF:x20font-weight:normal;\n\x20\x20text-align:center;\n")%r(HTTPOptions,C7
SF:,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/2\.2\.3\x20Python/3\.8\
SF:.10\r\nDate:\x20Thu,\x2004\x20Apr\x202024\x2002:36:40\x20GMT\r\nContent
SF:-Type:\x20text/html;\x20charset=utf-8\r\nAllow:\x20GET,\x20OPTIONS,\x20
SF:HEAD\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(RTSPRe
SF:quest,1F4,"<!DOCTYPE\x20HTML\x20PUBLIC\x20\"-//W3C//DTD\x20HTML\x204\.0
SF:1//EN\"\n\x20\x20\x20\x20\x20\x20\x20\x20\"http://www\.w3\.org/TR/html4
SF:/strict\.dtd\">\n<html>\n\x20\x20\x20\x20<head>\n\x20\x20\x20\x20\x20\x
SF:20\x20\x20<meta\x20http-equiv=\"Content-Type\"\x20content=\"text/html;c
SF:harset=utf-8\">\n\x20\x20\x20\x20\x20\x20\x20\x20<title>Error\x20respon
SF:se</title>\n\x20\x20\x20\x20</head>\n\x20\x20\x20\x20<body>\n\x20\x20\x
SF:20\x20\x20\x20\x20\x20<h1>Error\x20response</h1>\n\x20\x20\x20\x20\x20\
SF:x20\x20\x20<p>Error\x20code:\x20400</p>\n\x20\x20\x20\x20\x20\x20\x20\x
SF:20<p>Message:\x20Bad\x20request\x20version\x20\('RTSP/1\.0'\)\.</p>\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20<p>Error\x20code\x20explanation:\x20HTTPS
SF:tatus\.BAD_REQUEST\x20-\x20Bad\x20request\x20syntax\x20or\x20unsupporte
SF:d\x20method\.</p>\n\x20\x20\x20\x20</body>\n</html>\n");
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 143.79 seconds
```
- That is a lot of shit. And I do mean, **shit**.
Couple things to note...
- Port 8080 Werkzeug/2.2.3 Python/3.8.10
- Port 8000: nginx/1.18.0
- Port 80: Apache 2.4.41



Directory Enumeration:
`gobuster`, `feroxbuster`
```bash
feroxbuster --smart --url http://$IP:8080/
                                                                             
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.167.196:8080/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏦  Collect Backups       │ true
 🤑  Collect Words         │ true
 🏁  HTTP methods          │ [GET]
 🎶  Auto Tune             │ true
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        5l       31w      207c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      309l      666w     6206c http://10.10.167.196:8080/
200      GET       54l      108w     1609c http://10.10.167.196:8080/administrator
302      GET        5l       22w      215c http://10.10.167.196:8080/dashboard => http://10.10.167.196:8080/administrator
200      GET       53l      103w     1516c http://10.10.167.196:8080/forgot_password
200      GET        1l        2w       26c http://10.10.167.196:8080/password_reset
[####################] - 4m     30098/30098   0s      found:5       errors:0      
[####################] - 4m     30072/30072   132/s   http://10.10.167.196:8080/  
```
DNS/VHost Enumeration:
`ffuf`, `gobuster`




