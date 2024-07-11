## Table of Contents



We can access Windows Defender Firewall by accessing `WF.msc` in the Run dialogue.

![Accessing Firewall Software](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/0825e4e702f7d4c9e0b5c4df81878fe4.png)  

As mentioned in the [Windows Fundamentals room](https://tryhackme.com/room/windowsfundamentals3xzx), it has three main profiles `Domain, Public and Private`.  The Private profile must be activated with "Blocked Incoming Connections" while using the computer at home. 

View detailed settings for each profile by clicking on Windows Defender Firewall Properties.

Whenever possible, enable the Windows Defender Firewall default settings. For blocking all the incoming traffic, always configure the firewall with a 'default deny' rule before making an exception rule that allows more specific traffic.

**Disable unused Networking Devices**

Network devices like routers, ethernet cards, WiFI adapters etc., enable data sharing between computers. If the device is improperly configured or not being used by the owner, it is recommended to disable the interface so that threat actors cannot access them and use them for data retrieval from the victim's computer.  

To disable the unused Networking Devices, go to the `Control panel > System and Security Setting > System > Device Manager` and disable all the unused Networking devices.

![Disable Unused Networking Devices](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/381409d62e6ebaf2c542849dc941f58c.png)  

  

**Disable SMB protocol**

SMB is a file-sharing protocol exploited by hackers in the wild. The protocol is primarily used for file sharing in a network; therefore, you must disable the protocol if your computer is not part of a network by issuing the [following](https://docs.microsoft.com/en-us/windows-server/storage/file-server/troubleshoot/detect-enable-and-disable-smbv1-v2-v3) command in PowerShell.

Administrator - Windows PowerShell

```shell-session
user@machine$ Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
```


 **Protecting Local Domain Name System (DNS)** 

The domain name system (DNS) is a naming system that translates Fully Qualified Domain Names (FQDN) into IP addresses. If the attacker places himself in the middle, he may intercept and manipulate DNS requests and point them to attacker-controlled systems since DNS replies are neither authenticated nor encrypted.  

The hosts file located in Windows acts like local DNS and is responsible for resolving hostnames to IP addresses. Malicious actors try to edit the file's content to reroute traffic to their command and control server.  

The hosts file is located at `C:\Windows\System32\Drivers\etc\hosts`.

![Accessing Local DNS](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/011d5a2f065508cd740f211aa7a478ad.png)

**Mitigating Address Resolution Protocol Attack**  

The address resolution protocol resolves MAC addresses from given IP addresses saved in the workstations ARP cache. The ARP offers no authentication and accepts responses from any user in the network. An attacker can flood target systems with crafted ARP responses, which point to an attacker-controlled machine and put him in the middle of communication between the targeted hosts.

You can check ARP entries using the command **`arp -a`** in the command prompt.   

Command Prompt

```shell-session
user@machine$ arp -a
```


The table contains MAC addresses in the middle and IP addresses in the left.  If the table includes a MAC mapped to two IPs, you are probably susceptible to an ARP poisoning attack.

To clear the ARP cache and prevent the attack, issue the command `arp -d`.

  

**Preventing Remote Access to Machine**  

Remote access provides a way to connect to other computers/networks even located at a different geographical location for file sharing and remotely make changes to a workstation. Microsoft has developed a Remote Desktop Protocol (RDP) for connecting with other computers. Hackers have exploited the protocol in the past, like the famous [Blue Keep vulnerability](https://en.wikipedia.org/wiki/BlueKeep), to gain unauthorised access to the target system.

We must disable remote access (if not required) by going to `settings > Remote Desktop`. Do not attempt this in VM attached to this room.

![Accessing Remote Desktop](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/340bfd9d93e9a44e2f244fc1d2d302ea.png)



﻿**Trusted Application Store**  

Microsoft Store offers a complete range of applications (games, utilities) and allows downloading non-malicious files through a single click. Malicious actors bind legitimate software with trojans and viruses and upload it on the internet to infect and access the victim's computer. Therefore, downloading applications from the Microsoft Store ensures that the downloaded software is not malicious. 

We can access Microsoft Application Store by typing `ms-windows-store` in the Run dialogue.

![Image for Application Store](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/55413d1781c530e53fa175b3e2fadb84.png)  

  

**Safe App Installation**

Only allow installation of applications from the Microsoft Store on your computer.  

Go to `Setting > Select Apps and Features` and then select `The Microsoft Store only`.   

 ![Safe Application](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/adf2295862b9d01aec5d967aaa9c6433.png)

  

**Malware Removal through Windows Defender Anti Virus**

Windows Defender Anti Virus is a complete anti-malware program capable of identifying malicious programs and taking remedial measures like quarantine. The program used to have an entire Graphical User Interface; however, Windows 10 and newer versions manage the same through Windows Security Centre. Windows Defender primarily offers four main functionalities:

- **Real-time protection** - Enables periodic scanning of the computer.
- **Browser integration** - Enables safe browsing by scanning all downloaded files, etc.
- **Application Guard** - Allows complete web session sandboxing to block malicious websites or sessions to make changes in the computer.
- **Controlled Folder Access** - Protect memory areas and folders from unwanted applications.



