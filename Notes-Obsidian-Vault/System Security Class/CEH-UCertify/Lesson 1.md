## Table of Contents

  - [CEH v12](#CEH\v12)
  - [EC-Council](#EC-Council)
  - [Methodology of Ethical Hacking 1.4](#Methodology\of\Ethical\Hacking\1.4)
          - [Reconnaissance and Footprinting](#Reconnaissance\and\Footprinting)
          - [Scanning and Enumeration](#Scanning\and\Enumeration)
          - [Gaining access](#Gaining\access)
          - [Maintaining Access](#Maintaining\Access)
          - [Coverering Tracks](#Coverering\Tracks)
      - [Flashcards (Week 1)](#Flashcards\(Week\1))
- [Pre-Assessment](#pre-assessment)


- 80-90% of attacks use social engineering at some point


## CEH v12
- Intro to ethical hacking
- Footprinting and Recon
- Scanning netoworks
- enumeration
- vulnerability analysis
- System hacking
- Malware Threats
- Sniffing
- Social Engineering
- Denial of service
- Session hijacking
- Evading IDSs, Firewalls configuration, and honeypots
- hacking web servers
- hacking web applications
- SQL Injection
- Hacking wireless networks
- Mobiel Platforms
- IoT and OT Hacking
- Cloud Computing
- Cryptography

![[Pasted image 20240110213703.png]]

**Stage 1**: Reconnaissance.
	This is where the attacker identifie their target as well as potential points of attack. 

![[Pasted image 20240110213810.png]]




## EC-Council
Helps to ensure that hacking is kept ethical by requiring anyone who has obtained the Certified Ethical Hacker certification to agree to a code of conduct. This code of conduct host those who have their CEH certification to a set of standards ensuring that they behave ethically, in service to their employers. They are expected to not to do harm and to work toward improving the security posture rather than doing damage to that posture.


## Methodology of Ethical Hacking 1.4
###### Reconnaissance and Footprinting
Recon is where you gather information about your target.
- Begin to rule out what is in-scope and out-of-scope
Footprinting is gathering information based off of size/appearance and network hosts, blocks, locations, and people.
###### Scanning and Enumeration
Once you have network blocks identified, you will want to identify systems that are accessible within those network blocks; this is the scanning and enumeration stage. Most importantly you will want to identify services running on any available hosts. These services could be used as entry points. The objective is to gain access, that may be possible through exposed network services.
- Gather as much information as possible
###### Gaining access
This is where a shell in gained to further the attack. During this phase, potentially vulnerable services are exploited in an attempt to gain a shell. 
###### Maintaining Access
###### Coverering Tracks

#### Flashcards (Week 1)
What is the purpose of covering the track?
> Removing evidence of your presence in a system

what is the objective of recon/footprinting?
> Determining the size and scope of your test

What is the purpose of the command and control (C2) phase?
> Giving attackers remote access to the infected system














# Pre-Assessment
**How do you authenticate with SNMPv1?**
> Community string

**Sam, a penetration tester, has been actively scanning the client network on which he is doing a vulnerability assessment test. While conducting a port scan, he notices iopen ports in the range of 135 - 139 Which of the following is listening on those ports?**
**135 - RPC**
**137 - NetBIOS/SMB**
> SMB

Which type of social engineering technique are you using if you are leaving bad USB's with malware on them around an office, expecting users to plug them into their systems?
> Baiting


What is one advantage of static analysis over dynamic analysis of malware?
> Static analysis limits your exposure to infection


What would be necessary for a TCP conversation to be considered ESTABLISHED by a stateful firewall?
> 

An attacker has successfully compromised a machine on the network and found a server that is alive on the same network. The attacker tried to ping it but didn't get any response back. Which of the following is resposible for this sccenario?
> ICMP is disabled



























