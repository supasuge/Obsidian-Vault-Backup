## Table of Contents

- [Difference between Network Devices and Endpoint Devices](#difference\between\network\devices\and\endpoint\devices)
  - [Common Threats and Attack Vectors of Network Devices](#Common\Threats\and\Attack\Vectors\of\Network\Devices)
    - [Common Hardening Techniques](#Common\Hardening\Techniques)
      - [General Techniques](#General\Techniques)
- [Hardening Routers, Switches & Firewalls](#hardening\routers,\switches\&\firewalls)
    - [Additional Techniques in an Enterprise Environment](#Additional\Techniques\in\an\Enterprise\Environment)
- [Network Security Protocols](#network\security\protocols)
  - [TLS](#TLS)
    - [SSL/TLS Workflow](#SSL/TLS\Workflow)
  - [SOCKS5 Protocol](#SOCKS5\Protocol)
    - [SOCKS5 Workflow](#SOCKS5\Workflow)
    - [IPsec](#IPsec)
      - [Authentication Header (AH)](#Authentication\Header\(AH))
      - [Encapsulating Security Payload (ESP)](#Encapsulating\Security\Payload\(ESP))
    - [VPN](#VPN)

# Difference between Network Devices and Endpoint Devices

Before proceeding with our actual topic, it is imperative to understand the difference between network devices and endpoint devices. Endpoint devices refer to any device that can generate or consume data on a network, such as Laptops, Desktops, Smartphones, Tablets, Printers, Servers, and IoT Devices. They are typically located at the edge of a network and interact directly with users. The figure below shows the difference between endpoint and network devices based on their functionality, traffic, and configuration.

![difference between endpoint and network device](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/442f322a1e8013f07092337940676a94.png)

## Common Threats and Attack Vectors of Network Devices  

The unprecedented growth in today's Information and Communication Technology (ICT) networks has transformed the world into a global village. On the other hand, it has also given rise to multifarious malicious activities by cyber attackers. As we have already studied, the various network devices are the backbone of modern ICT networks. To ensure confidentiality, integrity, and availability (CIA) in our networks and systems, it is imperative that we implement security hardening measures to these devices. Some common goals of hardening include protection from unauthorised access & attacks/exploits, enforcement of access policies, prevention of data theft, and continuous availability of critical systems.

| **Threat  <br>**                | **Description  <br>**                                                                                                                                                                   | **Attack Vector**                                                                                                                                                                                                                                           |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Unauthorised access**         | Gain unauthorised control of a network device, and then the complete network.                                                                                                           | - Password attacks (brute force, dictionary & hybrid)<br>- Exploit known vulnerabilities, e.g. RCE<br>- Social Engineering/Phishing attack to trick network administrators into disclosing sensitive information such as usernames and passwords of devices |
| **Denial of Service (DoS)**     | Disruption of critical devices and services to make them unavailable to genuine users.                                                                                                  | - Flooding devices with fake requests<br>- Exploiting vulnerabilities in logical or resource handling<br>- Manipulating network packets                                                                                                                     |
| **Man-in-the-Middle Attacks**   | Intercept the network requests between two parties by masquerading as each other to steal sensitive information or alter/manipulate the requests.                                       | - ARP spoofing<br>- DNS spoofing<br>- Rogue access points                                                                                                                                                                                                   |
| **Privilege escalation**        | Gaining higher-level privileges or rights to perform restricted actions, e.g. accessing sensitive information or executing malicious code.                                              | - Weak passwords or use of the same passwords for user and admin accounts<br>- Exploiting vulnerabilities<br>- Misconfigurations                                                                                                                            |
| **Bandwidth theft/ hotlinking** | Linking a bandwidth-intensive resource (image or video) from an external website to its original website, without permission. This can cause increased traffic to the original website. | - Scraping large volumes of data  <br>    <br>- DoS attacks  <br>    <br>- Malware attacks                                                                                                                                                                  |
Q: The device that is used to control and manage network resource is called?
> `Network Device`


Q: A threat vector that includes disruption of critical devices and services to make them unavailable to genuine users is called?
> `Denial Of Service`



### Common Hardening Techniques
#### General Techniques
- **Updating and Patching**: Ensuring the latest version of the Operating system and underlying applications of all devices and systems and installing regular security patches is the core hardening measure. Outdated OS and applications contain vulnerabilities that attackers can exploit
- **Disabling Unnecessary services and ports**: Turn off all unnecessary services and block all ports (physical and virtual) that are not needed for system functionality 
- **Principle of Least Privilege (POLP)**: Restrict users and processes to only the minimum necessary permissions required to perform their functions.
- **Logs Monitoring**: Implement a log monitoring system to monitor for unusual activity or security events.
- **Backup regularly**: Take routine backups of systems and configurations as they can help recover from a security incident or system failure.
- **Enforcing Strong Passwords**: Change default login passwords and use strong passwords that are at least ten characters long with a combination of small letters, capital letters, special characters, and numbers. These types of passwords protect against dictionary and brute-force attacks.
- **Multi-Factor Authentication (MFA)**: MFA is an additional security layer requiring two or more types of identification before accessing the account or system. The two factors are generally something we know (like passwords) and something we have (like biometrics).

# Hardening Routers, Switches & Firewalls 
- **Manage Traffic Rules**: Network devices allow you to create and implement traffic rules that accept/deny network traffic. Ex: We notice that the data of users connected with out network device is being exfiltrated to a command and control server IP address. We can create a rule to block all traffic where the destination IP matches the attacker's command and server IP address. We can create a rule to block all traffic where the destination IP matches the attacker's command and control server. We can add/edit traffic rules through `Network > Firewall > Traffic Rules`, and click `Add` to create a new rule

- **Monitor traffic**: As a network administrator, keeping track of network traffic, like uploads and downloads of data at different intervals, is essential. For example, you have excessive data uploaded from one of the email servers to an unknown IP address. Such alerts enable you to take remedial measures and stop data pilferage timely. Usually, network devices provide real-time graphs to monitor the traffic. We can view real-time traffic statistics through `Status > Realtime Graph > Traffic`


**Note**: Since no client is connected with the network device, you won't see any traffic in the real-time traffic statistics on the target machine. 

- **Configuring port forwarding**: A firewall's port forwarding capability enables inbound traffic from the internet or other sources to be routed to a particular device or service on the internal network. The firewall can send incoming traffic to the appropriate device or service on the internal network by establishing port forwarding rules while blocking any other incoming traffic that does not comply with the rules. This feature helps host applications that need outside access, granting remote control of internal devices. Port forwarding should be used carefully because it can expose internal devices and services to potential security issues if improperly secured and configured. Threat actors could add new rules here for creating connections to external command and control servers. We can configure port forwarding through `Network > Firewall > Port Forwards`, and click the `Add` button. 

![how to carry port forwarding](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/d9f216eb279699879693674eaf1e195d.png)

- **Monitoring scheduled tasks**: It is important to monitor scheduled tasks to confirm that the original scheduled tasks lists are not modified by a threat actor. To add or remove scheduled tasks, which in our case are handled by cron, navigate to `System > Scheduled Tasks`, add the new cron job, and click `Save`. You can learn how to create cron jobs [here](https://phoenixnap.com/kb/set-up-cron-job-linux)

- **Update firmware**: It is essential to update the firmware and installed packages on a regular basis to avoid any know/unknown attacks. We can update the firmware through `System > Software`

### Additional Techniques in an Enterprise Environment  
A network device deployed in an enterprise environment generally provides an increased attack surface for an attacker to launch attacks. As enterprise environments include a variety of devices with different models, makes, and types, there are no definite rules to harden network devices; however, a few important ones are mentioned below:

- **Configuring port security**: This includes limiting the number of MAC addresses registered on a switch port and taking particular action whenever unauthorised access is detected. Enabling port security enables an administrator that data is coming from a valid source and will be forwarded to a legitimate receiver.
- **Preventing ARP spoofing**: ARP spoofing is one of the most common vectors for launching man-in-the-middle attacks on the network. The threat can be mitigated by enabling static ARP tables and implementing MAC address filtering. You can learn more about mitigating ARP spoofing [here](https://tryhackme.com/room/layer2).
- **Preventing rogue DHCP servers**: The attacker creates a spoofed DHCP server that can be later on used for assigning IPs to clients and launching MITM attacks. Mitigation measures to prevent such attacks include configuring static DHCP binding and ensuring no unknown devices are added to a network through network mapping tools. You can learn more about DHCP [here](https://tryhackme.com/room/introtolan).
- **Enabling IPv6**: Unlike IPv4, IPv6 has built-in support of IPsec that can be used to secure network communication and provide confidentiality, integrity, and authenticity. Moreover, this will help in protection against MITM, eavesdropping, and tampering of packets in transit.



# Network Security Protocols

## TLS
![[f1a7e8eaf28b773aab4b5d3ae0f563b6.png]]

### SSL/TLS Workflow

`SSL/TLS` handshake is performed to encrypt the communication between client and server through the following steps:

Note: This is not up-to-date with current updates made to TLS (v1.3), that includes encrypted Hello and Post-Quantum Crypto Standards.

1. **Client Hello Message**: The client sends a hello message to the server; it includes the client TLS version and the cypher suite that the client supports, in addition to random bytes.
2. **Server Hello Message**: The server responds with a hello message, highlighting its certificate, chosen cypher suite and random bytes.
3. **Authentication**: The client authenticates the server’s certificate through the certificate authority that issued it. For example, when we visit [Google](https://www.google.com), Google shares its certificate. The received certificate is verified by our browser, which is pre-installed with the certificates of various certificate authorities.
4. **Premaster Secret**: The client encrypts random bytes with the server’s public key. (The client retrieves the public key from the server’s certificate.)
5. **Decryption of Premaster**: The server decrypts the premaster with its private key.
6. **Session Keys Generated**: The client and the server generate session keys based on client random bytes, random server bytes and premaster secret. Both will arrive at the same results; **this session key is not transmitted**, and encryption and decryption are based on this key.
7. **Ready Messages**: The client and server send a “finished” message using the session key to indicate that the session is ready for transmission. The client and server are now ready to exchange messages over SSL/TLS encrypted connection.


## SOCKS5 Protocol
### SOCKS5 Workflow

- **Client Initiation**
    - Client A connects with the SOCKS5 proxy and sends the first byte (0x05) to the proxy where “5” is the SOCKS version.
    - Client A sends a second byte (0x01). One means authentication is supported.
    - Client A sends the third byte (0x00, 0x01, 0x02, or 0x03); these bytes denote the supported authentication methods and can be of variable length.
- **SOCKS5 Proxy Reply**
    - The proxy sends back a second byte, which is the chosen authentication method by the proxy server.
    - After the initiation packet, client A sends the request packet, which includes BHOST & BPORT numbers.
    - The successful session is established between client A and the proxy. The same steps are involved in the association of client B with the proxy.
- **Data Transfer**
    - After successfully associating both clients with a proxy server, both clients can exchange data and share information that will be routed through the proxy server.



### IPsec

IPsec stands for Internet Protocol Security. In this room, we use IPsec to refer to IPsec-v3. IPsec provides security by adding authentication and protecting the integrity and confidentiality of the network traffic. IPsec uses the following protocols:

1. Authentication Header (AH): Provides authentication and integrity.
2. Encapsulating Security Payload (ESP): Provides authentication, integrity, and confidentiality.
3. Security Association (SA): Is responsible for negotiating the encryption keys and algorithms. One example is Internet Key Exchange (IKE). Discussing SA in more detail is outside the scope of this room.

#### Authentication Header (AH)

Authentication Header (AH): The AH protocol is responsible for the authentication and the integrity of the traffic; however, it cannot protect the confidentiality of the data.

The AH protocol works in two modes, as shown in the figure below:

1. Transport Mode: Provides authentication for the TCP/UDP header and data.
2. Tunnel Mode: Provides authentication for the IP header, TCP/UDP header, and data.

![Figure showing IPsec Authentication Header (AH) in transport mode and in tunnel mode.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/d33c96d4525b25694790be6ff0c31d8e.png)

The AH protocol is suitable if providing authentication and integrity is enough without confidentiality. It is worth mentioning that AH is optional in IPsec-v3; however, it is mandatory to implement in IPsec-v2.

#### Encapsulating Security Payload (ESP)

Encapsulating Security Payload (ESP) provides encryption in addition to authentication and integrity. It works in two modes:

1. Transport Mode: Provides security (confidentiality and integrity) for the TCP/UDP header and data.
2. Tunnel Mode: Provides security (confidentiality and integrity) for the IP header, TCP/UDP header, and data.

![Figure showing IPsec Encapsulating Security Payload (ESP) protocol in transport mode and in tunnel mode](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/f8c38bc132212830e305f2bf19185a55.png)

### VPN

When the TCP/IP protocol was designed, security requirements such as confidentiality and integrity were not a design target. In contrast, availability was the priority as one of the purposes of the Internet is to withstand a nuclear attack, as is evident by the routing protocols adapting quickly when a link goes down. But we need to allow a corporation to use the existing Internet infrastructure to connect its offices securely. The answer lies in setting up a VPN.

A Virtual Private Network (VPN) makes it possible to establish a private connection over a public network. In other words, we can establish a secure connection over an insecure infrastructure.

For instance, in the figure below, we can see a remote office and a remote user connected over a VPN to the main office. A VPN connection requires a VPN client and a VPN server or concentrator. All the traffic between the VPN client and server is encrypted.

![[898973f759258b860a457724055fae7b.png]]



The two most common protocols used to establish VPN connections are:

1. **IPsec**
2. **SSL/TLS**

IPsec’s ESP is a perfect protocol for setting up secure tunnels between different networks or a computer and a network. ESP can provide security and integrity of all data transmitted between two points; moreover, even the IP address can be hidden in tunnel mode. Note that the system must be behind a VPN concentrator for the IP address to be hidden. Cisco VPN systems offer IPsec.

Although SSL was created to secure HTTP traffic, SSL/TLS has found its way to establish secure VPN connections with OpenVPN. Using various tools and libraries built around TLS, OpenVPN offers different authentication and encryption mechanisms to establish VPN connections.

Some older protocols that can be used to establish VPN connections are no longer considered secure. One example is Point to Point Tunneling Protocol (PPTP), which is no longer considered secure.










