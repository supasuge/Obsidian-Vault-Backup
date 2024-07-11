## Table of Contents

  - [Step 1 - Check wifi](#Step\1\-\Check\wifi)
  - [Sync clock](#Sync\clock)
- [CHOOSE THE RIGHT DISKS](#choose\the\right\disks)
    - [Set the boot flag!!!](#Set\the\boot\flag!!!)
- [Format as ext4](#format\as\ext4)
    - [Swap](#Swap)
    - [Mount the drives](#Mount\the\drives)
    - [Create a directory on /mnt for boot](#Create\a\directory\on\/mnt\for\boot)
- [boot partition](#boot\partition)
    - [Make the FSTab file](#Make\the\FSTab\file)
    - [Install Network Manager & Grub (boot)](#Install\Network\Manager\&\Grub\(boot))
    - [To enable NetworkManager on boot](#To\enable\NetworkManager\on\boot)
- [if it does not say](#if\it\does\not\say)
- [init gen](#init\gen)
    - [Define lang in /etc/locale.conf](#Define\lang\in\/etc/locale.conf)
      - [Set Hostname](#Set\Hostname)
      - [Set localtime zone](#Set\localtime\zone)
    - [DONE](#DONE)

Long as the disk is uner 2TB use `DOS`
## Step 1 - Check wifi
```bash
ping archlinux.org
```
## Sync clock
```bash
timedatectl set-ntp true
```
```bash
lsblk
---
# CHOOSE THE RIGHT DISKS
```
- Figure out ahead of time the disk name/size etc.
```bash
cfdisk /dev/sda

#set label type

```

If installing to new system, and disk is larger than 2TB use `GPT` ELSE: Use `DOS`
![[Pasted image 20231120035648.png]]


![[Pasted image 20231120035819.png]]
### Set the boot flag!!!
![[Pasted image 20231120035847.png]]
Press B
![[Pasted image 20231120040020.png]]
# Format as ext4
```bash
mkfs.ext4 /dev/sda1

mkfs.ext4 /dev/sda2
```


### Swap
	Don't technically need it
Allows computer to treat hard disk space as extra ram if it runs out.

### Mount the drives
```bash
mount /dev/sda2 /mnt
```
### Create a directory on /mnt for boot
```bash
mkdir /mnt/boot
```
```bash
mount /dev/sda1 /mnt/boot
# boot partition
```
![[Pasted image 20231120040735.png]]
```bash
pacstrap /mnt base base-devel linux linux-firmware vim nano
```
Next:
### Make the FSTab file
contains all the drives you may use when booting up.
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```
```bash
arch-chroot /mnt /bin/bash
```
### Install Network Manager & Grub (boot)
```bash
pacman -S networkmanager grub
```
### To enable NetworkManager on boot
`systemctl enable NetworkManager`
![[Pasted image 20231120042110.png]]
```bash
grub-install /dev/sda

grub-mkconfig -o /boot/grub/grub.cfg
# if it does not say 
```

Set Root
```
passwd

Type password:
again:

#generate locale
vim /etc/locale.gen

# init gen
locale-gen

```

![[Pasted image 20231120042600.png]]



![[Pasted image 20231120042633.png]]


### Define lang in /etc/locale.conf
```locale.conf
LANG=en-US.UTF-8
```

#### Set Hostname
```
nano /etc/hostname
---
archey
```

#### Set localtime zone
```
ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
```

### DONE


`exit`
`umount -R /mnt`
`reboot`
![[Pasted image 20231120043519.png]]


![[Pasted image 20231120043800.png]]

1. Download ISO and install to USB
2. `iwctl` - connect to wifi
	- `station wlan0 show`
	- `station wlan0 connect`
3. Update the Arch keyring
	- `pacman-key --init`
	- `pacman-key --populate archlinux`
4. Install Arch with installer
	- `archinstall`

