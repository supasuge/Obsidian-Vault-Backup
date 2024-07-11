## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Table of Contents](#Table\of\Contents)
    - [Capabilities use for Privilege Escalation](#Capabilities\use\for\Privilege\Escalation)
      - [Enumerating Capabilities](#Enumerating\Capabilities)
      - [Exploitation](#Exploitation)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Capabilities use for Privilege Escalation](#Capabilities\use\for\Privilege\Escalation)
      - [Enumerating Capabilities](#Enumerating\Capabilities)
      - [Exploitation](#Exploitation)

## Table of Contents

- [Capabilities use for Privilege Escalation](#Capabilities\use\for\Privilege\Escalation)
      - [Enumerating Capabilities](#Enumerating\Capabilities)
      - [Exploitation](#Exploitation)

Capabilities are a security feature in Linux operating systems that allow specific privileges to be granted to *processes*. Allowing them to perform specific actions that would otherwise be restricted. This allows for more fine-grained control over which processes have access to certain privileges, making it more secure than the traditional Unix model of granting privileges to users and groups.


`cap_sys_admin` - Allows administrative privileges, such as modifying the system files or changing system settings
`cap_sys_chroot` - Allows to change the root directory for the current process, allowing it to access files and directories that would otherwise be inaccessible
`cap_sys_ptrace` - Allows to attach to and debug other processes, potentially allowing it to gain access to sensitive information or modify the behavior of other processes
`cap_sys_nice` - Allows to raise or lower the priority of processes, potentially allowing it to gain access to resources that would otherwise be restricted.
`cap_sys_time` - Allows to modify the system clock, potentially allowing it to manipulate timestamps or cause other processes to behave in unexpected ways
`cap_sys_resource` - Allows to modify system resource limits, suhc as the maximum number of open file descriptors or the maximum amount of memory that can be allocated
`cap_sys_module` - Allows to load and unload kernel modules, potentially allowing it to modify the operating system's behavior or gain access to sensitive information
`cap_net_bind_service` - Allows to bind to network ports, potentially allowing it to gain access to sensitive information or perform ununauthorized actions


- When a binary is executed with capabilities, it can perform the actions that the capabilities allow. however, it will not be able to perform any actions not allowed by the capabilities. This allows for more fine-grained control over the binary's pivileges and can help prevent security vulnerabilities and unauthorized access to sensitive information

### Capabilities use for Privilege Escalation
`cap_setuid` - Allows a process to set its effective user ID, which can be used to gain the privileges of another user, including the `root` user.
`cap_setgid` - Allows to set its effective group ID, which can be used to gain the privileges of another group, including the `root` group.
`cap_sys_admin` - This capability provides a broad range of adminsitrative privileges, including the ability to perform many actions reserved for the `root` user, such as modifying system settings and mounting and unmounting file systems.
`cap_dac_override` - Allows bypassing of file, write, and execute permission checks.

#### Enumerating Capabilities
```bash
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```
This one-liner uses the `find` command to search for all binary executables in the directories where they are typically located and then uses the `-exec` flag to run the `getcap` command on each, showing the capabilities that have been set for that vbinary. The output of this comand will show a list of all binary executables on the system + capabilities


#### Exploitation
- `dac_cap_override`
```bash
getcap /usr/bin/vim.basic

/usr/bin/vim.basic cap_dac_override=eip
```

`/usr/bin/vim.basic` binary is run without special privileges, such as with `sudo`. However, because the binary has the `cap_dac_override` capability set it can escalate the privileges of the user who runs it. This would allow the penetration tester to gain the `cap_dac_overrode`
capability and perform tasks that require this capability




























