## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [What Are We Looking For?](#What\Are\We\Looking\For?)
    - [Where can this data be found?](#Where\can\this\data\be\found?)
    - [Finding Address Spaces](#Finding\Address\Spaces)
    - [DNS](#DNS)
    - [Public Data](#Public\Data)
    - [Sharepoint Admin Job Listing](#Sharepoint\Admin\Job\Listing)
  - [Example Enumeration Process](#Example\Enumeration\Process)
      - [Check for ASN/IP & Domain Data](#Check\for\ASN/IP\&\Domain\Data)
      - [Hunting E-mail Addresses](#Hunting\E-mail\Addresses)
      - [E-mail Dork Results](#E-mail\Dork\Results)
      - [Username Harvesting](#Username\Harvesting)
      - [Credential Hunting](#Credential\Hunting)
      - [Key Data Points](#Key\Data\Points)
  - [TTPs](#TTPs)
    - [Identifying Hosts](#Identifying\Hosts)
      - [Tcpdump Output](#Tcpdump\Output)
      - [Starting Responder](#Starting\Responder)
      - [FPing Active Checks](#FPing\Active\Checks)
      - [Nmap Scanning](#Nmap\Scanning)
- [Initial Enumeration](#initial\enumeration)
  - [Identifying Users](#Identifying\Users)
    - [Kerbrute - Internal AD Username Enumeration](#Kerbrute\-\Internal\AD\Username\Enumeration)
      - [Cloning Kerbrute GitHub Repo](#Cloning\Kerbrute\GitHub\Repo)
      - [Listing Compiling Options](#Listing\Compiling\Options)
      - [Compiling for Multiple Platforms and Architectures](#Compiling\for\Multiple\Platforms\and\Architectures)
      - [Testing the kerbrute_linux_amd64 Binary](#Testing\the\kerbrute_linux_amd64\Binary)
      - [Adding the Tool to our Path](#Adding\the\Tool\to\our\Path)
      - [Moving the Binary](#Moving\the\Binary)
      - [Enumerating Users with Kerbrute](#Enumerating\Users\with\Kerbrute)
  - [Identifying Potential Vulnerabilities](#Identifying\Potential\Vulnerabilities)
  - [LLMNR & NBT-NS Primer](#LLMNR\&\NBT-NS\Primer)
  - [Quick Example - LLMNR/NBT-NS Poisoning](#Quick\Example\-\LLMNR/NBT-NS\Poisoning)
  - [TTPs](#TTPs)
    - [Responder In Action](#Responder\In\Action)

## Table of Contents

  - [What Are We Looking For?](#What\Are\We\Looking\For?)
    - [Where can this data be found?](#Where\can\this\data\be\found?)
    - [Finding Address Spaces](#Finding\Address\Spaces)
    - [DNS](#DNS)
    - [Public Data](#Public\Data)
    - [Sharepoint Admin Job Listing](#Sharepoint\Admin\Job\Listing)
  - [Example Enumeration Process](#Example\Enumeration\Process)
      - [Check for ASN/IP & Domain Data](#Check\for\ASN/IP\&\Domain\Data)
      - [Hunting E-mail Addresses](#Hunting\E-mail\Addresses)
      - [E-mail Dork Results](#E-mail\Dork\Results)
      - [Username Harvesting](#Username\Harvesting)
      - [Credential Hunting](#Credential\Hunting)
      - [Key Data Points](#Key\Data\Points)
  - [TTPs](#TTPs)
    - [Identifying Hosts](#Identifying\Hosts)
      - [Tcpdump Output](#Tcpdump\Output)
      - [Starting Responder](#Starting\Responder)
      - [FPing Active Checks](#FPing\Active\Checks)
      - [Nmap Scanning](#Nmap\Scanning)
- [Initial Enumeration](#initial\enumeration)
  - [Identifying Users](#Identifying\Users)
    - [Kerbrute - Internal AD Username Enumeration](#Kerbrute\-\Internal\AD\Username\Enumeration)
      - [Cloning Kerbrute GitHub Repo](#Cloning\Kerbrute\GitHub\Repo)
      - [Listing Compiling Options](#Listing\Compiling\Options)
      - [Compiling for Multiple Platforms and Architectures](#Compiling\for\Multiple\Platforms\and\Architectures)
      - [Testing the kerbrute_linux_amd64 Binary](#Testing\the\kerbrute_linux_amd64\Binary)
      - [Adding the Tool to our Path](#Adding\the\Tool\to\our\Path)
      - [Moving the Binary](#Moving\the\Binary)
      - [Enumerating Users with Kerbrute](#Enumerating\Users\with\Kerbrute)
  - [Identifying Potential Vulnerabilities](#Identifying\Potential\Vulnerabilities)
  - [LLMNR & NBT-NS Primer](#LLMNR\&\NBT-NS\Primer)
  - [Quick Example - LLMNR/NBT-NS Poisoning](#Quick\Example\-\LLMNR/NBT-NS\Poisoning)
  - [TTPs](#TTPs)
    - [Responder In Action](#Responder\In\Action)

Before kicking off any pentest, it can be beneficial to perform `external reconnaissance` of your target. This can serve many different functions, such as:

- Validating information provided to you in the scoping document from the client
- Ensuring you are taking actions against the appropriate scope when working remotely
- Looking for any information that is publicly accessible that can affect the outcome of your test, such as **leaked credentials**

## What Are We Looking For?
|**Data Point**|**Description**|
|---|---|
|`IP Space`|Valid **ASN** for our target, netblocks in use for the organization's public-facing infrastructure, cloud presence and the hosting providers, DNS record entries, etc. |
|`Domain Information`|Based on **IP data**, **DNS**, and **site registrations**. Who administers the domain? Are there any **subdomains** tied to our target? Are there any *publicly accessible domain services* present? (**Mailservers, DNS, Websites, VPN portals**, etc.) Can we determine what kind of defenses are in place? (**SIEM, AV, IPS/IDS** in use, etc.) |
|`Schema Format`|Can we discover the organization's email accounts, AD usernames, and even password policies? Anything that will give us information we can use to build a valid username list to test external-facing services for password spraying, credential stuffing, brute forcing, etc.|
|`Data Disclosures`|For data disclosures we will be looking for publicly accessible files ( **.pdf, .ppt, .docx, .xlsx**, etc. ) for any information that helps shed light on the target. For example, any published files that contain `intranet` site listings, user metadata, shares, or other critical software or hardware in the environment (credentials pushed to a public GitHub repo, the internal AD username format in the metadata of a PDF, for example.) |
|`Breach Data`|Any publicly released usernames, passwords, or other critical information that can help an attacker gain a foothold.|
### Where can this data be found?
|**Resource**|**Examples**|
|---|---|
|`ASN / IP registrars`|[IANA](https://www.iana.org/), [arin](https://www.arin.net/) for searching the Americas, [RIPE](https://www.ripe.net/) for searching in Europe, [BGP Toolkit](https://bgp.he.net/)|
|`Domain Registrars & DNS`|[Domaintools](https://www.domaintools.com/), [PTRArchive](http://ptrarchive.com/), [ICANN](https://lookup.icann.org/lookup), manual DNS record requests against the domain in question or against well known DNS servers, such as `8.8.8.8`.|
|`Social Media`|Searching **Linkedin, Twitter, Facebook**, your region's major social media sites, news articles, and any relevant info you can find about the organization. |
|`Public-Facing Company Websites`|Often, the public website for a corporation will have relevant info embedded. News articles, embedded documents, and the "**About Us**" and "**Contact Us**" pages can also be gold mines. |
|`Cloud & Dev Storage Spaces`|[GitHub](https://github.com/), [AWS S3 buckets & Azure Blog storage containers](https://grayhatwarfare.com/), [Google searches using "Dorks"](https://www.exploit-db.com/google-hacking-database)|
|`Breach Data Sources`|[HaveIBeenPwned](https://haveibeenpwned.com/) to determine if any corporate email accounts appear in public breach data, [Dehashed](https://www.dehashed.com/) to search for corporate emails with cleartext passwords or hashes we can try to crack offline. **We can then try these passwords against any exposed login portals (Citrix, RDS, OWA, 0365, VPN, VMware Horizon, custom applications, etc.**) that may use AD authentication. |
### Finding Address Spaces

![image](https://academy.hackthebox.com/storage/modules/143/bgp-toolkit.png)

The `BGP-Toolkit` hosted by [Hurricane Electric](http://he.net/) is useful for:
- Locating Address blocks of an organization
	- ASN
	- IP's
	- Domains
Many large corporations will often self-host their infrastructure, and since they have such a large footprint, they will have their own ASN. This will typically not be the case for smaller organizations.


You have an agreement to test with the customer, not with others on the same server or with the provider. Questions around self-hosted or 3rd party managed infrastructure should be handled during the scoping process and be clearly listed in any scoping documents you receive.

In some cases, your client may need to get written approval from a third-party hosting provider before you can test. Others, such as AWS, have specific [guidelines](https://aws.amazon.com/security/penetration-testing/) for performing penetration tests and do not require prior approval for testing some of their services. Others, such as Oracle, ask you to submit a [Cloud Security Testing Notification](https://docs.oracle.com/en-us/iaas/Content/Security/Concepts/security_testing-policy_notification.htm). These types of steps should be handled by your company management, legal team, contracts team, etc. If you are in doubt, escalate before attacking any external-facing services you are unsure of during an assessment. It is our responsibility to ensure that we have explicit permission to attack any hosts (both internal and external), and stopping and clarifying the scope in writing never hurts.

### DNS

DNS is a great way to validate our scope and find out about reachable hosts the customer did not disclose in their scoping document. Sites like [domaintools](https://whois.domaintools.com/), and [viewdns.info](https://viewdns.info/)

We can get back many records and other data ranging from DNS resolution to testing for DNSSEC and if the site is accessible in more restricted countries

![[viewdnsinfo.png]]

This is also a great way to validate some of the data found from our **IP/ASN** searches. Not all information about the domain found will be current, and running checks that can validate what we see is always good practice.


### Public Data

Social media can be a treasure trove of interesting data that can clue us in to how the organization is structured, what kind of equipment they operate, potential software and security implementations, their schema, and more. On top of that list are job-related sites like LinkedIn, Indeed.com, and Glassdoor. Simple job postings often reveal a lot about a company. For example, take a look at the job listing below. It's for a `SharePoint Administrator` and can key us in on many things. We can tell from the listing that the company has been using SharePoint for a while and has a mature program since they are talking about security programs, backup & disaster recovery, and more. What is interesting to us in this posting is that we can see the company likely uses SharePoint 2013 and SharePoint 2016. That means they may have upgraded in place, potentially leaving vulnerabilities in play that may not exist in newer versions. This also means we may run into different versions of SharePoint during our engagements.
### Sharepoint Admin Job Listing

![image](https://academy.hackthebox.com/storage/modules/143/spjob2.png)


Tools like [Trufflehog](https://github.com/trufflesecurity/truffleHog) and sites like [Greyhat Warfare](https://buckets.grayhatwarfare.com/) are fantastic resources for finding these breadcrumbs.

We have spent some time discussing external enumeration and recon of an organization, but this is just one piece of the puzzle. For a more detailed introduction to OSINT and external enumeration, check out the [Footprinting](https://academy.hackthebox.com/course/preview/footprinting) and [OSINT:Corporate Recon](https://academy.hackthebox.com/course/preview/osint-corporate-recon) modules.


## Example Enumeration Process

We have already covered quite a few concepts pertaining to enumeration. Let's start putting it all together. We will practice our enumeration tactics on the `inlanefreight.com` domain without performing any heavy scans (such as Nmap or vulnerability scans which are out of scope). We will start first by checking our Netblocks data and seeing what we can find.

#### Check for ASN/IP & Domain Data

![image](https://academy.hackthebox.com/storage/modules/143/BGPhe-inlane.png)

From this first look, we have already gleaned some interesting info. BGP.he is reporting:

- `IP Address: 134.209.24.248`
- `Mail Server: mail1.inlanefreight.com`
- `Nameservers: NS1.inlanefreight.com & NS2.inlanefreight.com`

In the request above, we utilized `viewdns.info` to validate the IP address of our target. Both results match, which is a good sign. Now let's try another route to validate the two nameservers in our results.

External Recon and Enumeration Principles

```bash
gdxqpardo@htb[/htb]$ nslookup ns1.inlanefreight.com
```


#### Hunting E-mail Addresses

![image](https://academy.hackthebox.com/storage/modules/143/intext-dork.png)

Using the dork `intext:"@inlanefreight.com" inurl:inlanefreight.com`

This looks for any instance that appears similar to the end of an email address on the website. One promising result came up with a contact page. When we look at the page, we can see a list of employees and contact info for them. This information can be helpful since we can determine that these people are at least most likely active and still working with the company.

#### E-mail Dork Results

Browsing the [contact page](https://www.inlanefreight.com/index.php/contact/), we can see several emails for staff in different offices around the globe. We now have an idea of their email naming convention (first.last) and where some people work in the organization. This could be handy in later password spraying attacks or if social engineering/phishing were part of our engagement scope.

![image](https://academy.hackthebox.com/storage/modules/143/ilfreightemails.png)
#### Username Harvesting

We can use a tool such as [linkedin2username](https://github.com/initstring/linkedin2username) to scrape data from a company's LinkedIn page and create various mashups of usernames (flast, first.last, f.last, etc.) that can be added to our list of potential password spraying targets.

#### Credential Hunting

[Dehashed](http://dehashed.com/) is an excellent tool for hunting for cleartext credentials and password hashes in breach data. We can search either on the site or using a script that performs queries via the API. Typically we will find many old passwords for users that do not work on externally-facing portals that use AD auth (or internal), but we may get lucky! This is another tool that can be useful for creating a user list for external or internal password spraying.

Note: For our purposes, the sample data below is fictional.

External Recon and Enumeration Principles

```bash
gdxqpardo@htb[/htb]$ sudo python3 dehashed.py -q inlanefreight.local -p
```
For this first portion of the test, we are starting on an attack host placed inside the network for us. This is one common way that a client might select for us to perform an internal penetration test. A list of the types of setups a client may choose for testing includes:

- A penetration testing distro (typically Linux) as a virtual machine in their internal infrastructure that calls back to a jump host we control over VPN, and we can SSH into.
- A physical device plugged into an ethernet port that calls back to us over VPN, and we can SSH into.
- A physical presence at their office with our laptop plugged into an ethernet port.
- A Linux VM in either Azure or AWS with access to the internal network that we can SSH into using public key authentication and our public IP address whitelisted.
- VPN access into their internal network (a bit limiting because we will not be able to perform certain attacks such as LLMNR/NBT-NS Poisoning).
- From a corporate laptop connected to the client's VPN.
- On a managed workstation (typically Windows), physically sitting in their office with limited or no internet access or ability to pull in tools. They may also elect this option but give you full internet access, local admin, and put endpoint protection into monitor mode so you can pull in tools at will.
- On a VDI (virtual desktop) accessed using Citrix or the like, with one of the configurations described for the managed workstation typically accessible over VPN either remotely or from a corporate laptop.


These are the most common setups I have seen, though a client may come up with another variation of one of these. The client may also choose from a "grey box" approach where they give us just a list of in-scope IP addresses/CIDR network ranges, or "black box" where we have to plug in and do all discovery blindly using various techniques. Finally, they can choose either evasive, non-evasive, or hybrid evasive (starting "quiet" and slowly getting louder to see what threshold we are detected at and then switching to non-evasive testing. They may also elect to have us start with no credentials or from the perspective of a standard domain user.

Our customer Inlanefreight has chosen the following approach because they are looking for as comprehensive an assessment as possible. At this time, their security program is not mature enough to benefit from any form of evasive testing or a "black box" approach.

- A custom pentest VM within their internal network that calls back to our jump host, and we can SSH into it to perform testing.
- They've also given us a Windows host that we can load tools onto if need be.
- They've asked us to start from an unauthenticated standpoint but have also given us a standard domain user account (`htb-student`) which can be used to access the Windows attack host.
- "Grey box" testing. They have given us the network range 172.16.5.0/23 and no other information about the network.
- Non-evasive testing.

#### Key Data Points
|**Data Point**|**Description**|
|---|---|
|`AD Users`|We are trying to enumerate valid user accounts we can target for **password spraying**. |
|`AD Joined Computers`|Key Computers include **Domain Controllers**, **file servers**, S**QL servers, web servers, Exchange mail servers, database servers**, etc. |
|`Key Services`|**Kerberos**, **NetBIOS**, **LDAP**, **DNS** |
|`Vulnerable Hosts and Services`|Anything that can be a quick win. ( a.k.a an easy host to exploit and gain a foothold)|
___


## TTPs

Enumerating an AD environment can be overwhelming if just approached without a plan. There is an abundance of data stored in AD, and it can take a long time to sift if not looked at in progressive stages, and we will likely miss things. We need to set a game plan for ourselves and tackle it piece by piece. Everyone works in slightly different ways, so as we gain more experience, we'll start to develop our own repeatable methodology that works best for us. Regardless of how we proceed, we typically start in the same place and look for the same data points. We will experiment with many tools in this section and subsequent ones. It is important to reproduce every example and even try to recreate examples with different tools to see how they work differently, learn their syntax, and find what approach works best for us.

We will start with `passive` identification of any hosts in the network, followed by `active` validation of the results to find out more about each host (what services are running, names, potential vulnerabilities, etc.). Once we know what hosts exist, we can proceed with probing those hosts, looking for any interesting data we can glean from them. After we have accomplished these tasks, we should stop and regroup and look at what info we have. At this time, we'll hopefully have a set of credentials or a user account to target for a foothold onto a domain-joined host or have the ability to begin credentialed enumeration from our Linux attack host.

Let's look at a few tools and techniques to help us with this enumeration.

### Identifying Hosts

First, let's take some time to listen to the network and see what's going on. We can use `Wireshark` and `TCPDump` to "put our ear to the wire" and see what hosts and types of network traffic we can capture. This is particularly helpful if the assessment approach is "black box." We notice some [ARP](https://en.wikipedia.org/wiki/Address_Resolution_Protocol) requests and replies, [MDNS](https://en.wikipedia.org/wiki/Multicast_DNS), and other basic [layer two](https://www.juniper.net/documentation/us/en/software/junos/multicast-l2/topics/topic-map/layer-2-understanding.html) packets (since we are on a switched network, we are limited to the current broadcast domain) some of which we can see below. This is a great start that gives us a few bits of information about the customer's network setup.  

If we are on a host without a GUI (which is typical), we can use [tcpdump](https://linux.die.net/man/8/tcpdump), [net-creds](https://github.com/DanMcInerney/net-creds), and [NetMiner](https://www.netminer.com/en/product/netminer.php), etc., to perform the same functions. We can also use `tcpdump` to save a capture to a `.pcap` file, transfer it to another host, and open it in `Wireshark`.

#### Tcpdump Output
Initial Enumeration of the Domain
```bash
gdxqpardo@htb[/htb]$ sudo tcpdump -i ens224 
```

Our first look at network traffic pointed us to a couple of hosts via `MDNS` and `ARP`. Now let's utilize a tool called `Responder` to analyze network traffic and determine if anything else in the domain pops up.

[Responder](https://github.com/lgandx/Responder-Windows) is a tool built to listen, analyze, and poison `LLMNR`, `NBT-NS`, and `MDNS` requests and responses. It has many other functions as well, but for now we will but using the tool in its `analyze` mode. This will passively listen to the network and not send any poisoned packets. We'll cover this tool more in-depth in later sections

#### Starting Responder
```bash
sudo responder -I ens224 -A
```
As we start Responder with passive analysis mode enabled, we will see requests flow in our session. Notice below that we found a few unique hosts not previously mentioned in our Wireshark captures. It's worth noting these down as we are starting to build a nice target list of IPs and DNS hostnames.

#### FPing Active Checks
Here we'll start `fping` with a few flags: `a` to show targets that are alive, `s` to print stats at the end of the scan, `g` to generate a target list from the CIDR network, and `q` to not show per-target results.

Initial Enumeration of the Domain

```bash
gdxqpardo@htb[/htb]$ fping -asgq 172.16.5.0/23
```
#### Nmap Scanning

Now that we have a list of active hosts within our network, we can enumerate those hosts further. We are looking to determine what services each host is running, identify critical hosts such as `Domain Controllers` and `web servers`, and identify potentially vulnerable hosts to probe later. With our focus on AD, after doing a broad sweep, it would be wise of us to focus on standard protocols typically seen accompanying AD services, such as **DNS, SMB, LDAP, and Kerberos** name a few. Below is a quick example of a simple Nmap scan.

```bash
sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```

Our scans have provided us with the naming standard used by NetBIOS and DNS, we can see some hosts have RDP open, and they have pointed us in the direction of the primary `Domain Controller` for the INLANEFREIGHT.LOCAL domain (ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL). The results below show some interesting results surrounding a possibly outdated host (not in our current lab).

Always look for:
- Outdated hosts with potential known exploits
- `EternalBlue` etc.

# Initial Enumeration
## Identifying Users

If our client does not provide us with a user to start testing with (which is often the case), we will need to find a way to establish a foothold in the domain by either obtaining clear text credentials or an NTLM password hash for a user, a SYSTEM shell on a domain-joined host, or a shell in the context of a domain user account. Obtaining a valid user with credentials is critical in the early stages of an internal penetration test. This access (even at the lowest level) opens up many opportunities to perform enumeration and even attacks. Let's look at one way we can start gathering a list of valid users in a domain to use later in our assessment.

### Kerbrute - Internal AD Username Enumeration

[Kerbrute](https://github.com/ropnop/kerbrute) can be a stealthier option for domain account enumeration. It takes advantage of the fact that Kerberos pre-authentication failures often will not trigger logs or alerts. We will use Kerbrute in conjunction with the `jsmith.txt` or `jsmith2.txt` user lists from [Insidetrust](https://github.com/insidetrust/statistically-likely-usernames). This repository contains many different user lists that can be extremely useful when attempting to enumerate users when starting from an unauthenticated perspective. We can point Kerbrute at the DC we found earlier and feed it a wordlist. The tool is quick, and we will be provided with results letting us know if the accounts found are valid or not, which is a great starting point for launching attacks such as password spraying, which we will cover in-depth later in this module.

To get started with Kerbrute, we can download [precompiled binaries](https://github.com/ropnop/kerbrute/releases/latest) for the tool for testing from Linux, Windows, and Mac, or we can compile it ourselves. This is generally the best practice for any tool we introduce into a client environment. To compile the binaries to use on the system of our choosing, we first clone the repo:

#### Cloning Kerbrute GitHub Repo

Initial Enumeration of the Domain

```bash
gdxqpardo@htb[/htb]$ sudo git clone https://github.com/ropnop/kerbrute.git
```

Typing `make help` will show us the compiling options available.

#### Listing Compiling Options

Initial Enumeration of the Domain

```bash
gdxqpardo@htb[/htb]$ make help

help:            Show this help.
windows:  Make Windows x86 and x64 Binaries
linux:  Make Linux x86 and x64 Binaries
mac:  Make Darwin (Mac) x86 and x64 Binaries
clean:  Delete any binaries
all:  Make Windows, Linux and Mac x86/x64 Binaries
```

We can choose to compile just one binary or type `make all` and compile one each for use on Linux, Windows, and Mac systems (an x86 and x64 version for each).

#### Compiling for Multiple Platforms and Architectures

Initial Enumeration of the Domain

```bash
gdxqpardo@htb[/htb]$ sudo make all
```

We can then test out the binary to make sure it works properly. We will be using the x64 version on the supplied Parrot Linux attack host in the target environment.

#### Testing the kerbrute_linux_amd64 Binary

Initial Enumeration of the Domain

```bash
gdxqpardo@htb[/htb]$ ./kerbrute_linux_amd64 
```

We can add the tool to our PATH to make it easily accessible from anywhere on the host.

#### Adding the Tool to our Path

Initial Enumeration of the Domain

```bash
gdxqpardo@htb[/htb]$ echo $PATH
/home/htb-student/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/snap/bin:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/home/htb-student/.dotnet/tools
```

#### Moving the Binary

Initial Enumeration of the Domain

```bash
gdxqpardo@htb[/htb]$ sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute
```


#### Enumerating Users with Kerbrute

```bash
gdxqpardo@htb[/htb]$ kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```

## Identifying Potential Vulnerabilities

The [local system](https://docs.microsoft.com/en-us/windows/win32/services/localsystem-account) account `NT AUTHORITY\SYSTEM` is a built-in account in Windows operating systems. It has the highest level of access in the OS and is used to run most Windows services. It is also very common for third-party services to run in the context of this account by default. A `SYSTEM` account on a `domain-joined` host will be able to enumerate Active Directory by impersonating the computer account, which is essentially just another kind of user account. Having SYSTEM-level access within a domain environment is nearly equivalent to having a domain user account.

There are several ways to gain SYSTEM-level access on a host, including but not limited to:

- Remote Windows exploits such as MS08-067, EternalBlue, or BlueKeep.
- Abusing a service running in the context of the `SYSTEM account`, or abusing the service account `SeImpersonate` privileges using [Juicy Potato](https://github.com/ohpe/juicy-potato). This type of attack is possible on older Windows OS' but not always possible with Windows Server 2019.
- Local privilege escalation flaws in Windows operating systems such as the Windows 10 Task Scheduler 0-day.
- Gaining admin access on a domain-joined host with a local account and using Psexec to launch a SYSTEM cmd window

By gaining SYSTEM-level access on a domain-joined host, you will be able to perform actions such as, but not limited to:

- Enumerate the domain using built-in tools or offensive tools such as BloodHound and PowerView.
- Perform Kerberoasting / ASREPRoasting attacks within the same domain.
- Run tools such as Inveigh to gather Net-NTLMv2 hashes or perform SMB relay attacks.
- Perform token impersonation to hijack a privileged domain user account.
- Carry out ACL attacks.


## LLMNR & NBT-NS Primer

[Link-Local Multicast Name Resolution](https://datatracker.ietf.org/doc/html/rfc4795) (LLMNR) and [NetBIOS Name Service](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc940063(v=technet.10)?redirectedfrom=MSDN) (NBT-NS) are Microsoft Windows components that serve as alternate methods of host identification that can be used when DNS fails. If a machine attempts to resolve a host but DNS resolution fails, typically, the machine will try to ask all other machines on the local network for the correct host address via LLMNR. LLMNR is based upon the Domain Name System (DNS) format and allows hosts on the same local link to perform name resolution for other hosts. It uses port `5355` over UDP natively. If LLMNR fails, the NBT-NS will be used. NBT-NS identifies systems on a local network by their NetBIOS name. NBT-NS utilizes port `137` over UDP.


The kicker here is that when LLMNR/NBT-NS are used for name resolution, ANY host on the network can reply. This is where we come in with `Responder` to poison these requests. With network access, we can spoof an authoritative name resolution source ( in this case, a host that's supposed to belong in the network segment ) in the broadcast domain by responding to LLMNR and NBT-NS traffic as if they have an answer for the requesting host. This poisoning effort is done to get the victims to communicate with our system by pretending that our rogue system knows the location of the requested host. If the requested host requires name resolution or authentication actions, we can capture the NetNTLM hash and subject it to an offline brute force attack in an attempt to retrieve the cleartext password. The captured authentication request can also be relayed to access another host or used against a different protocol (such as LDAP) on the same host. LLMNR/NBNS spoofing combined with a lack of SMB signing can often lead to administrative access on hosts within a domain. SMB Relay attacks will be covered in a later module about Lateral Movement.

---

## Quick Example - LLMNR/NBT-NS Poisoning

Let's walk through a quick example of the attack flow at a very high level:

1. A host attempts to connect to the print server at `\\print01.inlanefreight.local`, but accidentally types in `\\printer01.inlanefreight.local`.
2. The DNS server responds, stating that this host is unknown.
3. The host then broadcasts out to the entire local network asking if anyone knows the location of `\\printer01.inlanefreight.local`.
4. The attacker (us with `Responder` running) responds to the host stating that it is the `\\printer01.inlanefreight.local` that the host is looking for.
5. The host believes this reply and sends an `authentication` request to the attacker with a username and `NTLMv2` password hash.
6. This hash can then be cracked offline or used in an `SMB` Relay attack if the right conditions exist.


## TTPs

We are performing these actions to collect authentication information sent over the network in the form of NTLMv1 and NTLMv2 password hashes. As discussed in the [Introduction to Active Directory](https://academy.hackthebox.com/course/preview/introduction-to-active-directory) module, NTLMv1 and NTLMv2 are authentication protocols that utilize the LM or NT hash. We will then take the hash and attempt to crack them offline using tools such as [Hashcat](https://hashcat.net/hashcat/) or [John](https://www.openwall.com/john/) with the goal of obtaining the account's cleartext password to be used to gain an initial foothold or expand our access within the domain if we capture a password hash for an account with more privileges than an account that we currently possess.

Several tools can be used to attempt LLMNR & NBT-NS poisoning:

|**Tool**|**Description**|
|---|---|
|[Responder](https://github.com/lgandx/Responder)|**Responder** is a purpose-built tool to poison LLMNR, NBT-NS, and MDNS, with many different functions. |
|[Inveigh](https://github.com/Kevin-Robertson/Inveigh)|**Inveigh is a cross-platform MITM platform** that can be used for s*poofing and poisoning attacks*. |
|[Metasploit](https://www.metasploit.com/)|**Metasploit** has several built-in scanners and spoofing modules made to deal with poisoning attacks. |
This section and the following one will show examples of using Responder and Inveigh to capture password hashes and attempt to crack them offline. We commonly start an internal penetration test from an anonymous position on the client's internal network with a Linux attack host. Tools such as Responder are great for establishing a foothold that we can later expand upon through further enumeration and attacks. Responder is written in Python and typically used on a Linux attack host, though there is a .exe version that works on Windows. Inveigh is written in both C# and PowerShell (considered legacy). Both tools can be used to attack the following protocols:

- **LLMNR**
- **DNS**
- **MDNS**
- **NBNS**
- **DHCP**
- **ICMP**
- **HTTP**
- **HTTPS**
- **SMB**
- **LDAP**
- **WebDAV**
- **Proxy Auth**

Responder also has support for:

- *MSSQL*
- *DCE-RPC*
- *FTP, POP3, IMAP, and SMTP auth*
### Responder In Action

Responder is a relatively straightforward tool, but is extremely powerful and has many different functions. In the `Initial Enumeration` section earlier, we utilized Responder in Analysis (passive) mode. This means it listened for any resolution requests, but did not answer them or send out poisoned packets. We were acting like a fly on the wall, just listening. Now, we will take things a step further and let Responder do what it does best. Let's look at some options available by typing `responder -h` into our console.

**LLMNR/NBT-NS Poisoning - from Linux**
```bash
gdxqpardo@htb[/htb]$ responder -h
Usage: responder -I eth0 -w -r -f
or:
responder -I eth0 -wrf

Options:
  
  --version             show program's version number and exit
  
  -h, --help            show this help message and exit
  
  -A, --analyze         Analyze mode. This option allows you to see NBT-NS,
                        BROWSER, LLMNR requests without responding.
  
  -I eth0, --interface=eth0
                        Network interface to use, you can use 'ALL' as a
                        wildcard for all interfaces
  
  -i 10.0.0.21, --ip=10.0.0.21
                        Local IP to use (only for OSX)
  
  -e 10.0.0.22, --externalip=10.0.0.22
                        Poison all requests with another IP address than
                        Responder's one.
  
  -b, --basic           Return a Basic HTTP authentication. Default: NTLM
  
  -r, --wredir          Enable answers for netbios wredir suffix queries.
                        Answering to wredir will likely break stuff on the
                        network. Default: False
  
  -d, --NBTNSdomain     Enable answers for netbios domain suffix queries.
                        Answering to domain suffixes will likely break stuff
                        on the network. Default: False
  
  -f, --fingerprint     This option allows you to fingerprint a host that
                        issued an NBT-NS or LLMNR query.
  
  -w, --wpad            Start the WPAD rogue proxy server. Default value is
                        False
  
  -u UPSTREAM_PROXY, --upstream-proxy=UPSTREAM_PROXY
                        Upstream HTTP proxy used by the rogue WPAD Proxy for
                        outgoing requests (format: host:port)
  
  -F, --ForceWpadAuth   Force NTLM/Basic authentication on wpad.dat file
                        retrieval. This may cause a login prompt. Default:
                        False
  
  -P, --ProxyAuth       Force NTLM (transparently)/Basic (prompt)
                        authentication for the proxy. WPAD doesn't need to be
                        ON. This option is highly effective when combined with
                        -r. Default: False
                        
  
  --lm                  Force LM hashing downgrade for Windows XP/2003 and
                        earlier. Default: False
  
  -v, --verbose         Increase verbosity.
  
```

`-A`: Analyze mode, allowign us to see NBT-NS, BROWSER, LLMNR requests in the environment without poisoning any responses.

Some common options we'll typically want to use are `-wf`; this will start the **WPAD rogue proxy server**, while `-f` will attempt to **fingerprint** the *remote host operating system and version*. We can use the `-v` flag for increased **verbosity** if we are running into issues, but this will lead to a lot of additional data printed to the console. Other options such as `-F` and `-P` can be used to force *NTLM or Basic authentication and force proxy authentication*, but may cause a login prompt, so they should be used sparingly. The use of the `-w` flag utilizes the built-in WPAD proxy server. This can be highly effective, especially in large organizations, because it will capture all HTTP requests by any users that launch Internet Explorer if the browser has [Auto-detect settings](https://docs.microsoft.com/en-us/internet-explorer/ie11-deploy-guide/auto-detect-settings-for-ie11) enabled.


With this configuration shown above, Responder will listen and answer any requests it sees on the wire. If you are successful and manage to capture a hash, Responder will print it out on screen and write it to a log file per host located in the `/usr/share/responder/logs` directory. Hashes are saved in the format `(MODULE_NAME)-(HASH_TYPE)-(CLIENT_IP).txt`, and one hash is printed to the console and stored in its associated log file unless `-v` mode is enabled. For example, a log file may look like `SMB-NTLMv2-SSP-172.16.5.25`. Hashes are also stored in a SQLite database that can be configured in the `Responder.conf` config file, typically located in `/usr/share/responder` unless we clone the Responder repo directly from GitHub.








