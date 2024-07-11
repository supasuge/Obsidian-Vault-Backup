## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [LLMNR & NBT-NS Primer](#LLMNR\&\NBT-NS\Primer)
  - [Quick Example - LLMNR/NBT-NS Poisoning](#Quick\Example\-\LLMNR/NBT-NS\Poisoning)
  - [TTPs](#TTPs)

## Table of Contents

  - [LLMNR & NBT-NS Primer](#LLMNR\&\NBT-NS\Primer)
  - [Quick Example - LLMNR/NBT-NS Poisoning](#Quick\Example\-\LLMNR/NBT-NS\Poisoning)
  - [TTPs](#TTPs)

At this poitn we will work through two different techniques side-by-side: network poisoning and password spraying. We will perform these with the goal of acquiring valid cleartext credentials for a domain user account.

This section and the next will cover a common way to gather credentials and gain an initial foothold during an assessment: a Man-in-the-Middle attack on Link-Local Multicast Name Resolution (LLMNR) and NetBIOS Name Service (NBT-NS) broadcasts.

## LLMNR & NBT-NS Primer

[Link-Local Multicast Name Resolution](https://datatracker.ietf.org/doc/html/rfc4795) (LLMNR) and [NetBIOS Name Service](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc940063(v=technet.10)?redirectedfrom=MSDN) (NBT-NS) are Microsoft Windows components that serve as alternate methods of host identification that can be used when DNS fails. If a machine attempts to resolve a host but DNS resolution fails, typically, the machine will try to ask all other machines on the local network for the correct host address via LLMNR. LLMNR is based upon the Domain Name System (DNS) format and allows hosts on the same local link to perform name resolution for other hosts. It uses port `5355` over UDP natively. If LLMNR fails, the NBT-NS will be used. NBT-NS identifies systems on a local network by their NetBIOS name. NBT-NS utilizes port `137` over UDP.


## Quick Example - LLMNR/NBT-NS Poisoning

Let's walk through a quick example of the attack flow at a very high level:

1. A host attempts to connect to the print server at \\print01.inlanefreight.local, but accidentally types in \\printer01.inlanefreight.local.
2. The DNS server responds, stating that this host is unknown.
3. The host then broadcasts out to the entire local network asking if anyone knows the location of \\printer01.inlanefreight.local.
4. The attacker (us with `Responder` running) responds to the host stating that it is the \\printer01.inlanefreight.local that the host is looking for.
5. The host believes this reply and sends an authentication request to the attacker with a username and NTLMv2 password hash.
6. This hash can then be cracked offline or used in an SMB Relay attack if the right conditions exist.

---

## TTPs

We are performing these actions to collect authentication information sent over the network in the form of NTLMv1 and NTLMv2 password hashes. As discussed in the [Introduction to Active Directory](https://academy.hackthebox.com/course/preview/introduction-to-active-directory) module, NTLMv1 and NTLMv2 are authentication protocols that utilize the LM or NT hash. We will then take the hash and attempt to crack them offline using tools such as [Hashcat](https://hashcat.net/hashcat/) or [John](https://www.openwall.com/john/) with the goal of obtaining the account's cleartext password to be used to gain an initial foothold or expand our access within the domain if we capture a password hash for an account with more privileges than an account that we currently possess.

Several tools can be used to attempt LLMNR & NBT-NS poisoning:

|**Tool**|**Description**|
|---|---|
|[Responder](https://github.com/lgandx/Responder)|Responder is a purpose-built tool to poison LLMNR, NBT-NS, and MDNS, with many different functions.|
|[Inveigh](https://github.com/Kevin-Robertson/Inveigh)|Inveigh is a cross-platform MITM platform that can be used for spoofing and poisoning attacks.|
|[Metasploit](https://www.metasploit.com/)|Metasploit has several built-in scanners and spoofing modules made to deal with poisoning atta|

To support most common GPUs, install [xf86-video-vesa](https://archlinux.org/packages/?name=xf86-video-vesa), [xf86-video-ati](https://archlinux.org/packages/?name=xf86-video-ati), [xf86-video-intel](https://archlinux.org/packages/?name=xf86-video-intel), [xf86-video-amdgpu](https://archlinux.org/packages/?name=xf86-video-amdgpu), [xf86-video-nouveau](https://archlinux.org/packages/?name=xf86-video-nouveau) and [xf86-video-fbdev](https://archlinux.org/packages/?name=xf86-video-fbdev).