## Table of Contents

  - [OS Installation](#OS\Installation)
    - [Unified Kernel Image](#Unified\Kernel\Image)

1. Flash USB With arch .iso file.
2. Enable wifi connection:
```bash
iwctl
station wlan0 connect SSID
<password-input>
exit
```


3. Disk Partitioning
- Overwrite with zero's
```
dd if=/dev/zero of=/dev/sda status=progress
^C Directly after
```

`gdisk /dev/sda
 > `2`
 > `n` (Enter Twice)
 > `Last Sector: 512M`
 > `Hex Code/GUID: EF00`
 > `n`
 > Enter, enter, Enter
 > `Hex Code/GUID: 8309 (Linux Luks)`
 > `Y`
 > `mkfs.fat -F32 /dev/sda1
> Format your Luks setup
> `cryptsetup luksFormat --type luks2 /dev/sda2`
> `YES > [Enter Password]
> `cryptsetup open --persistent /dev/sda2 cryptlvm`
> `pvcreate /dev/mapper/cryptlvm` 
> `vgcreate vg /dev/mapper cryptlvm`
> `lvcreate -l 100%FREE vg -n root`
> `mkfs.ext4 /dev/vg/root`
> `mount /dev/vg/root /mnt`
> `mkdir -p /mnt/boot/efi`
> `mount /dev/nvme0n1p1 /mnt/boot/efi`

## OS Installation
`pacstrap /mnt base linux-firmware intel-ucode sudo vim lvm2 dracut sbsigntools iwd git efibootmg gnu-netcat binutils dhcpcd linux-headers base-devel broadcom-wl`

1. Gen fstab
`genfstab -U /mnt >> /mnt/etc/fstab`
`arch-chroot /mnt`
Change password `passwd`
`pacman -Syu man-db`
`ln -sf /usr/share/zoneinfo/US/Detroit /etc/localtime`
`vim locale.gen`... then Uncomment desired locale (`US-UTF-8`)
`locale-gen`
`vim /etc/locale.conf`
```
LANG=en_US.UTF-8
```

Set hostname `/etc/hostname`
`useradd <user>`
`mkdir /home/<user>`
`chown <user>:<user> /home/<user>`
`passwd` - set password for the account
`visudo`
Uncomment (Wheel) group.
`systemctl enable dhcpcd`
`systemctl enable iwd`


### Unified Kernel Image
1. `vim /usr/local/bin/dracut-install.sh`
```bash
#!/usr/bin/env bash
mkdir -p /boot/efi/EFI/BOOT

while read -r line; do
		if [[ "$line" == 'usr/lib/modules/'+([^/])'/pkgbase' ]]; then
			kver="${line#'usr/lib/modules/'}"
			kver="${kver%'/pkgbase'}"
	
			dracut --force --uefi --kver "$kver" /boot/efi/EFI/Linux/arch-linux.efi
		fi
	done
```
2. Removal script
`vim /usr/local/bin/dracut-remove.sh`
```bash
#!/usr/bin/env bash
rm -f /boot/efi/EFI/Linux/arch-linux.efi
```
3. Make the scripts executable and create pacman's hook directory
```bash
chmod +x /usr/local/bin/dracut-*
mkdir /etc/pacman.d/hooks
```

4. `vim /etc/pacman.d/hooks/90-dracut-install.hook`
```bash
[Trigger]
Type = Path
Operation = Install
Operation = Upgrade
Target = usr/lib/modules/*/pkgbase
	
[Action]
Description = Updating linux EFI image
When = PostTransaction
Exec = /usr/local/bin/dracut-install.sh
Depends = dracut
NeedsTargets
```

Removal:
5. `vim /etc/pacman.d/hooks/60-dracut-remove.hook`
```bash
[Trigger]
Type = Path
Operation = Remove
Target = usr/lib/modules/*/pkgbase
	
[Action]
Description = Removing linux EFI image
When = PreTransaction
Exec = /usr/local/bin/dracut-remove.sh
NeedsTargets
```

Check UUID of your encrypted volume and write it to file you will edit next:
```bash
blkid -s UUID -o value /dev/nvme0n1p2 >> /etc/dracut.conf.d/cmdline.conf
```

Edit the file and fill with kernel arguments:
```
vim /etc/dracut.conf.d/cmdline.conf
-

kernel_cmdline="rd.luks.uuid=luks-YOUR_UUID rd.lvm.lv=vg/root root=/dev/mapper/vg-root rootfstype=ext4 rootflags=rw,relatime"


```

Create file with flags:

```bash
vim /etc/dracut.conf.d/flags.conf
-
compress="zstd"
hostonly="no"
```

Generate your image by re-installing `linux` package and making sure the hooks work properly:

```
pacman -S linux
```

You should have `arch-linux.efi` within your `/efi/EFI/Linux/`

Now you only have to add UEFI boot entry and create an order of booting:

```bash
efibootmgr --create --disk /dev/nvme0n1 --part 1 --label "Arch Linux" --loader 'EFI\Linux\arch-linux.efi' --unicode

efibootmgr 		# Check if you have left over UEFI entries, remove them with efibootmgr -b INDEX -B and note down Arch index

efibootmgr -o ARCH_INDEX_FROM_PREVIOUS_COMMAND # 0 or whatever number your Arch entry shows as
```

`poweroff`

make sure that secure boot setup mode is enabled.

Create keys and sign binaries:

```
sbctl create-keys
sbctl sign -s /boot/efi/EFI/Linux/arch-linux.efi #it should be single file with name verying from kernel version
```

Configure dracut to know where are signing keys:

```
vim /etc/dracut.conf.d/secureboot.conf
	uefi_secureboot_cert="/usr/share/secureboot/keys/db/db.pem"
	uefi_secureboot_key="/usr/share/secureboot/keys/db/db.key"
```

We also need to fix sbctl's pacman hook. Creating the following file will overshadow the real one:

```
vim /etc/pacman.d/hooks/zz-sbctl.hook
-
[Trigger]
Type = Path
Operation = Install
Operation = Upgrade
Operation = Remove
Target = boot/*
Target = efi/*
Target = usr/lib/modules/*/vmlinuz
Target = usr/lib/initcpio/*
Target = usr/lib/**/efi/*.efi*

[Action]
Description = Signing EFI binaries...
When = PostTransaction
Exec = /usr/bin/sbctl sign /boot/efi/EFI/Linux/arch-linux.efi
```


Enroll previously generated keys (drop microsoft option if you don't want their keys):

```
sbctl enroll-keys --microsoft
```

Reboot the system. Enable only UEFI boot in BIOS and set BIOS password so evil maid won't simply turn off the setting. If everything went fine you should first of all, boot into your system, and then verify with sbctl or bootctl:

```
sbctl status
-
  Installed:	✓ sbctl is installed
  Owner GUID:	YOUR_GUID
  Setup Mode:	✓ Disabled
  Secure Boot:	✓ Enabled
```
