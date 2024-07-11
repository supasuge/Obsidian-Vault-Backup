## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Service startup](#Service\startup)
- [Example of job definition:](#example\of\job\definition:)
- [.---------------- minute (0 - 59)](#.----------------\minute\(0\-\59))
- [|  .------------- hour (0 - 23)](#|\\.-------------\hour\(0\-\23))
- [|  |  .---------- day of month (1 - 31)](#|\\|\\.----------\day\of\month\(1\-\31))
- [|  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...](#|\\|\\|\\.-------\month\(1\-\12)\or\jan,feb,mar,apr\...)
- [|  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat](#|\\|\\|\\|\\.----\day\of\week\(0\-\6)\(sunday=0\or\7)\or\sun,mon,tue,wed,thu,fri,sat)
- [|  |  |  |  |](#|\\|\\|\\|\\|)
- [*  *  *  *  * user-name command to be executed](#*\\*\\*\\*\\*\user-name\command\to\be\executed)
    - [Service Startup](#Service\Startup)
  - [Log Files](#Log\Files)
      - [Syslogs](#Syslogs)
  - [Auth logs](#Auth\logs)
    - [Third-party logs](#Third-party\logs)

## Table of Contents

  - [Service startup](#Service\startup)
- [/etc/crontab: system-wide crontab](#/etc/crontab:\system-wide\crontab)
- [Unlike any other crontab you don't have to run the `crontab'](#unlike\any\other\crontab\you\don't\have\to\run\the\`crontab')
- [command to install the new version when you edit this file](#command\to\install\the\new\version\when\you\edit\this\file)
- [and files in /etc/cron.d. These files also have username fields,](#and\files\in\/etc/cron.d.\these\files\also\have\username\fields,)
- [that none of the other crontabs do.](#that\none\of\the\other\crontabs\do.)
- [Example of job definition:](#example\of\job\definition:)
- [.---------------- minute (0 - 59)](#.----------------\minute\(0\-\59))
- [|  .------------- hour (0 - 23)](#|\\.-------------\hour\(0\-\23))
- [|  |  .---------- day of month (1 - 31)](#|\\|\\.----------\day\of\month\(1\-\31))
- [|  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...](#|\\|\\|\\.-------\month\(1\-\12)\or\jan,feb,mar,apr\...)
- [|  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat](#|\\|\\|\\|\\.----\day\of\week\(0\-\6)\(sunday=0\or\7)\or\sun,mon,tue,wed,thu,fri,sat)
- [|  |  |  |  |](#|\\|\\|\\|\\|)
- [*  *  *  *  * user-name command to be executed](#*\\*\\*\\*\\*\user-name\command\to\be\executed)
    - [Service Startup](#Service\Startup)
  - [Log Files](#Log\Files)
      - [Syslogs](#Syslogs)
  - [Auth logs](#Auth\logs)
    - [Third-party logs](#Third-party\logs)


## Service startup

Like Windows, services can be set up in Linux that will start and run in the background after every system boot. A list of services can be found in the `/etc/init.d` directory. We can check the contents of the directory by using the `ls` utility.


```
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#**
```


### Service Startup
Like Windows, services can be set up in Linux that will start and run in the background after every system boot. A list of services can be found in the `/etc/init.d`


## Log Files
#### Syslogs
Contains messages that are recorded by the host about system activity.


The detail which is recorded in these messages is configurable through the logging level. We can use the `cat` utility to view the Syslog, which can be found in the file `/var/log/syslog`. Since the Syslog is a huge file, it is easier to use `tail`, `head`, `more` or `less` utilities to help make it more readable.

**Syslog**

```shell-session
user@machine$ cat /var/log/syslog* | head
Mar 29 00:00:37 tryhackme systemd-resolved[519]: Server returned error NXDOMAIN, mitigating potential DNS violation DVE-2018-0001, retrying transaction with reduced feature level UDP.
Mar 29 00:00:37 tryhackme rsyslogd: [origin software="rsyslogd" swVersion="8.2001.0" x-pid="635" x-info="https://www.rsyslog.com"] rsyslogd was HUPed
Mar 29 00:00:37 tryhackme systemd[1]: man-db.service: Succeeded.
Mar 29 00:00:37 tryhackme systemd[1]: Finished Daily man-db regeneration.
Mar 29 00:09:01 tryhackme CRON[7713]: (root) CMD (   test -x /etc/cron.daily/popularity-contest && /etc/cron.daily/popularity-contest --crond)
Mar 29 00:17:01 tryhackme CRON[7726]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Mar 29 00:30:45 tryhackme snapd[2930]: storehelpers.go:721: cannot refresh: snap has no updates available: "amazon-ssm-agent", "core", "core18", "core20", "lxd"
Mar 29 00:30:45 tryhackme snapd[2930]: autorefresh.go:536: auto-refresh: all snaps are up-to-date
Mar 29 01:17:01 tryhackme CRON[7817]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Mar 29 01:50:37 tryhackme systemd[1]: Starting Cleanup of Temporary Directories...
```



## Auth logs

We have already discussed the auth logs in the previous tasks. The auth logs contain information about users and authentication-related logs. The below terminal shows a sample of the auth logs.

Auth logs

```
user@machine$ cat /var/log/auth.log* |head Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: new group: name=ubuntu, GID=1000 Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: new user: name=ubuntu, UID=1000, GID=1000, home=/home/ubuntu, shell=/bin/bash, from=none Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'adm' Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'dialout' Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'cdrom' Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'floppy' Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'sudo' Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'audio' Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'dip' Feb 27 13:52:33 ip-10-10-238-44 useradd[392]: add 'ubuntu' to group 'video'`
```    

We can see above that the log stored information about the creation of a new group, a new user, and the addition of the user into different groups.

### Third-party logs

Similar to the syslog and authentication logs, the `/var/log/` directory contains logs for third-party applications such as webserver, database, or file share server logs. We can investigate these by looking at the `/var/log/` directory.

Third-party logs

           `user@machine$ ls /var/log Xorg.0.log          apt                    cloud-init.log  dmesg.2.gz      gdm3                    kern.log.1         prime-supported.log  syslog.2.gz Xorg.0.log.old      auth.log               cups            dmesg.3.gz      gpu-manager-switch.log  landscape          private              syslog.3.gz alternatives.log    auth.log.1             dist-upgrade    dmesg.4.gz      gpu-manager.log         lastlog            samba                syslog.4.gz alternatives.log.1  btmp                   dmesg           dpkg.log        hp                      lightdm            speech-dispatcher    syslog.5.gz amazon              btmp.1                 dmesg.0         dpkg.log.1      journal                 openvpn            syslog               unattended-upgrades apache2             cloud-init-output.log  dmesg.1.gz      fontconfig.log  kern.log                prime-offload.log  syslog.1             wtmp`
        

As is obvious, we can find the apache logs in the apache2 directory and samba logs in the samba directory.

Apache logs

           `user@machine$  ls /var/log/apache2/ access.log  error.log  other_vhosts_access.log`
        

Similarly, if any database server like MySQL is installed on the system, we can find the logs in this directory.

