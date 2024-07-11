 ## Very useful keybindigs to know...

[](https://github.com/gh0stzk/dotfiles?tab=readme-ov-file#very-useful-keybindigs-to-know)

|Keys|Action|
|:--|:--|
|super + Enter  <br>super + alt + Enter|Open a terminal  <br>Open a floating terminal.|
|alt + @space|Display menu to select a theme.|
|super + @space|Apps Menu.|
|super + alt + w|Opens a menu to select a wallpaper.|
|super + h  <br>super + u|Hides bar/s  <br>unhide bar/s|
|Print|Takes screenshot.|
|ctrl + alt + [plus,minus,t]|Changes transparency on focused window.|
|ctrl + super + alt + p  <br>ctrl + super + alt + r  <br>ctrl + super + alt + k|Power off computer  <br>Restart computer  <br>Brute kill a window/process|
|super + alt + r|Restart bspwm.|

And more.. You need to look sxhkdrc file for more.

## 📦 setup

[](https://github.com/gh0stzk/dotfiles?tab=readme-ov-file#-setup)

### 💾 Installation:

[](https://github.com/gh0stzk/dotfiles?tab=readme-ov-file#-installation)

The installer only works for **ARCH** Linux, and based distros.

**Open a terminal in HOME**

- **First download the installer**

```shell
curl https://raw.githubusercontent.com/gh0stzk/dotfiles/master/RiceInstaller -o $HOME/RiceInstaller

# Maybe you want a short url??

curl -L https://is.gd/gh0stzk_dotfiles -o $HOME/RiceInstaller
```

- **Now give it execute permissions**

```shell
chmod +x RiceInstaller
```

- **Finally run the installer**

```shell
./RiceInstaller
```



```bash
# 1.) Change into your Downloads folder (create the folder if not available)
cd ~/Downloads
# 2.) Clone the dotfiles repository into the Downloads folder
git clone --depth=1 https://gitlab.com/stephan-raabe/dotfiles.git
# 3.) Change into the dotfiles folder
cd dotfiles
# 4.) Start the installation
./install.sh
```

```bash
cd ~/Downloads

wget https://gitlab.com/stephan-raabe/dotfiles/-/raw/main/apps/ML4W_Dotfiles_Installer.AppImage
```

Monitor 1 (ACER) Resolution: 2560x1440
Monitor 2(Small ACER): 1920x1080

Check for internet connection
`ping google.com`...

`iwctl`
`device list` - Show network interfaces
`station wlan0 get-networks` - See a list of wifi netoworks
`station wlan0 connect <SSID>`
> Enter Password
- If not able to connect use `rfkill` to make sure it isn't blocking connections by default

- `ping`

`pacman -Sy`
`pacman -Sy archlinux-keyring`
`pacman -Sy archinstall`
`archinstall`

1. Choose mirrors
2. Choose disk configuration
	1. Select the External SSD Drive
3. Choose `btrfs`
4. Set encryption password
5. Set boot loader to Grub
6. Set Hostname
7. Set Root Password
8. Add a new user account
9. Add new user to sudo list
10. Set profile > Desktop
11. Choose Graphics drives > Nvidia propietary
12. Set audio server to pipewire
13. Set kernel to Linux/linux-hardened
14. Use NetworkManager
15. Set correct timezone

Additional Packages: `python312`, `git`, `discord`, `firefox`, `libreoffice-fresh`, `unrar`, `tar`, `p7zip` `p7zip-plugins`, `rsync`, `ufw`, `keepassxc`, `age`, `arch-audit`, `base-devel`, `iwd`, `vim`, `dracut`, `efibootmgr`, `binutils`, `dhcpcd`, `sbsigntools`, `lvm2`, `man-db`

`git curl wget less htop neofetch rust net-tools vim firefox man-db sbsigntools dhcpcd binutils iwd dracut ufw rsync`

## Method 2 (Manual)
Check for internet connection
`ping google.com`...

`iwctl`
`device list` - Show network interfaces
`station wlan0 get-networks` - See a list of wifi netoworks
`station wlan0 connect <SSID>`
> Enter Password
- If not able to connect use `rfkill` to make sure it isn't blocking connections by default

- `ping`

`pacman -Sy`
`pacman -Sy archlinux-keyring`

`fdisk -l` - locate disk.
1. Partitioning
`lsblk` - Find the name of the device to install to (`/dev/sda` in our case.)
- Overwrite with zero's
`dd if=/dev/zero of=/dev/sda status=progress`
- CTRL + C Quickly
 `sync`
 `gdisk /dev/sda` - Partition table
 - When tool loads create a 512MB Blank GPT table as shown below:
2 - Create Blank GPT
> `: 2`
Command: `n`
(Enter for partition number
First Sector: Blank(Enter)
Second Sector: `512M`
Hex Code or GUID: `EF00`

Command: `n`
Enter for partition number
First sector: Blank(Enter)
Second sector: Blank(Enter)
Hex code or GUID: `8309`

Command: `wq`
Proceed? Y

Format EFI partition
`mkfs.fat -F32 /dev/sda1`

Use `cryptsetup` for format the other partition
`cryptsetup luksFormat --type luks2 /dev/sda2`
> Enter & Verify Passphrase

Open the enccrypted partition and assign it a name
`cryptsetup open --persistent --perf-no_read_workqueue --perf-no_write_workqueue /dev/sda2 cryptlvm`

Configuring LVM and formatting root partition:
```
pvcreate /dev/mapper/cryptlvm
vgcreate vg /dev/mapper/cryptlvm
lvcreate -l 100%FREE vg -n root
mkfs.ext4 /dev/vg/root
```


Next, mount your drives:
```
mount /dev/vg/root /mnt
mkidir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
```

`pacstrap /mnt base linux linux-firmware intel-ucode sudo vim lvm2 dracut sbsigntools iwd efibootmgr binutils dhcpcd man-db git`

`genfstab -W /mnt >> /mnt/etc/fstab`

`arch-chroot /mnt`
> Change password using `passwd`

- Link timezone and set clock
`timedatectl`
`ln -sf /usr/share/zoneinfo/<Region>/<City> /etc/localtime`
`hwclock --systohc`

- Set locale's
`vim /etc/locale.gen`
> Uncomment the one for `EN_US`
`localegen`
`vim locale.conf`
```
LANG=en_US.UTF-8
```

Set you hostname
`vim /etc/hostname`

Create your user:
`useradd YOUR_NAME`
`mkdir /home/YOUR_NAME`
`chown YOUR_NAME:YOUR_NAME /home/YOUR_NAME`
`passwd YOUR_NAME`

`visudo`
> Uncomment the line with the `wheel` group
`%wheel	ALL=(ALL) ALL # Uncomment this line
- Add user to sudo
`usermod -aG wheel YOUR_NAME`

Enable some basic systemd units:
```bash
systemctl enable dhcpcd
systemctl enable iwd
```



## Configuring Secure Boot
Create dracut scripts that will hook into pacman:
vim `/usr/local/bin/dracut-install.sh`
```bash
#!/usr/bin/env bash
mkdir -p /boot/efi/EFI/Linux
while read -r line; do
	if [[ "$line" == 'usr/lib/modules/'+([^/])'/pkgbase' ]]; then
		kver="${line#'usr/lib/modules/'}"
		kver="${kver%'/pkgbase'}"
	
		dracut --force --uefi --kver "$kver" /boot/efi/EFI/BOOT/bootx64.efi
	fi
done
```
Removal script:
`vim /usr/local/bin/dracut-remove.sh`
```bash
#!/usr/bin/env bash
rm -f /boot/efi/EFI/BOOT/bootx64.efi
```

Make those scripts executable and create pacman's hook directory:

```bash
chmod +x /usr/local/bin/dracut-*
mkdir -p /etc/pacman.d/hooks
```

Now for the actual hooks, first for install and upgrade:
` vim /etc/pacman.d/hooks/90-dracut-install.hook`
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
And for removal:

`vim /etc/pacman.d/hooks/60-dracut-remove.hook`
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

- Check UUID of your encrypted volume and write it to file you will edit next:
`blkid -s UUID -o value /dev/sda2 >> /etc/dracut.conf.d/cmdline.conf`

- Edit the file and fill with kernel arguments:
`vim /etc/dracut.conf.d/cmdline.conf`
```
kernel_cmdline="rd.luks.uuid=luks-YOUR_UUID rd.lvm.lv=vg/root root=/dev/mapper/vg-root rootfstype=ext4 rootflags=rw,relatime"
```

- Create file with flags:
`vim /etc/dracut.conf.d/flags.conf`
```
compress="zstd"
hostonly="no"
```

Generate your image by re-installing `linux` package and making sure the hooks work properly:
```bash
pacman -S linux sbctl
```

- You should now have `arch-linux.efi` within your `/efi/EFI/Linux/`

Add UEFI boot entry and create an order of booting:
`efibootmgr`

`efibootmgr --create --disk /dev/sda1 --label "Arch Linux" --loader 'EFI\BOOT\bootx64.efi' --unicode`

`efibootmgr`

```
efibootmgr -o ARCH_INDEX_FROM_PREVIOUS_COMMAND # 0 or whatever number your Arch entry shows as
```

- Quit installer and reboot.

At this point go to boot menu and erase the keys.

Create keys.
`sbctl create-keys`

Sign bootx64.efi file
`sbctl sign -s /boot/efi/EFI/BOOT/bootx64.efi`

Configure dracut to know where the signing keys are:
`vim /etc/dracut.conf.d/secureboot.conf`
```
uefi_secureboot_cert="/usr/share/secureboot/keys/db/db.pem"
uefi_secureboot_key="/usr/share/secureboot/keys/db/db.key"
```

Fix sbctl's pacman hook. Creating the following file will overshadow the real one:
`vim /etc/pacman.d/hooks/zz-sbctl.hook`
```
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
Exec = /usr/bin/sbctl sign /boot/efi/EFI/BOOT/bootx64.efi
```

Enroll previously generated keys (drop microsoft option if you don't want their keys):
```bash
sbctl enroll-keys --microsoft
```

Reboot the system. Enable only UEFI boot in BIOS and set BIOS password so evil maid won't simply turn off the setting. If everything went fine you should first of all, boot into your system, and then verify with sbctl or bootctl:
```
sbctl status
  Installed:	✓ sbctl is installed
  Owner GUID:	YOUR_GUID
  Setup Mode:	✓ Disabled
  Secure Boot:	✓ Enabled
```
## Post-Installation
- Switch to new user
```bash
su YOUR_USER
Enter Password: ....
```
- Update System
```bash
sudo pacman -Syu
```
- Install additional packages
```bash
sudo pacman -Sy python312 git discord firefox libreoffice unrar tar p7zip p7zip-plugins rsync ufw keepassxc arch-audit base-devel iwd vim dracut efibootmgr binutils dhcpcd sbsigntools, lvm2, man-db
```
- Install CPU Microcode
```bash
sudo pacman -S intel-ucode
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
- Rank Mirrorlists for faster updates etc.
```bash
mv /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak #Create a backup of mirrorlist
rankmirrors /etc/pacman.d/mirrorlist.bak > /etc/pacman.d/mirrorlist #Rank all mirrors based on their speed
```
- Install `yay`
```bash
cd ~/Downloads
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
- Install Signal Desktop
```bash
yay -S signal-desktop
```

- Setup NFTables
```bash
sudo pacman -S nftables
```

Edit the `/etc/nftables.conf`. Proposed firewall rules:

- drop all forwarding traffic (we're not a router),
- allow loopback (127.0.0.0)
- allow ICMP for v4 and v6 (you can turn it off, but for v6 it will dissable [SLAAC](https://wiki.archlinux.org/title/IPv6#Stateless_autoconfiguration_(SLAAC))),
- allow returning packets for established connections,
- block all else.

```
#!/usr/bin/nft -f
 
 table inet filter
 delete table inet filter
 table inet filter {
   chain input {
     type filter hook input priority filter
     policy drop
 
     ct state invalid drop comment "early drop of invalid connections"
     ct state {established, related} accept comment "allow tracked connections"
     iifname lo accept comment "allow from loopback"
     ip protocol icmp accept comment "allow icmp"
     meta l4proto ipv6-icmp accept comment "allow icmp v6"
     pkttype host limit rate 5/second counter reject with icmpx type admin-prohibited
     counter
   }

   chain forward {
     type filter hook forward priority filter
     policy drop
   }
 }
```

Enable the nftables service and list loaded rules for confirmation:
```bash
systemctl enable --now nftables

nft list ruleset
```

Installing HyprV4 Desktop GUI
```bash
yay -S linux-headers nvidia-dkms qt5-wayland qt5ct libva libva-nvidia-driver-git
```

```
git clone https://github.com/SolDoesTech/HyprV4.git
cd HyprV4
chmod +x ./set_hypr
./set_hypr
```

### Get Copy of Notes
[Proton Drive](https://drive.proton.me)

### Hardening Firefox
[Firefox hardening guide](https://brainfucksec.github.io/firefox-hardening-guide)
- Ublock Origin, noscript
### Installing QEMU/KVM/Virt-Manager
- [Resource](https://computingforgeeks.com/install-kvm-qemu-virt-manager-arch-manjar/)

### Mullvad VPN
https://www.heeney.ie/2022/10/01/how-to-set-up-mullvad-vpn-cli-in-linux/
https://wiki.archlinux.org/title/Mullvad


#### Securing Arch
`kernelHarden.sh`
```bash
#!/bin/bash

# Function to create sysctl configuration files with specified content
create_sysctl_conf() {
    local filename=$1
    local content=$2
    local filepath="/etc/sysctl.d/${filename}"

    echo "Creating ${filename}..."
    echo "${content}" > "${filepath}"
    if [ $? -eq 0 ]; then
        echo "${filename} created successfully."
    else
        echo "Failed to create ${filename}."
        exit 1
    fi
}

# Kernel Security Enhancements

# Prevent kernel pointer leaks
create_sysctl_conf "kptr_restrict.conf" "kernel.kptr_restrict=2"

# Block non-root users from seeing kernel logs
create_sysctl_conf "dmesg_restrict.conf" "kernel.dmesg_restrict=1"

# Only root can use the BPF JIT compiler and harden it
create_sysctl_conf "harden_bpf.conf" "kernel.unprivileged_bpf_disabled=1
net.core.bpf_jit_harden=2"

# Only processes with CAP_SYS_PTRACE can use ptrace
create_sysctl_conf "ptrace_scope.conf" "kernel.yama.ptrace_scope=2"

# Disable kexec
create_sysctl_conf "kexec.conf" "kernel.kexec_load_disabled=1"

# Harden TCP
create_sysctl_conf "tcp_hardening.conf" "net.ipv4.tcp_syncookies=1
net.ipv4.tcp_rfc1337=1
net.ipv4.conf.default.rp_filter=1
net.ipv4.conf.all.rp_filter=1
net.ipv4.conf.all.accept_redirects=0
net.ipv4.conf.default.accept_redirects=0
net.ipv4.conf.all.secure_redirects=0
net.ipv4.conf.default.secure_redirects=0
net.ipv6.conf.all.accept_redirects=0
net.ipv6.conf.default.accept_redirects=0
net.ipv4.conf.all.send_redirects=0
net.ipv4.conf.default.send_redirects=0
net.ipv4.icmp_echo_ignore_all=1"

# Improve ASLR effectiveness for mmap
create_sysctl_conf "mmap_aslr.conf" "vm.mmap_rnd_bits=32
vm.mmap_rnd_compat_bits=16"

# Disable SysRq key
create_sysctl_conf "sysrq.conf" "kernel.sysrq=0"

# Disable unprivileged user namespaces
create_sysctl_conf "unprivileged_users_clone.conf" "kernel.unprivileged_userns_clone=0"

# Disable TCP SACK
create_sysctl_conf "tcp_sack.conf" "net.ipv4.tcp_sack=0"

# Reload sysctl configurations
echo "Reloading sysctl configuration..."
sysctl --system
if [ $? -eq 0 ]; then
    echo "Sysctl configuration reloaded successfully."
else
    echo "Failed to reload sysctl configuration."
    exit 1
fi
```

- Strong Passwords and User Access Control
- Encrypted system partitions
- Disable SSH/Password Authentication
	- Disable Root login, `fail2ban`, Adjust SSH daemon config to only allow secure ciphers/protocols
- 
##### Mandatory Access Control (MAC)
MAC systems give fine-grained control over what programs can access. This means your browser won't have access to your entire home directory or something similar.

To enable Apparmor, install the AppArmor package, enable the AppArmor systemd service and set the kernel parameters required for AppArmor described above

To create AppArmor profiles run

```bash
aa-genprof /usr/bin/(program)
```
##### Kernel Security
To change permanently add files to `/etc/sysctl.d`...

Create a file called `kptr_restrict.conf`
Add:
```
kernel.kptr_restric=2
```
- Precents any kernel pointer leaks

Create a file called `dmesg_restrict.conf`
Add:
```
kernel.dmesg_restric=1
```
- This blocks users other than root from being able to see the kernel logs

Create a file called `harden_bpf.conf`
Add:
```
kernel.unprivileged_bpf_disabled=1
net.core.bpf_jit_harden=2
```
- This makes it so that only root can use the BPF JIT compiler and to harden it


Create a file called `ptrace_scope.conf`
Add:
```
kernel.yama.ptrace_scope=2
```
- This makes only processes with CAP_SYS_PTRACE able to use ptrace.
	- Ptrace is a system call that allows a program to alter and inspect a running process.

Create a file called `kexec.conf`
Add:
```
kernel.kexec_load_disabled=1
```
- This disables kexec which can be used to replace the running kernel.


Create a file called `tcp_hardening.conf`
Add:
```
net.ipv4.tcp_syncookies=1
net.ipv4.tcp_rfc1337=1
net.ipv4.conf.default.rp_filter=1  
net.ipv4.conf.all.rp_filter=1
net.ipv4.conf.all.accept_redirects=0  
net.ipv4.conf.default.accept_redirects=0  
net.ipv4.conf.all.secure_redirects=0  
net.ipv4.conf.default.secure_redirects=0  
net.ipv6.conf.all.accept_redirects=0  
net.ipv6.conf.default.accept_redirects=0
net.ipv4.conf.all.send_redirects=0  
net.ipv4.conf.default.send_redirects=0
net.ipv4.icmp_echo_ignore_all=1
```
- Helps protect against SYN flood attacks (DoS)
- This protects against time-wait assassination. It drops RST packets for sockets in the time-wait state.
- These enable source validation of packets received from all interfaces of the machine. This protects against IP spoofing
- These disable ICMP redirect acceptance. If these settings are not set then an attacker can redirect an ICMP request to anywhere they want.
- Ignore ICMP Requests

Create a file called `mmap_aslr.conf`
Add:
```
vm.mmap_rnd_bits=32  
vm.mmap_rnd_compat_bits=16
```
- These settings are set to the highest value to improve ASLR effectiveness for mmap

Create a file called `sysrq.conf`
Add:
```
kernel.sysrq=0
```
- Disables the SysRq key which exposes tons of potentially dangerous debugging functionality to unprivileged local users.

Create a file called `unprivileged_users_clone.conf`
Add:
```
kernel.unprivileged_userns_clone=0
```
- This disables unprivileged user namespaces. User namespaces add a lot of attack surface for privilege escalation so it's best to restrict them to root only

Create a file called `tcp_sack.conf`
Add:
```
net.ipv4.tcp_sack=0
```
- This disables [TCP SACK](https://tools.ietf.org/html/rfc2018). SACK is [commonly exploited](https://github.com/Netflix/security-bulletins/blob/master/advisories/third-party/2019-001.md) and not needed for many circumstances so it should be disabled if you don't need it
##### Boot Parameters
- If using GRUB then edit `/etc/default/grub` and add your parameters at "`GRUB_CMDLINE_LINUX_DEFAULT=`"

- If using Syslinux then edit `/boot/syslinux/syslinux.cfg` and add them to the '`APPEND`' line.

- Add "`apparmor=1 security=apparmor`" to enable **AppArmor**. If using **SELinux** or other LSMs then do not use these settings.
- Add "`slab_nomerge`" to disable slab merging. Sometimes a slab can be used in a vulnerable way which an attacker can exploit.
- Add "`slub_debug=FZ`" to enable sanity checks (F) and **redzoning** (Z). Sanity checks make sure that memory has been overwrited correctly. **Redzoning** adds extra areas around slabs that detect when a slab is overwritten past its real size, which can help detect overflows.
- Add "`init_on_alloc=1 init_on_free=1`". This zeroes memory during allocation and free time to prevent leaking secrets in memory.
- Add "`mce=0`". This causes the kernel to panic on uncorrectable errors in ECC memory which could be exploited. This is not needed for systems without ECC memory.  
- Add "`pti=on`". This enables Kernel Page Table Isolation which mitigates Meltdown and prevents some KASLR bypasses.  
- Add "`mds=full,nosmt`" to enable all mitigations for the MDS vulnerability and disable SMT. This may have a significant performance decrease as it disables hyperthreading.  
- Add "`module.sig_enforce=1`". This only allows kernel modules [signed with a valid key](https://www.kernel.org/doc/html/v5.2-rc3/admin-guide/module-signing.html) to be loaded which increases security by making it harder to load a malicious kernel module. This prevents all out-of-tree kernel modules from being loaded. This includes modules such as the virtualbox modules, wireguard and nvidia drivers which may not be wanted depending on your setup.  
- Add "`oops=panic`". This causes the kernel to panic on oopses. This prevents the kernel from continuing to run a flawed process which can be exploited. Kernel exploits sometimes also cause an oops which this will help against. Sometimes, buggy drivers cause harmless oopses which will result in your system crashing so this boot parameter can only be used on certain hardware.  
- Optionally add "`ipv6.disable=1`" to disable the whole IPv6 stack which may have some privacy issues depending on your setup as it uses your MAC address to create an IPv6 address which can be used to track you. This can be solved with [IPv6 privacy extensions](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_privacy) rather than disabling it entirely. This also reduces attack surface so its best to disable it if not needed.

Finally:
```
GRUB_CMDLINE_LINUX_DEFAULT="apparmor=1 security=apparmor slab_nomerge slub_debug=FZ init_on_alloc=1 init_on_free=1 mce=0 pti=on mds=full,nosmt module.sig_enforce=1 oops=panic, intel_iommu=on quiet"
```
- Then Re-generate your GRUB Configuration file to apply these changes
```
grub-mkconfig -o /boot/grub/grub.cfg
```

##### Process ID's (hidepid)[1.3](https://theprivacyguide1.github.io/linux_hardening_guide#ref-18)
To allow users to only see their own processes, edit `/etc/fstab` and add:
```
proc /proc proc nosuid,nodev,noexec,hidepid=2,gid=proc 0 0
```

systemd-logind still needs to see other users' processes so, **for user sessions to work correctly** add this to `/etc/systemd/system/systemd-logind.service.d/hidepid.conf`:
```
[Service]  
SupplementaryGroups=proc
```

##### Netfilter
create `/etc/modprobe.d/no-conntrack-helper.conf`
Add:
```
options nf_conntrack nf_conntrack_helper=0
```

##### Sandboxing
[Bubblewrap](https://wiki.archlinux.org/title/Bubblewrap)
[Bubblejail](https://wiki.archlinux.org/title/Bubblejail)
##### Restricting `su`
To restrict the use of su to users within the 'wheel' group, edit `/etc/pam.d/su` and `/etc/pam.d/su-l` (that's the letter l, not the number one) and uncomment
```
auth required pam_wheel.so use_uid 
```

##### Locking the root account

To lock the root account to prevent anyone from logging in as root, run
```bash
passwd -l root
```

##### Denying Root Login via SSH

To prevent someone from logging in as root via SSH edit `/etc/ssh/sshd_config` and add
```
PermitRootLogin no
```

##### Increase the Number of Hashing Rounds
By default `shadow` uses 5000 rounds but this can be increased to as many as you want. 
Edit `/etc/pam.d/passwd`
```
password required pam_unix.so sha512 shadow nullok rounds=65536
```
This makes shadow perform 65536 rounds.  
  
Your passwords are not automatically rehashed after applying this setting so you need to reset the password with
```
passwd username
```

##### Systemd Sandboxing
Systemd has the ability to sandbox services so they can only access what they need. Here is an example of a sandboxed systemd service.
```
[Service]  
CapabilityBoundingSet=CAP_NET_BIND_SERVICE  
ProtectSystem=strict  
ReadWriteDirectories=/var/lib/tor/  
ProtectHome=true  
ProtectKernelTunables=true  
ProtectKernelModules=true  
ProtectControlGroups=true  
PrivateTmp=true  
PrivateUsers=yes  
MemoryDenyWriteExecute=true  
NoNewPrivileges=true  
RestrictRealtime=true  
RestrictAddressFamilies=AF_INET AF_UNIX  
SystemCallArchitectures=native  
RestrictNamespaces=yes  
RuntimeDirectoryMode=0700  
SystemCallFilter=~@clock @cpu-emulation @debug @keyring @module @mount @obsolete @raw-io  
AppArmorProfile=/etc/apparmor.d/usr.bin.tor
```
[systemd.exec](https://www.freedesktop.org/software/systemd/man/systemd.exec.html)


##### Firewalls
Always block incoming traffic unless you have a specific reason not to.
Use `iptables`, or `nftables` firewall. Or `ufw`.


##### Torsocks

Torsocks can force other programs' network traffic through Tor which allows you to anonymize more than just your browsing.

##### Stream Isolation

Stream isolation makes programs use different Tor circuits from each other to prevent identity correlation. [[34]](https://theprivacyguide1.github.io/linux_hardening_guide#ref-34) To enable this edit /etc/tor/torrc and configure more SocksPorts. 9050 is the default SocksPort. Once you have made more, configure your programs to use those new ports and you will have stream isolation.  
  
Pacman can use stream isolation by editing /etc/pacman.conf. Add this to /etc/pacman.conf

```
XferCommand = /usr/bin/curl --socks5-hostname localhost:9062 --continue-at - --fail --output %o %u
```
Replace `9062` with your SocksPort.


##### Transparent Proxy

You can configure your whole system to use Tor by default with a transparent proxy to anonymize all internet traffic.  
  
To do this add this to `/etc/tor/torrc`:

```
TransPort 9040  
DNSPort 5353  
SocksPort 9050
```

Then, create `/etc/iptables/iptables.rules` and add:
```
*nat
:PREROUTING ACCEPT [6:2126]
:INPUT ACCEPT [0:0]
:OUTPUT ACCEPT [17:6239]
:POSTROUTING ACCEPT [6:408]

-A PREROUTING ! -i lo -p udp -m udp --dport 53 -j REDIRECT --to-ports 5353
-A PREROUTING ! -i lo -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -j REDIRECT --to-ports 9040
-A OUTPUT -o lo -j RETURN
--ipv4 -A OUTPUT -d 192.168.0.0/16 -j RETURN
-A OUTPUT -m owner --uid-owner "tor" -j RETURN
-A OUTPUT -p udp -m udp --dport 53 -j REDIRECT --to-ports 5353
-A OUTPUT -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -j REDIRECT --to-ports 9040
COMMIT

*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT DROP [0:0]

-A INPUT -i lo -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
--ipv4 -A INPUT -p tcp -j REJECT --reject-with tcp-reset
--ipv4 -A INPUT -p udp -j REJECT --reject-with icmp-port-unreachable
--ipv4 -A INPUT -j REJECT --reject-with icmp-proto-unreachable
--ipv6 -A INPUT -j REJECT
--ipv4 -A OUTPUT -d 127.0.0.0/8 -j ACCEPT
--ipv4 -A OUTPUT -d 192.168.0.0/16 -j ACCEPT
--ipv6 -A OUTPUT -d ::1/8 -j ACCEPT
-A OUTPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A OUTPUT -m owner --uid-owner "tor" -j ACCEPT
--ipv4 -A OUTPUT -j REJECT --reject-with icmp-port-unreachable
--ipv6 -A OUTPUT -j REJECT
COMMIT
```
- Symlink this to /etc/iptables/ip6tables.rules if using IPv6
Edit `/etc/resolv.conf` and set it to `127.0.0.1`. You may need to configure `dnsmasq` or any other resolver to set your DNS as `127.0.0.1:5353`

  
Edit `/etc/dhcpcd.conf` and add "`nohook resolv.conf`" to stop it from overwriting `resolv.conf`. Run "`chattr +i /etc/resolv.conf`" to prevent all services from overwriting it.
- To use stream isolation with a transparent proxy then add more SocksPorts in your torrc and configure your programs to use those ports.

##### Wireless Devices
To block all wireless devices run
```bash
rfkill block all
```

You can also blacklist certain modules to prevent them from loading. For example, to blacklist the bluetooth module, create `/etc/modprobe.d/blacklist-bluetooth.conf` and add
```
install btusb /bin/true
install bluetooth /bin/true
```

##### MAC Address Spoofing
Install `macchanger` and run:
`macchanger -e [network interface]`

To find out your network interfaces run:
`ip a`
- systemd service to spoof the MAC address of eth0 at boot. Replace eth0 with your network interface
```
[Unit]  
Description=macchanger on eth0  
Wants=network-pre.target  
Before=network-pre.target  
BindsTo=sys-subsystem-net-devices-eth0.device  
After=sys-subsystem-net-devices-eth0.device  
  
[Service]  
ExecStart=/usr/bin/macchanger -e eth0  
Type=oneshot  
  
[Install]  
WantedBy=multi-user.target
```

##### Umask
The default file permissions for  newly created files is `022` which is not very secure. This gives read access to every user on the system for new files. 
Edit `/etc/profile` and change the umask to `0077` which makes new files not readable by anyone other than the owner

##### DMA Attacks
Disable these modules by creating "`/etc/modprobe.d/blacklist-dma.conf`" and blacklist these modules from loading add:
```
install firewire-core /bin/true
install thunderbolt /bin/true
```


##### Disable Core Dumps
Use: `sysctl, systemd and ulimit`
Create `/etc/systemd/coredump.conf.d/custom.conf`. You may need to create `/etc/systemd/coredump.conf.d/` first. Add this to disable core dumps.
```
[Coredump]  
Storage=none
```
###### Disabling setuid processes from dumping their memory
Process that run with elevated privileges (setuid) may still dump their memory even after these settings. To prevent them from doing this create `/etc/sysctl.d/suid_dumpable.conf` and add
```
fs.suid_dumpable=0
```

##### NTP
NTP is insecure (Network Time Protocol)
[Secure Time Synchronization](https://gitlab.com/madaidan/secure-time-sync) - Securely syncs the time to current UTC time.
- Uninstall any NTP clients and disable it by running
```bash
timedatectl set-ntp 0
# And
systemctl disable systemd-timesyncd.service
```

##### IPv6 Privacy Extensions
Privacy extensions generate a random IPv6 address out of your original one to prevent you from being tracked by it.

To enable these create `/etc/sysctl/ipv6_privacy.conf` and add
```
net.ipv6.conf.all.use_tempaddr = 2  
net.ipv6.conf.default.use_tempaddr = 2  
net.ipv6.conf.eth0.use_tempaddr = 2  
net.ipv6.conf.wlan0.use_tempaddr = 2
```
- Replace `eth0` and `wlan0` with the correct network interfaces found using `ip a`

##### NetworkManager

`NetworkManager` does not use these settings and still uses your original IPv6 address. To enable privacy extensions for NetworkManager, add these lines to `/etc/NetworkManager/NetworkManager.conf`
```
[Network]  
IPv6PrivacyExtensions=kernel
```

##### GRUB
To set a password for GRUB run:
`grub-mkpasswd-pbkdf2`
Enter your password and a string will be generated from that password.
Edit `/etc/grub.d/40_custom` and add
```
set superusers="root"  
password_pbkdf2 username (password)
```
The username will be for the superusers who are users that are permitted to use the GRUB command line, edit menu entries, and execute any menu entry. For most people you can just keep this as "root".
- Regenerate your configuration file with
```
grub-mkconfig /boot/grub/grub.cfg
```

To restrict only editing the boot parameters and accessing the GRUB console while still allowing you to boot, edit `/boot/grub/grub.cfg` and next to "`menuentry` '`OS Name`'" add "`--unrestricted`"
```
menuentry 'Arch Linux' --unrestricted
```

- Regenerate your configuration file again
```
grub-mkconfig /boot/grub/grub.cfg
```

##### Blacklist Uncommon Network Protocols
Obscure networking protocols in particular add a lot of attack surface and unused protocols should be blacklisted.
Create `/etc/modprobe.d/uncommon-network-protocols.conf` and add
```
install dccp /bin/true  
install sctp /bin/true  
install rds /bin/true  
install tipc /bin/true  
install n-hdlc /bin/true  
install ax25 /bin/true  
install netrom /bin/true  
install x25 /bin/true  
install rose /bin/true  
install decnet /bin/true  
install econet /bin/true  
install af_802154 /bin/true  
install ipx /bin/true  
install appletalk /bin/true  
install psnap /bin/true  
install p8023 /bin/true  
install llc /bin/true  
install p8022 /bin/true
```

##### Partition and Mount options
You should separate file systems into various partitions so you will have more fine grained control over their permissions and will add an extra layer of security. You can add different mount options to restrict what can happen on a file system. The main ones are:
`nodev`- Do not interpret devices on the file system.  
`nosuid` - Do not allow setuid or setgid bits.  
`noexec` - Do not allow execution of any binaries on the filesystem.

You should put these mount options wherever possible. Nodev can be put everywhere except / and chroot partitions. Noexec can be put anywhere that doesn't execute binaries inside it.
You can do this in /etc/fstab.  
  
An example of a /etc/fstab file:

```
/        /          ext4    defaults                      1 1
/tmp     /tmp       ext4    defaults,nosuid,noexec,nodev  1 2
/home    /home      ext4    defaults,nosuid,nodev         1 2
/var     /var       ext4    defaults,nosuid               1 2
/boot    /boot      ext4    defaults,nosuid,noexec,nodev  1 2
/tmp     /var/tmp   ext4    defaults,bind,nosuid,noexec,nodev 1 2
```

##### Disable Mounting of Uncommon Filesystems

There are a few uncommon filesystems that are very rarely used. These don't serve any purpose for most people and it would be best to disable mounting these to reduce attack surface. Create `/etc/modprobe.d/uncommon-filesystems.conf` and add

```
install cramfs /bin/true  
install freevxfs /bin/true  
install jffs2 /bin/true  
install hfs /bin/true  
install hfsplus /bin/true  
install squashfs /bin/true  
install udf /bin/true
```

##### Haveged
- `Haveged` is a random number generator that gathers more entropy. To install it, run
`sudo pacman -S haveged`
- Enable it by running
`sudo systemctl enable --now haveged.service`

##### Microphones & Webcams
To blacklist the webcam kernel module, create `/etc/modprobe.d/blacklist-webcam.conf` and add
```
install uvcvideo /bin/true
```

The kernel module for the microphone is the same as the one for the speaker. This means disabling the microphone in this method will also disable any speakers. To find the name of the module, look in `/proc/asound/modules`. Create `/etc/modprobe.d/blacklist-mic.conf` and add

```
install (module) /bin/true
```

Replace "(module)" with whatever you found in /proc/asound/modules. For example, if you found "snd_hda_intel", you would add
```
install snd_hda_intel /bin/true
```
- You should also disable the webcam and microphone in your BIOS if possible.


[Paranoid security guide](https://web.archive.org/web/20140220055801/http://crunchbang.org:80/forums/viewtopic.php?id=24722)