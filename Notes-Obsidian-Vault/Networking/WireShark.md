## Table of Contents



ARP or Address Resolution Protocol is a Layer 2 protocol that is used to connect IP Addresses with MAC Addresses. They will contain REQUEST messages and RESPONSE messages. To identify packets the message header will contain one of two operation codes:

- Request (1)
- Reply (2)

  

Below you can see a packet capture of multiple ARP requests and replies.

![](https://assets.tryhackme.com/additional/wireshark101/21.png)  

  

It is useful to note that most devices will identify themselves or Wireshark will identify it such as Intel_78, an example of suspicious traffic would be many requests from an unrecognized source. You need to enable a setting within Wireshark however to resolve physical addresses. To enable this feature, navigate to View > Name Resolution > Ensure that Resolve Physical Addresses is checked.

Looking at the below screenshot we can see that a Cisco device is sending ARP Requests, meaning that we should be able to trust this device, however you should always stay on the side of caution when analyzing packets.

  

![](https://assets.tryhackme.com/additional/wireshark101/22.png)  

  

ARP Traffic Overview

ARP Request Packets:

We can begin analyzing packets by looking at the first ARP Request packet and looking at the packet details.






