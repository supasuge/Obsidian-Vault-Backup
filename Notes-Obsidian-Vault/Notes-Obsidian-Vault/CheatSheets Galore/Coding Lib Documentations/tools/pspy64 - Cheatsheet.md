## Table of Contents

- [pspy64 Cheat Sheet](#pspy64\cheat\sheet)
  - [Overview](#Overview)
  - [Basic Usage](#Basic\Usage)
  - [Flags & Options](#Flags\&\Options)
  - [Advanced Options](#Advanced\Options)
    - [Monitoring File Events](#Monitoring\File\Events)
  - [Example Usage](#Example\Usage)

`pspy` is a command-line tool used for monitoring processes on a Unix-like system without needing root permissions. It's particularly useful in scenarios where you don't have access to standard process monitoring tools like `ps` or `top`. `pspy` is often used in the field of cybersecurity and ethical hacking to inspect running processes on a system, which can reveal valuable information about system activities and potential vulnerabilities.

Below is a concise markdown cheat sheet for `pspy64`, a 64-bit variant of `pspy`, focusing on its usage, flags, and options for process inspection:

---

# pspy64 Cheat Sheet

## Overview
`pspy64` is designed for monitoring processes on a Unix-like system in environments where you don’t have root access. It's particularly useful in system inspection and identifying cron jobs.

## Basic Usage

```bash
./pspy64
```

This command runs `pspy64` with default settings, which monitors and lists processes as they are created and terminated.

## Flags & Options

 `-h`, `--help`
Display help information.

```bash
./pspy64 -h
```

 `-p`, `--pid`
Only show events related to specific process IDs.

```bash
./pspy64 -p PID1,PID2
```

 `-i`, `--interval`
Set the interval for scanning procfs in milliseconds (default 1000).

```bash
./pspy64 -i 2000
```

 `-d`, `--debug`
Enable debug output.

```bash
./pspy64 -d
```

 `-v`, `--version`
Display version information.

```bash
./pspy64 -v
```

 `-f`, `--file`
Only show events related to specific file paths.

```bash
./pspy64 -f /path/to/file
```

 `--cmd-regex`
Filter processes by a command line regular expression.

```bash
./pspy64 --cmd-regex 'regex-pattern'
```

## Advanced Options

### Monitoring File Events
`pspy64` can also monitor file events using inotify. This requires additional flags:

 `-r`, `--dir`
Specify the directory to monitor file events.

```bash
./pspy64 -r /path/to/dir
```

 `--file-regex`
Filter file events by a regular expression.
```bash
./pspy64 --file-regex 'regex-pattern'
```

## Example Usage

1. **Monitor Specific Processes**

   Monitor processes with specific PIDs.
   ```bash
   ./pspy64 -p 1234,5678
   ```

2. **Process Monitoring with Increased Interval**
   Monitor processes, scanning every 2 seconds.
   ```bash
   ./pspy64 -i 2000
   ```

3. **File Event Monitoring**
   Monitor file events in a specific directory.
   ```bash
   ./pspy64 -r /var/log
   ```

---

**Note:** Always ensure that you are authorized to use `pspy64` on the system you are inspecting. Unauthorized use of such tools can be considered malicious activity and is against ethical hacking principles.