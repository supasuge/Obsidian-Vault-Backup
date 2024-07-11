 Windows systems present a vast attack surface. Just some of the ways that we can escalate privileges are:

| Term                                 | Term                                  |     |
| ------------------------------------ | ------------------------------------- | --- |
| Abusing Windows group privileges     | Abusing Windows user privileges       |     |
| Bypassing User Account Control       | Abusing weak service/file permissions |     |
| Leveraging unpatched kernel exploits | Credential theft                      |     |
| Traffic Capture                      | and more.                             |     |

# Useful Tools

---

There are many tools available to us to assist with enumerating Windows systems for common and obscure privilege escalation vectors. Below is a list of useful binaries and scripts, many of which we will cover within the coming module sections.

|Tool|Description|
|---|---|
|[Seatbelt](https://github.com/GhostPack/Seatbelt)|C# project for performing a wide variety of local privilege escalation checks|
|[winPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS)|WinPEAS is a script that searches for possible paths to escalate privileges on Windows hosts. All of the checks are explained [here](https://book.hacktricks.xyz/windows/checklist-windows-privilege-escalation)|
|[PowerUp](https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1)|PowerShell script for finding common Windows privilege escalation vectors that rely on misconfigurations. It can also be used to exploit some of the issues found|
|[SharpUp](https://github.com/GhostPack/SharpUp)|C# version of PowerUp|
|[JAWS](https://github.com/411Hall/JAWS)|PowerShell script for enumerating privilege escalation vectors written in PowerShell 2.0|
|[SessionGopher](https://github.com/Arvanaghi/SessionGopher)|SessionGopher is a PowerShell tool that finds and decrypts saved session information for remote access tools. It extracts PuTTY, WinSCP, SuperPuTTY, FileZilla, and RDP saved session information|
|[Watson](https://github.com/rasta-mouse/Watson)|Watson is a .NET tool designed to enumerate missing KBs and suggest exploits for Privilege Escalation vulnerabilities.|
|[LaZagne](https://github.com/AlessandroZ/LaZagne)|Tool used for retrieving passwords stored on a local machine from web browsers, chat tools, databases, Git, email, memory dumps, PHP, sysadmin tools, wireless network configurations, internal Windows password storage mechanisms, and more|
|[Windows Exploit Suggester - Next Generation](https://github.com/bitsadmin/wesng)|WES-NG is a tool based on the output of Windows' `systeminfo` utility which provides the list of vulnerabilities the OS is vulnerable to, including any exploits for these vulnerabilities. Every Windows OS between Windows XP and Windows 10, including their Windows Server counterparts, is supported|
|[Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)|We will use several tools from Sysinternals in our enumeration including [AccessChk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk), [PipeList](https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist), and [PsService](https://docs.microsoft.com/en-us/sysinternals/downloads/psservice)|

We can also find pre-compiled binaries of `Seatbelt` and `SharpUp` [here](https://github.com/r3motecontrol/Ghostpack-CompiledBinaries), and standalone binaries of `LaZagne` [here](https://github.com/AlessandroZ/LaZagne/releases/). It is recommended that we always compile our tools from the source if using them in a client environment.

Note: Depending on how we gain access to a system we may not have many directories that are writeable by our user to upload tools. It is always a safe bet to upload tools to `C:\Windows\Temp` because the `BUILTIN\Users` group has write access


## Situational Awareness
Network Information
- Interface(s), IP Addresses, DNS Information
```cmd
ipconfig /all
```

- ARP Table
```cmd
arp -a
```

- Routing Table
```cmd
route print
```

#### Enumerating Protections
Most environments will have anti-virus/Endpoint detection and response services running to monitor.

In a real-world engagement, the client will likely have protections in place that detect the most common tools/scripts (including those introduced in the previous section). There are ways to deal with these, and enumerating the protections in use can help us modify our tools in a lab environment and test them before using them against a client system. Some EDR tools detect on or even block usage of common binaries such as `net.exe`, `tasklist`, etc. Organizations may restrict what binaries a user can run or immediately flag suspicious activities, such as an accountant's machine showing specific binaries being run via cmd.exe. Early enumeration and a deep understanding of the client's environment and workarounds against common AV and EDR solutions can save us time during a non-evasive engagement and make or break an evasive engagement.


#### Check Windows Defender Status

```powershell
PS C:\htb> Get-MpComputerStatus
```


#### List AppLocker Rules

```powershell
PS C:\htb> Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

#### Test AppLocker Policy

```powershell
PS C:\htb> Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```


