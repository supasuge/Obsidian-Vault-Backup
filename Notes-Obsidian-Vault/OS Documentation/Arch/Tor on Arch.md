## Table of Contents

  - [Monitoring Network usage while using tor](#Monitoring\Network\usage\while\using\tor)
  - [Usage](#Usage)
  - [[1. Kernels](https://theprivacyguide1.github.io/linux_hardening_guide#kernel)](#[1.\Kernels](https://theprivacyguide1.github.io/linux_hardening_guide#kernel))
    - [[1.1 Sysctl](https://theprivacyguide1.github.io/linux_hardening_guide#sysctl)](#[1.1\Sysctl](https://theprivacyguide1.github.io/linux_hardening_guide#sysctl))
    - [[1.2 Boot Parameters](https://theprivacyguide1.github.io/linux_hardening_guide#boot_parameters)](#[1.2\Boot\Parameters](https://theprivacyguide1.github.io/linux_hardening_guide#boot_parameters))
    - [[1.4 Netfilter's connection tracking helper](https://theprivacyguide1.github.io/linux_hardening_guide#nf_conntrack_helper)](#[1.4\Netfilter's\connection\tracking\helper](https://theprivacyguide1.github.io/linux_hardening_guide#nf_conntrack_helper))
    - [[1.5 Linux-hardened](https://theprivacyguide1.github.io/linux_hardening_guide#linux-hardened)](#[1.5\Linux-hardened](https://theprivacyguide1.github.io/linux_hardening_guide#linux-hardened))
    - [[1.6 Grsecurity](https://theprivacyguide1.github.io/linux_hardening_guide#grsecurity)](#[1.6\Grsecurity](https://theprivacyguide1.github.io/linux_hardening_guide#grsecurity))
    - [[1.7 Compiling your own kernel](https://theprivacyguide1.github.io/linux_hardening_guide#compiling-your-own-kernel)](#[1.7\Compiling\your\own\kernel](https://theprivacyguide1.github.io/linux_hardening_guide#compiling-your-own-kernel))
    - [[2. Mandatory Access Control](https://theprivacyguide1.github.io/linux_hardening_guide#MAC)](#[2.\Mandatory\Access\Control](https://theprivacyguide1.github.io/linux_hardening_guide#MAC))
    - [[3. Sandboxes](https://theprivacyguide1.github.io/linux_hardening_guide#sandboxes)](#[3.\Sandboxes](https://theprivacyguide1.github.io/linux_hardening_guide#sandboxes))
    - [[3.1 Sandboxing Xorg](https://theprivacyguide1.github.io/linux_hardening_guide#sandboxing_xorg)](#[3.1\Sandboxing\Xorg](https://theprivacyguide1.github.io/linux_hardening_guide#sandboxing_xorg))
    - [[4. The Root Account](https://theprivacyguide1.github.io/linux_hardening_guide#root)](#[4.\The\Root\Account](https://theprivacyguide1.github.io/linux_hardening_guide#root))
    - [[4.1 /etc/securetty](https://theprivacyguide1.github.io/linux_hardening_guide#securetty)](#[4.1\/etc/securetty](https://theprivacyguide1.github.io/linux_hardening_guide#securetty))
    - [[4.2 Restricting su](https://theprivacyguide1.github.io/linux_hardening_guide#restricting_su)](#[4.2\Restricting\su](https://theprivacyguide1.github.io/linux_hardening_guide#restricting_su))
    - [[4.3 Locking the root account](https://theprivacyguide1.github.io/linux_hardening_guide#locking_root)](#[4.3\Locking\the\root\account](https://theprivacyguide1.github.io/linux_hardening_guide#locking_root))
    - [[4.4 Denying Root Login via SSH](https://theprivacyguide1.github.io/linux_hardening_guide#denying_root_login_ssh)](#[4.4\Denying\Root\Login\via\SSH](https://theprivacyguide1.github.io/linux_hardening_guide#denying_root_login_ssh))
    - [[4.5 Increase the Number of Hashing Rounds](https://theprivacyguide1.github.io/linux_hardening_guide#increase_number_of_rounds)](#[4.5\Increase\the\Number\of\Hashing\Rounds](https://theprivacyguide1.github.io/linux_hardening_guide#increase_number_of_rounds))
    - [[5. Systemd Sandboxing](https://theprivacyguide1.github.io/linux_hardening_guide#systemd_sandboxes)](#[5.\Systemd\Sandboxing](https://theprivacyguide1.github.io/linux_hardening_guide#systemd_sandboxes))
    - [[6. Restricting Xorg Root Access](https://theprivacyguide1.github.io/linux_hardening_guide#restricting_xorg)](#[6.\Restricting\Xorg\Root\Access](https://theprivacyguide1.github.io/linux_hardening_guide#restricting_xorg))
    - [[7. Firewalls](https://theprivacyguide1.github.io/linux_hardening_guide#firewalls)](#[7.\Firewalls](https://theprivacyguide1.github.io/linux_hardening_guide#firewalls))
    - [[8. Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor)](#[8.\Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor))
    - [[8.1 Tor browser](https://theprivacyguide1.github.io/linux_hardening_guide#tor_browser)](#[8.1\Tor\browser](https://theprivacyguide1.github.io/linux_hardening_guide#tor_browser))
    - [[8.2 Torsocks](https://theprivacyguide1.github.io/linux_hardening_guide#torsocks)](#[8.2\Torsocks](https://theprivacyguide1.github.io/linux_hardening_guide#torsocks))
    - [[8.3 Stream Isolation](https://theprivacyguide1.github.io/linux_hardening_guide#stream_isolation)](#[8.3\Stream\Isolation](https://theprivacyguide1.github.io/linux_hardening_guide#stream_isolation))
    - [[8.4 Transparent Proxy](https://theprivacyguide1.github.io/linux_hardening_guide#transparent_proxy)](#[8.4\Transparent\Proxy](https://theprivacyguide1.github.io/linux_hardening_guide#transparent_proxy))
    - [[8.5 Tor over Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor_over_tor)](#[8.5\Tor\over\Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor_over_tor))
    - [[8.6 Configuring the Tor Browser to prevent Tor over Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor_browser_tor_over_tor)](#[8.6\Configuring\the\Tor\Browser\to\prevent\Tor\over\Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor_browser_tor_over_tor))
    - [[9. Hostnames and Usernames](https://theprivacyguide1.github.io/linux_hardening_guide#hostnames)](#[9.\Hostnames\and\Usernames](https://theprivacyguide1.github.io/linux_hardening_guide#hostnames))
    - [[10. Wireless Devices](https://theprivacyguide1.github.io/linux_hardening_guide#bluetooth)](#[10.\Wireless\Devices](https://theprivacyguide1.github.io/linux_hardening_guide#bluetooth))
    - [[11. MAC Address Spoofing](https://theprivacyguide1.github.io/linux_hardening_guide#mac_address)](#[11.\MAC\Address\Spoofing](https://theprivacyguide1.github.io/linux_hardening_guide#mac_address))
    - [[12. Umask](https://theprivacyguide1.github.io/linux_hardening_guide#umask)](#[12.\Umask](https://theprivacyguide1.github.io/linux_hardening_guide#umask))
    - [[13. USBs](https://theprivacyguide1.github.io/linux_hardening_guide#usbs)](#[13.\USBs](https://theprivacyguide1.github.io/linux_hardening_guide#usbs))
    - [[14. Thunderbolt and Firewire](https://theprivacyguide1.github.io/linux_hardening_guide#thunderbolt_and_firewire)](#[14.\Thunderbolt\and\Firewire](https://theprivacyguide1.github.io/linux_hardening_guide#thunderbolt_and_firewire))
    - [[15. Virtual Machines](https://theprivacyguide1.github.io/linux_hardening_guide#virtual_machines)](#[15.\Virtual\Machines](https://theprivacyguide1.github.io/linux_hardening_guide#virtual_machines))
    - [[15.1 Virtualbox](https://theprivacyguide1.github.io/linux_hardening_guide#virtualbox)](#[15.1\Virtualbox](https://theprivacyguide1.github.io/linux_hardening_guide#virtualbox))
    - [[15.2 KVM/QEMU](https://theprivacyguide1.github.io/linux_hardening_guide#kvm/qemu)](#[15.2\KVM/QEMU](https://theprivacyguide1.github.io/linux_hardening_guide#kvm/qemu))
    - [[15.3 Nested Virtualization](https://theprivacyguide1.github.io/linux_hardening_guide#nested_virtualization)](#[15.3\Nested\Virtualization](https://theprivacyguide1.github.io/linux_hardening_guide#nested_virtualization))
    - [[16. Core Dumps](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps)](#[16.\Core\Dumps](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps))
    - [[16.1 Sysctl](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_sysctl)](#[16.1\Sysctl](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_sysctl))
    - [[16.2 Systemd](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_systemd)](#[16.2\Systemd](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_systemd))
    - [[16.3 Ulimit](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_ulimit)](#[16.3\Ulimit](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_ulimit))
    - [[16.4 Disabling setuid processes from dumping their memory](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_setuid)](#[16.4\Disabling\setuid\processes\from\dumping\their\memory](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_setuid))
    - [[17. Microcode Updates](https://theprivacyguide1.github.io/linux_hardening_guide#microcode)](#[17.\Microcode\Updates](https://theprivacyguide1.github.io/linux_hardening_guide#microcode))
    - [[18. ICMP Timestamps](https://theprivacyguide1.github.io/linux_hardening_guide#icmp_timestamps)](#[18.\ICMP\Timestamps](https://theprivacyguide1.github.io/linux_hardening_guide#icmp_timestamps))
    - [[19. NTP](https://theprivacyguide1.github.io/linux_hardening_guide#ntp)](#[19.\NTP](https://theprivacyguide1.github.io/linux_hardening_guide#ntp))
    - [[20. IPv6 Privacy Extensions](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_privacy)](#[20.\IPv6\Privacy\Extensions](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_privacy))
    - [[20.1 NetworkManager](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_networkmanager)](#[20.1\NetworkManager](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_networkmanager))
    - [[20.2 systemd-networkd](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_systemd-networkd)](#[20.2\systemd-networkd](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_systemd-networkd))
    - [[21. Bootloaders](https://theprivacyguide1.github.io/linux_hardening_guide#bootloaders)](#[21.\Bootloaders](https://theprivacyguide1.github.io/linux_hardening_guide#bootloaders))
    - [[21.1 GRUB](https://theprivacyguide1.github.io/linux_hardening_guide#grub)](#[21.1\GRUB](https://theprivacyguide1.github.io/linux_hardening_guide#grub))
    - [[21.2 Syslinux](https://theprivacyguide1.github.io/linux_hardening_guide#syslinux)](#[21.2\Syslinux](https://theprivacyguide1.github.io/linux_hardening_guide#syslinux))
    - [[21.3 systemd-boot](https://theprivacyguide1.github.io/linux_hardening_guide#systemd-boot)](#[21.3\systemd-boot](https://theprivacyguide1.github.io/linux_hardening_guide#systemd-boot))
    - [[22. PAM](https://theprivacyguide1.github.io/linux_hardening_guide#pam)](#[22.\PAM](https://theprivacyguide1.github.io/linux_hardening_guide#pam))
    - [[23. Blacklist Uncommon Network Protocols](https://theprivacyguide1.github.io/linux_hardening_guide#uncommon_protocols)](#[23.\Blacklist\Uncommon\Network\Protocols](https://theprivacyguide1.github.io/linux_hardening_guide#uncommon_protocols))
    - [[24. Partitioning and Mount Options](https://theprivacyguide1.github.io/linux_hardening_guide#partitioning)](#[24.\Partitioning\and\Mount\Options](https://theprivacyguide1.github.io/linux_hardening_guide#partitioning))
    - [[25. Disable Mounting of Uncommon Filesystems](https://theprivacyguide1.github.io/linux_hardening_guide#uncommon_filesystems)](#[25.\Disable\Mounting\of\Uncommon\Filesystems](https://theprivacyguide1.github.io/linux_hardening_guide#uncommon_filesystems))
    - [[26. Editing Files as Root](https://theprivacyguide1.github.io/linux_hardening_guide#editing_as_root)](#[26.\Editing\Files\as\Root](https://theprivacyguide1.github.io/linux_hardening_guide#editing_as_root))
    - [[27. Entropy](https://theprivacyguide1.github.io/linux_hardening_guide#entropy)](#[27.\Entropy](https://theprivacyguide1.github.io/linux_hardening_guide#entropy))
    - [[27.1 Haveged](https://theprivacyguide1.github.io/linux_hardening_guide#haveged)](#[27.1\Haveged](https://theprivacyguide1.github.io/linux_hardening_guide#haveged))
    - [[27.2 Jitterentropy](https://theprivacyguide1.github.io/linux_hardening_guide#jitterentropy)](#[27.2\Jitterentropy](https://theprivacyguide1.github.io/linux_hardening_guide#jitterentropy))
    - [[28. Microphones and Webcams](https://theprivacyguide1.github.io/linux_hardening_guide#microphones_and_webcams)](#[28.\Microphones\and\Webcams](https://theprivacyguide1.github.io/linux_hardening_guide#microphones_and_webcams))
    - [[29. Best Practices](https://theprivacyguide1.github.io/linux_hardening_guide#best_practices)](#[29.\Best\Practices](https://theprivacyguide1.github.io/linux_hardening_guide#best_practices))

[Install](https://wiki.archlinux.org/title/Install "Install") the `torbrowser-launcher` [torbrowser-launcher](https://archlinux.org/packages/?name=torbrowser-launcher) package to use the [Tor Browser](https://www.torproject.org/download/), which is the only supported way to browse the web anonymously using Tor.

Users intending to manually use Tor with other software, run relays, or host onion services should install the [tor](https://archlinux.org/packages/?name=tor)(`tor`) package.
## Monitoring Network usage while using tor
_Nyx_ is a command line monitor for Tor, it provides bandwidth usage, connection details and on-the-fly configuration editing. To use it, [install](https://wiki.archlinux.org/title/Install "Install") the [nyx](https://archlinux.org/packages/?name=nyx) package.


## Usage

[Start/enable](https://wiki.archlinux.org/title/Start/enable "Start/enable") `tor.service`. Alternatively, launch it manually as the tor user:
```bash
[tor]$ /usr/bin/tor
```
To use a program over Tor, configure it to use `127.0.0.1` or `localhost` as a SOCKS5 proxy, with port `9050` for plain Tor with standard settings.


The proxy supports remote DNS resolution: use `socks5h://localhost:9050` for DNS resolution from the exit node (instead of `socks5` for a local DNS resolution).

To check if Tor is functioning properly, visit [https://check.torproject.org/](https://check.torproject.org/)


[Arch Linux hardening guide](https://theprivacyguide1.github.io/linux_hardening_guide)

## [1. Kernels](https://theprivacyguide1.github.io/linux_hardening_guide#kernel)  

### [1.1 Sysctl](https://theprivacyguide1.github.io/linux_hardening_guide#sysctl)  
### [1.2 Boot Parameters](https://theprivacyguide1.github.io/linux_hardening_guide#boot_parameters)  
[1.3 hidepid](https://theprivacyguide1.github.io/linux_hardening_guide#hidepid)  
### [1.4 Netfilter's connection tracking helper](https://theprivacyguide1.github.io/linux_hardening_guide#nf_conntrack_helper)  
### [1.5 Linux-hardened](https://theprivacyguide1.github.io/linux_hardening_guide#linux-hardened)  
### [1.6 Grsecurity](https://theprivacyguide1.github.io/linux_hardening_guide#grsecurity)  
### [1.7 Compiling your own kernel](https://theprivacyguide1.github.io/linux_hardening_guide#compiling-your-own-kernel)  

  
### [2. Mandatory Access Control](https://theprivacyguide1.github.io/linux_hardening_guide#MAC)  
  
### [3. Sandboxes](https://theprivacyguide1.github.io/linux_hardening_guide#sandboxes)  
  

### [3.1 Sandboxing Xorg](https://theprivacyguide1.github.io/linux_hardening_guide#sandboxing_xorg)  

  
### [4. The Root Account](https://theprivacyguide1.github.io/linux_hardening_guide#root)  
  

### [4.1 /etc/securetty](https://theprivacyguide1.github.io/linux_hardening_guide#securetty)  
### [4.2 Restricting su](https://theprivacyguide1.github.io/linux_hardening_guide#restricting_su)  
### [4.3 Locking the root account](https://theprivacyguide1.github.io/linux_hardening_guide#locking_root)  
### [4.4 Denying Root Login via SSH](https://theprivacyguide1.github.io/linux_hardening_guide#denying_root_login_ssh)  
### [4.5 Increase the Number of Hashing Rounds](https://theprivacyguide1.github.io/linux_hardening_guide#increase_number_of_rounds)  
### [5. Systemd Sandboxing](https://theprivacyguide1.github.io/linux_hardening_guide#systemd_sandboxes)  
  
### [6. Restricting Xorg Root Access](https://theprivacyguide1.github.io/linux_hardening_guide#restricting_xorg)  
  
### [7. Firewalls](https://theprivacyguide1.github.io/linux_hardening_guide#firewalls)  
  
### [8. Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor)  
  

### [8.1 Tor browser](https://theprivacyguide1.github.io/linux_hardening_guide#tor_browser)  
### [8.2 Torsocks](https://theprivacyguide1.github.io/linux_hardening_guide#torsocks)  
### [8.3 Stream Isolation](https://theprivacyguide1.github.io/linux_hardening_guide#stream_isolation)  
### [8.4 Transparent Proxy](https://theprivacyguide1.github.io/linux_hardening_guide#transparent_proxy)  
### [8.5 Tor over Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor_over_tor)  
### [8.6 Configuring the Tor Browser to prevent Tor over Tor](https://theprivacyguide1.github.io/linux_hardening_guide#tor_browser_tor_over_tor)  

### [9. Hostnames and Usernames](https://theprivacyguide1.github.io/linux_hardening_guide#hostnames)  
  
### [10. Wireless Devices](https://theprivacyguide1.github.io/linux_hardening_guide#bluetooth)  
  
### [11. MAC Address Spoofing](https://theprivacyguide1.github.io/linux_hardening_guide#mac_address)  
  
### [12. Umask](https://theprivacyguide1.github.io/linux_hardening_guide#umask)  
  
### [13. USBs](https://theprivacyguide1.github.io/linux_hardening_guide#usbs)  
  
### [14. Thunderbolt and Firewire](https://theprivacyguide1.github.io/linux_hardening_guide#thunderbolt_and_firewire)  
  
### [15. Virtual Machines](https://theprivacyguide1.github.io/linux_hardening_guide#virtual_machines)  
  

### [15.1 Virtualbox](https://theprivacyguide1.github.io/linux_hardening_guide#virtualbox)  
### [15.2 KVM/QEMU](https://theprivacyguide1.github.io/linux_hardening_guide#kvm/qemu)  
### [15.3 Nested Virtualization](https://theprivacyguide1.github.io/linux_hardening_guide#nested_virtualization)  

  
### [16. Core Dumps](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps)  
  

### [16.1 Sysctl](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_sysctl)  
### [16.2 Systemd](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_systemd)  
### [16.3 Ulimit](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_ulimit)  
### [16.4 Disabling setuid processes from dumping their memory](https://theprivacyguide1.github.io/linux_hardening_guide#core_dumps_setuid)  
### [17. Microcode Updates](https://theprivacyguide1.github.io/linux_hardening_guide#microcode)  
### [18. ICMP Timestamps](https://theprivacyguide1.github.io/linux_hardening_guide#icmp_timestamps)  
### [19. NTP](https://theprivacyguide1.github.io/linux_hardening_guide#ntp)  
### [20. IPv6 Privacy Extensions](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_privacy)  
### [20.1 NetworkManager](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_networkmanager)  
### [20.2 systemd-networkd](https://theprivacyguide1.github.io/linux_hardening_guide#ipv6_systemd-networkd)  
### [21. Bootloaders](https://theprivacyguide1.github.io/linux_hardening_guide#bootloaders)  
### [21.1 GRUB](https://theprivacyguide1.github.io/linux_hardening_guide#grub)  
### [21.2 Syslinux](https://theprivacyguide1.github.io/linux_hardening_guide#syslinux)  
### [21.3 systemd-boot](https://theprivacyguide1.github.io/linux_hardening_guide#systemd-boot)  
### [22. PAM](https://theprivacyguide1.github.io/linux_hardening_guide#pam)  
### [23. Blacklist Uncommon Network Protocols](https://theprivacyguide1.github.io/linux_hardening_guide#uncommon_protocols)    
### [24. Partitioning and Mount Options](https://theprivacyguide1.github.io/linux_hardening_guide#partitioning)    
### [25. Disable Mounting of Uncommon Filesystems](https://theprivacyguide1.github.io/linux_hardening_guide#uncommon_filesystems)    
### [26. Editing Files as Root](https://theprivacyguide1.github.io/linux_hardening_guide#editing_as_root)  
### [27. Entropy](https://theprivacyguide1.github.io/linux_hardening_guide#entropy)  
### [27.1 Haveged](https://theprivacyguide1.github.io/linux_hardening_guide#haveged)  
### [27.2 Jitterentropy](https://theprivacyguide1.github.io/linux_hardening_guide#jitterentropy)  
### [28. Microphones and Webcams](https://theprivacyguide1.github.io/linux_hardening_guide#microphones_and_webcams)  
### [29. Best Practices](https://theprivacyguide1.github.io/linux_hardening_guide#best_practices)



