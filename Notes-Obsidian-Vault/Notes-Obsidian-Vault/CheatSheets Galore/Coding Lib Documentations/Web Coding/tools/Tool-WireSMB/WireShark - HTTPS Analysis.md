## Table of Contents

- [Overview](#overview)
    - [Protocol](#Protocol)

# Overview

### Protocol
1. Client and server agree on a protocol version
2. Client and server select a cryptographic algorithm
3. The Client and server can authenticate to each other (*Optional*)
4. Create a secure tunnel with a public key


Begin analyzing HTTPS by looking at packets for the handshake between the client and the server. Below is a `Client Hello` packet showing the SSLv2 Record Layer, Handshake Type, and SSL Version.
![[Pasted image 20231125211632.png]]

*NEXT* is the Client Key Exchange packet, this part of the handshake will determine the public key to use to encrypt further messages between the Client and Server

![[Pasted image 20231125211823.png]]
Notable:
- `SSLv3 Record Layer: Handshake Protocol: Client Key Exchange`
- `Handshake Type: Client Key Exchange (16)`
- `SSLv3 Record Layer: Change Cipher Spec Protocol: Change Cipher Spec`
- `Version: SSL 3.0 (0x300)`
- `SSLv3 Record Layer: Handshake protocol: Encrypted handshake message`

In the next packet, the server will confirm the public key and create the secure tunnel, all traffic after this point will be encrypted based on the agreed-upon specifications listed above.

![](https://assets.tryhackme.com/additional/wireshark101/41.png)  

  

The traffic between the Client and the Server is now encrypted and you will need the secret key in order to decrypt the data stream being sent between the two hosts.

![](https://assets.tryhackme.com/additional/wireshark101/42.png)


We can confirm from the packet details that the Application Data is encrypted. You can use an RSA key in Wireshark in order to view the data unencrypted. In order to load an RSA key navigate to Edit > Preferences > Protocols > TLS >  [+] . If you are using an older version of Wireshark then this will be SSL instead of TLS. You will need to fill in the various sections on the menu with the following preferences:

IP Address: 127.0.0.1

Port: start_tls

Protocol: http

Keyfile: RSA key location

  

![](https://assets.tryhackme.com/additional/wireshark101/45.png)  

  

Now that we have an RSA key imported into Wireshark, if we go back to the packet capture we can see that the data stream is now unencrypted.


Zerologon PCAP Overview

We have gathered PCAP files from a recent Windows Active Directory Exploit called Zerologon or CVE-2020-1472. The scenario within the PCAP file contains a Windows Domain Controller with a private IP of 192.168.100.6 and an attacker with the private IP of 192.168.100.128. Let's walk through the steps of analyzing the PCAP and coming to a hypothesis of the events that happened.

  

![](https://assets.tryhackme.com/additional/wireshark101/49.png)  

  

Identifying the Attacker

Immediately upon opening the PCAP file we see some things that may be out of the ordinary. First, we see some normal traffic from OpenVPN, ARP, etc. We then start to identify what would be known as unknown protocols in this case DCERPC and EPM.
Looking at the packets we see that 192.168.100.128 is sending all of the requests, so we can assume that the device is the attacker. We can continue looking at packets coming from this IP to narrow down our hunt.

  

Zerologon POC Connection Analysis

![](https://assets.tryhackme.com/additional/wireshark101/50.png)  

  

We can set a filter for the src of the IP that we believe to be suspicious. When analyzing PCAPS we need to be aware of IOCs or Indicators of Compromise particular exploits may have with them. This is known as Threat Intelligence, which is out of the scope of this room; I recommend that after completing this room if you're interested more then do your own research on the topic. In this case, if we had background knowledge of the Zerologon exploit, we would know that the exploit uses multiple RPC connections, and DCERPC requests to change the machine account password, which could be verified with the PCAP.