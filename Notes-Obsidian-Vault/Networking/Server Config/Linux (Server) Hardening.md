## Table of Contents

    - [Linux Server Hardening Commands](#Linux\Server\Hardening\Commands)
      - [1. Regular System Updates](#1.\Regular\System\Updates)
      - [2. Create a New User and Grant Sudo](#2.\Create\a\New\User\and\Grant\Sudo)
      - [3. Configure SSH](#3.\Configure\SSH)
      - [4. Set Up Firewall (UFW)](#4.\Set\Up\Firewall\(UFW))
      - [5. Install and Configure Fail2Ban](#5.\Install\and\Configure\Fail2Ban)
      - [6. Disable Unused Network Protocols (IPv6 if not used)](#6.\Disable\Unused\Network\Protocols\(IPv6\if\not\used))
      - [7. Secure Shared Memory](#7.\Secure\Shared\Memory)
      - [8. Harden Network with sysctl Settings](#8.\Harden\Network\with\sysctl\Settings)
      - [9. Audit System with Lynis](#9.\Audit\System\with\Lynis)
      - [10. Check for Rootkits with RKHunter](#10.\Check\for\Rootkits\with\RKHunter)
      - [11. Implement Mandatory Access Controls with AppArmor or SELinux](#11.\Implement\Mandatory\Access\Controls\with\AppArmor\or\SELinux)
      - [12. Set Up Automatic Security Updates](#12.\Set\Up\Automatic\Security\Updates)
      - [13. Monitor Logs with Logwatch](#13.\Monitor\Logs\with\Logwatch)
      - [14. Ensure No Accounts Have Empty Passwords](#14.\Ensure\No\Accounts\Have\Empty\Passwords)
      - [15. Check for SUID/SGID Files Regularly](#15.\Check\for\SUID/SGID\Files\Regularly)
    - [Extreme Paranoia Linux Hardening Steps](#Extreme\Paranoia\Linux\Hardening\Steps)
      - [16. Kernel Hardening](#16.\Kernel\Hardening)
      - [17. Minimize Installed Packages](#17.\Minimize\Installed\Packages)
      - [18. Filesystem Hardening](#18.\Filesystem\Hardening)
      - [19. Restrict Module Loading](#19.\Restrict\Module\Loading)
      - [20. Host-based Intrusion Detection System (HIDS)](#20.\Host-based\Intrusion\Detection\System\(HIDS))
      - [21. Advanced Firewall Configuration](#21.\Advanced\Firewall\Configuration)
      - [22. Network Traffic Encryption](#22.\Network\Traffic\Encryption)
      - [23. Disable USB and CD/DVD Drives](#23.\Disable\USB\and\CD/DVD\Drives)
      - [24. Advanced SSH Hardening](#24.\Advanced\SSH\Hardening)
      - [25. System Integrity Checkers](#25.\System\Integrity\Checkers)
      - [26. Implement Mandatory Access Control](#26.\Implement\Mandatory\Access\Control)
      - [27. Periodic Security Audits](#27.\Periodic\Security\Audits)
      - [28. Secure Boot](#28.\Secure\Boot)
      - [29. CPU Microcode Updates](#29.\CPU\Microcode\Updates)
      - [30. Harden Network Services](#30.\Harden\Network\Services)
      - [31. Use a Real-time Malware Scanner](#31.\Use\a\Real-time\Malware\Scanner)
      - [32. Centralized Logging and Monitoring](#32.\Centralized\Logging\and\Monitoring)


![[Pasted image 20240129163022.png]]
### Linux Server Hardening Commands

#### 1. Regular System Updates
```bash
sudo apt update && sudo apt upgrade
```

#### 2. Create a New User and Grant Sudo
```bash
sudo adduser [newuser]
sudo usermod -aG sudo [newuser]
```

#### 3. Configure SSH
- Edit SSH configuration:
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
- Common changes:
    - Change default SSH port: `Port [new_port_number]`
    - Disable root login: `PermitRootLogin no`
    - Allow only specific users: `AllowUsers [username]`

- Restart SSH service:
    ```bash
    sudo systemctl restart sshd
    ```

#### 4. Set Up Firewall (UFW)
```bash
sudo ufw allow [ssh_port_number]
sudo ufw enable
```

#### 5. Install and Configure Fail2Ban
```bash
sudo apt install fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
sudo systemctl restart fail2ban
```

#### 6. Disable Unused Network Protocols (IPv6 if not used)
- Edit sysctl configuration:
    ```bash
    sudo nano /etc/sysctl.conf
    ```
- Add:
    ```bash
    net.ipv6.conf.all.disable_ipv6 = 1
    net.ipv6.conf.default.disable_ipv6 = 1
    ```
- Apply changes:
    ```bash
    sudo sysctl -p
    ```

#### 7. Secure Shared Memory
- Edit fstab file:
    ```bash
    sudo nano /etc/fstab
    ```
- Add line:
    ```bash
    tmpfs     /run/shm     tmpfs     defaults,noexec,nosuid     0     0
    ```

#### 8. Harden Network with sysctl Settings
- Edit sysctl configuration for network hardening:
    ```bash
    sudo nano /etc/sysctl.conf
    ```
- Add recommended settings (TCP/IP Stack Hardening, Ignore ICMP Requests, etc).

#### 9. Audit System with Lynis
```bash
sudo apt install lynis
sudo lynis audit system
```

#### 10. Check for Rootkits with RKHunter
```bash
sudo apt install rkhunter
sudo rkhunter --update
sudo rkhunter --check
```

#### 11. Implement Mandatory Access Controls with AppArmor or SELinux
- AppArmor is usually pre-installed on Ubuntu. Check status:
    ```bash
    sudo apparmor_status
    ```
- Configure AppArmor profiles as needed.

#### 12. Set Up Automatic Security Updates
```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

#### 13. Monitor Logs with Logwatch
```bash
sudo apt install logwatch
sudo logwatch --detail high
```

#### 14. Ensure No Accounts Have Empty Passwords
```bash
sudo awk -F: '($2 == "") {print}' /etc/shadow
```

#### 15. Check for SUID/SGID Files Regularly
```bash
sudo find / -type f \( -perm -4000 -o -perm -2000 \) -exec ls -l {} \;
```

---
For an extreme paranoia level of system lockdown, you would implement additional stringent security measures beyond the standard practices. Here's a continuation of the advanced Linux hardening steps, focusing on a high-security environment.

---

### Extreme Paranoia Linux Hardening Steps

#### 16. Kernel Hardening
- **Use a security-focused kernel** like Grsecurity/PaX or a kernel with built-in security features.
    ```bash
    # This requires downloading and compiling a custom kernel
    ```

#### 17. Minimize Installed Packages
- **Remove unnecessary packages** to reduce the attack surface.
    ```bash
    sudo apt autoremove --purge [package_name]
    ```

#### 18. Filesystem Hardening
- **Implement a strict mount option** in `/etc/fstab`. For instance, `nodev`, `nosuid`, and `noexec` on `/tmp` and other non-system partitions.
- **Use a filesystem with built-in encryption** like LUKS for data partitions.

#### 19. Restrict Module Loading
- Disable module loading after boot:
    ```bash
    echo 1 | sudo tee /proc/sys/kernel/modules_disabled
    ```
- Make this permanent in `/etc/sysctl.conf`.

#### 20. Host-based Intrusion Detection System (HIDS)
- Install and configure an HIDS like AIDE or Tripwire.
    ```bash
    sudo apt install aide
    sudo aideinit
    sudo aide --check
    ```

#### 21. Advanced Firewall Configuration
- **Implement a default DROP policy** for all incoming and outgoing connections, only allowing necessary traffic.
- **Use advanced tools** like `iptables`, `nftables`, or `firewalld`.

#### 22. Network Traffic Encryption
- Enforce **VPN or SSH tunneling** for all outgoing connections.
- Use tools like `stunnel` for creating encrypted tunnels.

#### 23. Disable USB and CD/DVD Drives
- Disable kernel modules for USB storage and CD/DVD drives.
    ```bash
    echo "install usb-storage /bin/false" | sudo tee -a /etc/modprobe.d/disable-usb-storage.conf
    echo "install cdrom /bin/false" | sudo tee -a /etc/modprobe.d/disable-cdrom.conf
    ```

#### 24. Advanced SSH Hardening
- **Use Public Key Authentication only**, disabling password authentication.
- **Restrict SSH to specific IPs** where possible.
- **Implement multi-factor authentication** for SSH.
- **Restrict the Root user from being able to log in from SSH**. It's best to keep the SSH(*able*) user's to a minimum. The less attack surface, the better

#### 25. System Integrity Checkers
- Regularly run tools like **Rkhunter** and **Chkrootkit**.
- Schedule **integrity checks** with a tool like AIDE.

#### 26. Implement Mandatory Access Control
- Use SELinux or AppArmor for **fine-grained access control**.
- **Customize policies** strictly based on the least privilege principle.

#### 27. Periodic Security Audits
- Regularly perform **security audits and penetration testing**.
- Use tools like **Nessus**, **OpenVAS**, or **Metasploit** for vulnerability scanning.

#### 28. Secure Boot
- Ensure **UEFI Secure Boot** is enabled.
- Use a bootloader with **integrity checking**, like TrustedGRUB.

#### 29. CPU Microcode Updates
- Regularly update **CPU microcode** to mitigate hardware-level vulnerabilities.

#### 30. Harden Network Services
- For each service (HTTP, SMTP, etc.), apply strict configurations, **limiting access and permissions**.
- Regularly **update and patch** all network services.

#### 31. Use a Real-time Malware Scanner
- Implement a **real-time malware detection tool**, such as ClamAV with real-time scanning features.

#### 32. Centralized Logging and Monitoring
- Implement **centralized log management** with tools like Splunk or ELK Stack.
- Regularly **monitor and analyze** logs for suspicious activities.

---

These steps represent a rigorous approach to server security, suitable for environments where security is paramount. Always test changes in a controlled environment before applying to a production server, as some configurations can significantly impact system functionality or accessibility.