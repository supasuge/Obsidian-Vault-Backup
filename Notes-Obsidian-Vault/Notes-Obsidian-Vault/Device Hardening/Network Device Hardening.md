## Table of Contents

  - [Defining the difference between a Network Device and an Endpoint Device](#Defining\the\difference\between\a\Network\Device\and\an\Endpoint\Device)
        - [Network Devices](#Network\Devices)
        - [Endpoint Devices](#Endpoint\Devices)
      - [Common Threats and Attack Vectors of Network Devices](#Common\Threats\and\Attack\Vectors\of\Network\Devices)
- [General Techniques](#general\techniques)
    - [Hardening VPNs](#Hardening\VPNs)


## Defining the difference between a Network Device and an Endpoint Device
##### Network Devices

##### Endpoint Devices
- any device that can generate or consume data on a network, such as laptops, Desktops, Smartphones, Tables, Printers, Servers, and IoT Devices.
	- Located at the edge of a network and interact directly with users. 
		Figure below shows the difference between endpoint and network devices based on their functionality, traffic, and configuration.
![[Pasted image 20231210010250.png]]

#### Common Threats and Attack Vectors of Network Devices

**Threat  
****Description  
**Attack Vector**  
**Unauthorised access**  
Gain unauthorised control of a network device, and then the complete network.  

- Password attacks (brute force, dictionary & hybrid)
- Exploit known vulnerabilities, e.g. RCE
- Social Engineering/Phishing attack to trick network administrators into disclosing sensitive information such as usernames and passwords of devices

**Denial of Service (DoS)**  
Disruption of critical devices and services to make them unavailable to genuine users.  

- Flooding devices with fake requests
- Exploiting vulnerabilities in logical or resource handling
- Manipulating network packets

**Man-in-the-Middle Attacks**  
Intercept the network requests between two parties by masquerading as each other to steal sensitive information or alter/manipulate the requests.  

- ARP spoofing
- DNS spoofing
- Rogue access points

**Privilege escalation**  
Gaining higher-level privileges or rights to perform restricted actions, e.g. accessing sensitive information or executing malicious code.  

- Weak passwords or use of the same passwords for user and admin accounts
- Exploiting vulnerabilities
- Misconfigurations

**Bandwidth theft/ hotlinking**  
Linking a bandwidth-intensive resource (image or video) from an external website to its original website, without permission. This can cause increased traffic to the original website.  

- Scraping large volumes of data  
    
- DoS attacks  
    
- Malware attacks


# General Techniques

- Updating and Patching: Ensuring the latest version of the Operating System and underlying applications of all devices and systems and installing regular security patches is the core hardening measure. Outdated OS and applications contain vulnerabilities that attackers can exploit.
- **Disabling unnecessary services and ports**: Turn of necessary services and block all ports (physical and virtual) that are not needed for a specific system functionality that is needed
- `POLP` - Principle of Least Privilege
- **Logs Monitoring**: Implement a log monitoring system to monitor for unusual activity or security events
- **Backup regularly**: Take routine backups of systems and configurations as they help recover from a security incident or system failure
- **Multi-Factor Authentication**: MFA is an additional security layer requiring two or more types of identification before accessing the account or system. The two factors are generally something we know 

Removal/Blocking of Insecure Protocols 

In addition to using secure protocols, removing and blocking access to those insecure protocols is equally essential, which will decrease an attacker's attack surface. Most important are the protocols that transmit data in clear text without encrypting them, like FTP, HTTP, Telnet, SMTP, and more. Moreover, there are inherently secure protocols (e.g. LDAP, RDP, SIPS); however, they can allow attackers to exploit the network if configured incorrectly.


Implementation of Monitoring and Logging Controls

Logging in network devices is essential for detecting and investigating security incidents, identifying performance issues, and complying with regulatory requirements. It provides a record of events and activities on the device, which can be used for troubleshooting, forensic analysis, and auditing purposes. The following techniques are generally used for logging:

- **Syslog**: A protocol to standardise the transfer of log messages, with the purpose of storing and analysing log messages to a central server.
- **SNMP**: Traps a notification sent by a network device to a management system when a predefined event occurs.
- **NetFlow**: A protocol used to collect and analyse network traffic data for monitoring and security analysis.
- **Packet Captures**: Capturing network traffic and storing it for analysis using a tool like Wireshark.


### Hardening VPNs

- Multi-Factor Authentication
- Encryption/TLS(SSL)
	- This is not a "default secure config".... 
		- Many precautions needed.

All VPN Server consist of server-side and client-side configurations through a `config` file.
- System admin's must understand the file's content and edit it as per best security practices
	- `/etc/openvpn/server/server.conf`
	- `sudo nano /etc/openvpn/server/server.conf`
		- **Use AES-256-CBC**
			- Preferably with some sort of strong authentication mechanism involved within as well.
			- Authentication through means of **Transport Layer Security**(TLS), and a secure hashing algorithm (SHA-256). - You can set the auth directive through the following command:
    
```bash
thm@machine$ sudo nano /etc/openvpn/server/server.conf

local MACHINE_IP 

port 1194

proto udp

dev tun

ca ca.crt

cert server.crt

auth SHA256 #use this to change the auth parameter

tls-crypt tc.key

topology subnet
```



- Change Default Settings: Change the usernames and passwords to something unique to reduce the risk of unauthorised access to the gateway
- **Enable Perfect Forward Secrecy(PFS)**: This simply means that OpenVPN generates unique session keys for each session to strengthen the security of the VPN connection. Because of this, even if a hacker successfully obtained a session key, they could not use it to decode more sessions. PFS generates a new set of encryption keys, preventing the possibility of remotely decrypting previously acquired material. 
	- `tls-crypt`: Directive in the OpenVPN configuration file to enable PFS. The `tls-crypt` directive requires a key that can be generated using the command `sudo openvpn --genkey --secret my.key` and should be placed in the same directory on the server.
	- `AES-256-CBC`, `auth`, `SHA-256`
- supports PFS if combined with tls-crypt. The exact configuration is mentioned below:
    
```shell
thm@machine$ sudo nano /etc/openvpn/server/server.conf

local MACHINE_IP 

port 1194

proto udp

dev tun

ca ca.crt

cert server.crt

cipher AES-256-CBC 

auth SHA256 

tls-crypt my.key #use this to change the tls-crypt parameter

tls-version-min 1.2  #use this to change the TLS version

topology subnet
```



















































