
# Video
Update pacman and get keyring
```bash
pacman -Sy

pacman --needed -Sy archlinux-keyring

pacman -Sy archinstall
```


## Table of Contents

  - [Download](#Download)
  - [Prepare VM](#Prepare\VM)
  - [Pre-flight Checks](#Pre-flight\Checks)
  - [Installation](#Installation)
    - [Partition the Disk](#Partition\the\Disk)
    - [Format and Mount Partition](#Format\and\Mount\Partition)
    - [Install Base System](#Install\Base\System)
    - [Generate fstab](#Generate\fstab)
    - [Chroot into the System](#Chroot\into\the\System)
    - [Timezone and Clock](#Timezone\and\Clock)
    - [Locale Setup](#Locale\Setup)
    - [Set Hostname](#Set\Hostname)
    - [Enable DHCP client daemon](#Enable\DHCP\client\daemon)
    - [Set Root Password](#Set\Root\Password)
    - [Install GRUB](#Install\GRUB)
    - [Reboot](#Reboot)
  - [Post-installation Steps](#Post-installation\Steps)
    - [Add Non-root User](#Add\Non-root\User)
    - [Setup Swap](#Setup\Swap)
    - [Install X11](#Install\X11)
    - [Install Video Driver](#Install\Video\Driver)
    - [Install Fonts](#Install\Fonts)
    - [System Update](#System\Update)
    - [Install Hyprland (Wayland compositor)](#Install\Hyprland\(Wayland\compositor))
    - [Basic Hardening Steps](#Basic\Hardening\Steps)

Certainly! Here's your revised installation guide with commands in code blocks for easy copying and pasting:

## Download

- Download the latest version from [Arch Linux Downloads](https://www.archlinux.org/download/).
- Verify checksums.

## Prepare VM

- Create a new VM, choose type Linux and version Arch Linux (64-bit).
- Allocate a reasonable amount of RAM and create a new hard disk.
- Ensure two network adapters: Adapter 1 to NAT, Adapter 2 to Host-only (`vboxnet0`).

## Pre-flight Checks

```bash
ping archlinux.org
```

## Installation

### Partition the Disk

```bash
fdisk /dev/sda
n
p
1
(enter)
(enter)
w
```

### Format and Mount Partition

```bash
mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt
```

### Install Base System

```bash
pacstrap /mnt base linux linux-firmware
```

### Generate fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

### Chroot into the System

```bash
arch-chroot /mnt
```

### Timezone and Clock

```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime # Replace Region/City with your timezone
hwclock --systohc
```

### Locale Setup

1. Edit `/etc/locale.gen` and uncomment your locale (e.g., `en_US.UTF-8`).
2. Then:

   ```bash
   locale-gen
   echo "LANG=en_US.UTF-8" > /etc/locale.conf
   ```

### Set Hostname

```bash
echo your_hostname > /etc/hostname # Replace your_hostname with your desired hostname
```

### Enable DHCP client daemon

```bash
systemctl enable dhcpcd.service
```

### Set Root Password

```bash
passwd
```

### Install GRUB

```bash
pacman -S grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

### Reboot

```bash
exit
umount -R /mnt
reboot
```

## Post-installation Steps

### Add Non-root User

```bash
pacman -S sudo zsh
useradd -m -g users -s /usr/bin/zsh username # Replace username
passwd username
EDITOR=vi visudo
```

In `visudo`, add `username ALL=(ALL) ALL`.

### Setup Swap

```bash
pacman -S systemd-swap
systemctl enable systemd-swap.service
```

### Install X11

```bash
pacman -S xorg-server xorg-apps xterm
```

### Install Video Driver

For VirtualBox, use the VBoxGuestAdditions:

```bash
pacman -S virtualbox-guest-utils
```

### Install Fonts

```bash
pacman -S ttf-dejavu ttf-droid ttf-inconsolata
```

### System Update

```bash
pacman -Syu
```

### Install Hyprland (Wayland compositor)

```bash
pacman -S hyprland
```

### Basic Hardening Steps

1. Ensure only necessary services are enabled.
2. Regularly update your system: `pacman -Syu`.
3. Install a firewall, like `ufw`:

   ```bash
   pacman -S ufw
   systemctl enable ufw
   ufw enable
   ```

4. Consider using `fail2ban` for additional security.

This guide should set up a basic Arch Linux system in VirtualBox with Hyprland installed. For specific configurations of Hyprland and additional hardening, refer to the relevant documentation and best practices.




```
sudo pacman -Syu && sudo pacman -S --needed base-devel git
```

```bash
git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si && yay --version || echo "Failed..."
```

