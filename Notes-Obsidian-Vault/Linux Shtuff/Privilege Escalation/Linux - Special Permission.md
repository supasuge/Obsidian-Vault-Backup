## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Finding all SUID Permission](#Finding\all\SUID\Permission)
        - [GTFO Bins](#GTFO\Bins)

## Table of Contents

          - [Finding all SUID Permission](#Finding\all\SUID\Permission)
        - [GTFO Bins](#GTFO\Bins)

**SUID**: (setuid) - *Set User ID upon Execution*
- Can allow a user to execute a program or script with the permissions of another user, typically with elevated privileges. 
- the `setuid` bit appears as an **s**

###### Finding all SUID Permission
```bash
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```
- May be possible with the SETUID bit set, identify a vulnerability, and exploit this to escalate our privileges. Many programs have additional features that can be leveraged to execute commands and if the `setuid` bit is set on them, these can used for our purpose.

**SGID**:The Set-Group-ID (`setgid`)
- allows us to run binaries as if we were part of the group that created them. These files can be enumerated using the following command:
```bash
find / -uid 0 -perm -6000 -type f 2>/dev/null
```
- These files can be leveraged in the same manner as the `setuid` binaries to escalate privileges

```bash
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```
This [resource](https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits) has more information about the `setuid` and `setgid` bits, including how to set the bits.

##### GTFO Bins
The [GTFOBins](https://gtfobins.github.io) project is a curated list of binaries and scripts that can be used by an attacker to bypass security restrictions. Each page details the program's features that can be used to break out of restricted shells, escalate privileges, spawn reverse shell connections, and transfer files.
Ex: `apt-get` can be used to break out of restricted environments and spawn a shell by adding a Pre-Invoke command:
```bash
sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
```

**Find a file with the setuid bit set that was not shown in the section command output(Full Path)**:


**Find a file with the setgid bit set that was not shown in the section command output(Full Path)**:








