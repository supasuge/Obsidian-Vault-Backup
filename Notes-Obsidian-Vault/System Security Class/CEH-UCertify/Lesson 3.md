## Table of Contents

- [Security Foundations](#security\foundations)
          - [Triad of Information Security](#Triad\of\Information\Security)
  - [Organizing Your Protections(3.4)](#Organizing\Your\Protections(3.4))
          - [The ATT&CK Framework](#The\ATT&CK\Framework)
  - [3.5 Security Technology](#3.5\Security\Technology)
      - [Firewalls](#Firewalls)
        - [Packet Filters](#Packet\Filters)
- [Security Technology](#security\technology)
  - [Firewalls](#Firewalls)
    - [Types of Firewalls](#Types\of\Firewalls)
  - [Intrusion Detection Systems (IDS)](#Intrusion\Detection\Systems\(IDS))
  - [Intrusion Prevention Systems (IPS)](#Intrusion\Prevention\Systems\(IPS))
  - [Endpoint Detection and Response (EDR)](#Endpoint\Detection\and\Response\(EDR))
  - [Security Information and Event Management (SIEM)](#Security\Information\and\Event\Management\(SIEM))
    - [Key Takeaways:](#Key\Takeaways:)
- [Security Concepts Cheatsheet](#security\concepts\cheatsheet)
  - [Defense in Depth](#Defense\in\Depth)
  - [Defense in Breadth](#Defense\in\Breadth)
  - [Defensible Network Architecture (DNA)](#Defensible\Network\Architecture\(DNA))
  - [Logging and Auditing](#Logging\and\Auditing)
    - [Key Takeaways:](#Key\Takeaways:)
    - [1. iptables Policy Settings](#1.\iptables\Policy\Settings)
    - [2. iptables State Rules](#2.\iptables\State\Rules)
    - [3. mod-security Rule](#3.\mod-security\Rule)
    - [4. Snort Rules](#4.\Snort\Rules)

# Security Foundations
- Elements of information security
- Risk management
- Information assurance

###### Triad of Information Security
**CIA**
- Confidentiality
- Integrity
- Availability
	- Non-Repudation
![[Pasted image 20240121091851.png]]
Must know *what* you are protecting in order to identify potential risks and associated potential losses can help with decision making around where to expend limited resources - budget, staffing, capital expenditures, and so on.

Once policies are in place, decisions can be made about how to best protect information resources. Beyond stagging, there is technology that can be brought to bear. There are a number of devices that can be palced into a network to help with information protection. 

**Static Confidentiality**
Protecting data that is considered "at rest", probablty stored on disk and not beinig used or manipulated. 

**Dynamic Confidentiality**
Protecting data that is moving, or "in motion". may include you web browser asking for and then retrieving information from a web server. As the data is being transmistted, it is in motion. it's this transmission that makes it dynamic and not necessarily that it is being altered, though data being sent from one place to another could definitely be experiencing alteration through interaction with the user and the application.

**Breaches of confidentiality:**
- *Possession (or Control)*: If you had mistakenly handed the external drive mentioned earlier to a friend, thinkin you were handing them back their drive to look at it, the data on it would be subject to a breach of confidentiality. 
- *Authenticity:* this is sometimes referred to as non-repudiation. I.e., the source of the data or document is what it purpots to be. (Digital Signatures).
- *utility*: Let's say you have that external drive, it's many years later, and it's been in a drawer for a long time. Drive may be unusable because interfaces on your PC have changed. 


Information assurance is about handling risk. This means not just identifying risk and how to categorize it, but also making business decisions about how to handle risk.
- *Acceptance*: Risk acceptance is an informed process where the business fully understands the nature of the risk, meaning the impact (amount of money the actualized risk will incur) and probability, and chooses not to do anything about the risk. This means if the risk is actualized, the business will be responsible for all the losses and costs associated with it, as well as any other damage that may result. This approach means nothing else is done to the cirumstances related to the risk identified.
- *Transference*
- *Mitigation*
- *Avoidance*

**Race Condition**: Programmatic situation where one process or thread is writing data while another process or thread is reading data. If they are not tightly in sync, it's possible for the data to be read before it's written. It may also be possible to manipulate the data in between writing and reading. This is a synchronization problem.


## Organizing Your Protections(3.4)
###### The ATT&CK Framework
- **Reconnaissance:** The attacker is gathering information about the target in this stage. They may be searching through open sources where data is freely available. They may also be using sources they have access to. Additionally, they may be performing scanning or performing phishing techniques to trick victims to give up data.
- **Resource Devlopment**: The attacker has to do a lot of work to create an infrastructure they will use to launch attacks from. This may include registering domain names, creating accounts, acquiring systems (by compromising other victims and using their computing infrastructure). They may aslo need to acquire tools they don't have themselves. This could be renting or purchasing toolset that may be available through criminal channels.
- **Initial Access**: The attacker here compromises the first systems. They may compromise an existing public-facing application or website. They may use phishing to get malware installed on a target system. They are getting their inital foothold into the organization in this stage. Any technique that gives them access, even if it's simple as making use of a compromised set of credentials, falls into this stage.
- **Execution:** There are a lot of techniques an attacker may use to get code executed. The attacker doesn't always have direct interactive access to the system, meaning they can't just double click an icon or run a command-line program. They may rely on othe techniques to get their code executed. There are a lot of ways of doing this, including using scheduled tasks, interprocess communication, system services, or operating system-specific techniques like using the Windows Management Instrumentstation(WMI) system.
- **Persistence:** With persistence, the attacker is trying to maintain consistent access to the system/environment they have compromised. This is another case where system services are useful, though scheduled tasks are also useful as are system and use configurations through techniques like registry keys on Windows Systems. Linux systems may use startup files such as `.bashrc` to get programs executed when a user  logs in.
- **Privilege Escalation:** With privilege escalation, attackers are trying to get the highest level of privileges they can. Most users don't have much in the way of access to their systems. They can get to their data and run programs, but they can't do useful things like install systems services or extract system memory. This is where getting additional privileges is helpful. This may be done by injecting code into an existing process that already has admin privileges, they may get a service to run at boot time, among many other possible PrivEsc TTP's.
- **Defense Evasion**:
- **Credential Access**
- **Discovery**
- **Lateral Movement**
- **Collection**
- **Command and Control**
- **Exfiltration**
- **Impact**

## 3.5 Security Technology
#### Firewalls
The firewall is a traditional security device in a network. Just saying that a firewall is in place though, doesn't really explaiin what is happening because there are several types of firewalls running up the network stack.

##### Packet Filters
At a basic level, a firewall is a packet filter. Lots of devices offer the capability to filter packets, which is sometimes accomplished with access control lists. Routers and switches will often have the ability to perform packet filtering. This packet filtering is sometimes implemented as a access control list. Packet filters make determinations about the disposition of packets based on protocol, ports, and addresses. Ports and addresses can be filtered based on both source and destination.

Packets can be dropped, meaning they don't get forwarded on to the destination, and there is also no response message sent to the originating system. They can also be rejected, meaning they won't be sent to the destination, but the rejecting device, if it is following protocol specifications. will send an ICMP error message to the sender indicating that the destination is unreachable. This is not only the polite approach, since drops will incur retransmits; it's also generally considered the correct approach from the protocol standpoint. However, when it comes to system security, dropping messages may just make more sense. If messages just get dropped, it's unclear to the sending system what happened. The target system is essentially in a black hole.

You may have run across packet filters if you run Linux systems. While the host‐based firewall included in most Linux distributions has other capabilities, it can also function as a basic packet filter. You can set a policy on different chains with the iptables firewall that is in the Linux kernel. As an example, the following lines show running iptables to set a default deny policy on the INPUT, OUTPUT, and FORWARD chains, which are collections of rules applied to specific message flows.  
  
```
[iptables Policy Settings]  
iptables -P INPUT DROP  
iptables -P OUTPUT DROP  
iptables -P FORWARD DROP
```



**Stateful Filtering**  
Not long after the initial development of packet filters came the development of stateful firewalls. The first stateful firewall was developed in the late 1980s, just like packet filters were. These are firewall types we should know well because they have been around for about three decades at this point. This does not mean, though, that these stateful filters have been in use all that time.

A stateful firewall keeps track of the state of messages within the conversation between client and server. This means the firewall has to have a state table, so it knows about all of the traffic flows passing through it. In the case of the Transmission Control Protocol (TCP), it's theoretically easier since the flags tell the story when it comes to the state of the traffic flow. A message that has just the SYN flag turned on is a NEW connection. It remains in this state until the three‐way handshake has been completed. At that point, the state of the flow becomes ESTABLISHED. In some cases, you may have message flows that are RELATED. As an example, the File Transfer Protocol (FTP) will sometimes originate connections from the inside to the outside, meaning from the server to the client. In this case, the server‐to‐client connection for transferring the file is related to the control connection from the client to the server.

Even with TCP, the flags don't tell the whole story. After all, it would be easy enough to send messages with the correct flags set to get through a firewall that was only looking at the flags to determine what the state of the communication is. The User Datagram Protocol (UDP) has no state that is inherent to the protocol. This makes it impossible to look at any flags or headers to infer state. Stateful firewalls don't just look at the flags or headers, however. They keep track of all the communication streams so they aren't relying on the protocol. They watch messages coming in and going out and note them along with their directionality to determine what the state is. This means the firewall knows which end is the client and which end is the server.  
  
When you have a stateful firewall, you not only can make decisions based on the ports and addresses, you can also add in the state of a connection. For example, you can see a pair of iptables rules in the following code listing that allow all connections that are NEW or ESTABLISHED into port 22, which is the Secure Shell (SSH) port. Additionally, connections that are established are allowed out on interface eth0. With a default deny policy, new connections won't be allowed out of the interface.  
  

```
[iptables State Rules]  
iptables -A INPUT -i eth0 -p tcp --dport 22 -m state --state \  
NEW,ESTABLISHED -j ACCEPT  
iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state --state ESTABLISHED \  
-j ACCEPT
```

**Deep Packet Inspection**  
One of the biggest issues when it comes to firewalls is that they allow known services through. A firewall will allow connections to a web server through. This means attack traffic to the web server will just be let through the firewall. Attack traffic that uses the application against itself will set up connections just like any other client. From the standpoint of the packet filters and stateful filtering, the messages will pass through. Because of that, we need to go deeper. Just looking at the protocol headers is insufficient, because everything will look correct and legal in the headers. We need to start looking at higher layers of the stack.

A deep packet inspection (DPI) firewall looks beyond the headers and into the payload of the packet. With this approach, it's easier to identify malware and other inbound attacks. A DPI firewall would require signatures that it should look for in the packet to determine whether something is going to be malicious so it can block the traffic from coming into the network. To do this, the firewall has to parse the entire message before it can make any determinations. This means it has to have at least the entire packet, meaning any fragmentation at the IP layer has to arrive and be reassembled. In some cases, it may need the entire stream, whether that's UDP or TCP. This certainly means a little latency on the arrival of messages.

**Application Layer Firewalls**  
There are application layer firewalls in addition to the DPI firewalls. While these firewalls also inspect the packet, they commonly are specific to a particular protocol. For example, in voice over IP (VoIP) networks, a device called a _session border controller_ (SBC) can be used. This is a device that understands the VoIP protocols—commonly either H.323 or the Session Initiation Protocol (SIP). As such, it not only can make determinations about the validity of the messaging but also can open up dynamic pinholes to allow the Real‐time Transport Protocol (RTP) media messages through, since they would be over different ports and protocols than the signaling messages would be.  
  
An SBC would be an example of an application layer firewall, since it has the capability of making decisions about allowing traffic through. It makes these decisions based on understanding the application layer protocol and common state flows of that protocol.  
  
Another common application layer firewall would be a web application firewall (WAF). The WAF uses a set of rules to detect and block requests and responses. Given the number of web‐based attacks, keeping up with these rules can be challenging. While there are several commercial WAFs, there is also ModSecurity, which is an open source module that can be used with Apache web servers. The rules can get complicated, and you can see an example in the following code listing.

```
[mod‐security Rule]  
        SecRule RESPONSE_BODY "@rx (?i)(?:supplied argument is not a valid  
MySQL|Column count doesn't match value count at row|mysql_fetch_array\(\)|on  
MySQL result index|You have an error in your SQL syntax;|You have an error in  
your SQL syntax near|MySQL server version for the right syntax to  
use|\[MySQL\]\[ODBC|Column count doesn't match|Table '[^']+' doesn't  
exist|SQL syntax.*MySQL|Warning.*mysql_.*|valid MySQL result|MySqlClient\.)"  
\  
             "capture,\  
             setvar:'tx.msg=%{rule.msg}',\  
             setvar:tx.outbound_anomaly_score=+%{tx.critical_anomaly_score},\  
             setvar:tx.sql_injection_score=+%{tx.critical_anomaly_score},\  
             setvar:tx.anomaly_score=+%{tx.critical_anomaly_score},\  
             setvar:tx.%{rule.id}-OWASP_CRS/LEAKAGE/ERRORS-  
             %{matched_var_name}=%{tx.0}"
```


# Security Technology 

## Firewalls

### Types of Firewalls

1. **Packet Filters:** 
   - Basic level, filters packets based on protocol, ports, addresses.
   - Actions: drop (silent), reject (with ICMP error), accept.
   - Commonly used in routers, switches, and Linux systems (e.g., iptables).

2. **Stateful Filtering:**
   - Tracks state of messages in a conversation (e.g., TCP states: NEW, ESTABLISHED, RELATED).
   - Uses state tables to monitor traffic flows.
   - More advanced than packet filters, allows for rules based on connection state.

3. **Deep Packet Inspection (DPI):**
   - Inspects packet payloads, not just headers.
   - Identifies malware and sophisticated attacks.
   - Can be limited by encrypted traffic.

4. **Application Layer Firewalls:**
   - Specific to a particular protocol (e.g., SBC for VoIP, WAF for web servers).
   - Makes decisions based on application layer protocols.
   - Examples: Session Border Controllers (SBCs), Web Application Firewalls (WAFs).

5. **Unified Threat Management (UTM):**
   - Consolidates multiple security functions (firewall, IDS/IPS, antivirus).
   - Single point of security management, but a single point of failure.

## Intrusion Detection Systems (IDS)

1. **Host-based IDS:**
   - Monitors activities on a local system.
   - Watches for changes in critical system files and logs.

2. **Network IDS:**
   - Monitors network traffic, not just a single host.
   - Can be placed at strategic points in a network.
   - Utilizes rules to generate log messages.
   - Examples: Snort (with community rules).

## Intrusion Prevention Systems (IPS)

- An extension of IDS; placed inline in network traffic.
- Actively blocks or rejects malicious packets.
- Uses dynamic rules based on packet content.

## Endpoint Detection and Response (EDR)

- Software suite for security operations.
- Functions include anti-malware, log investigation, remote system assessment.
- Capabilities: artifact collection, process listings, memory dumps.
- Can perform host isolation for containment.

## Security Information and Event Management (SIEM)

- Advanced log management with correlation and analysis.
- Aids in visualizing and understanding security data.
- Example: Elastic Stack (Elasticsearch, Logstash, Kibana).
- Central to a Security Operations Center (SOC).

### Key Takeaways:

- Firewalls have evolved from basic packet filters to sophisticated application-layer gateways.
- IDS and IPS are critical for detecting and preventing intrusions, with different focuses.
- EDR tools provide comprehensive endpoint security and response capabilities.
- SIEM systems are central to modern security operations, enabling data correlation and analysis.

![[Pasted image 20240124152443 1.png]]


![[Pasted image 20240124152833 1.png]]




# Security Concepts Cheatsheet

## Defense in Depth
- **Concept**: Layered security approach, like Minas Tirith's concentric walls in "Lord of the Rings".
- **Objective**: Delay attackers, create artifacts for detection, ideally preventing access to sensitive information.
- **Implementation**:
  - **Physical Controls**: Prevent unauthorized physical access.
  - **Technical Controls**: Firewalls, IDS, SIEMs, IAM, antivirus, software/hardware to prevent unauthorized use.
  - **Administrative Controls**: Policies, standards, procedures (hiring standards, data handling policies).

## Defense in Breadth
- **Focus**: Holistic risk evaluation, considering human factors.
- **Strategy**:
  - Beyond prevention, assume potential breach, prepare for response.
  - Emphasize detection, historical data, incident response teams.
  - Promote cross-team communication, break traditional silos.
  - Integrate security in all organizational aspects (DevSecOps approach).
- **Goal**: Efficient protection and swift response to restore operations.

## Defensible Network Architecture (DNA)
- **Design Philosophy**: Build networks for easy monitoring and control.
- **Key Features**:
  - Network segmentation with firewalls at entry/exit points.
  - Enable monitoring, control over potential attackers' movements.
  - Focus on restricting access and lateral movement within network.

## Logging and Auditing
- **Importance**: Vital for historical data, essential in incident response.
- **Unix/Linux**:
  - Use syslog for system/application logging.
  - Centralized log hosts for log integrity.
- **Windows**:
  - Utilize event subsystem, queryable binary storage.
  - Event Viewer for monitoring and analyzing logs.
- **Auditing**:
  - **Windows**: Security function, monitors success/failure of system events.
  - **Linux**: `auditctl` for detailed monitoring (file, application activities, system calls).
- **Best Practices**:
  - Configurable logging levels.
  - Separate and secure log storage.

### Key Takeaways:

- **Defense in Depth and Breadth**: Essential strategies for comprehensive security, focusing on layered protection and holistic risk management.
- **DNA**: A strategic approach to network design, emphasizing control and monitoring to counteract lateral movements of attackers.
- **Logging and Auditing**: Crucial for tracking activities, ensuring data integrity, and aiding in effective incident response.

Absolutely, let's delve deeper into the key commands and concepts mentioned in the text, focusing on their purpose and application in network security and system management.

### 1. iptables Policy Settings
```bash
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
```
- **Purpose**: These commands set the default policies for the `INPUT`, `OUTPUT`, and `FORWARD` chains in iptables (a Linux firewall).
- **Explanation**: 
  - `iptables -P INPUT DROP`: Drops all incoming packets unless explicitly allowed by a rule.
  - `iptables -P OUTPUT DROP`: Drops all outgoing packets unless explicitly allowed.
  - `iptables -P FORWARD DROP`: Drops all forwarded packets, affecting traffic routed through the system.
- **Application**: Essential for creating a secure baseline where all traffic is denied by default, requiring explicit permissions for specific traffic.

### 2. iptables State Rules
```bash
iptables -A INPUT -i eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -o eth0 -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
```
- **Purpose**: To allow SSH (port 22) traffic, considering connection states.
- **Explanation**: 
  - The first rule allows incoming SSH connections and established connections on interface `eth0`.
  - The second rule allows outgoing traffic for established SSH connections.
- **Application**: These rules are vital in stateful filtering, allowing only valid SSH connections and responses, enhancing security for remote administration.

### 3. mod-security Rule
```apache
SecRule RESPONSE_BODY "@rx (?i)(?:supplied argument is not a valid MySQL|Column count doesn't match value count at row|mysql_fetch_array\(\)|on MySQL result index|You have an error in your SQL syntax;|You have an error in your SQL syntax near|MySQL server version for the right syntax to use|\[MySQL\]\[ODBC|Column count doesn't match|Table '[^']+' doesn't exist|SQL syntax.*MySQL|Warning.*mysql_.*|valid MySQL result|MySqlClient\.)"
```
- **Purpose**: Detects various SQL injection attack signatures in the response body.
- **Explanation**: 
  - This rule uses a regular expression to identify common error messages generated by a failed SQL injection attack.
  - If a response contains these error messages, it likely indicates an attempted SQL injection attack.
- **Application**: Vital for a Web Application Firewall (WAF) to detect and prevent SQL injection attacks, a common and dangerous web application vulnerability.

### 4. Snort Rules
```bash
alert tcp $EXTERNAL_NET any -> $SQL_SERVERS 7210 (msg:"SQL SAP MaxDB shell command injection attempt"; ...; sid:13356; rev:7;)
alert tcp $EXTERNAL_NET any -> $HOME_NET 21064 (msg:"SQL Ingres Database uuid_from_char buffer overflow attempt"; ...; sid:12027; rev:11;)
```
- **Purpose**: These are Snort IDS rules to alert on specific network traffic patterns indicating potential attacks.
- **Explanation**: 
  - The rules specify the traffic type (`tcp`), source and destination (e.g., `$EXTERNAL_NET` to `$SQL_SERVERS`), and the port numbers.
  - The `msg` part describes the nature of the alert.
  - `sid` and `rev` are the rule identifier and revision number, respectively.
- **Application**: These rules are crucial for Intrusion Detection Systems (IDS) to alert administrators about potential malicious activities targeting specific vulnerabilities.
![[Pasted image 20240124153124 1.png]]