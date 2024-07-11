The [File Transfer Protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol) (`FTP`) is a standard network protocol used to transfer files between computers. It also performs directory and files operations, such as changing the working directory, listing files, and renaming and deleting directories or files. By default, FTP listens on port `TCP/21`.


To attack an FTP Server, we can abuse misconfiguration or excessive privileges, exploit known vulnerabilities or discover new vulnerabilities. Therefore, after gaining access to the FTP Service, we need to be aware of the content in the directory so we can search for sensitive or critical information, as we previously discussed.


## Enumeration

`Nmap` default scripts `-sC` includes the [ftp-anon](https://nmap.org/nsedoc/scripts/ftp-anon.html) Nmap script which checks if a FTP server allows anonymous logins. The version enumeration flag `-sV` provides interesting information about FTP services, such as the FTP banner, which often includes the version name. We can use the `ftp` client or `nc` to interact with the FTP service. By default, FTP runs on TCP port 21.

#### Nmap

Attacking FTP

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap -sC -sV -p 21 192.168.2.142 

Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-10 22:04 EDT
Nmap scan report for 192.168.2.142
Host is up (0.00054s latency).

PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--   1 1170     924            31 Mar 28  2001 .banner
| d--x--x--x   2 root     root         1024 Jan 14  2002 bin
| d--x--x--x   2 root     root         1024 Aug 10  1999 etc
| drwxr-srwt   2 1170     924          2048 Jul 19 18:48 incoming [NSE: writeable]
| d--x--x--x   2 root     root         1024 Jan 14  2002 lib
| drwxr-sr-x   2 1170     924          1024 Aug  5  2004 pub
|_Only 6 shown. Use --script-args ftp-anon.maxlist=-1 to see all.
```

---

## Misconfigurations

As we discussed, anonymous authentication can be configured for different services such as FTP. To access with anonymous login, we can use the `anonymous` username and no password. This will be dangerous for the company if read and write permissions have not been set up correctly for the FTP service. Because with the anonymous login, the company could have stored sensitive information in a folder that the anonymous user of the FTP service could have access to.

This would enable us to download this sensitive information or even upload dangerous scripts. Using other vulnerabilities, such as path traversal in a web application, we would be able to find out where this file is located and execute it as PHP code, for example.

### Protocol Specifiecs Attacks
Many different attacks and methods are protocol-based. However, it is essential to note we aren't attacking the individual protocols themselves but the services that use them. 

Since there are dozens of services for a single protocol and they process the corresponding information differently, we will look at some.

#### Brute Forcing

If there is no anonymous authentication available, we can also brute-force the login for the FTP services using a list of the pre-generated usernames and passwords. There are many different tools to perform a brute-forcing attack. Let us explore one of them, [Medusa](https://github.com/jmk-foofus/medusa). With `Medusa`, we can use the option `-u` to specify a single user to target, or you can use the option `-U` to provide a file with a list of usernames. The option `-P` is for a file containing a list of passwords. We can use the option `-M` and the protocol we are targeting (FTP) and the option `-h` for the target hostname or IP address.

**Note:** Although we may find services vulnerable to brute force, most applications today prevent these types of attacks. A more effective method is Password Spraying.

#### Brute Forcing with Medusa

Attacking FTP

```shell-session
gdxqpardo@htb[/htb]$ medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp 
                                                             
Medusa v2.2 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>                                                      
ACCOUNT CHECK: [ftp] Host: 10.129.203.7 (1 of 1, 0 complete) User: fiona (1 of 1, 0 complete) Password: 123456 (1 of 14344392 complete)
ACCOUNT CHECK: [ftp] Host: 10.129.203.7 (1 of 1, 0 complete) User: fiona (1 of 1, 0 complete) Password: 12345 (2 of 14344392 complete)
ACCOUNT CHECK: [ftp] Host: 10.129.203.7 (1 of 1, 0 complete) User: fiona (1 of 1, 0 complete) Password: 123456789 (3 of 14344392 complete)
ACCOUNT FOUND: [ftp] Host: 10.129.203.7 User: fiona Password: family [SUCCESS]
```

#### FTP Bounce Attack

An FTP bounce attack is a network attack that uses FTP servers to deliver outbound traffic to another device on the network. The attacker uses a `PORT` command to trick the FTP connection into running commands and getting information from a device other than the intended server.

Consider we are targetting an FTP Server `FTP_DMZ` exposed to the internet. Another device within the same network, `Internal_DMZ`, is not exposed to the internet. We can use the connection to the `FTP_DMZ` server to scan `Internal_DMZ` using the FTP Bounce attack and obtain information about the server's open ports. Then, we can use that information as part of our attack against the infrastructure.

![text](https://academy.hackthebox.com/storage/modules/116/ftp_bounce_attack.png) Source: [https://www.geeksforgeeks.org/what-is-ftp-bounce-attack/](https://www.geeksforgeeks.org/what-is-ftp-bounce-attack/)

The `Nmap` -b flag can be used to perform an FTP bounce attack:

Attacking FTP

```shell-session
gdxqpardo@htb[/htb]$ nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2

Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-27 04:55 EDT
Resolved FTP bounce attack proxy to 10.10.110.213 (10.10.110.213).
Attempting connection to ftp://anonymous:password@10.10.110.213:21
Connected:220 (vsFTPd 3.0.3)
Login credentials accepted by FTP server!
Initiating Bounce Scan at 04:55
FTP command misalignment detected ... correcting.
Completed Bounce Scan at 04:55, 0.54s elapsed (1 total ports)
Nmap scan report for 172.17.0.2
Host is up.

PORT   STATE  SERVICE
80/tcp open http

<SNIP>
```

Modern FTP servers include protections that, by default, prevent this type of attack, but if these features are misconfigured in modern-day FTP servers, the server can become vulnerable to an FTP Bounce attack.

When you spawn your target, please wait up to 60 more seconds after seeing the IP address to ensure the corresponding service is launched correctly.


## Latest FTP Vulnerabilities
In this case, we will discuss the `CoreFTP before build 727` vulnerability assigned [CVE-2022-22836](https://nvd.nist.gov/vuln/detail/CVE-2022-22836). This vulnerability is for an FTP service that does not correctly process the `HTTP PUT` request and leads to an `authenticated directory`/`path traversal,` and `arbitrary file write` vulnerability. This vulnerability allows us to write files outside the directory to which the service has access.

---

## The Concept of the Attack

This FTP service uses an HTTP `POST` request to upload files. However, the CoreFTP service allows an HTTP `PUT` request, which we can use to write content to files. Let's have a look at the attack based on our concept. The [exploit](https://www.exploit-db.com/exploits/50652) for this attack is relatively straightforward, based on a single `cURL` command.

#### CoreFTP Exploitation

Latest FTP Vulnerabilities

```shell-session
gdxqpardo@htb[/htb]$ curl -k -X PUT -H "Host: <IP>" --basic -u <username>:<password> --data-binary "PoC." --path-as-is https://<IP>/../../../../../../whoops
```

We create a raw HTTP `PUT` request (`-X PUT`) with basic auth (`--basic -u <username>:<password>`), the path for the file (`--path-as-is https://<IP>/../../../../../whoops`), and its content (`--data-binary "PoC."`) with this command. Additionally, we specify the host header (`-H "Host: <IP>"`) with the IP address of our target system.

#### The Concept of Attacks

![](https://academy.hackthebox.com/storage/modules/116/attack_concept2.png)

In short, the actual process misinterprets the user's input of the path. This leads to access to the restricted folder being bypassed. As a result, the write permissions on the HTTP `PUT` request are not adequately controlled, which leads to us being able to create the files we want outside of the authorized folders. However, we will skip the explanation of the `Basic Auth` process and jump directly to the first part of the exploit.

#### Directory Traversal

|**Step**|**Directory Traversal**|**Concept of Attacks - Category**|
|---|---|---|
|`1.`|The user specifies the type of HTTP request with the file's content, including escaping characters to break out of the restricted area.|`Source`|
|`2.`|The changed type of HTTP request, file contents, and path entered by the user are taken over and processed by the process.|`Process`|
|`3.`|The application checks whether the user is authorized to be in the specified path. Since the restrictions only apply to a specific folder, all permissions granted to it are bypassed as it breaks out of that folder using the directory traversal.|`Privileges`|
|`4.`|The destination is another process that has the task of writing the specified contents of the user on the local system.|`Destination`|

Up to this point, we have bypassed the constraints imposed by the application using the escape characters (`../../../../`) and come to the second part, where the process writes the contents we specify to a file of our choice. This is when the cycle starts all over again, but this time to write contents to the target system.

#### Arbitrary File Write

|**Step**|**Arbitrary File Write**|**Concept of Attacks - Category**|
|---|---|---|
|`5.`|The same information that the user entered is used as the source. In this case, the filename (`whoops`) and the contents (`--data-binary "PoC."`).|`Source`|
|`6.`|The process takes the specified information and proceeds to write the desired content to the specified file.|`Process`|
|`7.`|Since all restrictions were bypassed during the directory traversal vulnerability, the service approves writing the contents to the specified file.|`Privileges`|
|`8.`|The filename specified by the user (`whoops`) with the desired content (`"PoC."`) now serves as the destination on the local system.|`Destination`|


