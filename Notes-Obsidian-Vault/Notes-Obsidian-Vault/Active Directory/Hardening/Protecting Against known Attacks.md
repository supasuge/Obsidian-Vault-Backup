# Table of Contents
- Kerberoasting
- Weak Passwords
- Brute forcing/Remote Access
- Publicly accessible shares
## Kerberoasting
[Kerberoasting](https://tryhackme.com/room/attackingkerberos) is a common and successful post-exploitation technique for attackers to get privileged access to AD. The attacker exploits Kerberos Ticket Granting Service (TGS) to request an encrypted password, and then the attacker cracks it offline through various brute force techniques. These attacks are difficult to detect as the request is made through an approved user, and no unusual traffic pattern is generated during this process. You can prevent the attack by ensuring an additional layer of authentication through MFA or by frequent and periodic Kerberos Key Distribution Centre (KDC) service account password reset. You can learn more about the attack [here](https://microsoft.com/security/blog/2020/08/27/stopping-active-directory-attacks-and-other-post-exploitation-behavior-with-amsi-and-machine-learning/).

## Weak and Easy to Guess Passwords
The easiest target for intruders to breach security is the weak and easy-to-guess old passwords. The best recommendation is to use strong passwords and avoid already known ones. A strong password consists of a combination of uppercase and lowercase letters, numbers, and special characters. There are many tools to help perform password Auditing in AD. You can see a report generated through a free tool on `Desktop > Password-Report.png.`

## Brute Forcing Remote Desktop Protocol 

The intruders or attackers use scanning tools to brute force the weak credentials. Once the brute force is successful, they quickly access the compromised systems and try to do privilege escalation along with a persistent foothold﻿ in the target's computer. The best recommendation is to never expose RDP without additional security controls to the public internet. Continuous audits for scanning attacks or brute-force attempts are also an important step.

## Publically Accessible Share

During AD configuration, some share folders are publicly accessible or left unauthenticated, providing an initial foothold for attackers for lateral movement. You can use the `Get-SmbOpenFile` cmdlet in PowerShell to look for any undesired share on the network and configure access accordingly.

Are you sure your AD is secure from all types of attacks?

Hardening of an AD is a continuous process and demands collective efforts by System Administrators and end-users. There have been various system hardening standards, and we discussed a few in this room. Below is a quick summary of the hardening techniques that will enable System Administrators to harden AD quickly.

![Image for cheatsheet](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/d6681547760638d55d47149cb895b9ac.svg)  

