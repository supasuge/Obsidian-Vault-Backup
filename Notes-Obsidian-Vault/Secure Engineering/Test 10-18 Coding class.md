## Table of Contents

- [Secure Software by Design: Study Guide](#secure\software\by\design:\study\guide)
  - [Lesson 2: Networking and Security](#Lesson\2:\Networking\and\Security)
    - [Objectives for Learning](#Objectives\for\Learning)
    - [Terminology on Networking](#Terminology\on\Networking)
      - [LAN & WAN](#LAN\&\WAN)
      - [IP Address](#IP\Address)
      - [Subnet Mask](#Subnet\Mask)
      - [Firewall](#Firewall)
    - [Secure Networking](#Secure\Networking)
      - [VPN (Virtual Private Network)](#VPN\(Virtual\Private\Network))
      - [IDS/IPS](#IDS/IPS)
      - [DMZ (Demilitarized Zone)](#DMZ\(Demilitarized\Zone))
    - [How Networks Work](#How\Networks\Work)
    - [Network Devices](#Network\Devices)
      - [Router](#Router)
      - [Switch](#Switch)
      - [Proxy Server](#Proxy\Server)
    - [Protections](#Protections)
      - [Firewalls](#Firewalls)
      - [Antivirus](#Antivirus)
      - [Encryption](#Encryption)
    - [How Apps Communicate](#How\Apps\Communicate)
    - [Network Protocols](#Network\Protocols)
      - [TCP/IP (Transmission Control Protocol/Internet Protocol)](#TCP/IP\(Transmission\Control\Protocol/Internet\Protocol))
      - [TLS (Transport Layer Security) & SSL (Secure Sockets Layer)](#TLS\(Transport\Layer\Security)\&\SSL\(Secure\Sockets\Layer))
    - [Summary](#Summary)
  - [Lesson 4: The Operating System Environment](#Lesson\4:\The\Operating\System\Environment)
    - [Objectives for Learning](#Objectives\for\Learning)
    - [What Is Operating System Security?](#What\Is\Operating\System\Security?)
    - [Linux Operating Systems](#Linux\Operating\Systems)
      - [Definition](#Definition)
      - [Pros](#Pros)
      - [Cons](#Cons)
    - [Common Security Flaws](#Common\Security\Flaws)
      - [Buffer Overflow](#Buffer\Overflow)
      - [Insecure Default Settings](#Insecure\Default\Settings)
      - [Privilege Escalation](#Privilege\Escalation)
    - [Common Terminology](#Common\Terminology)
    - [Best Practices](#Best\Practices)

- `1` (Group Permissions) – Execute only for the group
- `1` (Others Permissions) – Execute only for others

**StakeHolder**
Anyone with an interest in a software system; this can include developers, end users, or administrators. The level of influence or involvement varies from stakeholder to stakeholder.




*SDLC Steps*  -- > 
- `Stage 1: Project Planning. The first stage of SDLC is all about “What do we want?” ...``
- `Stage 2: Gathering Requirements & Analysis. ...``
- `Stage 3: Design. ...``
- `Stage 4: Coding or Implementation. ...``
- `Stage 5: Testing. ...`
- `Stage 6: Deployment. ...``
- `Stage 7: Maintenance.``


# Secure Software by Design: Study Guide

## Lesson 2: Networking and Security

### Objectives for Learning

- Master key terminology related to networking.
- Understand the principles of secure networking.
- Learn how networks operate, including the devices involved.
- Explore how applications communicate over a network.
- Understand important network protocols like TCP/IP, TLS, and SSL.

---

### Terminology on Networking

#### LAN & WAN

- **LAN (Local Area Network)**: A network limited to a small geographic area.
- **WAN (Wide Area Network)**: A network that spans a large geographic area.

#### IP Address

- **Definition**: A unique identifier for a device on a network.

#### Subnet Mask

- **Definition**: Used to divide an IP address into network and host addresses.

#### Firewall

- **Definition**: A network security system that monitors and filters incoming and outgoing network traffic.

---

### Secure Networking

#### VPN (Virtual Private Network)

- **Definition**: Creates a secure network connection over a public network.
- **Use Cases**: Secure data transmission, remote access to internal network.

#### IDS/IPS

- **Intrusion Detection System (IDS)**: Monitors network for signs of malicious activity.
- **Intrusion Prevention System (IPS)**: Actively blocks detected security threats.

#### DMZ (Demilitarized Zone)

- **Definition**: A subnetwork that exposes an organization's external services to a larger untrusted network, usually the internet.

---

### How Networks Work

- **Data Packets**: Information is broken down into smaller packets for transmission.
- **Routing**: Data packets are sent through a network of routers to reach the final destination.

---

### Network Devices

#### Router

- **Definition**: Connects different networks and directs data packets.

#### Switch

- **Definition**: Connects devices within the same local network.

#### Proxy Server

- **Definition**: Serves as an intermediary for requests from clients seeking resources from other servers.

---

### Protections

#### Firewalls

- Types: Hardware and Software Firewalls.
- **Purpose**: To block unauthorized access.

#### Antivirus

- **Purpose**: To scan, detect, and remove viruses.

#### Encryption

- **Purpose**: To protect data during transmission.

---

### How Apps Communicate

- **APIs (Application Programming Interfaces)**: Enable different software applications to communicate.
- **Ports**: Logical endpoints for communication between applications.

---

### Network Protocols

#### TCP/IP (Transmission Control Protocol/Internet Protocol)

- **Definition**: A suite of communication protocols used to interconnect network devices.

#### TLS (Transport Layer Security) & SSL (Secure Sockets Layer)

- **Definition**: Cryptographic protocols designed to provide secure communication.
- **Difference**: TLS is the updated, more secure version of SSL.

---

### Summary

Understanding networking is crucial for secure software design. From mastering basic terminologies like IP addresses and subnets to understanding secure networking practices like using VPNs and firewalls, every aspect is vital. Knowing how data packets move, what devices they pass through, and what protections are in place can greatly aid in creating secure software.

## Lesson 4: The Operating System Environment

### Objectives for Learning

- In-depth understanding of Linux operating systems, including their pros and cons.
- Description and mitigation of common security flaws.
- Familiarization with common terminology and best practices in operating system security.

---

### What Is Operating System Security?

- **Operating System**: A manager for hardware and software resources on a computer. It controls resource usage, access, and provides a means for user interaction.
- **Security Aspect**: Consists of the operating system itself and the network it connects to. Security measures must be in place on both.

---

### Linux Operating Systems

#### Definition

- **Linux**: An open-source Unix-like operating system. It's known for its stability, performance, and security features.

#### Pros

- **Open Source**: Easily customizable.
- **Stable and Robust**: Rarely crashes, suitable for servers.
- **Secure**: Strong user permission model, SELinux, iptables for firewall configurations.

#### Cons

- **Complexity**: Can be difficult for non-technical users.
- **Software Compatibility**: Not all software is available for Linux.
- **Driver Support**: Limited hardware driver support compared to Windows.

---

### Common Security Flaws

#### Buffer Overflow

- **Attack**: Overloading a system buffer, thereby executing arbitrary code.
- **Defense**: Input validation, stack canaries, ASLR (Address Space Layout Randomization).

#### Insecure Default Settings

- **Attack**: Exploiting systems that have not changed default settings (e.g., passwords).
- **Defense**: Always change default settings; establish a secure baseline.

#### Privilege Escalation

- **Attack**: Gaining higher-level privileges on the system.
- **Defense**: Principle of Least Privilege (PoLP), regular system auditing.

---

### Common Terminology

- **Firewall**: A system designed to prevent unauthorized access to or from a private network.
- **Patch**: A set of changes to a computer program or its supporting data designed to update, fix, or improve it.
- **Hardening**: The process of securing a system by reducing its surface of vulnerability.

--

### Best Practices

- **Regular Updates**: Always keep the operating system and software up-to-date.
- **User Training**: Educate users on the importance of security measures like strong passwords.
- **Multi-factor Authentication (MFA)**: Use at least two forms of authentication.


<!--File Name: DE2/items/nav_item.tpl
Area: student
Details: Item question Area
-->





:authority:

www.ucertify.com

:method:

POST

:path:

/app/?func=navigate_items

:scheme:

https

Accept:

*/*

Accept-Encoding:

gzip, deflate, br

Accept-Language:

en-US,en;q=0.5

Content-Length:

395

Content-Type:

application/x-www-form-urlencoded; charset=UTF-8

Cookie:

FINGER_PRINT=11102023; editor_path=editor%2Fv2%2F; testcookie=testvalue; CRN=OAKCC-SSD.AG1; test_view=continious; annotation04gM20AfZs=08bB4%2C0AfZs; 0AfZs-last_mode=t; 0AfZs-last_content_type=q; PHPSESSID=4hfo6f1s8vbtdijlpfei9t566t; mentor_theme=bootstrap4; browserDetection=webkit; 0AfZs-last_content_guid=02PJc

Origin:

https://www.ucertify.com

Referer:

https://www.ucertify.com/app/?func=navigate_items&item_sequence=7

Sec-Ch-Ua:

"Chromium";v="118", "Brave";v="118", "Not=A?Brand";v="99"

Sec-Ch-Ua-Mobile:

?0

Sec-Ch-Ua-Platform:

"Linux"

Sec-Fetch-Dest:

empty

Sec-Fetch-Mode:

cors

Sec-Fetch-Site:

same-origin

Sec-Gpc:

1

User-Agent:

Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36

X-Requested-With:

XMLHttpRequest




