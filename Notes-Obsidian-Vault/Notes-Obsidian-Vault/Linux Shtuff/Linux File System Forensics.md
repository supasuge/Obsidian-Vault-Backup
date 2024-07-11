# Intro
Live forensic file system analysis is often an early part of incident response and is crucial in assessing and determining potential security breaches. This includes: Examining digital artefacts, system logs, users, and file structures to uncover evidence of unauthorized access.

Understanding common artefacts of Linux file systems, permissions, and log mechanisms are vital to the timely detection and mitigation of security incidents. 

## Objectives

- Learn how to perform live file system analysis on a Linux system.
- Understand common artefacts, log mechanisms, and file system activities in Linux forensics.
- Reconstruct an event timeline in a hands-on incident response scenario.

In the assumed example scenario, we have already acquired all necessary backups and have isolated the system from the network to prevent further compromise or tampering.

As this is a potentially compromised host, it is a good idea to ensure we are using known good binaries and libraries to conduct our information gathering and analysis. Often, this can be done by mounting a USB or drive containing binaries from a clean Debian-based installation. This has been simulated on the attached VM by copying the _/bin_, _/sbin_, _/lib_, and _/lib64_ folders from a clean installation into the _/mnt/usb_ mount on the affected system.

- This effort aims to mitigate the risk of inadvertently executing malicious code or compromised utilities on systems. Suppose an attacker gains privileged access to a system. In that case, they may replace or alter existing utilities with malicious binaries ir libraries that could cause further harm when run by an unsuspecting investigator. By using a trusted source, it enhances the reliability and integrity of our investigation.
We can modify our `PATH` and `LD_LIBRARY_PATH` (shared libraries) environment variables to use these trusted binaries:

Modifying Environment Variables to Include Trusted Paths

```
investigator@10.10.74.7:~$ export PATH=/mnt/usb/bin:/mnt/usb/sbin investigator@10.10.74.7:~$ export LD_LIBRARY_PATH=/mnt/usb/lib:/mnt/usb/lib64
```

### Files, Permissions, and Timestamps
#### Identifying the Foothold
During briefing, we learn the web server is susceptible to a file upload vulnerability, leading the attacker gaining root access to the system. 

To start our investigation, we need to be able to explore the system's files effectively.

To identify clues that the file upload feature was exploited, we should focus our search on the web directories and review the uploaded files on the server. First, navigate to the web directory at `/var/www/html/` and run `ls -al` to list out the web files and directories:
           
```bash
investigator@10.10.74.7:~$ ls -al /var/www/html/
total 32
drwxr-xr-x 4 root     root      4096 Feb 12 23:05 .
drwxr-xr-x 3 root     root      4096 Feb 12 16:25 ..
drwxr-xr-x 2 www-data www-data  4096 Feb 13 00:32 assets
-rw-r--r-- 1 root     root     10918 Feb 12 16:25 index.html
-rwxr-xr-x 1 www-data www-data   905 Feb 12 16:33 upload.php
drwxr-xr-x 2 www-data www-data  4096 Feb 13 00:31 uploads
```

### Ownership and Permissions
File ownership and permissions are critical aspects of system security. We also determined that the _www-data_ user owned the file, which typically represents the user account associated with the web server process. As such, we should investigate additional activity and files owned by _www-data_ to determine what the attacker may have done with their newfound access.

Attackers often target directories with write permissions to upload malicious files. Common writable directories include:

- **/tmp**: The temporary directory is writable by all users, making it a common choice.
- **/var/tmp**: Another temporary directory commonly with world write permissions.
- **/dev/shm**: The shared memory file system, which is also normally writable by al

It's a good idea to run the ls -al command to list files in these directories and investigate any suspicious files or changes. We can also leverage the find command to quickly identify files that match our criteria. For example:
Listing Files Owned by www-data

```bash    
investigator@10.10.74.7:~$ find / -user www-data -type f 2>/dev/null | less
/var/www/html/assets/reverse.elf
/var/www/html/uploads/MzCxVeR.jpeg
/var/www/html/uploads/AzSxWqE.jpeg
/var/www/html/uploads/QaWsEdR.jpeg
/var/www/html/uploads/TyHjKlM.jpeg
...
```

After running this we find a file that sticks out: `/var/www/html/assets/reverse.elf`. Next list the file with more detail using: `ls -l` to see the file permissions.
```bash
investigator@10.10.74.7:~$ ls -l /var/www/html/assets/reverse.elf 
-rwxr-xr-x 1 www-data www-data 250 Feb 13 00:26 /var/www/html/assets/reverse.elf
```
Before we go further, there are several other useful `find` commands that can be used to pull particular files during an investigation:
```bash
find / -group GROUPNAME 2>/dev/null # Retrieve a list of files and directoreis owned by a specific group

find / -perm -o+w 2>/dev/null # Retrieve a list of all world-writable files and directories.

find / -type f -cmin -5 2>/dev/null # Retrieve a list of files created or changed within the last five minutes.
```

### Metadata

Metadata refers to the embedded information that describes files, which provides insights into a file's characteristics, origins, and attributes. It can include various types of information, such as file creation dates, author details, composition, and file types.

When performing live analysis on a system, metadata can be highly useful in determining the origin and modification timestamps and, in some cases, author details of specific files.

Exiftool is a Perl-based command-line utility with extensive capabilities for extracting and altering metadata from files by parsing their headers and embedded metadata structures.

For example, we can analyse the metadata of the suspicious reverse.elf file by running the following command:
Viewing the Metadata of `reverse.elf`   
```bash
investigator@10.10.74.7:~$ exiftool /var/www/html/assets/reverse.elf 
ExifTool Version Number         : 11.88
File Name                       : reverse.elf
Directory                       : /var/www/html/assets
File Size                       : 250 bytes
File Modification Date/Time     : 2024:02:13 00:26:28+00:00
File Access Date/Time           : 2024:02:13 02:31:29+00:00
File Inode Change Date/Time     : 2024:02:13 00:34:50+00:00
File Permissions                : rwxr-xr-x
File Type                       : ELF executable
File Type Extension             : 
MIME Type                       : ***********/*****-******
CPU Architecture                : 64 bit
CPU Byte Order                  : Little endian
Object File Type                : Executable file
CPU Type                        : AMD x86-64
```

### Analysing Checksums

Checksums are unique values generated from data using cryptographic hash functions (such as MD5 or SHA-256). These functions produce fixed-size strings of characters representing the data so that even a minor change in the data will result in a significantly different checksum.

Checksums are often used for data integrity verification, ensuring that data has not been altered or corrupted. For an incident responder, they can also be used to identify malicious files and executables based on known signatures.

We can leverage two useful checksum utilities to inspect the hashes of the reverse.elf file we identified previously. We can run both md5sum and sha256sum on the file to output its hash:
Analysing the File's Checksums
   
```bash
investigator@10.10.74.7:~$ md5sum /var/www/html/assets/reverse.elf 
c6cbdba1c147fbb7239284b7df2aa653  /var/www/html/assets/reverse.elf
investigator@10.10.74.7:~$ sha256sum /var/www/html/assets/reverse.elf 
ee0ea8d8bc205c4e2e2cc9ff7ddb71dee22ac0a50c2042701d49e565e0821410  /var/www/html/assets/reverse.elf
```

### Timestamps

Timestamps are additional pieces of metadata associated with files or events that indicate when a particular action occurred. Timestamps are one of the most essential aspects for tracking the creation, modification, and access times of files and directories in incident response and forensic activities. These timestamps are invaluable in forensic investigations as they provide essential clues about the sequence of events and the actions performed on a system and help establish a timeline.

In unix systems three main timestamps are commonly recorded:
- **Modify Timestamp (mtime):** This timestamp reflects the last time the **contents** of a file were modified or altered. Whenever a file is written to or changed, its _mtime_ is updated.
- **Change Timestamp (ctime):** This timestamp indicates the last time a file's **metadata** was changed. Metadata includes attributes like permissions, ownership, or the filename itself. Whenever any metadata associated with a file changes, its _ctime_ is updated.
- **Access Timestamp (atime):** This timestamp indicates the last time a file was **accessed** or read. Whenever a file is opened, its _atime_ is updated.

We can easily view the Modify Timestamp (mtime) of a file by running the following command:
Viewing the Modify Timestamp (mtime)
   
```bash
investigator@10.10.74.7:~$ ls -l /var/www/html/assets/reverse.elf 
-rwxr-xr-x 1 www-data www-data 250 Feb 13 00:26 /var/www/html/assets/reverse.elf
```

To view the Change Timestamp (`ctime`) of the same file, we can run:
```bash
ls -lc /var/www/html/assets/reverse.elf
```

Lastly, to view the Access Timestamp (`atime`) of the file, we can run:
Viewing the Access Timestamp (`atime`)
   
```bash
investigator@10.10.74.7:~$ ls -lu /var/www/html/assets/reverse.elf 
-rwxr-xr-x 1 www-data www-data 250 Feb 13 02:31 /var/www/html/assets/reverse.elf
```

A file's Access Timestamp (`atime`) can be easily and inadvertently updated as we perform investigate actions. When we viewed the metadata using `efixtool` or analysed its checksums with `md5sum`, and `sha256sum`. we performed read actions on `reverse.elf`, thus altering its access time.

While it's useful to recall the three commands above, we can also leverage the `stat` command to quickly see all three timestamps at once:
Viewing Timestamps with stat
   
```bash
investigator@10.10.74.7:~$ stat /var/www/html/assets/reverse.elf
File: /var/www/html/assets/reverse.elf
  Size: 250       	Blocks: 8          IO Block: 4096   regular file
Device: ca01h/51713d	Inode: 526643      Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (   33/www-data)   Gid: (   33/www-data)
Access: 2024-02-13 02:31:29.256000000 +0000
Modify: 2024-02-13 00:26:28.000000000 +0000
Change: 2024-02-13 00:34:50.679215113 +0000
 Birth: -
```

## Users and Groups
As we continue, we should focus on the system's users and groups. By doing so, we may uncover evidence of the attacker moving laterally or maintaining access throughout the system by exploiting additional vulnerabilities.

### Identifying User Accounts

Within UNIX-like systems, the /etc/ directory is a central location that stores configuration files and system-wide settings. Specifically, when investigating user accounts, /etc/passwd is a colon-separated plaintext file that contains a list of the system's accounts and their attributes, such as the user ID (UID), group ID (GID), home directory location, and the login shell defined for the user.

Let's view the user accounts on the affected system by reading the file:
Viewing the Contents of `/etc/passwd`
```bash
investigator@10.10.74.7:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
```

Attackers can maintain access to a system by creating a backdoor user with root permissions. We can leverage the cut and grep commands to identify this type of user account backdoor quickly. The following command extracts and displays all user accounts with the user ID (UID) of 0. The presence of a user with UID 0, other than the legitimate root user account, can quickly suggest a potential backdoor account.
Filtering the Contents of `/etc/passwd`   
```bash
investigator@MACHINE_IP:~$ cat /etc/passwd | cut -d: -f1,3 | grep ':0$'
root:0
**********:0
```

In the above command, we first display the contents of the `/etc/passwd` file. We then take the contents and perform a `cut` action to extract only the first (username) and third (user ID) fields from each line, delimited (`-d`) by the `:` character. We then use the `grep` command to extract specific entries containing `:0`, by signing a user ID of 0.

This, however, is not a foolproof method, as the backdoor account could have been created with legitimate user and group IDs. For further investigation, we can take a look at groups.

Identifying Groups

In Linux systems, certain groups grant specific privileges that attackers may target to escalate their privileges. Some important Linux groups that might be of interest to an attacker include:

- `sudo` or `wheel`: Members of the `sudo` (or `wheel`) group have the authority to execute commands with elevated privileges using `sudo`.
- `adm`: The `adm` group typically has read access to system log files.
- `shadow`: The `shadow` group is related to managing user authentication and password information. With this membership, a user can read the `/etc/shadow` file, which contains the password hashes of all users on the system.
- `disk`: Members of the disk group have almost unrestricted read and limited write access inside the system.

We can view the groups and their respective Group IDs on the system by reading the `/etc/group` file:
```bash
cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,ubuntu,investigator
```

To determine which groups a specific user is a member of, we can run the following command:   
```bash
investigator@10.10.74.7:~$ groups investigator
investigator : investigator adm dialout cdrom floppy sudo audio dip video plugdev netdev lxd
```

Alternatively, to list all of the members of a specific group, we can run the following command:
```bash
getent group adm
adm:x:4:syslog,ubuntu,investigator
```
If mulitiple users are in a group, their usernames will be listed in a comma-separated format in the entry.

To list all users in the `sudo` group, we can provide either the name "sudo" or the group ID, typically `27`.
```bash
getent group 27
```

### Users Logins and Activity
Checking user logins and activity is valuable for performing a real-time analysis of a compromised system. Fortunately, a couple of useful utilities and logs can assist us.

**last and lastb**

The `last` command is an excellent tool for examining user logins and sessions. It is used to display the history of the last logged-in users. It works by reading the `/var/log/wtmp` file, which is a file that contains every login and logout activity on the system. Similarly, `lastb` specifically tracks failed login attempts by reading the contents of `/var/log/btmp`, which can help identify login and password attacks.

```bash
investigator@10.10.74.7:~$ last
investig pts/1        10.10.152.206    Tue Feb 13 02:37   still logged in
investig pts/0        10.10.101.34     Tue Feb 13 02:29   still logged in
reboot   system boot  5.4.0-1029-aws   Tue Feb 13 02:28   still running
investig pts/1        10.10.101.34     Tue Feb 13 02:23 - crash  (00:05)
investig pts/0        10.10.101.34     Tue Feb 13 02:16 - 02:22  (00:05)
reboot   system boot  5.4.0-1029-aws   Tue Feb 13 02:14   still running
```

**lastlog**
Unlike the `last` command, which provides information about all user logins, the `lastlog` command focuses on a user's most recent login activity and reads from the `/var/log/lastlog` file.   
```bash
investigator@10.10.74.7:~$ lastlog
Username         Port     From             Latest
root                                       **Never logged in**
daemon                                     **Never logged in**
bin                                        **Never logged in**
sys                                        **Never logged in**
sync                                       **Never logged in**
...
```

**Failed Login  Attempts**
In addition to `lastb` there are others ways to view failed login attempts on Linux through specific log files. The `/var/log/auth.log` file or `/var/log/secure` on some distributions like CentOS and Red Hat) contains records of authentication-related events, including both successful and failed login attempts.

**who**
The `who` command is a very straight forward command that can be used to display the users that are currently logged into the machine.

The output of this command can provide details such as the name of the user logged in, the terminal device used, the time that the session was established, idle activity, the process ID of the shell, and additional comments that may include details such as the initial command used to start the session.

who Example

```bash
investigator@10.10.74.7:~$ who investigator pts/0        
2024-02-13 02:29 (10.10.101.34)
```

**Sudo**

The `/etc/sudoers` file is a particularly sensitive configuration file within Unix-like systems. It determines which users possess `sudo` privileges, enabling them to execute commands as other users, typically the root user.

As a result, it can be a target for attackers seeking persistence. For instance, if an attacker can find a way to insert their user account (or one that they control) into the `sudoers` file, they could grant themselves elevated privileges without requiring authentication. Alternatively, they may alter existing entries to broaden their access.

For example, a line in a `sudoers` file might look like this:
`/etc/sudoers` Example

```bash
user@tryhackme$ sudo cat /etc/sudoers
richard   ALL=(ALL) /sbin/ifconfig
```

More specifically this line specifies:
- **richard** is the username being granted sudo privileges.
- **ALL** indicated that the privilege applies to all hosts.
- **(ALL)** specifies that the user can run the command as any user.
- `/sbin/ifconfig` is the path to the specific binary, in this case, the `ifconfig` utility.

With this configuration, Richard can execute `ifconfig` with elevated sudo privileges to manage network interfaces as necessary.

## User Directories and Files
Previously, we identified a backdoor account that the attacker created and gained access to. However, we should take a step back and determine how the attacker got the privileges to create that account in the first place. To expand our investigation into the system's users and groups, we should also look into each user's personal directory, files, history, and configurations.

### User Home Directories
Home directories in Linux contain personalised settings, configurations, and user-specific data. There directories are located under the `/home` directory and are named after the corresponding usernames on the system. Recall viewing the `/etc/passwd` file and identifying various users and their home directories.

We can list out the home directories with a simple ls -l command:
Listing the `/home` Directories
   
```bash
investigator@10.10.74.7:~$ ls -l /home
total 16
drwxr-xr-x 4 bob          bob          4096 Feb 12 19:32 bob
drwxr-xr-x 3 investigator investigator 4096 Feb 13 02:22 investigator
drwxr-xr-x 4 jane         jane         4096 Feb 13 00:36 jane
drwxr-xr-x 5 ubuntu       ubuntu       4096 Feb 12 21:23 ubuntu
```

### Hidden Files

Hidden files, identified by a leading dot in their filenames, often store sensitive configurations and information within a user's home directory. By default, we cannot list out these hidden files using ls. To view them, we need to provide the -a argument, which will include all entries starting with a dot.

To list out the hidden files within Jane's home directory, run:
Listing Jane's Hidden Files and Directories   
```bash
investigator@10.10.74.7:~$ ls -a /home/jane
.  ..  .bash_history  .bash_logout  .bashrc  .cache  .profile  .ssh
```

Some common files that would be of interest during an investigation include:

- `.bash_history`: This file contains a user's command history and can be used to show previous commands executed by the user.
- `.bashrc` and `.profile`: These are configuration files used to customise a user's Bash shell sessions and login environment, respectively.

Additionally, we can look at other files and directories of interest, like browser profiles and the `.ssh` directory.


### SSH and Backdoors
The `.ssh` directory is a susceptible area containing configuration and key files related to SSH connections. The `authorized_keys` file within the directory is critical because it lists public keys allowed to connect to a user's account over SSH.

If a malicious user gains unauthorised access to a system and wants to persistently access another user's account by adding their public key to the `authorized_keys` files, we can potentially uncover artefacts that hint at these actions.

First, navigate to the `.ssh` directory within Jane's home folder. From here, we can run an ls -al to list the contained files:
Listing Jane's `.ssh` Directory
   
```bash
investigator@10.10.74.7:~$ ls -al /home/jane/.ssh
total 20
drwxr-xr-x 2 jane jane 4096 Feb 12 17:15 .
drwxr-xr-x 4 jane jane 4096 Feb 13 00:36 ..
-rw-rw-rw- 1 jane jane 1136 Feb 13 00:34 authorized_keys
-rw------- 1 jane jane 3389 Feb 12 17:12 id_rsa
-rw-r--r-- 1 jane jane  746 Feb 12 17:12 id_rsa.pub
```

Let's view the file to see if we can identify any unintended authorised public keys:
Viewing Jane's `authorized_keys` Entries

```
investigator@10.10.74.7:~$ cat /home/jane/.ssh/authorized_keys 
ssh-rsa ******************** jane@ip-10-10-25-169
ssh-rsa ******************** backdoor
```

Notice that there are two entries. The first belongs to Jane, as signified by the ending comment. However, the second entry appears to be related to an entirely different keypair with the comment "backdoor". The attacker was likely able to edit this file and append their own public key, allowing them SSH access as Jane.

We can further confirm this by returning to the `stat` command. By running it on the file, we can see that it was last modified around a similar timeframe to when we confirmed the attacker gained an initial foothold on the system.
   
```bash
investigator@10.10.74.7:~$ stat /home/jane/.ssh/authorized_keys 
  File: /home/jane/.ssh/authorized_keys
  Size: 1136      	Blocks: 8          IO Block: 4096   regular file
Device: ca01h/51713d	Inode: 257561      Links: 1
Access: (0666/-rw-rw-rw-)  Uid: ( 1002/    jane)   Gid: ( 1002/    jane)
Access: 2024-02-13 00:34:53.692530853 +0000
Modify: ****-**-** **:**:**.********* +****
Change: 2024-02-13 00:34:16.005897449 +0000
 Birth: -
```        

If we look back to the output of the `ls -al` command, we can identify the permission misconfiguration that made this possible:
```bash
investigator@10.10.74.7:~$ ls -al /home/jane/.ssh/authorized_keys 
-rw-rw-rw- 1 jane jane 1136 Feb 13 00:34 /home/jane/.ssh/authorized_keys
```

As identified by the third `rwx` permissions, this file is world-writable, which should never be the case for sensitive files. Consequently, by exploiting this misconfiguration, the attacker gained unauthorised SSH access to the system as if they were Jane.

## Binaries and Executables
Another area to look at within our compromised host's file system is identifying binaries and executables that the attacker may have created, altered, or exploit through permission misconfigurations.
### Identifying Suspicious Binaries
We can use the `find` command on Unix based systems to discover all executable files within the filesystem quickly:
```bash
investigator@10.10.74.7:~$ find / -type f -executable 2> /dev/null
/snap/core/16574/etc/init.d/single
/snap/core/16574/etc/init.d/ssh
/snap/core/16574/etc/init.d/ubuntu-fan
/snap/core/16574/etc/init.d/udev
```
- The above command recursively traverses the file system starting from the root directory and lists any executable file it finds. Note that this provides a huge amount of output. As such, it's often a good idea to limit the scope of the search through additional parameters.

Once we identify an executable or binary that we want to investigate further, we can perform metadata analysis as we have done previously, performing integrity checking on it using checksums or inspecting its human-readable strings and raw content.

### Strings

The strings command is valuable for extracting human-readable strings from binary files. These strings can sometimes include function names, variable names, and even plain text messages embedded within the binary. Analysing this information can help responders determine what the binary is used for and if there is any potential malicious activity involved. To run the strings command on a file, we need to provide the file as a single argument:

```bash
strings example.elf
```

### Debsums
Like the integrity checking we performed earlier, `debsums` is a command-line utility for Debian-based Linux systems that verifies the integirty of installed package files. `debsums` automatically compares the MD5 checksums of files installed from Debian packages against the known checksums stored in the package's metadata.

If any files have been modified or corrupted, `debsums` will report them, citing potential issues with the package's integrity. This can be useful in detecting malicious modifications and integrity issues within the system's packages. We can perform this check on the compromised system by running the following command:
Scanning Packages with `debsums`   
```bash
investigator@10.10.74.7:~$ sudo debsums -e -s
debsums: changed file /***/******* (from sudo package)
```

-  `-e` flag to only perform a configuration file check. In addition
- `-s` flag to silence any error output that may fill the screen.

### Binary Permissions
SetUID(SUID) and SetGID(SGID) are special permission bits in Unix operating systems. These permissions bits change the behaviour of executable files, allowing them to run with the privileges of the file owner or group rather than the privileges of the user who executes the file.

If a binary or executable on the system is misconfigured with an SUID or SGID permission set, an attacker may abuse the binary to break out of a restricted (unprivileged) shell through legitimated but unintended use of that binary. For example if the PHP binary contained a SUID bit to run as root, it's trivial for an attacker to abuse it to run system commands through PHP' system exec functions as root.

Identifying SetUID(SUID) binaries on a Linux system involves examining file permissions and explicitly looking for executables with the SetUID bit set. We can return to the `find` command to retrieve a list of the `SetUID`(SUID) binaries on the system.
```bash
find / -perm -u=s -type f 2>/dev/null
```

Specifically, the above command looks for files where the user permission has the SUID bit set (-u=s).

Much of the output here is expected as these binaries require the SUID bit and are not vulnerable. However, two of these results stand out. Firstly, Python should never be given SUID permission, as it is trivial to escalate privileges to the owner. Additionally, any SUID binaries in the /tmp or /var/tmp directory stand out as these directories are typically writable by all users, and unauthorised creation of SUID binaries in these directories poses a notable risk.

We can investigate further by looking in Jane's bash history for any commands related to Python or bash:
Correlating SUID Abuse in `bash_history`
   
```bash
investigator@MACHINE_IP:~$ sudo cat /home/jane/.bash_history | grep -B 2 -A 2 "python"
ls -al
find / -perm -u=s -type f 2>/dev/null
/usr/bin/python3.8 -c 'import os; os.execl("/bin/sh", "sh", "-p", "-c", "cp /bin/bash /var/tmp/bash && chown root:root /var/tmp/bash && chmod +s /var/tmp/bash")'
ls -al /var/tmp
/var/tmp/bash -p
exit
```


From the output, we've discovered evidence of Jane's user account identifying SUID binaries with the `find` command and abusing the SUID permission on the Python binary to run system commands as the root user. With this level of command execution, the attacker was able to create a copy of the `/bin/bash` binary (the Bash shell executable) and place it into the `/var/tmp` folder. Additionally, the attacker changed the owner of this file to root and added the SUID permission to it (`chmod +s`).

After making an SUID copy of `/bin/bash`, the attacker elevated to root by running `/var/tmp/bash -p`. We can further verify the `bash` binary by performing an integrity check on the original:
```bash
investigator@10.10.74.7:~$ md5sum /var/tmp/bash 
7063c393************d3b340f1ad2c  /var/tmp/bash
investigator@10.10.74.7:~$ md5sum /bin/bash
7063c393************d3b340f1ad2c  /bin/bash
```

Run the `debsums` utility on the compromised host to check only configuration files. Which file came back as altered?
> `/etc/sudoers`

What is the `md5sum` of the binary that the attacker created to escalate privileges to root?
> `7063c3930affe123baecd3b340f1ad2c`

## Rootkits
A rootkit is a type of malicious set of tools or software designed to gain administrator-level control of a system while remaining undetected by the system of user. The term "rootkit" derives from "root". The highest-level user in Unix-based systems, and "kit" which refers to a set of tools used to maintain access.

Rootkits are dangerous because they can hide their presence on a system and allow attackers to maintain long-term access without detection. Attackers can also use them to stage other malicious activities on the target, exfiltrate sensitive information, or command and control the compromised system remotely.

### Chkrootkit

[Chkrootkit (Check Rootkit)](https://www.chkrootkit.org/) is a popular Unix-based utility used to examine the filesystem for rootkits. It operates as a simple shell script, leveraging common Linux binaries like `grep` and `strings` to scan the core system programs to identify signatures. It can use the signatures from files, directories, and processes to compare the data and identify common patterns of known rootkits. As it does not perform an in-depth analysis, it is an excellent tool for a first-pass check to identify potential compromise, but it may not catch all types of rootkits.

Additionally, modern rootkits might deliberately attempt to identify and target copies of the _chkrootkit_ program or adopt other strategies to evade its detection.

We can access the _chkrootkit_ on the compromised system using our mounted binaries. We can perform a simple check by running `chkrootkit`:

           
```bash
investigator@MACHINE_IP:~$ sudo chkrootkit
ROOTDIR is `/'
Checking `amd'...                                           not found
Checking `basename'...                                      not infected
Checking `biff'...                                          not found
Checking `chfn'...                                          not infected
Checking `chsh'...                                          not infected
Checking `cron'...                                          not infected
Checking `crontab'...                                       not infected
Checking `date'...                                          not infected
...
```
This scan will produce a large output, but it indicates the results of various checks for known rootkit-related files or patterns.

### RKHunter
[RKHunter (Rootkit Hunter)](https://rkhunter.sourceforge.net/) is another helpful tool designed to detect and remove rootkits on Unix-like operating systems. It offers a more comprehensive and feature-rich rootkit detection check compared to _chkrootkit_. _RKHunter_ can compare SHA-1 hashes of core system files with known good ones in its database to search for common rootkit locations, wrong permissions, hidden files, and suspicious strings in kernel modules. It is an excellent choice for a more comprehensive assessment of the affected system.

Because rkhunter leverages a live database of known rootkit signatures, checking for database updates `rkhunter --update` before running in the field is crucial. Because this system is isolated, we won't be able to run a database update here. The latest version was acquired before mounting our tools to the system.

To perform a simple scan with rkhunter, we can run the following command:
`Rkhunter` Example
   
```bash
investigator@MACHINE_IP:~$ sudo rkhunter -c -sk
[ Rootkit Hunter version 1.4.6 ]

Checking system commands...

  Performing 'strings' command checks
    Checking 'strings' command                               [ OK ]

  Performing 'shared libraries' checks
    Checking for preloading variables                        [ None found ]
...
```
This check will take some time to run but we have bypassed the user interaction prompts with the `-sk` argument. Afterwards, you will receive a system check summary detailing what was found.













