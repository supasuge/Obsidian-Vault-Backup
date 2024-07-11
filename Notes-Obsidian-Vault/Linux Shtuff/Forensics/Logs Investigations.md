#### Objectives
- Learn about the different types of logs recorded on Linux systems.
- Learn how to perform forensic analysis through logs on Linux systems, focused on determining malicious processes, services, and scripts.
- Hunt malicious processes, services, and configurations to mitigate further compromise in a hands-on IR scenario.


## Logging Levels and kernel Logs
Logs provide a breadcrumb trail of system activities, errors, and events crucial for troubleshooting, auditing, and security analysis

It's important to understand the two primary types of logging mechanisms: kernel and user
- **Kernel** logs provide a backstage pass into your system's inner workings. They include messages related to hardware events, driver operations, and system errors.
- **User** logs capture user interactions between users, applications, and the operating system. They include login attempts, command executions, and app-specific activities.


### Logging Levels
| Level Name    | Description                                                                                              |
| ------------- | -------------------------------------------------------------------------------------------------------- |
| **EMERGENCY** | The highest level is adopted by messages when the system is unusable or crashes.                         |
| **ALERT**     | Provides information where user attention is immediately required.                                       |
| **CRITICAL**  | Provides information about critical errors, whether hardware or software-related.                        |
| **ERROR**     | Notifies the user about non-critical errors, such as failed device recognition or driver-related issues. |
| **WARNING**   | The default log level to display warnings about non-imminent errors.                                     |
| **NOTICE**    | Notifies about events which are worth having a look at.                                                  |
| **INFO**      | Provides informational messages about system actions and operations.                                     |
| **DEBUG**     | Detailed information used for debugging, mostly relevant during development or troubleshooting.          |

### Kernel Logging

The **kernel** is the OS's core. Orchestrating hardware interactions, process management, and resource allocation. Kernel logs, often referred to as kernel messages, provide a backstage pass into the inner workings of your system. From hardware init, to driver crashes, kernel logs chronicle everything. These logs are critical for diagnosing hardware failures, identifying problematic drivers and troubleshooting system crashes

The console outputs information about the system startup during a Linux host's boot process. Logging is done via the **kernel ring buffer to avoid data loss**, which handles messages from the kernel and kernel modules. The kernel ring buffer only exists in memory and is of a fixed size, which means that once full, older entries are overwritten with every new log encountered.

We can view the current contents of the ring buffer by running the command `dmesg` as root or privileged user on the host. 

Reading the kernel ring buffer

```shell-session
ubuntu@tryhackme:~$ sudo dmesg
```

Additionally, it is essential to know that kernel logs are stored in the `/var/log/kern.log` file, which is managed by the **rsyslog** or **syslog** service. We can view the kern.log file using common file-viewing commands such as **cat**, **less,** or **tail**.

1. Which type of logs provide messages related to hardware events and system errors?
> `Kernel`

2. What is the memory space used to store system messages?
> `Kernel ring buffer`

3. What is the default log level used to inform about non-imminent errors?
> `Warning`


### Exploring the /var/log directory
The `/var/log` directory in Linux systems is a critical repository of log files that provide insights into system activities and events. These logs are indispensable in forensic investigations, as they contain records of system processes, user activities, network connections, and much more.

#### kern.log and dmesg
Continuing on the kernel logs covered in the previous task, the `/var/log/kern.log` file is dedicated to recording kernel messages. This log is essential for diagnosing hardware failures and understanding deeper system issues that attackers could exploit.

By running the commands shown below, we can simulate a custom kernel rootkit installation and investigate the logs generated. Note that the kernel module file used here has already been generated in the VM.

```shell-session
ubuntu@tryhackme:~$ sudo insmod /home/ubuntu/exploit/custom_kernel.ko 
ubuntu@tryhackme:~$ sudo tail -f /var/log/kern.log
```

  

Another location to look for kernel logs is the `/var/log/dmesg` file. Examining this log can help you detect unusual messages during system startup, which might indicate tampering or hardware issues. However, what you would notice is that it is quite difficult to follow this format of the logs.

Investigating kernel logs with dmesg

```shell-session
ubuntu@tryhackme:~$ sudo tail /var/log/dmesg
```

### **auth.log**

The `/var/log/auth.log` file is your go-to for all authentication-related events on your Linux system. It records every login attempt, whether successful or failed, along with commands executed using elevated privileges and SSH logins. This makes it an invaluable resource for tracking potential security breaches.

For example, we can simulate an attacker who may be attempting to gain unauthorised access to the system through SSH brute-force attacks by generating SSH authentication logs by running:

Unauthorised login attempts in auth.log

```shell-session
ubuntu@tryhackme:~$ ssh root@localhost -p 22
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is ------
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
root@localhost: Permission denied (publickey).
```

  

Then, investigate the authentication logs for any output from the command:

Unauthorised login attempts in auth.log

```shell-session
ubuntu@tryhackme:~$ sudo tail -f /var/log/auth.log
Jun 27 08:55:45 tryhackme sshd[70808]: Connection closed by authenticating user root 127.0.0.1 port 43086 [preauth]
Jun 27 08:56:10 tryhackme sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/tail -f /var/log/auth.log
Jun 27 08:56:10 tryhackme sudo: pam_unix(sudo:session): session opened for user root by ubuntu(uid=0)
```

  
It is also important to monitor successful login attempts, especially those occurring outside normal business hours or from unusual IP addresses, by running `grep 'Accepted password' /var/log/auth.log` to filter these

```shell-session
grep 'Accepted password' /var/log/auth.log
```


Additionally, tracking commands executed with elevated privileges can provide insights into potential malicious activities. By running `grep 'sudo' /var/log/auth.log`, you can review all commands run with `sudo`. Giving you a clear view of who has performed critical system operations.

```shell
grep 'sudo' /var/log/auth.log
```

### **syslog**

The `/var/log/syslog` file serves as a catch-all for various system messages, making it a central point for understanding system-wide events. This log captures everything from cron job executions to kernel activities, providing a comprehensive view of your system's health and activities.

For instance, we can look at cron job executions in syslog by searching for entries containing '**CRON**'. This can help you verify that scheduled tasks are running as expected and identify any suspicious or unauthorised cron jobs. 

Cron job executions in syslog

```shell-session
ubuntu@tryhackme:~$ grep 'CRON' /var/log/syslog
```

Kernel-related messages can also be viewed via syslog. These entries can indicate hardware issues or potential attacks targeting the kernel. By running `grep 'kernel' /var/log/syslog`, you can sift through these messages and pinpoint problems that might otherwise go unnoticed.

```bash
grep 'kernel' /var/log/syslog
```

### btmp and wtmp
The `/var/log/btmp` file logs failed login attempts, while the `/var/log/wtmp` records every login and logout activity. We can be assured these files are critical in identifying potential brute-force attacks and training unauthorized access, similar to the `/var/log/auth.log` file.

```shell
sudo last -f /var/log/wtmp
```


1. Which log file can be used to record failed login attempts only?
> `btmp`





