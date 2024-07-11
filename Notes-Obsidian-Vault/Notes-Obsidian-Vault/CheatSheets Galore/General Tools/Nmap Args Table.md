## Table of Contents



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

