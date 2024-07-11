## Table of Contents

  - [Network Security](#Network\Security)
      - [Typical network Security Management Operation is explained in the given table:](#Typical\network\Security\Management\Operation\is\explained\in\the\given\table:)
- [P1](#p1)
  - [Traffic Analysis / Network Traffic Analysis](#Traffic\Analysis\/\Network\Traffic\Analysis)
    - [Conclusion](#Conclusion)

## Network Security
The essential concern of Network Security focuses on two core concepts: authentication, and authorisation. There are a variety of tools, technologies, and approaches to ensure and measure implementations of these two key concepts and go beyond to provide continuity and reliability. network security operations contain three base control levels to ensure the maximum available security management
**Base Network Security Control Levels**
- Physical: Physical security controls prevent unauthorized physical access to networking devices, cables boards, and all linked components.
- Technical: Data security controls prevent unauthorized access to net work data, like installing tunnels and implementing security layers.
- Administrative: Administrative security controls provide consistency in security operations like creating policies, access levels and authentication processes.


**Access Control**
- The starting point of network security. It is a set of controls to ensure authentication and authorization
**Threat Control**
- Detecting and preventing anomalous/,malicious activities on the network. It contains both internal (trusted) and external data probes.

| *Term*  | *Definition*  |
|---|---|
|**Firewall Protection  <br>**|Controls incoming and outgoing network traffic with predetermined security rules. Designed to block suspicious/malicious traffic and application-layer threats while allowing legitimate and expected traffic.|
|**Network Access Control (NAC)** |Controls the devices' suitability before access to the network. Designed to verify device specifications and conditions are compliant with the predetermined profile before connecting to the network.|
|**Identity and Access Management (IAM)**|Controls and manages the asset identities and user access to data systems and resources over the network.|
|**Load Balancing**|Controls the resource usage to distribute (based on metrics) tasks over a set of resources and improve overall data processing flow.|
|**Network Segmentation  <br>**|Creates and controls network ranges and segmentation to isolate the users' access levels, group assets with common functionalities, and improve the protection of sensitive/internal devices/data in a safer network.|
|**Virtual Private Networks (VPN)  <br>**|Creates and controls encrypted communication between devices (typically for secure remote access) over the network (including communications over the internet).|
|**Zero Trust Model**|Suggests configuring and implementing the access and permissions at a minimum level (providing access required to fulfil the assigned role). The mindset is focused on: "Never trust, always verify".|

The key elements of Threat Control:

| Term  | Definition  |
|---|---|
|**Intrusion Detection and Prevention (IDS/IPS)  <br>**|Inspects the traffic and creates alerts (IDS) or resets the connection (IPS) when detecting an anomaly/threat.|
|**Data Loss Prevention (DLP)  <br>**|Inspects the traffic (performs content inspection and contextual analysis of the data on the wire) and blocks the extraction of sensitive data.|
|**Endpoint Protection  <br>**|Protecting all kinds of endpoints and appliances that connect to the network by using a multi-layered approach like encryption, antivirus, antimalware, DLP, and IDS/IPS.|
|**Cloud Security**|Protecting cloud/online-based systems resources from threats and data leakage by applying suitable countermeasures like VPN and data encryption.|
|**Security Information and Event Management (SIEM)  <br>**|Technology that helps threat detection, compliance, and security incident management, through available data (logs and traffic statistics) by using event and context analysis to identify anomalies, threats, and vulnerabilities.|
|**Security Orchestration Automation and Response (SOAR)  <br>**|Technology that helps coordinate and automates tasks between various people, tools, and data within a single platform to identify anomalies, threats, and vulnerabilities. It also supports vulnerability management, incident response, and security operations.|
|**Network Traffic Analysis & Network Detection and Response**|Inspecting network traffic or traffic capture to identify anomalies and threats.|


#### Typical network Security Management Operation is explained in the given table:
**Deployment**
- Device and software installation
- Initial configuration
- Automation
**Configuration**
- Feature configuration
- Initial network access configuration
**Management**
- Security policy implementation
- NAT and VPN implementation
- Threat mitigation
**Monitoring**
- System monitoring
- User activity monitoring
- Threat monitoring   
- Log and traffic sample capturing
**Maintenance**
- Upgrades
- Security updates
- Rule adjustments
- Liscense management
- Configuration updates

# P1
Which Security Control Level contains creating security policies?
*Administrative*

Which access control element works with data metrics
*Load Balancing*

Which technology helps correlate different tool outputs and data sources?
*SOAR*


## Traffic Analysis / Network Traffic Analysis
Method of intercepting, recording/monitoring, and analysing network data and communication patterns to detect and respond to system health issues, network anomalies, and threats. the network is a rich data source, so traffic analysis is useful for security and operational matters. The operational issues cover system availability checks and measuring performance, and the security issues cover anomaly and suspicious activity detection on the network.
- Network Sniffing and Packet Analysis (Covered in [**Wireshark room**](https://tryhackme.com/room/wiresharkthebasics))
- Network Monitoring (Covered in [**Zeek room**](https://tryhackme.com/room/zeekbro))
- Intrusion Detection and Prevention (Covered in [**Snort room**](https://tryhackme.com/room/snort))  
- Network Forensics (Covered in [**NetworkMiner room**](https://tryhackme.com/room/networkminer))
- Threat Hunting (Covered in [**Brim room**](https://tryhackme.com/room/brim))



| **Flow Analysis** | **Packet Analysis** |
| ---- | ---- |
| Collecting data/evidence from the networking devices. This type of analysis aims to provide statistical results through the data summary without applying in-depth packet-level investigation.<br><br>- **Advantage:** Easy to collect and analyse.<br>- **Challenge:** Doesn't provide full packet details to get the root cause of a case. | Collecting all available network data. Applying in-depth packet-level investigation (often called Deep Packet Inspection (DPI) ) to detect and block anomalous and malicious packets.<br><br>- **Advantage:** Provides full packet details to get the root cause of a case.<br>- **Challenge:** Requires time and skillset to analyse. |

Benefits of the Traffic Analysis:

- Provides full network visibility.
- Helps comprehensive baselining for asset tracking.
- Helps to detect/respond to anomalies and threats.

![[Pasted image 20240103094845.png]]

- `10.10.99.62` - This IP is sending malicious packets (Red)
	- IDS/IPS System traffic: Multiple Login Attempts
- `10.10.99.99` - Metasploit Traffic, `PORT: 4444`
![[Pasted image 20240103095154.png]]

Level-2: 3 `destination ports` 
`4444, 7777, 2222`


### Conclusion
In this task we covered the basic foundations of the network security and traffic analysis concepts:
- Network Security Operations
- Network Traffic Analysis










