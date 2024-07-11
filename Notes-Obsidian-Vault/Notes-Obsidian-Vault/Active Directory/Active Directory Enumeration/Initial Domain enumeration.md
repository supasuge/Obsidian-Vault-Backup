## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Identifying Hosts](#Identifying\Hosts)
- [Starting Wireshark from CMD](#starting\wireshark\from\cmd)
      - [Wireshark Output](#Wireshark\Output)
      - [FPing Active Checks](#FPing\Active\Checks)
- [Kerbrute AD Username (internal) Enumeration](#kerbrute\ad\username\(internal)\enumeration)
      - [Cloning Kerbrute GitHub Repo](#Cloning\Kerbrute\GitHub\Repo)
      - [isting Compiling Options](#isting\Compiling\Options)
      - [Compiling for Multiple Platforms and Architectures](#Compiling\for\Multiple\Platforms\and\Architectures)
      - [Adding the Tool to our Path](#Adding\the\Tool\to\our\Path)
      - [Moving the Binary](#Moving\the\Binary)
      - [Enumerating Users with Kerbrute](#Enumerating\Users\with\Kerbrute)

## Table of Contents

    - [Identifying Hosts](#Identifying\Hosts)
- [Starting Wireshark from CMD](#starting\wireshark\from\cmd)
      - [Wireshark Output](#Wireshark\Output)
      - [FPing Active Checks](#FPing\Active\Checks)
- [Kerbrute AD Username (internal) Enumeration](#kerbrute\ad\username\(internal)\enumeration)
      - [Cloning Kerbrute GitHub Repo](#Cloning\Kerbrute\GitHub\Repo)
      - [isting Compiling Options](#isting\Compiling\Options)
      - [Compiling for Multiple Platforms and Architectures](#Compiling\for\Multiple\Platforms\and\Architectures)
      - [Adding the Tool to our Path](#Adding\the\Tool\to\our\Path)
      - [Moving the Binary](#Moving\the\Binary)
      - [Enumerating Users with Kerbrute](#Enumerating\Users\with\Kerbrute)


We will start with `passive` identification of any hosts in the network, followed by `active` validation of the results to find out more about each host (what services are running, names, potential vulnerabilities, etc.).

### Identifying Hosts

First, let's take some time to listen to the network and see what's going on. We can use `Wireshark` and `TCPDump` to "put our ear to the wire" and see what hosts and types of network traffic we can capture.



# Starting Wireshark from CMD
sudo -E wireshark

#### Wireshark Output

![image](https://academy.hackthebox.com/storage/modules/143/ea-wireshark.png)

- ARP packets make us aware of the hosts: 172.16.5.5, 172.16.5.25 172.16.5.50, 172.16.5.100, and 172.16.5.125.

  

![image](https://academy.hackthebox.com/storage/modules/143/ea-wireshark-mdns.png)

- MDNS makes us aware of the ACADEMY-EA-WEB01 host.

If we are on a host without a GUI (which is typical), we can use [tcpdump](https://linux.die.net/man/8/tcpdump), [net-creds](https://github.com/DanMcInerney/net-creds), and [NetMiner](http://www.netminer.com/main/main-read.do), etc., to perform the same functions. We can also use tcpdump to save a capture to a .pcap file, transfer it to another host, and open it in Wireshark.
[Responder](https://github.com/lgandx/Responder-Windows)

Our passive checks have given us a few hosts to note down for a more in-depth enumeration. Now let's perform some active checks starting with a quick ICMP sweep of the subnet using `fping`.

#### FPing Active Checks

Here we'll start `fping` with a few flags: `a` to show targets that are alive, `s` to print stats at the end of the scan, `g` to generate a target list from the CIDR network, and `q` to not show per-target results.

  FPing Active Checks

```shell
gdxqpardo@htb[/htb]$ fping -asgq 172.16.5.0/23
```

Now that we have a list of active hosts within our network, we can enumerate those hosts further. We are looking to determine what services each host is running, identify critical hosts such as `Domain Controllers` and `web servers`, and identify potentially vulnerable hosts to probe later. With our focus on AD, after doing a broad sweep, it would be wise of us to focus on standard protocols typically seen accompanying AD services, such as DNS, SMB, LDAP, and Kerberos name a few. Below is a quick example of a simple Nmap scan.

Code: bash

```bash
sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```

https://github.com/ropnop/kerbrute
# Kerbrute AD Username (internal) Enumeration
- Stealth option for domain acc enumeration. 
	- kerberos pre-auth failures odten dont trigger logs/alerts.
- Used in conjunction with jsmith.txt or jsmith2.txt user lists from https://github.com/insidetrust/statistically-likely-usernames
https://github.com/ropnop/kerbrute/releases/latest
#### Cloning Kerbrute GitHub Repo

  Cloning Kerbrute GitHub Repo

```shell
gdxqpardo@htb[/htb]$ sudo git clone https://github.com/ropnop/kerbrute.git
```

#### isting Compiling Options

  Listing Compiling Options

```shell
gdxqpardo@htb[/htb]$ make help

help:            Show this help.
windows:  Make Windows x86 and x64 Binaries
linux:  Make Linux x86 and x64 Binaries
mac:  Make Darwin (Mac) x86 and x64 Binaries
clean:  Delete any binaries
all:  Make Windows, Linux and Mac x86/x64 Binaries
```

#### Compiling for Multiple Platforms and Architectures

  Compiling for Multiple Platforms and Architectures

```shell-session
gdxqpardo@htb[/htb]$ sudo make all
```

#### Adding the Tool to our Path

  Adding the Tool to our Path

```shell-session
gdxqpardo@htb[/htb]$ echo $PATH
/home/htb-student/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/snap/bin:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/home/htb-student/.dotnet/tools
```

#### Moving the Binary

  Moving the Binary

```shell-session
gdxqpardo@htb[/htb]$ sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute
```


#### Enumerating Users with Kerbrute

  Enumerating Users with Kerbrute

```shell
gdxqpardo@htb[/htb]$ kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
___
userenum : username enumeration
-d : domain
--dc : domain controller IP
jsmith.txt : username wordlist
-o valid.. : output as <name>

```

The [local system](https://docs.microsoft.com/en-us/windows/win32/services/localsystem-account) account `NT AUTHORITY\SYSTEM` is a built-in account in Windows operating systems. It has the highest level of access in the OS and is used to run most Windows services. It is also very common for third-party services to run in the context of this account by default. A `SYSTEM` account on a `domain-joined` host will be able to enumerate Active Directory by impersonating the computer account, which is essentially just another kind of user account. Having SYSTEM-level access within a domain environment is nearly equivalent to having a domain user account.
- Remote Windows exploits such as MS08-067, EternalBlue, or BlueKeep.
- Abusing a service running in the context of the `SYSTEM account`, or abusing the service account `SeImpersonate` privileges using [Juicy Potato](https://github.com/ohpe/juicy-potato). This type of attack is possible on older Windows OS' but not always possible with Windows Server 2019.
- Local privilege escalation flaws in Windows operating systems such as the Windows 10 Task Scheduler 0-day.
- Gaining admin access on a domain-joined host with a local account and using Psexec to launch a SYSTEM cmd window
- 
**By gaining SYSTEM-level access on a domain-joined host, you will be able to perform actions such as, but not limited to:**

- Enumerate the domain using built-in tools or offensive tools such as BloodHound and PowerView.
- Perform Kerberoasting / ASREPRoasting attacks within the same domain.
- Run tools such as Inveigh to gather Net-NTLMv2 hashes or perform SMB relay attacks.
- Perform token impersonation to hijack a privileged domain user account.
- Carry out ACL attacks.
