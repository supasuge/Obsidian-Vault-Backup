## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Top 5 Main Ideas from the Whole Body of Content](#Top\5\Main\Ideas\from\the\Whole\Body\of\Content)
    - [Longer Summary](#Longer\Summary)
    - [Shorter Summary](#Shorter\Summary)
    - [Outline of the Whole Body of Content](#Outline\of\the\Whole\Body\of\Content)
  - [Os release Information](#Os\release\Information)
  - [User accounts](#User\accounts)
  - [Group Information](#Group\Information)
  - [User Accounts](#User\Accounts)
    - [Group Information](#Group\Information)
    - [Sudoers List](#Sudoers\List)
- [This file MUST be edited with the 'visudo' command as root.](#this\file\must\be\edited\with\the\'visudo'\command\as\root.)
- [Please consider adding local content in /etc/sudoers.d/ instead of](#please\consider\adding\local\content\in\/etc/sudoers.d/\instead\of)
- [directly modifying this file.](#directly\modifying\this\file.)
- [See the man page for details on how to write a sudoers file.](#see\the\man\page\for\details\on\how\to\write\a\sudoers\file.)
- [Host alias specification](#host\alias\specification)
- [User alias specification](#user\alias\specification)
- [Cmnd alias specification](#cmnd\alias\specification)
- [User privilege specification](#user\privilege\specification)
- [Members of the admin group may gain root privileges](#members\of\the\admin\group\may\gain\root\privileges)
- [Allow members of group sudo to execute any command](#allow\members\of\group\sudo\to\execute\any\command)
- [See sudoers(5) for more information on "#include" directives:](#see\sudoers(5)\for\more\information\on\"#include"\directives:)
    - [Login Information](#Login\Information)
  - [Authentication logs](#Authentication\logs)
  - [System Configuration](#System\Configuration)
      - [Hostname](#Hostname)
      - [Timezone](#Timezone)
      - [Network Configuration](#Network\Configuration)
    - [Active network connections](#Active\network\connections)
    - [Running Processes](#Running\Processes)
    - [Dns Information](#Dns\Information)

## Table of Contents

- [Top 5 Main Ideas from the Whole Body of Content](#Top\5\Main\Ideas\from\the\Whole\Body\of\Content)
    - [Longer Summary](#Longer\Summary)
    - [Shorter Summary](#Shorter\Summary)
    - [Outline of the Whole Body of Content](#Outline\of\the\Whole\Body\of\Content)
  - [Os release Information](#Os\release\Information)
  - [User accounts](#User\accounts)
  - [Group Information](#Group\Information)
  - [User Accounts](#User\Accounts)
    - [Group Information](#Group\Information)
    - [Sudoers List](#Sudoers\List)
- [This file MUST be edited with the 'visudo' command as root.](#this\file\must\be\edited\with\the\'visudo'\command\as\root.)
- [Please consider adding local content in /etc/sudoers.d/ instead of](#please\consider\adding\local\content\in\/etc/sudoers.d/\instead\of)
- [directly modifying this file.](#directly\modifying\this\file.)
- [See the man page for details on how to write a sudoers file.](#see\the\man\page\for\details\on\how\to\write\a\sudoers\file.)
- [Host alias specification](#host\alias\specification)
- [User alias specification](#user\alias\specification)
- [Cmnd alias specification](#cmnd\alias\specification)
- [User privilege specification](#user\privilege\specification)
- [Members of the admin group may gain root privileges](#members\of\the\admin\group\may\gain\root\privileges)
- [Allow members of group sudo to execute any command](#allow\members\of\group\sudo\to\execute\any\command)
- [See sudoers(5) for more information on "#include" directives:](#see\sudoers(5)\for\more\information\on\"#include"\directives:)
    - [Login Information](#Login\Information)
  - [Authentication logs](#Authentication\logs)
  - [System Configuration](#System\Configuration)
      - [Hostname](#Hostname)
      - [Timezone](#Timezone)
      - [Network Configuration](#Network\Configuration)
    - [Active network connections](#Active\network\connections)
    - [Running Processes](#Running\Processes)
    - [Dns Information](#Dns\Information)


1. **Understanding Ubuntu 20.04.1 LTS**
   - **Concept**: Linux Operating System, Version: 20.04.1 LTS (Long Term Support).
   - **Relevance**: Preferred for stability and security, especially in forensic analysis.
   - **Jargon Explained**: LTS - Extended support period, typically for enterprise or long-term use.

2. **User Account Management in Linux**
   - **Concept**: `/etc/passwd` file storing user information.
   - **Details**: Lists all user accounts, showing usernames, encrypted passwords, user IDs (UIDs), group IDs (GIDs), full names, home directories, and default shells.
   - **Jargon Explained**: UID - Unique identifier for each user; GID - Group identifier.

3. **Linux Group Management Insights**
   - **Concept**: `/etc/group` file containing group information.
   - **Details**: Defines groups, lists members, and links to security roles.
   - **Jargon Explained**: Group - Collection of users for assigning shared permissions.

4. **Sudoers File: Privilege Management**
   - **Concept**: `/etc/sudoers` file defining superuser privileges.
   - **Details**: Specifies which users/groups can execute commands as the superuser.
   - **Jargon Explained**: Sudoers - Users granted permission to execute commands as root.

5. **Monitoring Logins: wtmp and btmp Files**
   - **Concept**: Binary files tracking login attempts.
   - **Details**: `wtmp` logs successful logins, `btmp` tracks failed attempts.
   - **Jargon Explained**: wtmp/btmp - Files used for auditing and tracking user activities.

6. **Authentication Logs Analysis**
   - **Concept**: `/var/log/auth.log` for tracking authentication.
   - **Details**: Records user logins, sudo attempts, and other authentication events.
   - **Jargon Explained**: Auth log - Log file detailing user authentication activities.

### Top 5 Main Ideas from the Whole Body of Content

1. **Ubuntu 20.04.1 LTS**: Its role in Linux-based forensic investigations.
2. **User Account Management**: The significance of `/etc/passwd` in tracking user information.
3. **Group Management**: Understanding how Linux manages user groups via `/etc/group`.
4. **Sudoers List**: The critical role of `/etc/sudoers` in privilege management.
5. **Authentication and Login Monitoring**: Importance of log files (`wtmp`, `btmp`, `/var/log/auth.log`) in security and forensic analysis.

### Longer Summary

The provided content delves into forensic artifacts in a Linux environment, specifically focusing on Ubuntu 20.04.1 LTS. It underscores the importance of various system files like `/etc/passwd` for user account management, detailing the structure and function of these files. The notes elaborate on the criticality of group management in Linux, explained through the `/etc/group` file. They also highlight the sudoers list in the `/etc/sudoers` file, which is pivotal in managing user privileges and system security. Additionally, the significance of login and authentication tracking is emphasized through the analysis of `wtmp`, `btmp`, and `/var/log/auth.log` files. These aspects are crucial in understanding system configurations, user activities, and potential security breaches in Linux-based systems.

### Shorter Summary

This comprehensive overview focuses on key forensic artifacts in Ubuntu 20.04.1 LTS, emphasizing critical system files like `/etc/passwd`, `/etc/group`, and `/etc/sudoers`. It highlights the management of user accounts, group roles, and superuser privileges, along with the importance of monitoring login and authentication activities through specific log files. These elements are essential in Linux forensics for ensuring system integrity and security.

### Outline of the Whole Body of Content

1. **Introduction to Linux Forensics**
   - Role of Ubuntu 20.04.1 LTS
   - Forensic artifacts in Linux systems

2. **User Accounts in Linux**
   - Structure and purpose of `/etc/passwd`
   - UID and GID explained

3. **Group Management in Linux**
   - Understanding `/etc/group`
   - Security implications

4. **Sudoers and Privilege Management**
   - Role and configuration of `/etc/sudoers`
   - Superuser privileges explained

5. **Login and Authentication Monitoring**
   - Importance of `wtmp`, `btmp`, and `/var/log/auth.log`
   - Security auditing and user activity tracking
___
## Os release Information
```bash
cat /etc/os-release
```
## User accounts
The `/etc/passwd` file contains information about the user accounts that exist on a Linux system
___
## Group Information
**You can use the following command to make it more readable:**
`cat /etc/passwd| column -t -s :`
`getent group`
___
## User Accounts
The `/etc/passwd` file contains information about the user accounts that exist on a Linux system
You can use the following command to make it more readable:
```bash
cat /etc/passwd| column -t -s :
```
- password information field shows `x`, which signifies that the password information is stored in the `/etc/shadow` file. The `uid` of the user is `1000`. the Gid is also `1000`. The description, which often contains the full name or contact information.
- In Ubuntu, the home directory is set to `/home/ubuntu`, the default shell is set to `/bin/bash`

### Group Information
The `/etc/group` file contains information about the different user groups present on the host. It can be read using 

```bash
user@machine$ cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,ubuntu
tty:x:5:syslog
```
We can see that the user `ubuntu` belongs to the `adm` group, which has a password stored in the `/etc/shadow` file, signified by the `x` character. The gid is 4, and the group contains 2 users, Syslog, and ubuntu.
___
### Sudoers List
A Linux host allows only those users to elevate privileges to `sudo`, which are present in the Sudoers list. This list is stored in the file `/etc/sudoers` and can be read using the `cat` utility. *You will need to elevate privileges to access this file*.
```bash           
user@machine$ sudo cat /etc/sudoers
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults	env_reset
Defaults	mail_badpass
Defaults	secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root	ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo	ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
```
___
### Login Information
In the /var/log directory, we can find log files of all kinds including `wtmp` and `btmp`. The `btmp` file saves information about failed logins, while the `wtmp` keeps historical data of logins. These files are not regular text files that can be read using `cat`, `less` or `vim`; instead, they are binary files, which have to be read using the `last` utility. You can learn more about the `last` utility by reading its man page.

`man last`

The following terminal shows the contents of `wtmp` being read using the `last` utility.

```bash
user@machine$ sudo last -f /var/log/wtmp
reboot   system boot  5.4.0-1029-aws   Tue Mar 29 17:28   still running
reboot   system boot  5.4.0-1029-aws   Tue Mar 29 04:46 - 15:52  (11:05)
reboot   system boot  5.4.0-1029-aws   Mon Mar 28 01:35 - 01:51 (1+00:16)

wtmp begins Mon Mar 28 01:35:10 2022
```
___
## Authentication logs
Every user that authenticates on a Linux host is logged in the auth log. The auth log is a file placed in the location `/var/log/auth.log`. It can be read using the `cat` utility, however, given the size of the file, we can use `tail`, `head`, `more` or `less` utilities to make it easier to read.

```bash
user@machine$ cat /var/log/auth.log |tail
Mar 29 17:28:48 tryhackme gnome-keyring-daemon[989]: The PKCS#11 component was already initialized
Mar 29 17:28:48 tryhackme gnome-keyring-daemon[989]: The SSH agent was already initialized
Mar 29 17:28:49 tryhackme polkitd(authority=local): Registered Authentication Agent for unix-session:2 (system bus name :1.73 [/usr/lib/x86_64-linux-gnu/polkit-mate/polkit-mate-authentication-agent-1], object path /org/mate/PolicyKit1/AuthenticationAgent, locale en_US.UTF-8)
Mar 29 17:28:58 tryhackme pkexec[1618]: ubuntu: Error executing command as another user: Not authorized [USER=root] [TTY=unknown] [CWD=/home/ubuntu] [COMMAND=/usr/lib/update-notifier/package-system-locked]
Mar 29 17:29:09 tryhackme dbus-daemon[548]: [system] Failed to activate service 'org.bluez': timed out (service_start_timeout=25000ms)
Mar 29 17:30:01 tryhackme CRON[1679]: pam_unix(cron:session): session opened for user root by (uid=0)
Mar 29 17:30:01 tryhackme CRON[1679]: pam_unix(cron:session): session closed for user root
Mar 29 17:49:52 tryhackme sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/cat /etc/sudoers
Mar 29 17:49:52 tryhackme sudo: pam_unix(sudo:session): session opened for user root by (uid=0)
Mar 29 17:49:52 tryhackme sudo: pam_unix(sudo:session): session closed for user root
```
___
**Which two users are the members of the group `audio`?**
> `ubuntu, pulse`

**In the attached VM, there is a user account named tryhackme. What is the uid of this account?**
> `1001`


**A session was started on this machine Sat Apr 16 20:10. How long did this session last?**
> `01:32`
___
## System Configuration
#### Hostname
Typically stored in the `/etc/hostname` file.

#### Timezone
This is important as it gives indicators of the general location of the device.
```bash
cat /etc/hostname
```

#### Network Configuration
To find information about the network interfaces, we can `cat` the `/etc/network/interfaces` file. The output on your machine might be different from the one shown here, depending on your configuration.
```bash
cat /etc/network/interfaces
```

Similarly, to find information about the MAC and IP addresses of the different interfaces, we can use the `ip` utility. To learn more about the `ip` utility, we can see its `man` page.

`man ip`

The below terminal shows the usage of the `ip` utility. Note that this will only be helpful on a live system.

IP information

`user@machine$ ip address show`

### Active network connections
On live systems, active connections provide insight into further context to the investigation. `netstat` is used for this.

**Active network connections**
```
user@machine$ netstat -natp
```
___
### Running Processes
If performing forensics on a live system, it's helpful to check the running processes. The `ps` util shows details about the running proccesses
The below terminal shows the usage of the `netstat` utility.
___
**Active network connections**
`user@machine$ netstat -natp`

### Dns Information
The file `/etc/hosts` contains the configuration for the DNS name assignment.

The information about DNS servers that a Linux host talks to for DNS resolution is stored in the resolv.conf file. Its location is `/etc/resolv.conf`. We can use the `cat` utility to read this file.