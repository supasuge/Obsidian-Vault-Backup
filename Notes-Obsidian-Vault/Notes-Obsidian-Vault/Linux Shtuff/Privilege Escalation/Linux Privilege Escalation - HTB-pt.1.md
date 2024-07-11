## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Enumeration](#Enumeration)
          - [List Current Processes](#List\Current\Processes)
  - [Getting started with some situational awareness](#Getting\started\with\some\situational\awareness)
          - [Commands](#Commands)
      - [How privileges are created and delegated in Linux](#How\privileges\are\created\and\delegated\in\Linux)
      - [All Hidden Files](#All\Hidden\Files)
      - [All Hidden Directories](#All\Hidden\Directories)
    - [(3)Default Folders for temporary files.](#(3)Default\Folders\for\temporary\files.)
  - [Linux Services and Internals Enumeration](#Linux\Services\and\Internals\Enumeration)
      - [Hosts](#Hosts)
      - [Users last Login](#Users\last\Login)
      - [Logged In Users](#Logged\In\Users)
      - [Finding History Files](#Finding\History\Files)
      - [Cron](#Cron)

## Table of Contents

- [Enumeration](#Enumeration)
          - [List Current Processes](#List\Current\Processes)
  - [Getting started with some situational awareness](#Getting\started\with\some\situational\awareness)
          - [Commands](#Commands)
      - [How privileges are created and delegated in Linux](#How\privileges\are\created\and\delegated\in\Linux)
      - [All Hidden Files](#All\Hidden\Files)
      - [All Hidden Directories](#All\Hidden\Directories)
    - [(3)Default Folders for temporary files.](#(3)Default\Folders\for\temporary\files.)
  - [Linux Services and Internals Enumeration](#Linux\Services\and\Internals\Enumeration)
      - [Hosts](#Hosts)
      - [Users last Login](#Users\last\Login)
      - [Logged In Users](#Logged\In\Users)
      - [Finding History Files](#Finding\History\Files)
      - [Cron](#Cron)


- If a linux machine is domain joined and you have `root` we can get the NTLM Hash and begin enumerating and attacking Active Directory.
### Enumeration
Enumeration is the key to privilege escalation. Several helper scripts (such as [LinEnum](https://github.com/rebootuser/LinEnum)) exist to assist with enumeration

-  OS Version
	- Knowing the distribution will give you anidea of the types of tools that may be available. This will help to find publicly available exploits 
-  Kernel Version
	- Public Exploits 
-  Running Services
	- Knowing the services running is **crucial**. Especially the processes that are running as `root`. A misconfigured or vulnerable service running as root can be an easy win for privilege escalation. 
-  Installed Packages and Versions:
	- Import to check for any out-of-date or vulnerable packages that may be easily leveraged for privilege escalation. 
-  Logged in Users: Knowing which other users are logged into the system and what they are doing
-  User Home Directories
###### List Current Processes
```bash
ps aux | grep root
```

## Getting started with some situational awareness
- `whoami` - user we are running under
- `id` - what groups
- `hostname` - what is the server named
- `ifconfig` or `ip -a` - what subnet did we land in, does the host have additional NIC's in other subnets?
- `sudo -l` - view permissions

###### Commands
```bash
ps aux #list current processes

ps aux | grep root # list current processes running as root

cat /etc/os-release #Kernel/OS info

echo $PATH #check the current user's PATH

env #check environment variables

uname -a #note the kernel version for public exploits

cat /proc/version #note the kernel version for public exploits

lscpu  #Gather info about the host itself and the CPU type/version

lsblk -l

cat /etc/shells  #what login shells exist on the server?

cat /etc/fstab # Check for credentials in any mounted dr5ives
route
          # see what other networks are                # available
netstat -rn

arp -a   #check the ARP Table to see what other hosts the target has been communicating with

cat /etc/passwd    #check for existing users
```
- In a domain environment make sure to check:
`/etc/resolv.conf` to see if the host is configured to use internal DNS may be able to use this as a starting point to query the Active Directory environment.

- Environment enumeration also includes knowledge about the users that exist on the target system. This is because individual users are often configured during the installation of applications and services to limit the service's privileges. The reason for this is to maintain the security of the system itself. If a service is running with the highest privileges (root) and this is brought under control by an attacker, the attacker automatically has the highest rights over the entire system. 

#### How privileges are created and delegated in Linux
- All users on the system are stored in `/etc/passwd`
	- Format is as follows:
	1. Username
	2. Password
	3. User ID
	4. Group ID
	5. User ID Info
	6. Home Directory
	7. Shell
```bash

rw-r--r--
^
rw- : Permissions for User Owner
r-- : Permissions for Group Owner
r-- : Permissions for Other

'-' is reserved as a special permission notation marker that can vary. 
```
- Another special permission that can be used here is known as `SUID`, or `GUID` or the sticky bit.
**Permission Groups**:
- Owner
- Groups
- All Users
**Permission Types**:
- Read = `4`
- Write = `2`
- Execute = `1`
- No Perms set = `0`
`- rw-r-r- 1 root root 2107 Aug22 2017 /etc/passwd`
`-`: File Type
`rw-r-r-`: Permissions
`1`: Hard Link count
`root`: User Owner
`root`: Group Owner
`Aug22 2017`: Modification Timestamp
`/etc/passwd`: File Name
**Special Pemissions**(First Bit in permissions):
- `_`: No special permissions set
- `d`: Directory
- `l`: file has symbolic links
- `s`: setuid or setgid is set
- `t`: sticky bit set

Some Defense tools to particularly look out for:
- [Exec Shield](https://en.wikipedia.org/wiki/Exec_Shield)
- [iptables](https://linux.die.net/man/8/iptables)
- [AppArmor](https://apparmor.net/)
- [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux)
- [Fail2ban](https://github.com/fail2ban/fail2ban)
- [Snort](https://www.snort.org/faq/what-is-snort)
- [Uncomplicated Firewall (ufw)](https://wiki.ubuntu.com/UncomplicatedFirewall)

Occassionally we will see the password hashes directly in the `/etc/passwd` file. But unless you are on a box with an extremely outdated kernel, this is likely not the case. This file is readable by all users, and as with hashes in the `/etc/shadow` file, these can be subjected to an offline password cracking attack. This configuration, while not common can sometimes be seen on embedded devices and routers
```bash
cat /etc/passwd | cut -f1 -d:
```

With Linux, several different hash algorithms can be used to make the passwords unrecognizable. Identifying them from the first hash blocks can help us to use and work with them later if needed. Here is a list of the most used ones:

|**Algorithm**|**Hash**|
|:---:|:---:|
|Salted MD5|`$1$`...|
|SHA-256|`$5$`...|
|SHA-512|`$6$`...|
|BCrypt|`$2a$`...|
|Scrypt|`$7$`...|
|Argon2|`$argon2i$`...|


**Check which users have login shells**
```bash
grep "*sh$" /etc/passwd
```

Each user in Linux systems is assigned to a specific group or groups and thus receives special privileges.
The information about the available groups can be found in the `/etc/group` file
```shell
cat /etc/group #lists all of the groups on the system

getent group sudo # list memebers of <sudo> group
```
#### All Hidden Files
```bash
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
```
#### All Hidden Directories
```bash
find / -type d -name ".*" -ls 2>/dev/null
```
### (3)Default Folders for temporary files.
These folders are visible to all users and can be read. In addition, temporary logs or script output can be found there.
Both `/tmp` and `/var/tmp` are used to store data temporarily.
However, the key difference is how long the data is stored in these file systems.
```bash
ls -l /tmp /var/tmp /dev/shm
```

Though we are focusing on manual enumeration in this module, its worth running the [linPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS) Script.



## Linux Services and Internals Enumeration
- What services and applications are installed?
- what Services are running?
- What sockets are in use?
- What users, admin, and groups exist on the systeM?
- Who is Currently logged in? What users recently logged in?
- What password policies, if any, are enforced on the host?
- Is the host joined to an Active Directory Domain?
- What types of interesting information can we find in history, log, and backup files
- Which files have been modified recently and how often? Are there any interesting patterns in file modification that could indicate a cron job?
- Current IP Addressing
- 

#### Hosts
```
cat /etc/hosts
```

#### Users last Login
```bash
lastlog
```


#### Logged In Users
```bash
w
```
- It's important to check a user's bash history in case they may be passing passwwords as an argument on the command line. Working with git repo's, setting up crontabs, and more.


#### Finding History Files
```bash
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null
```
___
#### Cron
```bash
ls -la /etc/cron.daily/
```
`proc`, `procfs` is a particular filesystem in Linux that contains information about system processess, hardware, and other system info












|**Command**|**Description**|
|:---:|:---|
|`ssh htb-student@<target IP>`|SSH to lab target|
|`ps aux \| grep root`|See processes running as root|
|`ps au`|See logged in users|
|`ls /home`|View user home directories|
|`ls -l ~/.ssh`|Check for SSH keys for current user|
|`history`|Check the current user's Bash history|
|`sudo -l`|Can the user run anything as another user?|
|`ls -la /etc/cron.daily`|Check for daily Cron jobs|
|`lsblk`|Check for unmounted file systems/drives|
|`find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null`|Find world-writeable directories|
|`find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null`|Find world-writeable files|
|`uname -a`|Check the Kernel versiion|
|`cat /etc/lsb-release`|Check the OS version|
|`gcc kernel_expoit.c -o kernel_expoit`|Compile an exploit written in C|
|`screen -v`|Check the installed version of `Screen`|
|`./pspy64 -pf -i 1000`|View running processes with `pspy`|
|`find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null`|Find binaries with the SUID bit set|
|`find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null`|Find binaries with the SETGID bit set|
|`sudo /usr/sbin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root`|Priv esc with `tcpdump`|
|`echo $PATH`|Check the current user's PATH variable contents|
|`PATH=.:${PATH}`|Add a `.` to the beginning of the current user's PATH|
|`find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null`|Search for config files|
|`ldd /bin/ls`|View the shared objects required by a binary|
|`sudo LD_PRELOAD=/tmp/root.so /usr/sbin/apache2 restart`|Escalate privileges using `LD_PRELOAD`|
|`readelf -d payroll \| grep PATH`|Check the RUNPATH of a binary|
|`gcc src.c -fPIC -shared -o /development/libshared.so`|Compiled a shared libary|
|`lxd init`|Start the LXD initialization process|
|`lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine`|Import a local image|
|`lxc init alpine r00t -c security.privileged=true`|Start a privileged LXD container|
|`lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true`|Mount the host file system in a container|
|`lxc start r00t`|Start the container|
|`showmount -e 10.129.2.12`|Show the NFS export list|
|`sudo mount -t nfs 10.129.2.12:/tmp /mnt`|Mount an NFS share locally|
|`tmux -S /shareds new -s debugsess`|Created a shared `tmux` session socket|
|`./lynis audit system`|Perform a system audit with `Lynis`|









