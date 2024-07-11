## Table of Contents

- [Port Scanning Techniques Detailed Cheatsheet](#port\scanning\techniques\detailed\cheatsheet)
  - [Overview](#Overview)
  - [Scanning Techniques and Their Packet Mechanics](#Scanning\Techniques\and\Their\Packet\Mechanics)
    - [1. SYN Scan (Stealth Scan)](#1.\SYN\Scan\(Stealth\Scan))
    - [2. Full Connect Scan](#2.\Full\Connect\Scan)
    - [3. UDP Scan](#3.\UDP\Scan)
    - [4. Xmas Scan](#4.\Xmas\Scan)
    - [5. FIN Scan](#5.\FIN\Scan)
    - [6. ACK Scan](#6.\ACK\Scan)
    - [7. NULL Scan](#7.\NULL\Scan)
    - [Operating System Scan with Nmap](#Operating\System\Scan\with\Nmap)

# Port Scanning Techniques Detailed Cheatsheet

**Date:** 2024-02-05  
**Categories:** Network Security, Penetration Testing  
**Tags:** Port Scanning, nmap, TCP, UDP, SYN Scan, Full Connect Scan  

---
## Overview
Port scanning is a critical technique in network security used to determine open, closed, or filtered ports on a network host. Different types of scans manipulate TCP/UDP packet headers to infer the status of network ports.

---
## Scanning Techniques and Their Packet Mechanics

### 1. SYN Scan (Stealth Scan)
- **Command**: `sudo nmap -sS [target]`
- **Mechanism**: Sends a SYN (synchronize) packet to initiate a TCP connection. If the port is open, the target responds with a SYN-ACK (synchronize-acknowledge) packet. The scanner then sends an RST (reset) packet to terminate the connection before it's fully established.
- **Usage**: Ideal for stealthily mapping open ports without establishing a full connection.
### 2. Full Connect Scan
- **Command**: `nmap -sT [target]`
- **Mechanism**: Completes the TCP three-way handshake process. Sends a SYN packet and, if the port is open, responds to SYN-ACK with an ACK (acknowledge) packet, establishing a full connection, then properly closes it.
- **Usage**: Useful when stealth is not a concern, providing a definitive open/closed status for ports.
### 3. UDP Scan
- **Command**: `nmap -sU [target]`
- **Mechanism**: Sends a UDP packet to each target port. If a port is closed, an ICMP (Internet Control Message Protocol) Port Unreachable message is returned. Open or filtered ports don't respond, or responses are blocked, making it less definitive.
- **Usage**: For identifying open UDP ports, but less reliable due to the nature of UDP and potential ICMP rate limiting.
### 4. Xmas Scan
- **Command**: `nmap -sX [target]`
- **Mechanism**: Sends TCP packets with FIN, PSH, and URG flags set, resembling a lit Christmas tree. Closed ports respond with RST, while open/filtered ports don't respond.
- **Usage**: Used to probe for open/filtered ports in a stealthy manner, effective against systems not adhering strictly to TCP protocol standards.
### 5. FIN Scan
- **Command**: `nmap -sF [target]`
- **Mechanism**: Sends a TCP FIN packet to close a connection. Closed ports reply with RST, while open/filtered ports do not respond.
- **Usage**: A stealthy alternative to SYN scans, useful for bypassing some firewall rules.
### 6. ACK Scan
- **Command**: `nmap -sA [target]`
- **Mechanism**: Sends a TCP ACK packet to check how the target responds to non-synchronized packets. Useful for inferring whether the port is stateful (filtered) or not.
- **Usage**: Primarily used for mapping firewall rulesets, identifying filtered and unfiltered ports.
### 7. NULL Scan
- **Command**: `nmap -sN [target]`
- **Mechanism**: Sends a TCP packet with no flags set. Closed ports respond with RST, while open/filtered ports do not respond.
- **Usage**: Used to identify open/filtered ports, exploiting systems not compliant with TCP standards.
---
###  Operating System Scan with Nmap
  - **Command**: `sudo nmap -O 192.168.1.144 192.168.1.214`





