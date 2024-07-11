Sure, here is a working table of contents and a general summary for your markdown notes on auditd.

---

# Table of Contents

1. [Introduction](#introduction)
2. [Defining Audit Rules](#defining-audit-rules)
    1. [Example 1: Tracking Users Using /etc/passwd](#Example\1:\Tracking\Users\Using\/etc/passwd)
    2. [Example 2: Monitoring Execve Syscalls](#example-2-monitoring-execve-syscalls)
3. [Reviewing Audit Logs](#reviewing-audit-logs)
    1. [Querying Audit Logs](#querying-audit-logs)
    2. [Example Output of ausearch](#example-output-of-ausearch)
4. [Generating Reports](#generating-reports)
5. [Real-Time Monitoring](#real-time-monitoring)

---

## Introduction

Auditd is a tool used to enhance the security of a Linux system. It works in the User-space component of the Linux Auditing system. The Linux Audit Daemon (auditd) collects and writes log files to disk. These logs can include information about file access, user logins, process execution, etc.

## Defining Audit Rules

We need to define audit rules to set up what activities need to be logged. These rules can be specified in the `/etc/audit/audit.rules` file or added using the `auditctl` utility. Adding the rules to the `/etc/audit/audit.rules` file makes them persistent, while adding using `auditctl` is temporary.

### Example 1: Tracking Users Using /etc/passwd

From a security point of view, user activity is one of the most important types of events. Attackers often try to leverage the `/etc/passwd` file to identify users, their permissions, groups, etc. Therefore, tracking any changes to this file is important from a security standpoint. We can do this by creating the following rules:

```bash
sudo auditctl -w /etc/passwd -p wra -k users
```

- `-w`: Watch `/etc/passwd` for write, read and change attributes
- `-p wra`: Change attributes
- `-k`: Tag its users

### Example 2: Monitoring Execve Syscalls

The `execve` utility is used to execute a program from the caller. To monitor using the `auditctl` utility for `execve` syscalls, use the following rule:

```bash
sudo auditctl -a always,exit -F arch=b64 -S execve -k execve_syscalls
```

This rule logs every program execution through the execve system call on a 64-bit architecture. It then tags these events with the key `execve_syscall`.

- `-a always,exit`: Tells auditctl to always allocate an audit context at the start of the syscall and fill it with the details of an audit event at the exit of the syscall.

## Reviewing Audit Logs

So far, we have learned how to set up rules to create auditd events. Now, we will learn how we can query and parse them. Audit logs are stored in the `/var/log/audit/audit.log` file. We can query these logs using the `ausearch` utility.

### Querying Audit Logs

Using `ausearch` to query audit logs is simple. We can query audit logs by matching the key assigned while creating the rule. 

To search for the `users` key, which we created to track changes to `/etc/passwd`, we can use:

```bash
sudo ausearch -k users
```

Similarly, to view events matching the `execve_syscalls` key, we can use:

```bash
sudo ausearch -k execve_syscalls
```

### Example Output of ausearch

The below terminal window shows an example output of `ausearch` for a rule named `tests-changes`, which monitors changes to a file named `tests.sh`:

```shell-session
ubuntu@tryhackme:~$ sudo ausearch -k tests-changes
----
time->Wed Jun 12 22:23:12 2024
type=PROCTITLE msg=audit(1718230992.098:90): proctitle=617564697463746C002D77002F7661722F7777772F68746D6C2F74657374732E7368002D70007761002D6B0074657374732D6368616E676573
type=SYSCALL msg=audit(1718230992.098:90): arch=c000003e syscall=44 success=yes exit=1092 a0=4 a1=7ffd779c1470 a2=444 a3=0 items=0 ppid=6050 pid=6051 auid=1000 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts1 ses=2 comm="auditctl" exe="/usr/sbin/auditctl" subj=unconfined key=(null)
type=CONFIG_CHANGE msg=audit(1718230992.098:90): auid=1000 ses=2 subj=unconfined op=add_rule key="tests-changes" list=4 res=1
----
time->Wed Jun 12 22:33:22 2024
type=PROCTITLE msg=audit(1718231602.640:256): proctitle=76696D002F7661722F7777772F68746D6C2F74657374732E7368
type=PATH msg=audit(1718231602.640:256): item=0 name="/var/www/html/tests.sh" inode=1024384 dev=ca:01 mode=0100755 ouid=33 ogid=33 rdev=00:00 nametype=NORMAL cap_fp=0 cap_fi=0 cap_fe=0 cap_fver=0 cap_frootid=0
type=CWD msg=audit(1718231602.640:256): cwd="/home/ubuntu"
type=SYSCALL msg=audit(1718231602.640:256): arch=c000003e syscall=188 success=yes exit=0 a0=5615097f1e30 a1=7f75a4bca0b3 a2=561509a1f3c0 a3=1c items=1 ppid=6401 pid=6402 auid=1000 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts1 ses=2 comm="vim" exe="/usr/bin/vim.basic" subj=unconfined key="tests-changes"
```

Here, we can see two events where the rule has been triggered. The first is related to the creation of the rule, whereas the second is about the time when the file `/var/www/html/tests.sh` has been accessed. The event also provides us with other information, such as the program used to access the file (`vim`), the current working directory (`/home/ubuntu`), as well as the date and time.

One thing we might note here is that the `proctitle` information here is in hex. We can use the same `ausearch` utility to decode this information as well. We can use the `-i` option for decoding the `proctitle` from hex to ASCII, as shown below:

```shell-session
sudo ausearch -i -k serverdir-changes
----
time->Sun Jun 23 21:28:50 2024
type=PROCTITLE msg=audit(1719178130.725:305): proctitle=auditctl -w /var/www/html -p wax -k serverdir-changes
type=SYSCALL msg=audit(1719178130.725:305): arch=c000003e syscall=44 success=yes exit=1088 a0=4 a1=7ffda2ec3d70 a2=440 a3=0 items=0 ppid=15948 pid=15949 auid=1000 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts0 ses=2 comm="auditctl" exe="/usr/sbin/auditctl" subj=unconfined key=(null)
type=CONFIG_CHANGE msg=audit(1719178130.725:305): auid=1000 ses=2 subj=unconfined op=add_rule key="serverdir-changes" list=4 res=1
----
```

## Generating Reports

The `aureport` utility provides a summary of audit events. This can be useful for displaying information to third parties and providing information in an easy-to-view manner. We can pipe the output of `ausearch` to the `aureport` utility to generate a report.

```bash
sudo ausearch -k users | aureport -f user-logs
```

## Real-Time Monitoring

For real-time monitoring of audit events, `audispd` (Audit Dispatch Daemon) utility can be configured to send audit logs to other systems or to trigger specific actions when certain events occur. An example action can be sending the logs to a SIEM, where we can trigger further rules or playbooks as soon as these logs are generated. Overall, auditd can be a really helpful tool that can help us in real-time monitoring as well as post-incident response activity.

---
Which utility is used to search for auditd logs?
> `ausearch`


