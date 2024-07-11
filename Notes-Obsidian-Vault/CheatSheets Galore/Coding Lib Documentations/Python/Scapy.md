## Table of Contents

- [Python Network Programming Cheatsheet (Advanced)](#python\network\programming\cheatsheet\(advanced))
  - [Overview](#Overview)
  - [Scapy Basics](#Scapy\Basics)
  - [Packet Crafting and Sniffing](#Packet\Crafting\and\Sniffing)
  - [Network Discovery](#Network\Discovery)
  - [Packet Analysis](#Packet\Analysis)

# Python Network Programming Cheatsheet (Advanced)

**Title:** Advanced Python Network Programming  
**Date:** 2024-01-31  
**Categories:** Programming, Networking  
**Tags:** Python, Scapy, Advanced Networking, Packet Crafting

---

## Overview

This section extends the basic Python network programming cheatsheet to include advanced libraries like Scapy, which is used for packet crafting and network discovery. Scapy allows for a deeper interaction with network protocols and can be used for tasks like network scanning, packet sniffing, and crafting custom packets.

---

## Scapy Basics

- **Installing Scapy:**
  ```bash
  pip install scapy
  ```

- **Importing Scapy:**
  ```python
  from scapy.all import *
  ```

---

## Packet Crafting and Sniffing

- **Crafting and Sending Packets:**
  ```python
  packet = IP(dst="192.168.1.1") / ICMP()
  send(packet)
  ```

- **Sniffing Packets:**
  ```python
  def packet_callback(packet):
      print(packet.show())

  sniff(filter="icmp", prn=packet_callback, count=10)
  ```

- **Creating a TCP Packet:**
  ```python
  tcp_packet = IP(dst="192.168.1.1") / TCP(dport=80, flags="S")
  send(tcp_packet)
  ```

- **ARP Request:**
  ```python
  arp_request = ARP(pdst='192.168.1.1')
  arp_packet = Ether() / arp_request
  sendp(arp_packet)
  ```

---

## Network Discovery

- **ARP Ping:**
  ```python
  ans, unans = srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst="192.168.1.0/24"), timeout=2)
  ans.summary(lambda s, r: r.sprintf("%Ether.src% %ARP.psrc%"))
  ```

- **Traceroute:**
  ```python
  traceroute(["www.google.com", "www.yahoo.com"], maxttl=20)
  ```

---

## Packet Analysis

- **Analyzing Packet Layers:**
  ```python
  packets = sniff(count=5)
  for packet in packets:
      packet.show()
  ```

- **Filtering Packets:**
  ```python
  packets = sniff(filter="tcp and port 80", count=10)
  for packet in packets:
      packet.show()
  ```

- **Writing and Reading PCAP Files:**
  ```python
  # Writing to a file
  packets = sniff(count=10)
  wrpcap('packets.pcap', packets)

  # Reading from a file
  packets = rdpcap('packets.pcap')
  for packet in packets:
      packet.show()
  ```

---
