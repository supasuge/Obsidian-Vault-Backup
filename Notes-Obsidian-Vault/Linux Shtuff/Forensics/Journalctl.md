# Table of Contents
- [Understanding the Journal](#Understanding\The\Journal)
	- [Using Journalctl](#Using\Journalctl)
		- [Filtering Logs by Date and Time](#Filtering\Logs\by\Date\and\Time)
		- [Relative time filtering](#Relative\time\filtering)
	- [Filtering Logs by Service](#Filtering\Logs\by\Service)
	- [Filtering Logs by Priority](Filtering\Logs\by\Priority)
`journal` and `journalctl` provide powerful logging system for modern linux distributions. Capturing log messages from the kernel, system services, and user applications. This guide will explain how to use these tools to monitor system acitivites.

## Understanding the Journal
The `journal` is a binary logging system used by systemd-based distributions. Unlike traditional text-based log files, the journal provides structured, indexed logs, allowing for efficient querying and filtering.

Typically, journal logs are volatile and would be erased on the next system restart. This can be cahnged within it's configuration file: `/etc/systemd/journald.conf` by setting the storage parameter to persistent and restarting the systemd journal daemon for the setting to take effect

### Using Journalctl

`journalctl` is the command-line tool used to interact with the systemd journal. It allows you to view, filter, and analyse log messages efficiently.

To view logs, simply type the following command. It will reveal all logs in reverse chronological order.
```shell
sudo journalctl
-- Logs begin at Wed 2023-09-06 07:57:36 UTC, end at Tue 2024-06-11 08:17:01 UTC. --
Sep 06 07:57:36 ubuntu kernel: Linux version 5.4.0-1029-aws (buildd@lcy01-amd64-022) (gcc version 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04)) #30-Ubuntu SMP Tue Oct 20 10:06:38 UTC 2020 (Ub>
Sep 06 07:57:36 ubuntu kernel: Command line: BOOT_IMAGE=/boot/vmlinuz-5.4.0-1029-aws root=PARTUUID=da63a61e-01 ro console=tty1 console=ttyS0 nvme_core.io_timeout=4294967295 panic=-1
Sep 06 07:57:36 ubuntu kernel: KERNEL supported cpus:
Sep 06 07:57:36 ubuntu kernel:   Intel GenuineIntel
Sep 06 07:57:36 ubuntu kernel:   AMD AuthenticAMD
Sep 06 07:57:36 ubuntu kernel:   Hygon HygonGenuine
Sep 06 07:57:36 ubuntu kernel:   Centaur CentaurHauls
Sep 06 07:57:36 ubuntu kernel:   zhaoxin   Shanghai  
Sep 06 07:57:36 ubuntu kernel: x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
Sep 06 07:57:36 ubuntu kernel: x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
Sep 06 07:57:36 ubuntu kernel: x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
Sep 06 07:57:36 ubuntu kernel: x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
Sep 06 07:57:36 ubuntu kernel: x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.
```

The command supports a plethora of options that filter and format the logs. The common flags worth knowing are listed in the table below:

| **Argument** | **Description**                                            | **Example**                           |
| ------------ | ---------------------------------------------------------- | ------------------------------------- |
| `-f`         | Follow the journal and show new entries as they are added. | `journalctl -f`                       |
| `-k`         | Show only kernel messages.                                 | `journalctl -k`                       |
| `-b`         | Show messages from a specific boot.                        | `journalctl -b -1`                    |
| `-u`         | Filter messages by a specific unit.                        | `journalctl -u apache.service`        |
| `-p`         | Filter messages by priority.                               | `journalctl -p err`                   |
| `-S`         | Show messages since a specific time.                       | `journalctl -S "2021-05-24 14:08:01"` |
| `-U`         | Show messages until a specific time.                       | `journalctl -U "2021-05-24 15:46:01"` |
| `-r`         | Reverse the output, showing the newest entries first.      | `journalctl -r`                       |
| `-n`         | Limit the number of shown lines.                           | `journalctl -n 20`                    |
| `--no-pager` | Do not pipe the output into a pager.                       | `journalctl --no-pager`               |

#### Filtering Logs by Date and Time

Sometimes, we may need to focus our investigations on specific periods, especiially when  working with servers with significant uptime. we can filter by arbitrary time limits using `--since or -S` and `--until or -U`

Time values come in variuos formats and if we need to filter by absolute time values, we should use the format `YYY-MM-DD HH:MM:SS`. example:
To view the logs from Feb 6th, 2024, at 15:30 hrs to Feb 17th, 2024, at 15:30 hrs, the command will look as follows:

Filter by absolute date and time

```shell-session
ubuntu@tryhackme:~$ sudo journalctl -S "2024-02-06 15:30:00" -U "2024-02-17 15:29:59"
```


It is worth noting that defaults will be applied if any of the format’s components are omitted. For example, if no date is provided, the current date will be used, and if the time component is not specified, _00:00:00_ as midnight will be used.

Accompanying absolute values, the journal can understand relative values and shortcuts. We can use terms such as _“**today**”, “**yesterday**”, “**now**”_, or “**2 hours ago**” to retrieve a set of logs.

If we want to check logs from 2 hours ago, we will run:

#### Relative time filtering

```shell-session
ubuntu@tryhackme:~$ sudo journalctl -S "2 hours ago"
```


#### Filtering Logs by Service
In addition to filtering by time, `journalctl` can filter logs by the services and units running on the host machine. To do so, we use the `-u` option and specify the service needed.
```bash
sudo journalctl -u nginx.service
```



#### Filtering Logs by Priority

Another vital information discussed in previous tasks is the log message priority. With `journalctl`, we can display messages of a specific priority or above using the `-p` option.

To show entries logged at a critical level or above on the host, that is, _critical, alert, and emergency_ levels, we will run the command:

Priority filtering

```shell-session
ubuntu@tryhackme:~$ sudo journalctl -p crit 
```

To configure the persistence of journal logs, which parameter has to be modified within the journald configuration file?
> `Storage`

