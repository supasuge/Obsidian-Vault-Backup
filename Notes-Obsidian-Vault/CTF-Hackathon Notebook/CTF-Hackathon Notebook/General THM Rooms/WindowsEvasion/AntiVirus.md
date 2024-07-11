## Table of Contents

  - [AV Static Detection](#AV\Static\Detection)
      - [Static Detection](#Static\Detection)
      - [Yara Rules for Static Detection](#Yara\Rules\for\Static\Detection)
    - [Other](#Other)
- [AV Testing and Finger Printing](#av\testing\and\finger\printing)

What is AV software?

Antivirus (AV) software is an extra layer of security that aims to detect and prevent the execution and spread of malicious files in a target operating system.

It is a host-based application that runs in real-time (in the background) to monitor and check the current and newly downloaded files. The AV software inspects and decides whether files are malicious using different techniques, which will be covered later in this room.


Traditional AV software looks for **malware** with predefined malicious patterns or signatures. Malware is harmful software whose primary goal is to cause damage to a target machine, including but not limited to:

- Gain full access to a target machine.
- Steal sensitive information such as passwords.
- Encrypt files and cause damage to files.
- Inject other malicious software or unwanted advertisements.
- Used the compromised machine to perform further attacks such as botnet attacks.

Most AV products share the same common features but are implemented  differently, including but not limited to:

- Scanner
- Detection techniques
- Compressors and Archives
- Unpackers
- Emulators
An AV detection technique searches for and detects malicious files; different detection techniques can be used within the AV engine, including:

- Signature-based detection is the traditional AV technique that looks for predefined malicious patterns and signatures within files.
- Heuristic detection is a more advanced technique that includes various behavioral methods to analyze suspicious files.
- Dynamic detection is a technique that includes monitoring the system calls and APIs and testing and analyzing in an isolated environment.

We will cover these techniques in the next task. A good AV engine is accurate and quickly detects malicious files with fewer false-positive results. We will showcase several AV products that provide inaccurate results and misclassify a file.  

Compressors and Archives

The "Compressors and Archives" feature should be included in any AV software. It must support and be able to deal with various system file types, including compressed or archived files: ZIP, TGZ, 7z, XAR, RAR, etc. Malicious code often tries to evade host-based security solutions by hiding in compressed files. For this reason, AV software must decompress and scan through all files before a user opens a file within the archive.  

PE (Portable Executable) Parsing and Unpackers

Malware hides and packs its malicious code by compressing and encrypting it within a payload. It decompresses and decrypts itself during runtime to make it harder to perform static analysis. Thus, AV software must be able to detect and unpack most of the known packers (UPX, Armadillo, ASPack, etc.) before the runtime for static analysis.

Malware developers use various techniques, such as Packing, to shrink the size and change the malicious file's structure. Packing compresses the original executable file to make it harder to analyze. Therefore, AV software must have an unpacker feature to unpack protected or compressed executable files into the original code.  

Another feature that AV software must have is Windows Portable Executable (PE) header parser. Parsing PE of executable files helps distinguish malicious and legitimate software (.exe files). The PE file format in Windows (32 and 64 bits) contains various information and resources, such as object code, DLLs, icon files, font files, and core dumps.

An emulator is an Antivirus feature that does further analysis on suspicious files. Once an emulator receives a request, the emulator runs the suspect (exe, DLL, PDF, etc.) files in a virtualized and controlled environment. It monitors the executable files' behavior during the execution, including the Windows APIs calls, Registry, and other Windows files. The following are examples of the artifacts that the emulator may collect:

- API calls
- Memory dumps
- Filesystem modifications
- Log events
- Running processes
- Web requests  
    

An emulator stops the execution of a file when enough artifacts are collected to detect malware.

Other common features

The following are some common features found in AV products:

- A self-protection driver to guard against malware attacking the actual AV.
- Firewall and network inspection functionality.
- Command-line and graphical interface tools.
- A daemon or service.
- A management console.




## AV Static Detection
AV can be classified into three main approaches:
- Static Detection
- Dynamic Detection
- Heuristic and Behavioral Detection

#### Static Detection
A static detection technique is the simplest type of Antivirus detection, which is based on predefined signatures of malicious files. Simply, it uses pattern-matching techniques in the detection, such as finding a unique string, CRC (Checksums), sequence of bytecode/Hex values, and Cryptographic hashes (MD5, SHA1, etc.).


In this task, we will be using a signature-based detection method to see how antivirus products detect malicious files. It is important to note that this technique works against known malicious files only with pre-generated signatures in a database. Thus, the database needs to be updated from time to time.

We will use the ClamAV antivirus software to demonstrate how signature-based detection identifies malicious files. The ClamAV software is pre-installed in the provided VM, and we can access it in the following path: c:\Program Files\ClamAV\clamscan.exe. We will also scan a couple of malware samples, which can be found on the desktop. The Malware samples folder contains the following files:



EICAR - test file containing ASCII strings used to test AV softwares effectiveness instead of real malware that could damage your machine. 
1. **Backdoor 1** is a C# program that uses a well-known technique to establish a reverse connection, including creating a process and executing a Metasploit Framework shellcode.
2. **Backdoor 2** is a C# program that uses process injection and encryption to establish a reverse connection, including injecting a Metasploit shellcode into an existing and running process.
3. **AV-Check** is a C# program that enumerates AV software in a target machine. Note that this file is not malicious. We will discuss this tool in more detail in task 6. 
4. **notes.txt** is a text file that contains a command line. Note that this file is not malicious.


ClamAV comes with its database, and during the installation, we need to download the recently updated version. Let's try to scan the Malware sample folder using the clamscan.exe binary and check how ClamAV performs against these samples.


The above output shows that ClamAV software correctly analyzed and flagged two of our tested files (EICAR, backdoor1, AV-Check, and notes.txt) as malicious. However, it incorrectly identified the backdoor2 as non-malicious while it does.

You can run clamscan.exe --debug <file_to_scan>, and you will see all modules loaded and used during the scanning. For example, it uses the unpacking method to split the files and look for a predefined malicious sequence of bytecode values, and that is how it was able to detect the C# backdoor 1. The bytecode value of the Metasploit shellcode used in backdoor 1 was previously identified and added to ClamAV's database.   

However, backdoor 2 uses an encryption technique (XOR) for the Metasploit shellcode, resulting in different sequences of bytecode values that it doesn't find in the ClamAV database. 

While the ClamAV was able to detect the EICAR.COM test file as malicious using the md5 signature-based technique. To confirm this, we can re-scan the EICAR.COM test file again in debug mode (--debug). At some point in the output, you will see the following message:

```javascript
LibClamAV debug: FP SIGNATURE: 44d88612fea8a8f36de82e1278abb02f:68:Win.Test.EICAR_HDB-1  # Name: eicar.com, Type: CL_TYPE_TEXT_ASCII
```


﻿Now let's generate the md5 value of the EICAR.COM if it matches what we see in the previous message from the output. We will be using the sigtool for that:   

Command Prompt  

```shell-session
c:\>"c:\Program Files\ClamAV\sigtool.exe" --md5 c:\Users\thm\Desktop\Samples\eicar.com
44d88612fea8a8f36de82e1278abb02f:68:eicar.com
```

Create Your Own Signature Database 

One of ClamAV's features is creating your own database, allowing you to include items not found in the official ClamAV database. Let's try to create a signature for Backdoor 2, which ClamAV already missed, and add it to a database. The following are the required steps:

1. Generate an MD5 signature for the file.
2. Add the generated signature into a database with the extension ".hdb".
3. Re-scan the ClamAV against the file using our new database.

First, we will be using the sigtool tool, which is included in the ClamAV suite, to generate an MD5 hash of backdoor2.exe  using the --md5 argument.


Generate an MD5 hash  

```shell-session
C:\Users\thm\Desktop\Samples>"c:\Program Files\ClamAV\sigtool.exe" --md5 backdoor2.exe
75047189991b1d119fdb477fef333ceb:6144:backdoor2.exe 
```

As shown in the output, the generated hash string contains the following structure: Hash:Size-in-byte:FileName. Note that ClamAV uses the generated value in the comparison during the scan.

Now that we have the MD5 hash, now let's create our own database. We will use the sigtool tool and save the output into a file using the > thm.hdb as follows,

Generate our new database  

```shell-session
C:\Users\thm\Desktop\Samples>"c:\Program Files\ClamAV\sigtool.exe" --md5 backdoor2.exe > thm.hdb
```

As a result, a thm.hdb file will be created in the current directory that executes the command. 

We already know that ClamAV did not detect the backdoor2.exe using the official database! Now, let's re-scan it using the database we created, thm.hdb, and see the result!

Re-scanning backdoor2.exe using the new database!  

```shell-session
C:\Users\thm\Desktop\Samples>"c:\Program Files\ClamAV\clamscan.exe" -d thm.hdb backdoor2.exe
Loading:     0s, ETA:   0s [========================>]        1/1 sigs
Compiling:   0s, ETA:   0s [========================>]       10/10 tasks

C:\Users\thm\Desktop\Samples\backdoor2.exe: backdoor2.exe.UNOFFICIAL FOUND
```

As we expected, the ClamAV tool flagged the backdoor2.exe binary as malicious based on the database we provided. As a practice, add the AV-Check.exe's MD5 signature into the same database we already created, then check whether ClamAV can flag AV-Check.exe as malicious.


#### Yara Rules for Static Detection
http://virustotal.github.io/yara/

Yara is a tool that allows malware engineers to classify and detect malware. yara uses rule-based detection, so in order to detect new malware, we need to create a new rule. ClamAV can also deal with Yara rules to detect malicious files. the rule will be the same as in our database in the previous section
o create a rule, we need to examine and analyze the malware; based on the findings, we write a rule. Let's take AV-Check.exe as an example and write a rule for it. 

First, let's analyze the file and list all human-readable strings in the binary using the strings tool. As a result, we will see all functions, variables, and nonsense strings. But, if you look closely, we can use some of the unique strings in our rules to detect this file in the future. The AV-Check uses a program database (.pdb), which contains a type and symbolic debugging information of the program during the compiling.

Command Prompt  

```shell-session
C:\Users\thm\Desktop\Samples>strings AV-Check.exe | findstr pdb
C:\Users\thm\source\repos\AV-Check\AV-Check\obj\Debug\AV-Check.pdb
```

We will use the path in the previous command's output as our unique string example in the Yara rule that we will create. The signature could be something else in the real world, such as Registry keys, commands, etc. If you are not familiar with Yara, then we suggest checking the [Yara THM room](https://tryhackme.com/room/yara). The following is Yara's rule that we will use in our detection:

```yara
rule thm_demo_rule {
	meta:
		author = "THM: Intro-to-AV-Room"
		description = "Look at how the Yara rule works with ClamAV"
	strings:
		$a = "C:\\Users\\thm\\source\\repos\\AV-Check\\AV-Check\\obj\\Debug\\AV-Check.pdb"
	condition:
		$a
}
```

Let's explain this Yara's rule a bit more.

- The rule starts with rule thm_demo_rule, which is the name of our rule. ClamAV uses this name if a rule matches.
- The metadata section, which is general information, contains the author and description, which the user can fill.
- The strings section contains the strings or bytecode that we are looking for. We are using the C# program's database path in this case. Notice that we add an extra \ in that path to escape the special character, so it does not break the rule. 
- In the condition section, we specify if the defined string is found in the string section, then flag the file.

Note that Yara rules must store in a .yara extension file for ClamAV to deal with it. Let's re-scan the c:\Users\thm\Desktop\Samples folder again using the Yara rule we created. You can find a copy of the Yara rule on the desktop at c:\Users\thm\Desktop\Files\thm-demo-1.yara.

Scanning using the Yara rule  

```shell-session
C:\Users\thm>"c:\Program Files\ClamAV\clamscan.exe" -d Desktop\Files\thm-demo-1.yara Desktop\Samples
Loading:     0s, ETA:   0s [========================>]        1/1 sigs
Compiling:   0s, ETA:   0s [========================>]       40/40 tasks

C:\Users\thm\Desktop\Samples\AV-Check.exe: YARA.thm_demo_rule.UNOFFICIAL FOUND
C:\Users\thm\Desktop\Samples\backdoor1.exe: OK
C:\Users\thm\Desktop\Samples\backdoor2.exe: OK
C:\Users\thm\Desktop\Samples\eicar.com: OK
C:\Users\thm\Desktop\Samples\notes.txt: YARA.thm_demo_rule.UNOFFICIAL FOUND
```
As a result, ClamAV can detect the AV-Check.exe binary as malicious based on the Yara rule we provide. However, ClamAV gave a false-positive result where it flagged the notes.txt file as malicious. If we open the notes.txt file, we can see that the text contains the same path we specified in the rule.

Let's improve our Yara rule to reduce the false-positive result. We will be specifying the file type in our rule. Often, the types of a file can be identified using magic numbers, which are the first two bytes of the binary. For example, [executable files](https://en.wikipedia.org/wiki/DOS_MZ_executable) (.exe) always start with the ASCII "MZ" value or "4D 5A" in hex.

To confirm this, let's use the [HxD](https://mh-nexus.de/en/hxd/) application, which is a freeware Hex Editor, to examine the AV-Check.exe binary and see the first two bytes. Note that the HxD is already available in the provided VM.
Knowing this will help improve the detection, let's include this in our Yara rule to flag only the .exe files that contain our signature string as malicious. The following is the improved Yara rule:

```yara
rule thm_demo_rule {
	meta:
		author = "THM: Intro-to-AV-Room"
		description = "Look at how the Yara rule works with ClamAV"
	strings:
		$a = "C:\\Users\\thm\\source\\repos\\AV-Check\\AV-Check\\obj\\Debug\\AV-Check.pdb"
		$b = "MZ"
	condition:
		$b at 0 and $a
}
```

In the new Yara rule, we defined a unique string ($b) equal to the MZ as an identifier for the .exe file type. We also updated the condition section, which now includes the following conditions:

1. If the string "MZ" is found at the 0 location, the file's beginning.
2. If the unique string (the path) occurs within the binary.
3. In the condition section, we used the  AND operator for both definitions in 1 and 2 are found, then we have a match. 

You can find the updated rule in Desktop\Files\thm-demo-2.yara. Now that we have our updated Yara rule, now let's try it again.Scanning using the Yara rule  

```shell-session
C:\Users\thm>"c:\Program Files\ClamAV\clamscan.exe" -d Desktop\Files\thm-demo-2.yara Desktop\Samples
```

"C:\Program Files\ClamAV\sigtool.exe" --md5 AV-Check.exe




### Other

The concept of static detection is relatively simple. In this section, we will discuss the different types of detection techniques.  
  

Dynamic Detection  

The dynamic detection approach is advanced and more complicated than static detection. Dynamic detection is focused more on checking files at runtime using different methods. The following diagram shows the dynamic detection scanning flow:
![Dynamic detection flow](https://tryhackme-images.s3.amazonaws.com/user-uploads/5c549500924ec576f953d9fc/room-content/5d0d8cd90a2c4b2ce035af1f3b735563.png)

First method is by monitoring Windows APIs. The detection engine inspect Windows application calls and monitros Windows API calls using Windows [Hooks](https://docs.microsoft.com/en-us/windows/win32/winmsg/about-hooks)


Heuristic and Behavioral Detection  

Heuristic and behavioral detection have become essential in today's modern AV products. Modern AV software relies on this type of detection to detect malicious software. The heuristic analysis uses various techniques, including static and dynamic heuristic methods:

1. Static Heuristic Analysis is a process of decompiling (if possible) and extracting the source code of the malicious software. Then, the extracted source code is compared to other well-known virus source codes. These source codes are previously known and predefined in a heuristic database. If a match meets or exceeds a threshold percentage, the code is flagged as malicious.
2. Dynamic Heuristic Analysis is based on predefined behavioral rules. Security researchers analyzed suspicious software in isolated and secured environments. Based on their findings, they flagged the software as malicious. Then, behavioral rules are created to match the software's malicious activities within a target machine.

The following are examples of behavioral rules: 

- If a process tries to interact with the LSASS.exe process that contains users' NTLM hashes, Kerberos tickets, and more
- If a process opens a listening port and waits to receive commands from a Command and Control (C2) server



# AV Testing and Finger Printing

AV Vendors

Many AV vendors in the market mainly focus on implementing a security product for home or enterprise users. Modern AV software has improved and now combines antivirus capabilities with other security features such as Firewall, Encryption, Anti-spam, EDR, vulnerability scanning, VPN, etc.

It is important to note that it is hard to recommend which AV software is the best. It all comes down to user preferences and experience. Nowadays, AV vendors focus on business security in addition to end-user security. We suggest checking the [AV comparatives website](https://www.av-comparatives.org/list-of-enterprise-av-vendors-pc/) for more details on enterprise AV vendors.  

AV Testing Environment   

AV testing environments are a great place to check suspicious or malicious files. You can upload files to get them scanned against various AV software vendors. Moreover, platforms such as VirusTotal use various techniques and provide results within seconds. As a red teamer or a pentester, we must test a payload against the most well-known AV applications to check the effectiveness of the bypass technique.  

VirusTotal   

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/368287c08435172ac9740a2a68e4e3fa.png)  

VirusTotal is a well-known web-based scanning platform for checking suspicious files. It allows users to upload files to be scanned with over 70 antivirus detection engines. VirusTotal passes the uploaded files to the Antivirus engines to be checked, returns the result, and reports whether it is malicious or not. Many checkpoints are applied, including checking for blacklisted URLs or services, signatures, binary analysis, behavioral analysis, as well as checking for API calls. In addition, the binary will be run and checked in a simulated and isolated environment for better results. For more information and to check other features, you may visit the [VirusTotal](https://www.virustotal.com/) website.  
  

VirusTotal alternatives  

**Important Note:** VirusTotal is a handy scanning platform with great features, but it has a sharing policy. All scanned results will be passed and shared with antivirus vendors to improve their products and update their databases for known malware. As a red teamer, this will burn a dropper or a payload you use in engagements. Thus, alternative solutions are available for testing against various security product vendors, and the most important advantage is that they do not have a sharing policy. However, there are other limitations. You will have a limited number of files to scan per day; otherwise, a subscription is needed for unlimited testing. For those reasons, we recommend you only test your malware on sites that do not share information, such as:

- [AntiscanMe](https://antiscan.me/) (6 free scans a day)
- [Virus Scan Jotti's malware scan](https://virusscan.jotti.org/)  
      
    

Fingerprinting AV software  

As a red teamer, we do not know what AV software is in place once we gain initial access to a target machine. Therefore, it is important to find and identify what host-based security products are installed, including AV software. AV fingerprinting is an essential process to determine which AV vendor is present. Knowing which AV software is installed is also quite helpful in creating the same environment to test bypass techniques.    

This section introduces different ways to look at and identify antivirus software based on static artifacts, including service names, process names, domain names, registry keys, and filesystems.

The following table contains well-known and commonly used AV software.

|   |   |   |
|---|---|---|
|**Antivirus Name**|**Service Name**|**Process Name**|
|Microsoft Defender|WinDefend|MSMpEng.exe|
|Trend Micro|TMBMSRV|TMBMSRV.exe|
|Avira|AntivirService, Avira.ServiceHost|avguard.exe, Avira.ServiceHost.exe|
|Bitdefender|VSSERV|bdagent.exe, vsserv.exe|
|Kaspersky|AVP<Version #>|avp.exe, ksde.exe|
|AVG|AVG Antivirus|AVGSvc.exe|
|Norton|Norton Security|NortonSecurity.exe|
|McAfee|McAPExe, Mfemms|MCAPExe.exe, mfemms.exe|
|Panda|PavPrSvr|PavPrSvr.exe|
|Avast|Avast Antivirus|afwServ.exe, AvastSvc.exe|

SharpEDRChecker

One way to fingerprint AV is by using public tools such as [SharpEDRChecker](https://github.com/PwnDexter/SharpEDRChecker). It is written in C# and performs various checks on a target machine, including checks for AV software, like running processes, files' metadata, loaded DLL files, Registry keys, services, directories, and files.

We have pre-downloaded the SharpEDRChecker from the [GitHub repo](https://github.com/PwnDexter/SharpEDRChecker) so that we can use it in the attached VM. Now we need to compile the project, and we have already created a shortcut to the project on the desktop (SharpEDRChecker). To do so, double-click on it to open it in Microsoft Visual Studio 2022. Now that we have our project ready, we need to compile it, as shown in the following screenshot:

Once it is compiled, we can find the path of the compiled version in the output section, as highlighted in step 3. We also added a copy of the compiled version in the C:\Users\thm\Desktop\Files directory. Now let's try to run it and see the result as follows:



As a result, the Windows Defender is found based on folders and services. Note that this program may be flagged by AV software as malicious since it does various checks and APIs calls. 

C# Fingerprint checks

Another way to enumerate AV software is by coding our own program. We have prepared a C# program in the provided Windows 10 Pro VM, so we can do some hands-on experiments! You can find the project's icon on the desktop (AV-Check) and double-click it to open it using Microsoft Visual Studio 2022. 

The following C# code is straightforward, and its primary goal is to determine whether AV software is installed based on a predefined list of well-known AV applications.

```csharp
using System;
using System.Management;

internal class Program
{
    static void Main(string[] args)
    {
        var status = false;
        Console.WriteLine("[+] Antivirus check is running .. ");
        string[] AV_Check = { 
            "MsMpEng.exe", "AdAwareService.exe", "afwServ.exe", "avguard.exe", "AVGSvc.exe", 
            "bdagent.exe", "BullGuardCore.exe", "ekrn.exe", "fshoster32.exe", "GDScan.exe", 
            "avp.exe", "K7CrvSvc.exe", "McAPExe.exe", "NortonSecurity.exe", "PavFnSvr.exe", 
            "SavService.exe", "EnterpriseService.exe", "WRSA.exe", "ZAPrivacyService.exe" 
        };
        var searcher = new ManagementObjectSearcher("select * from win32_process");
        var processList = searcher.Get();
        int i = 0;
        foreach (var process in processList)
        {
            int _index = Array.IndexOf(AV_Check, process["Name"].ToString());
            if (_index > -1)
            {
                Console.WriteLine("--AV Found: {0}", process["Name"].ToString());
                status = true;
            }
            i++;
        }
        if (!status) { Console.WriteLine("--AV software is not found!");  }
    }
}
```

If we upload the AV-Check program to the [VirusTotal website](https://www.virustotal.com/gui/home/upload) and check the result, surprisingly, VirusTotal showed that two AV vendors (MaxSecure and SecureAge APEX) flagged our program as malicious! Thus, this is a false-positive result where it incorrectly identifies a file as malicious where it is not. One of the possible reasons is that these AV vendors' software uses a machine-learning classifier or rule-based detection method that is poorly implemented. For more details about the actual submission report, see [here](https://www.virustotal.com/gui/file/5f7d3e6cf58596a0186d89c20004c76805769f9ef93dc39e346e7331eee9e7ff?nocache=1). There are four main sections: Detection, Details, Behavior, and Community. If we check the Behavior section, we can see all calls of Windows APIs, Registry keys, modules, and the WMIC query.

In the Detection section, there are Sigma rules that, if a system event during execution is matched (in the sandbox environment), consider the file malicious. This result is likely based on the rules; VirusTotal flagged our program because of the [Process Ghosting technique](https://pentestlaboratories.com/2021/12/08/process-ghosting/), as shown in the following screenshot.


Now let's re-compile the C# program using an x64 CPU and check if the check engines act differently. In our submission attempt this time, three AV vendors' software (Cyren AV is added to the list) flagged the file as malicious. For more details about the actual submission report, look [here](https://www.virustotal.com/gui/file/b092173827888ed62adea0c2bf4f451175898d158608cf7090e668952314e308?nocache=1).
















