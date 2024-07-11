## Table of Contents

    - [Physical Security for Computer Systems](#Physical\Security\for\Computer\Systems)
      - [Security Measures:](#Security\Measures:)
  - [File System Encryption and Partitioning](#File\System\Encryption\and\Partitioning)
  - [LUKS (Linux Unified Key Setup) Overview](#LUKS\(Linux\Unified\Key\Setup)\Overview)
    - [Key Components:](#Key\Components:)
    - [Encryption:](#Encryption:)
    - [Decryption:](#Decryption:)
  - [Setting Up LUKS from the Command Line:](#Setting\Up\LUKS\from\the\Command\Line:)
- [Firewalls](#firewalls)
  - [Netfilter](#Netfilter)
  - [iptables](#iptables)
  - [nftables](#nftables)
  - [Firewall Policy](#Firewall\Policy)
- [Remote Access](#remote\access)
    - [Protecting Against Password Guessing](#Protecting\Against\Password\Guessing)
  - [Use sudo](#Use\sudo)
  - [Disable root](#Disable\root)
  - [Enforce a Strong Password Policy](#Enforce\a\Strong\Password\Policy)
  - [Disable Unused Accounts](#Disable\Unused\Accounts)

  
What command can you use to create a password for the GRUB bootloader?
```shell
grub2-mkpasswd-pbkdf2
```
### Physical Security for Computer Systems

- **Primary Threat**: If adversaries gain physical access to computer systems, they can easily execute simple attacks, such as removing the disk drive.

#### Security Measures:

1. **Prevent Unauthorized Access**:
    
    - Ensure that intruders cannot remove disk drives or the entire computer system.
    - Utilize complex system passwords that are difficult to guess.
2. **Potential Vulnerability with Physical Access**:
    
    - Despite having secure passwords, an intruder with physical access can use **GRUB** (a popular Linux bootloader) to reset the root password.
    - This vulnerability reinforces the saying: **"boot access = root access"**.
3. **Additional Protection Layers**:
    
    - **BIOS/UEFI Boot Password**:
        
        - Many BIOS and UEFI firmware support adding a boot password.
        - Benefits: Prevents unauthorized booting of the system.
        - Limitation: Not ideal for servers as it necessitates someone's physical presence for password input.
    - **GRUB Password**:
        
        - Suitable for protecting specific Linux systems from unauthorized boot configurations.
        - Tool: `grub2-mkpasswd-pbkdf2`
            - Function: Prompts for a password and generates a hash.
            - Usage: Add the hash to the relevant configuration file for the Linux distribution (e.g., Fedora, Ubuntu).
            - Outcome: Prevents unauthorized users from resetting the root password and accessing advanced boot configurations via GRUB

## File System Encryption and Partitioning
Certainly! Here's a structured and readable Markdown representation of the information you provided:

---
LUKS (Linux Unified Key Setup), let’s cover it in more detail.

When a partition is encrypted with LUKS, the disk layout would look as shown in the figure below.

![LUKS disk layout: LUKS Partition Header, KM1, KM2, ..., KM8, Bulk Data](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/ee1310ecb1558e550a9bff3a53ece0ff.png)  

We have the following fields:  

- **LUKS phdr**: It stands for _LUKS Partition Header_. LUKS phdr stores information about the UUID (Universally Unique Identifier), the used cipher, the cipher mode, the key length, and the checksum of the master key.
- **KM**: KM stands for _Key Material_, where we have KM1, KM2, …, KM8. Each key material section is associated with a key slot, which can be indicated as active in the LUKS phdr. When the key slot is active, the associated key material section contains a copy of the master key encrypted with a user's password. In other words, we might have the master key encrypted with the first user's password and saved in KM1, encrypted with the second user's password and saved in KM2, and so on.
- **Bulk Data**: This refers to the data encrypted by the master key. The master key is saved and encrypted by the user's password in a key material section.
- 
## LUKS (Linux Unified Key Setup) Overview

### Key Components:

- **LUKS phdr (LUKS Partition Header)**:
  - Stores details such as:
    - UUID (Universally Unique Identifier)
    - Used cipher
    - Cipher mode
    - Key length
    - Master key's checksum

- **KM (Key Material)**:
  - Consists of KM1, KM2, …, KM8.
  - Each key material is linked to a key slot, which can be marked as active in the LUKS phdr.
  - An active key slot indicates that the master key is encrypted with a user's password and saved in the respective KM.

- **Bulk Data**:
  - Represents data encrypted using the master key.
  - The master key itself is stored encrypted using the user's password within a key material section.

### Encryption:

LUKS leverages existing block encryption methods. The pseudocode for data encryption is:

```pseudo
enc_data = encrypt(cipher_name, cipher_mode, key, original, original_length)
```

Where:
- `original` is plaintext data.
- The key for encryption is derived using PBKDF2 (password-based key derive function 2):

```pseudo
key = PBKDF2(password, salt, iteration_count, derived_key_length)
```

A combination of a salt and repeated hash function iterations ensures the derived key's security.

### Decryption:

To decrypt data and retrieve the plaintext, LUKS employs:

```pseudo
original = decrypt(cipher_name, cipher_mode, key, enc_data, original_length)
```

---

## Setting Up LUKS from the Command Line:

1. **Installation**:
   - Install `cryptsetup-luks` package:
     - Ubuntu/Debian: `apt install cryptsetup`
     - RHEL/Cent OS: `yum install cryptsetup-luks`
     - Fedora: `dnf install cryptsetup-luks`

2. **Identify Partition**:
   - Use `fdisk -l`, `lsblk`, or `blkid` to confirm the partition name.
   - If necessary, create a partition using `fdisk`.

3. **Encrypt the Partition**:
   - Command: `cryptsetup -y -v luksFormat /dev/sdb1`
   - This initializes the partition for LUKS encryption. Replace `/dev/sdb1` with your partition name.

4. **Access Encrypted Partition**:
   - Map the partition for access: `cryptsetup luksOpen /dev/sdb1 EDCdrive`
   - Confirm mapping: `ls -l /dev/mapper/EDCdrive` and `cryptsetup -v status EDCdrive`

5. **Data Overwrite**:
   - Overwrite existing data: `dd if=/dev/zero of=/dev/mapper/EDCdrive`

6. **Format Partition**:
   - Use `mkfs.ext4` to format the partition: `mkfs.ext4 /dev/mapper/EDCdrive -L "Strategos USB"`

7. **Mount and Use**:
   - Mount and utilize it like a regular partition: `mount /dev/mapper/EDCdrive /media/secure-USB`

---

To access the encrypted file `/home/tryhackme/secretvault.img`, you'll need to follow a series of steps to decrypt and mount it. Here's a step-by-step guide:

1. **Open the Encrypted File with cryptsetup**: 
Use the `cryptsetup` tool to access the encrypted image. You can create a mapping to this file so that you can treat it like a block device.

```bash
sudo cryptsetup luksOpen /home/tryhackme/secretvault.img myvault
```

When prompted, enter the password `2N9EdZYNkszEE3Ad`.

2. **Mount the Decrypted File to an Empty Directory**: 
Before you can mount it, ensure you have an empty directory. If not, create one:

```bash
mkdir ~/myvault
```

Now, mount the decrypted file to this directory:

```bash
sudo mount /dev/mapper/myvault ~/myvault
```

3. **Access the Content of the Secret Vault**:
Navigate to the mounted directory and list its contents:

```bash
cd ~/myvault
ls
```

Look for any files that might contain the flag. If there's a file named `flag.txt` or similar, display its contents:

```bash
cat flag.txt
```

The content of the file should display the flag you're looking for.

4. **Cleanup**:
After retrieving the flag and any other necessary information, it's good practice to unmount the image and close the cryptsetup mapping:

```bash
sudo umount ~/myvault
sudo cryptsetup luksClose myvault
```

This will ensure that the encrypted image is no longer accessible until you decrypt and mount it again in the future.


# Firewalls
he first Linux firewall was a packet filtering firewall, i.e., a stateless firewall. A stateless firewall can inspect certain fields in the IP and TCP/UDP headers to decide upon a packet but does not maintain information about ongoing TCP connections. As a result, a packet can manipulate a few TCP flags to appear as if it is part of an ongoing connection and evade certain restrictions. Current Linux firewalls are stateful firewalls; they keep track of ongoing connections and restrict packets based on specific fields in the IP and TCP/UDP headers and based on whether the packet is part of an ongoing connection.

The IP header fields that find their way into the firewall rules are:

1. Source IP address
2. Destination IP address

The TCP/UDP header fields that are of primary concern for firewall rules are:

1. Source TCP/UDP port
2. Destination TCP/UDP port

It is worth noting that it is impossible to allow and deny packets based on the process but instead on the port number. If you want the web browser to access the web, you must allow the respective ports, such as ports 80 and 443. This limitation differs from MS Windows’ built-in firewall, which can restrict and allow traffic per application.

On a Linux system, a solution such as [SELinux](https://github.com/SELinuxProject) or [AppArmor](https://www.apparmor.net/) can be used for more granular control over processes and their network access. For example, we can allow only the `/usr/bin/apache2` binary to use ports 80 and 443 while preventing any other binary from doing so on the underlying system. Both tools enforce access control policies based on the specific process or binary, providing a more comprehensive way to secure a Linux system.

Let’s look take a closer look at the different available Linux firewalls.


## Netfilter

At the very core, we have netfilter. The netfilter project provides packet-filtering software for the Linux kernel 2.4.x and later versions. The netfilter hooks require a front-end such as `iptables` or `nftables` to manage.

In the following examples, we use different front-ends to netfilter in order to allow incoming SSH connections to the SSH server on our Linux system. As shown in the figure below, we want our SSH server to be accessible to anyone on the Internet with an SSH client.
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/812543382c7fd172dfa1190c2e0363d8.png)



## iptables

As a front-end, iptables provides the user-space command line tools to configure the packet filtering rule set using the netfilter hooks. For filtering the traffic, iptables has the following default chains:

- **Input**: This chain applies to the packets incoming to the firewall.
- **Output**: This chain applies to the packets outgoing from the firewall.
- **Forward** This chain applies to the packets routed through the system.

Let’s say that we want to be able to access the SSH server on our system remotely. For the SSH server to be able to communicate with the world, we need two things:

1. Accept incoming packets to TCP port 22.
2. Accept outgoing packets from TCP port 22.

Let’s translate the above two requirements into `iptables` commands:
```shell
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
- `-A INPUT` appends to the INPUT chain, i.e., packets destined for the system.
- `-p tcp --dport 22` applies to TCP protocol with destination port 22.
- `-j ACCEPT` specifies (jump to) target rule ACCEPT.
```shell
iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT
```
- `-A OUTPUT` append to the OUTPUT chain, i.e., packets leaving the system.
- `-p tcp --sport 22` applies to TCP protocol with source port 22.

Let’s say you only want to allow traffic to the local SSH server and block everything else. In this case, you need to add two more rules to set the default behaviour of your firewall:

- `iptables -A INPUT -j DROP` to block all incoming traffic not allowed in previous rules.
- `iptables -A OUTPUT -j DROP` to block all outgoing traffic not allowed in previous rules.

In brief, the rules below need to be applied in the following order:

AttackBox Terminal

```shell
root@AttackBox$ iptables -A INPUT -p tcp --dport 22 -j ACCEPT
				iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT
				iptables -A INPUT -j DROP
				iptables -A OUTPUT -j DROP
```
In practice, you should flush (delete) previous rules before applying new ones. This action can be achieved using `iptables -F`.

## nftables

Better than iptables bc of scalability and performance
Example will create a simple nftables configuration that allows traffic to our local SSH server

- Starts with no tables or chaines
- must add necessary chains/tables before adding rules


To begin, we will create a table, `fwfilter`.

`nft add table fwfilter`

- `add` is used to add a table. Other commands include `delete` to delete a table, `list` to list the chains and rules in a table, and `flush` to clear all chains and rules from a table.
- `table TABLE_NAME` is used to specify the name of the table we want to create or work on.

In our newly created table, `fwfilter`, we will add an _input_ chain and an _output_ chain for incoming and outgoing packets, respectively.

- `nft add chain fwfilter fwinput { type filter hook input priority 0 \; }`
- `nft add chain fwfilter fwoutput { type filter hook output priority 0 \; }`

The above two commands add two chains to the table `fwfilter`:

- `fwinput` is the input chain. It is of type `filter` and applies to the input hook.
- `fwoutput` is the output chain. It is of type `filter` and applies to the output hook.

With the two chains created within our table, we can add the necessary rule to allow SSH traffic. The following two rules are added to the table `fwfilter` to the chains `fwinput` and `fwoutput`, respectively:

- `nft add fwfilter fwinput tcp dport 22 accept` accepts TCP traffic to the local system’s destination port 22.
- `nft add fwfilter fwoutput tcp sport 22 accept` accepts TCP traffic from the local system’s source port 22.

We can check the shape of the `fwfilter` table using the command `nft list table fwfilter`:
```shell
root@AttackBox$ >>>	sudo nft list table fwfilter
table ip fwfilter {
	chain fwinput {
		type filter hook input priority filter;
		tcp dport 22 accept }
		chain fwoutput { type filter hook output priority filter;
		tcp sport 22 accept 
		} 
	}
```


Example front-ends to iptables are shown in the figure below and can be divided into:

- Command-line Interface (CLI) front-ends, such as firewalld and ufw
- Graphical User Interface (GUI) front-ends, such as fwbuilder

![Figure showing the relationship between netfilter, iptables, and the different front ends.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/f50217a465f06499ce979228323d2945.png)  

UFW stands for uncomplicated firewall. Let’s see how it stands for its promise of being _uncomplicated_. We will allow SSH traffic. This firewall rule can be achieved through one of the following commands:

`ufw allow 22/tcp`

It configures the firewall to `allow` traffic to TCP port 22. We can confirm our settings with the command `ufw status`.

AttackBox Terminal

```shell
root@AttackBox[~] sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
22/tcp (v6)                ALLOW       Anywhere (v6)
```

## Firewall Policy

Before configuring a firewall, you need to decide upon the firewall policy. You might be the decision maker regarding the firewall policy or an enforcer of an existing security policy that covers firewall configuration. It all depends on the system you are protecting.

We will not go into security policies as this is outside the scope of this room. We will mention that the two main approaches are:

- Block everything and allow certain exceptions.
- Allow everything and block certain exceptions.





# Remote Access


### Protecting Against Password Guessing
Because your SSH server will be configured to listen for incoming connections 24 hours a day, 365 days a year, evil users have all the time in the world to attempt one password after another. There are a few guidelines that you can use:

1. Disable remote login as `root`; force login as non-root users.
2. Disable password authentication; force public key authentication instead.

The configuration of the OpenSSH server can be controlled via the `sshd_config` file, usually located at `/etc/ssh/sshd_config`. You can disable the root login by adding the following line:

`PermitRootLogin no`

Although a password such as `9bNfX2gmDZ4o` is difficult to guess, most users find memorising it inconvenient. Imagine if the account belongs to the _sudoers_ (`sudo` group), and the user needs to type this password every time they need to issue a command with `sudo`. You may have to discipline to do that, but you cannot expect this to work for everyone.

Many users are tempted to select a user-friendly password or share the same password across multiple accounts. Either approach would make the password easier for the attacker to guess.

It would be best to rely on public key authentication with SSH to help improve the security of the remote login system and make it as fail-proof as possible.

If you haven’t created an SSH key pair, you must issue the command `ssh-keygen -t rsa`. It will generate a private key saved in `id_rsa` and a public key saved in `id_rsa.pub`.

For the SSH server to authenticate you using your public key instead of your passwords, your public key needs to be copied to the target SSH server. An easy way to do it would be by issuing the command `ssh-copy-id username@server` where `username` is your username, and `server` is the hostname or IP address of the SSH server.

It is best to ensure you have access to the physical terminal before you disable password authentication to avoid locking yourself out. You might need to ensure having the following two lines in your `sshd_config` file.

- `PubkeyAuthentication yes` to enable public key authentication
    
- `PasswordAuthentication no` to disable password authentication

The `root` account carries with it tremendous power and hence risk. You are at risk of rendering your system unbootable with a simple mistake. Using a non-root account for everyday work is recommended to avoid sabotaging your system. However, `root` privileges are still needed for system maintenance, installing/removing software packages, and updating/configuring the system.

## Use sudo

To avoid logging in as `root`, the better approach would be to have an account -created for administrative purposes- added to the _sudoers_, i.e. group who can use the `sudo` command. `sudo` stands for Super User Do and it should precede any command that requires `root` privileges.

Depending on the Linux distribution, we can add a user to the sudoers group in the following ways. Some distributions, such as Debian and Ubuntu, call the sudoers group `sudo`. In this case, you would need to issue the following command:

`usermod -aG sudo username`

- `usermod` modifies a user account.
- `-aG` appends to group.
- `sudo` is the name of the group of users who can use `sudo` on Debian-based distributions.
- `username` is the name of the user account you want to modify.

Other distributions, such as RedHat and Fedora, refer to the sudoers group as `wheel`. Consequently, you would need to issue the following command:

`usermod -aG wheel username`

The only difference is the name of the sudoers group.

## Disable root

Once you have created an account for administrative purposes and added it to the `sudo`/`wheel` group, you might consider disabling the `root` account. A straightforward way is to modify the `/etc/passwd` and change the `root` shell to `/sbin/nologin`. In other words, edit `/etc/passwd` and change the line `root:x:0:0:root:/root:/bin/bash` to `root:x:0:0:root:/root:/sbin/nologin`.
## Enforce a Strong Password Policy

The `libpwquality` library provides many options for password constraints. The configuration file can be found at:

- `/etc/security/pwquality.conf` on RedHat and Fedora
- `/etc/pam.d/common-password` on Debian and Ubuntu. You can install it using `apt-get install libpam-pwquality`

Here are a few example options:

- `difok` allows you to specify the number of characters in the new password that were not present in the old password.
- `minlen` sets the minimum allowed length for new passwords.
- `minclass` specifies the minimum number of required classes of characters; a class can be uppercase, lowercase, and digits, among others.
- `badwords` provides a space-separated list of words that must not be contained in the chosen password.
- `retry=N` prompts the user `N` times before returning an error.

Below is an example of `/etc/security/pwquality.conf`.

## Disable Unused Accounts
Dont leave untouched user accounts

Can disable a user account in the same way as root acc. Ez way is to edit the /etc/passwd file and set the shell of the user to disable to /sbin/nologin


Let’s say that we want to disable the account of the user Michael with username `michael`.

- Enabled account: `michael:x:1000:1000:Michael:/home/michael:/usr/bin/fish`
- Disabled account: `michael:x:1000:1000:Michael:/home/michael:/sbin/nologin`
We should do the same for local services. In other words, we should set the shell to `sbin/nologin` for all the local service accounts such as `www-data`, `mongo`, and `nginx`, to name a few. The reason is that these services need accounts to run on the system but would never need to log in and access a shell. Any of these services could perhaps have an RCE (Remote Code Execution) vulnerability, and by setting the shell to `nologin`, we can at least prevent interactive logins for the account of the affected service.


**Disable Unnecessary Services**

**Block Unneeded Network Ports**

**Avoid Legacy Protocols**

**Remove Identification Settings**


Most log files on Linux systems are stored in the **`/var/log`** directory. Here are a few of the logs that can be referenced when looking into threats:

- `/var/log/messages` - a general log for Linux systems
- `/var/log/auth.log` - a log file that lists all authentication attempts (Debian-based systems)
- `/var/log/secure` - a log file that lists all authentication attempts (Red Hat and Fedora-based systems)
- `/var/log/utmp` - an access log that contains information regarding users that are currently logged into the system
- `/var/log/wtmp` - an access log that contains information for all users that have logged in and out of the system
- `/var/log/kern.log` - a log file containing messages from the kernel
- `/var/log/boot.log` - a log file that contains start-up messages and boot information

We will keep this task short and include only two handy commands for large log files.

- Since new events are appended to the log file, you can view the last few lines using `tail`. For example, `tail -n 12 boot.log` will display the last 12 lines.
- One way to search log lines containing a specific keyword is using the command `grep`. For instance, `grep FAILED boot.log` will only show the lines with the word `FAILED`.

Note that you must be logged in as root or precede your commands with `sudo` to view the system log files.




