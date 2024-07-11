## Table of Contents

- [Packet Crafting and Manipulation Notesheet](#packet\crafting\and\manipulation\notesheet)
  - [Overview](#Overview)
  - [hping](#hping)
  - [packETH](#packETH)
  - [fragroute](#fragroute)
  - [Key Concepts](#Key\Concepts)
  - [Common Flags and Parameters](#Common\Flags\and\Parameters)
  - [Tools for Packet Crafting and Manipulation](#Tools\for\Packet\Crafting\and\Manipulation)
    - [hping](#hping)
    - [packETH](#packETH)
    - [fragroute](#fragroute)
  - [Practical Usage Examples](#Practical\Usage\Examples)

Below is a concise notesheet in Markdown format, focusing on the important features, commands, tools, and usage examples related to packet crafting and manipulation as described in your book excerpt.

---
# Packet Crafting and Manipulation Notesheet
## Overview
- Packet crafting is essential for sending data in a customized manner, especially when testing network behavior or security.
- Tools like `hping`, `packETH`, and `fragroute` are instrumental in crafting and manipulating packets.
## hping
- **Description**: A versatile packet crafting tool that can manipulate TCP/IP packets.
- **Usage Examples**:
  - **Basic SYN Packet**: `sudo hping3 -S -p 80 192.168.86.1` - Sends SYN packets to port 80.
  - **ICMP Mode**: `hping3 -1 192.168.86.1` - Uses ICMP to ping the target.
  - **UDP Port Scan**: `hping3 --scan 1-1023 -2 192.168.86.1` - Scans UDP ports 1-1023.
  - **Spoofing Source IP**: `hping3 -a 10.15.24.5 -p 80 192.168.86.1` - Spoofs source IP address in packets.
  - **Custom Packet Size**: `hping3 -d [size] -p 80 192.168.86.1` - Sets the body size of the packet.

## packETH
- **Description**: A GUI-based packet generator that allows for detailed customization of packet headers and payloads.
- **Features**:
  - **GUI for Setting Fields**: Adjust header fields for various protocols.
  - **Payload Customization**: Add custom data or patterns to the payload.
  - **Packet Sending Options**: Offers `Gen-b` and `Gen-s` for burst or stream packet sending.
  - **File Operations**: Save and load packet configurations or PCAP files for repeated use.

## fragroute
- **Description**: Intercepts, modifies, and rewrites egress traffic for specified targets to test network responses.
- **Configuration Example**:
  - **File**: `frag.conf` 
  - **Directives**:
    ```
    delay random 1
    dup last 30%
    ip_chaff dup
    ip_frag 128 new
    tcp_chaff null 16
    order random
    print
    ```
- **Usage**: `fragroute -f /etc/fragroute.conf 184.159.210.190` - Applies the specified config file to manipulate packets sent to the target IP.

## Key Concepts
- **Packet Crafting**: The process of manually creating packets to test or attack networks.
- **ICMP, TCP, UDP**: Core protocols for which these tools can craft packets.
- **Raw Sockets**: Allows `hping` and similar tools to bypass the OS's network stack for packet manipulation.

## Common Flags and Parameters
- **-S**: SYN flag for `hping`.
- **-1**: ICMP mode for `hping`.
- **--scan**: Port scanning with `hping`.
- **-a**: Spoof source IP with `hping`.
- **-d**: Specify data size for `hping`.

This notesheet captures the essence of packet crafting and manipulation tools as outlined in your book excerpt, focusing on practical commands and usage scenarios.



## Tools for Packet Crafting and Manipulation

### hping
- **Description**: A command-line oriented TCP/IP packet assembler/analyzer.
- **Capabilities**:
  - Craft custom TCP, UDP, ICMP packets.
  - Perform network scans, ping sweeps, and port scans.
  - Test firewall rules, perform traceroute-like actions.

- **Key Commands**:
  - Basic SYN scan: `sudo hping3 -S -p 80 <target-IP>`
  - ICMP mode: `sudo hping3 -1 <target-IP>`
  - Sending SYN Messages: `sudo hping3 -S -p <port> <target-IP>`
  - UDP Mode: `sudo hping3 --udp -p <port> <target-IP>`
  - Spoofing Source IP: `sudo hping3 -a <spoofed-IP> -p <port> <target-IP>`
  - Fragmentation: `sudo hping3 -f -p <port> <target-IP>`
  - Sending Custom Data: `sudo hping3 -d <size> -E <file> -p <port> <target-IP>`
  - Port Scanning: `sudo hping3 --scan 1-1023 -S <target-IP>`

### packETH
- **Description**: A GUI-based packet crafting and sending tool.
- **Capabilities**:
  - Craft packets for various protocols (Ethernet, ARP, IP, TCP, UDP).
  - Modify any field of the protocol headers.
  - Send packets at a defined rate or in bursts.

- **Usage**:
  - Set protocol and header fields using GUI.
  - Define payload or load data from a file.
  - Send packets directly or save for later use.

### fragroute
- **Description**: Intercepts, modifies, and rewrites egress traffic for a specified host.
- **Capabilities**:
  - Delay packets, duplicate, fragment, or reorder before sending.
  - Configure via simple script to simulate network conditions.

- **Example Configuration (`frag.conf`)**:
```
delay
random
1 dup
last
30%
ip_chaff
dup
ip_frag
128
new
tcp_chaff
null
16
order
random
print
```



- **Usage**:
- Run against a target: `fragroute -f /path/to/frag.conf <target-IP>`

## Practical Usage Examples

- **Testing Firewall Rules**:
- Use `hping3` to craft packets with specific flags set to test how firewalls respond to various types of traffic.

- **Network Scans**:
- Employ `hping3` or `packETH` to perform scans, identifying open ports and assessing network security postures.

- **Simulating Attacks**:
- Use `fragroute` to manipulate packet traffic in real-time, simulating network attacks and testing IDS/IPS effectiveness.

- **Traffic Analysis**:
- Craft packets with `packETH`, sending them through the network to analyze how devices and applications respond.


