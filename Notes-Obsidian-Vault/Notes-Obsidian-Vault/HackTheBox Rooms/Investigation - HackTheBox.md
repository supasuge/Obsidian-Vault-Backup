## Enumeration
**IP=10.10.11.197**
- **Nmap**
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-19 12:47 EDT
Nmap scan report for 10.10.11.197
Host is up (0.14s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 2f:1e:63:06:aa:6e:bb:cc:0d:19:d4:15:26:74:c6:d9 (RSA)
|   256 27:45:20:ad:d2:fa:a7:3a:83:73:d9:7c:79:ab:f3:0b (ECDSA)
|_  256 42:45:eb:91:6e:21:02:06:17:b2:74:8b:c5:83:4f:e0 (ED25519)
80/tcp open  http    Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://eforenzics.htb/
Service Info: Host: eforenzics.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.99 seconds

```

Next we add the domain found to `/etc/hosts`
```bash
echo '10.10.11.197 eforenzics.htb' | sudo tee -a /etc/hosts
```

Im gonna go ahead and check out the website on port 80 now, however I'm first going to startup burpsuite so that it is ready when I need it.

The first vulnerability presented here was the file upload. In which `exiftool` was being ran on the image. However the vulnerability here is that the mechanism is simply running the command `exiftool <image uploaded>`. So if you somehow escape the exiftool, or put it as `$()`, then you can get an RCE on the host. 
https://ine.com/blog/exiftool-command-injection-cve-2021-22204
https://github.com/cowsecurity/CVE-2022-23935

Using this we can get initial access as the `www-data` user. From here, I searched the filesystem for the different user with home directories, then I searched for any files belonging to that user we had access to by any chance. From there I found a Windows Event Log email. After moving that file back on to my machine and extracting the zip file using `binwalk`, I eventually found the password for the `smorton` user where he had accidentally put his password in the `username` field. Leading to a Windows Event Log failure code of `4625` and the entry being saved with the password in plaintext. 

Then, once we have `ssh` access to the remote machine and checking for any binaries we can run as `sudo`. There is an inconsipicous binary at `/usr/bin/binary`

![[Pasted image 20240420141258.png]]

After decompiling the binary, we can see that it is looking for a second argument of `lDnxUysaQn`. From here, it expect a first argument of the IP addressto send a curl request to in `"wb"` mode. Then it prints the param_2 (lDnxUysaQn) as the filename it writes to and calls `"perl ./%s"`. This means we can simply run any command we wan to escalate privileges using perl's `system()` command. `system("cp /bin/bash /tmp/bash && chmod u+s /tmp/bash");`. This command simply copy's bash to /tmp/bash and gives it SUID permissions so that you can run `/tmp/bash -p` and very easily escalate privileges to the root user and get the root flag.


