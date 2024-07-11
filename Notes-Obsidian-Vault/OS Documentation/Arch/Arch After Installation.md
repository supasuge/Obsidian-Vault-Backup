## Table of Contents

  - [1. Update the System](#1.\Update\the\System)
  - [2. Install a Display Server (GUI)](#2.\Install\a\Display\Server\(GUI))
- [Create users](#create\users)
  - [To Update Yay](#To\Update\Yay)

## 1. Update the System
`sudo pacman -Syyu`

## 2. Install a Display Server (GUI)
popular go-to is the xorg option but other's exist.

# Create users
```bash
useradd -mg wheel blu3
user: kenny
group: wheel - all have access to sudo but need password
#change blu3's password

passwd blu3


Give the user sudo access:
nano /etc/sudoers
```


![[Pasted image 20231120045141.png]]


![[Pasted image 20231120045908.png]]

## To Update Yay
`yay -Suy `

https://www.youtube.com/watch?v=lfUWwZqzHmA&t=43s


https://github.com/akr3ch/CheatSheet


Alternatively, if you are on Linux, you can [use the dd command to create a live USB](https://itsfoss.com/live-usb-with-dd-command/). Replace _/path/to/archlinux.iso_ with the path where you have downloaded the ISO file, and _/dev/sdx_ with your USB drive in the example below. You can get your drive information using [_lsblk_](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/s1-sysinfo-filesystems?ref=itsfoss.com) command.

```
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress && sync
```