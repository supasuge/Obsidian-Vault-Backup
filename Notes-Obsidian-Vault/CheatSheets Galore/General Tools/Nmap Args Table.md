
| **Nmap Flag**    | **Description**           | **Situational Usage Example**                                                                                                             |
| ---------------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `-sS`            | Stealth SYN scan          | Use when you want to perform a quick and stealthy scan to detect open ports without completing a TCP connection.                          |
| `-sT`            | Connect scan              | Use when stealth is not a priority and you're scanning in an environment where SYN scans are not permitted.                               |
| `-sU`            | UDP scan                  | Use for identifying open UDP ports on target systems, important for services like DNS, SNMP, or DHCP.                                     |
| `-sV`            | Service version detection | Use when you need to determine the version of the services running on open ports.                                                         |
| `-O`             | OS detection              | Use to attempt to determine the operating system of the target machine.                                                                   |
| `-A`             | Aggressive scan           | Use for a more aggressive scan that enables OS detection, version detection, script scanning, and traceroute.                             |
| `-p`             | Port selection            | Use when you want to specify particular ports or port ranges to scan, e.g., `-p 80,443` or `-p 1-1000`.                                   |
| `--script`       | Script scan               | Use to execute specific NSE (Nmap Scripting Engine) scripts against target hosts. Ideal for vulnerability scanning or advanced discovery. |
| `-T<0-5>`        | Timing template           | Use to control scan speed and stealthiness. `-T4` is a good balance for fast and reliable scans in most environments.                     |
| `--open`         | Show only open ports      | Use when you are only interested in services that are actively accepting connections.                                                     |
| `-Pn`            | No ping                   | Use when you want to skip host discovery and scan all given IP addresses, useful in tightly firewalled networks.                          |
| `--reason`       | Show reason               | Use to display the reason a port is set to a particular state, helpful for troubleshooting.                                               |
| `-v`             | Verbose mode              | Use to increase the verbosity level, providing more details about the scan process and results.                                           |
| `--packet-trace` | Packet trace              | Use for debugging or learning purposes to show all packets sent and received.                                                             |
| `--spoof-mac`    | MAC address spoofing      | Use to spoof your MAC address, potentially bypassing access controls based on MAC filtering.                                              |

| **Nmap Flag**         | **Description**                   | **Situational Usage Example**                                                                                              |
| --------------------- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `-sn`                 | Ping scan                         | Use to quickly discover hosts on the network without scanning for open ports. Ideal for mapping network structure.         |
| `--scanflags`         | Customize TCP scan flags          | Use when you need to fine-tune the TCP scan flags sent during scan, allowing for customized packet crafting.               |
| `--traceroute`        | Perform traceroute                | Use after a scan to trace the path packets take to the target, helpful for mapping network routes.                         |
| `--dns-servers`       | Specify DNS servers               | Use when you want to manually specify DNS servers for Nmap to use during the scan, useful if the default is not preferred. |
| `-oN`                 | Normal output file                | Use to save the scan results in a human-readable form to a file, e.g., `-oN output.txt`.                                   |
| `-oX`                 | XML output file                   | Use to save the scan results in XML format, ideal for integration with other tools or automated processing.                |
| `-oG`                 | Grepable output file              | Use to save the scan results in a format easily parsed by grep or similar tools, e.g., `-oG output.txt`.                   |
| `-oA`                 | Output in all formats             | Use to save the scan results in all major formats (normal, XML, and grepable) with a base filename.                        |
| `--resume`            | Resume a paused scan              | Use to resume a previously paused scan from a save file, ensuring continuity in long or interrupted scans.                 |
| `--exclude`           | Exclude hosts                     | Use to exclude specific hosts or networks from the scan, helpful for adhering to permission boundaries.                    |
| `--excludefile`       | Exclude hosts from file           | Use to exclude a list of hosts specified in a file, streamlining larger scan exclusions.                                   |
| `--script-args`       | Provide script arguments          | Use to pass arguments to NSE scripts, customizing their execution based on your needs.                                     |
| `--script-updatedb`   | Update the script database        | Use to refresh the script database to ensure you're using the latest versions of NSE scripts.                              |
| `--version-intensity` | Control version scan intensity    | Use to set the intensity of version detection, balancing between speed and accuracy.                                       |
| `--version-light`     | Lighter version detection         | Use for faster version scans with potentially less accuracy, suitable for quick assessments.                               |
| `--version-all`       | Try all version detection methods | Use when you want the most comprehensive version detection, at the cost of time and bandwidth.                             |
| `--version-trace`     | Trace version scan activity       | Use for debugging or detailed analysis of the version scanning process.                                                    |
| `-d`                  | Increase debug level              | Use to provide more detailed debug information, helpful for troubleshooting complex scan issues.                           |
| `--badsum`            | Send packets with a bad checksum  | Use for firewall and IDS evasion, as some systems might ignore or treat differently packets with bad checksums.            |

| **Nmap Flag**       | **Description**                    | **Situational Usage Example**                                                                                               |
| ------------------- | ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `-6`                | Enable IPv6 scanning               | Use when targeting systems on an IPv6 network.                                                                              |
| `--scanflags`       | Customize TCP scan flags           | Use for crafting custom TCP packets, such as setting SYN/ACK flags manually.                                                |
| `-F`                | Fast mode - scans fewer ports      | Use when you want a quicker scan by limiting the number of ports Nmap scans.                                                |
| `--script-args`     | Provide arguments to scripts       | Use when specific NSE scripts require or accept arguments for more targeted or detailed scanning.                           |
| `--dns-servers`     | Specify DNS servers                | Use to manually specify DNS servers for Nmap to use during the scan, helpful if the default servers are slow or unreliable. |
| `--exclude`         | Exclude hosts from a scan          | Use to omit specific hosts or networks from your scan, helpful in large scans where certain systems must be left untouched. |
| `--excludefile`     | Exclude list from a file           | Use when you have a list of hosts or networks to exclude in a file, making it easier to manage large or dynamic lists.      |
| `-sC`               | Run default scripts                | Use for a basic script scan using the default set of scripts, good for general vulnerability scanning or enumeration.       |
| `-oN`               | Output scan in normal format       | Use to save the scan results in a human-readable format to a file.                                                          |
| `-oX`               | Output scan in XML format          | Use when you need the results in a format that can be easily imported into other tools or scripts.                          |
| `-oG`               | Output scan in Grepable format     | Use for saving results in a format easy to parse with grep or similar tools.                                                |
| `--resume`          | Resume a previous scan             | Use to resume a scan from a point where it was previously interrupted, using the output file as a checkpoint.               |
| `--randomize-hosts` | Randomize target scan order        | Use to avoid detection by scanning targets in a random order, useful in stealthier scans or when scanning large networks.   |
| `--data-length`     | Append random data to sent packets | Use to make the scan less detectable by IDS/IPS systems by adding random data to the packets.                               |
| `--ttl`             | Set IP time-to-live                | Use to modify the TTL of packets sent during the scan, potentially bypassing some types of network security measures.       |
| `--badsum`          | Send packets with a bad checksum   | Use in advanced scenarios to test how devices respond to corrupted packets, potentially identifying security flaws.         |

# Nmap Cheatsheet

## Table of Contents
- [Target Specification](#target\specification)
- [Nmap Scan Techniques](#nmap\scan\techniques)
- [Host Discovery](#host\discovery)
- [Port Specification](#port\specification)
- [Service and Version Detection](#service\and\version\detection)
- [OS Detection](#os\detection)
- [Timing and Performance](#timing\and\performance)
- [NSE Scripts](#nse\scripts)
- [Firewall / IDS Evasion and Spoofing](#firewall\\ids\evasion\and\spoofing)
- [Output](#output)
- [Miscellaneous Nmap Flags](#miscellaneous\nmap\flags)
- [Other Useful Nmap Commands](#other\useful\nmap\commands)

---

## Target Specification
Specify targets to scan.
- **Scan a single IP:** `nmap 192.168.1.1`
- **Scan specific IPs:** `nmap 192.168.1.1 192.168.2.1`
- **Scan a range:** `nmap 192.168.1.1-254`
- **Scan a domain:** `nmap scanme.nmap.org`
- **Scan using CIDR notation:** `nmap 192.168.1.0/24`
- **Scan targets from a file:** `nmap -iL targets.txt`
- **Scan 100 random hosts:** `nmap -iR 100`
- **Exclude listed hosts:** `nmap -exclude 192.168.1.1`

## Nmap Scan Techniques
Different scanning techniques to discover open ports and services.
- **TCP SYN port scan (Default):** `nmap 192.168.1.1 -sS`
- **TCP connect port scan:** `nmap 192.168.1.1 -sT`
- **UDP port scan:** `nmap 192.168.1.1 -sU`
- **TCP ACK port scan:** `nmap 192.168.1.1 -sA`
- **TCP Window port scan:** `nmap 192.168.1.1 -sW`
- **TCP Maimon port scan:** `nmap 192.168.1.1 -sM`

## Host Discovery
Techniques to identify live hosts on a network.
- **List targets only:** `nmap 192.168.1.1-3 -sL`
- **Host discovery only:** `nmap 192.168.1.1/24 -sn`
- **Port scan only:** `nmap 192.168.1.1-5 -Pn`
- **TCP SYN discovery on port x:** `nmap 192.168.1.1-5 -PS22-25,80`
- **TCP ACK discovery on port x:** `nmap 192.168.1.1-5 -PA22-25,80`
- **UDP discovery on port x:** `nmap 192.168.1.1-5 -PU53`
- **ARP discovery on local network:** `nmap 192.168.1.1-1/24 -PR`
- **Never do DNS resolution:** `nmap 192.168.1.1 -n`

## Port Specification
Specify which ports to scan.
- **Port scan for port x:** `nmap 192.168.1.1 -p 21`
- **Port range:** `nmap 192.168.1.1 -p 21-100`
- **Port scan multiple TCP and UDP ports:** `nmap 192.168.1.1 -p U:53,T:21-25,80`
- **Port scan all ports:** `nmap 192.168.1.1 -p-`
- **Port scan from service name:** `nmap 192.168.1.1 -p http,https`
- **Fast port scan (100 ports):** `nmap 192.168.1.1 -F`
- **Port scan the top x ports:** `nmap 192.168.1.1 -top-ports 2000`
- **Leaving off initial port in range makes the scan start at port 1:** `nmap 192.168.1.1 -p-65535`
- **Leaving off end port in range makes the scan go through to port 65535:** `nmap 192.168.1.1 -p0-`

## Service and Version Detection
Detect services and their versions running on ports.
- **Determine the version of the service running on port:** `nmap 192.168.1.1 -sV`
- **Set version intensity level:** `nmap 192.168.1.1 -sV -version-intensity 8`
- **Enable light mode (faster, less accurate):** `nmap 192.168.1.1 -sV -version-light`
- **Enable intensity level 9 (slower, more accurate):** `nmap 192.168.1.1 -sV -version-all`
- **Enable OS detection, version detection, script scanning, and traceroute:** `nmap 192.168.1.1 -A`

## OS Detection
Identify the operating system of the target.
- **Remote OS detection using TCP/IP stack fingerprinting:** `nmap 192.168.1.1 -O`
- **OS detection only if at least one open and one closed TCP port are found:** `nmap 192.168.1.1 -O -osscan-limit`
- **Guess more aggressively:** `nmap 192.168.1.1 -O -osscan-guess`
- **Set the maximum number of OS detection tries:** `nmap 192.168.1.1 -O -max-os-tries 1`
- **Enable OS detection, version detection, script scanning, and traceroute:** `nmap 192.168.1.1 -A`

## Timing and Performance
Adjust scan timing and performance.
- **Paranoid Intrusion Detection System evasion:** `nmap 192.168.1.1 -T0`
- **Sneaky Intrusion Detection System evasion:** `nmap 192.168.1.1 -T1`
- **Polite mode (uses less bandwidth and resources):** `nmap 192.168.1.1 -T2`
- **Normal speed (default):** `nmap 192.168.1.1 -T3`
- **Aggressive mode (faster scans):** `nmap 192.168.1.1 -T4`
- **Insane mode (fastest scans):** `nmap 192.168.1.1 -T5`

## NSE Scripts
Use Nmap Scripting Engine (NSE) for advanced scans.
- **Scan with default NSE scripts:** `nmap 192.168.1.1 -sC`
- **Scan with a single script:** `nmap 192.168.1.1 -script=banner`
- **Scan with a wildcard:** `nmap 192.168.1.1 -script=http*`
- **Scan with two scripts:** `nmap 192.168.1.1 -script=http,banner`
- **Scan default but remove intrusive scripts:** `nmap 192.168.1.1 -script "not intrusive"`
- **NSE script with arguments:** `nmap -script snmp-sysdescr -script-args snmpcommunity=admin 192.168.1.1`

### Useful NSE Script Examples
- **HTTP site map generator:** `nmap -Pn -script=http-sitemap-generator scanme.nmap.org`
- **Fast search for random web servers:** `nmap -n -Pn -p 80 -open -sV -vvv -script banner,http-title -iR 1000`
- **Brute force DNS hostnames guessing subdomains:** `nmap -Pn -script=dns-brute domain.com`
- **Safe SMB scripts to run:** `nmap -n -Pn -vv -O -sV -script smb-enum*,smb-ls,smb-mbenum,smb-os-discovery,smb-s*,smb-vuln*,smbv2* -vv 192.168.1.1`
- **Whois query:** `nmap -script whois* domain.com`
- **Detect cross-site scripting vulnerabilities:** `nmap -p80 -script http-unsafe-output-escaping scanme.nmap.org`
- **Check for SQL injections:** `nmap -p80 -script http-sql-injection scanme.nmap.org`

## Firewall / IDS Evasion and Spoofing
Evade firewalls and intrusion detection systems.
- **Use tiny fragmented IP packets:** `nmap 192.168.1.1 -f`
- **Set your own offset size:** `nmap 192.168.1.1 -mtu 32`
- **Send scans from spoofed IPs:** `nmap -D 192.168.1.101,192.168.1.102,192.168.1.103,192.168.1.23 192.168.1.1`
- **Scan from Microsoft to Facebook (example):** `nmap -S www.microsoft.com www.facebook.com`
- **Use given source port number:** `nmap -g 53 192.168.1.1`
- **Relay connections through HTTP/SOCKS4 proxies:** `nmap -pro

xies http://192.168.1.1:8080, http://192.168.1.2:8080 192.168.1.1`
- **Append random data to sent packets:** `nmap -data-length 200 192.168.1.1`

## Output
Customize and manage Nmap output formats.
- **Normal output to a file:** `nmap 192.168.1.1 -oN normal.file`
- **XML output to a file:** `nmap 192.168.1.1 -oX xml.file`
- **Grepable output to a file:** `nmap 192.168.1.1 -oG grep.file`
- **Output in the three major formats at once:** `nmap 192.168.1.1 -oA results`
- **Grepable output to screen:** `nmap 192.168.1.1 -oG -`
- **Append a scan to a previous scan file:** `nmap 192.168.1.1 -oN file.file -append-output`
- **Increase verbosity level:** `nmap 192.168.1.1 -v`
- **Increase debugging level:** `nmap 192.168.1.1 -d`
- **Display the reason a port is in a particular state:** `nmap 192.168.1.1 -reason`
- **Only show open (or possibly open) ports:** `nmap 192.168.1.1 -open`
- **Show all packets sent and received:** `nmap 192.168.1.1 -packet-trace`
- **Show the host interfaces and routes:** `nmap -iflist`
- **Resume a scan:** `nmap -resume results.file`

### Helpful Nmap Output Examples
- **Scan for web servers and show which IPs are running web servers:** `nmap -p80 -sV -oG - -open 192.168.1.1/24 | grep open`
- **Generate a list of the IPs of live hosts:** `nmap -iR 10 -n -oX out.xml | grep "Nmap" | cut -d " " -f5 > live-hosts.txt`
- **Append IP to the list of live hosts:** `nmap -iR 10 -n -oX out2.xml | grep "Nmap" | cut -d " " -f5 >> live-hosts.txt`
- **Compare output from Nmap using ndiff:** `ndiff scanl.xml scan2.xml`
- **Convert Nmap XML files to HTML files:** `xsltproc nmap.xml -o nmap.html`
- **Reverse sorted list of how often ports turn up:** `grep " open " results.nmap | sed -r 's/ +/ /g' | sort | uniq -c | sort -rn | less`

## Miscellaneous Nmap Flags
- **Enable IPv6 scanning:** `nmap -6 2607:f0d0:1002:51::4`
- **Display Nmap help screen:** `nmap -h`

## Other Useful Nmap Commands
- **Discovery only on ports x, no port scan:** `nmap -iR 10 -PS22-25,80,113,1050,35000 -v -sn`
- **ARP discovery only on local network, no port scan:** `nmap 192.168.1.1-1/24 -PR -sn -vv`
- **Traceroute to random targets, no port scan:** `nmap -iR 10 -sn -traceroute`
- **Query the Internal DNS for hosts, list targets only:** `nmap 192.168.1.1-50 -sL -dns-server 192.168.1.1`
- **Show details of packets sent and received during a scan and capture the traffic:** `nmap 192.168.1.1 --packet-trace`

---
