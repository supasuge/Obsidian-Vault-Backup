## Table of Contents

- [JohnTheRipper - Installation](#JohnTheRipper\-\Installation)
      - [Extract Hash](#Extract\Hash)
  - [Example 2 - Cracking Password Protected Zip Files](#Example\2\-\Cracking\Password\Protected\Zip\Files)
      - [Set Password for a ZIP File](#Set\Password\for\a\ZIP\File)
      - [Extract Hash](#Extract\Hash)
      - [Extract Hash](#Extract\Hash)
  - [Example 4 - Cracking Protected PDF Files](#Example\4\-\Cracking\Protected\PDF\Files)

Various tools exist to help us extract the password hashes from these files in a format that `Hashcat` can understand. The password cracking tool `JohnTheRipper` comes with many of these tools written in C that are available when installing `JohnTheRipper` or compiling it from its source code. They can be viewed [here](https://github.com/magnumripper/JohnTheRipper/tree/bleeding-jumbo/src). To use these tools, we need to compile them.
#### JohnTheRipper - Installation

  JohnTheRipper - Installation

```shell
gdxqpardo@htb[/htb]$ sudo git clone https://github.com/magnumripper/JohnTheRipper.git
gdxqpardo@htb[/htb]$ cd JohnTheRipper/src
gdxqpardo@htb[/htb]$ sudo ./configure && make
```

There are also Python ports of most of these tools available that are very easy to work with. The majority of them are contained in the `JohnTheRipper` jumbo GitHub repo [here](https://github.com/magnumripper/JohnTheRipper/tree/bleeding-jumbo/run).

One additional tool ported to Python by @Harmj0y is the [keepass2john.py](https://gist.github.com/HarmJ0y/116fa1b559372804877e604d7d367bbc#file-keepass2john-py) tool for extracting a crackable hash from KeePass 1.x/2.x databases that can be run through `Hashcat`

`Hashcat` can be used to attempt to crack password hashes extracted from some Microsoft Office documents using the [office2john.py](https://raw.githubusercontent.com/magnumripper/JohnTheRipper/bleeding-jumbo/run/office2john.py) tool.

`Hashcat` supports the following hash modes for Microsoft Office documents:

|**Mode**|**Target**|
|---|---|
|`9400`|MS Office 2007|
|`9500`|MS Office 2010|
|`9600`|MS Office 2013|

#### Extract Hash

  Extract Hash

```shell-session
gdxqpardo@htb[/htb]$ python office2john.py hashcat_Word_example.docx 

hashcat_Word_example.docx:$office$*2013*100000*256*16*6e059661c3ed733f5730eaabb41da13a*aa38e007ee01c07e4fe95495934cf68f*2f1e2e9bf1f0b320172cd667e02ad6be1718585b6594691907b58191a6
```

## Example 2 - Cracking Password Protected Zip Files

During an assessment, we may find an interesting zip file, but it is password protected! We can extract these hashes using the compiled version of the [zip2john](https://github.com/magnumripper/JohnTheRipper/blob/bleeding-jumbo/src/zip2john.c) tool. `Hashcat` supports a variety of compressed file formats such as:

|**Mode**|**Target**|
|---|---|
|`11600`|7-Zip|
|`13600`|WinZip|
|`17200`|PKZIP (Compressed)|
|`17210`|PKZIP (Uncompressed)|
|`17220`|PKZIP (Compressed Multi-File)|
|`17225`|PKZIP (Mixed Multi-File)|
|`17230`|PKZIP (Compressed Multi-File Checksum-Only)|
|`23001`|SecureZIP AES-128|
|`23002`|SecureZIP AES-192|
|`23003`|SecureZIP AES-256|

For our example, we can take any document and add it to a password protected zip file in Parrot using the following command:

#### Set Password for a ZIP File

  Set Password for a ZIP File

```shell-session
gdxqpardo@htb[/htb]$ zip --password zippyzippy blueprints.zip dummy.pdf 

adding: dummy.pdf (deflated 7%)
```

We can then use the compiled version of `zip2john` to extract the hash in a format that can be run through `Hashcat`.

#### Extract Hash

  Extract Hash

```shell-session
gdxqpardo@htb[/htb]$ zip2john ~/Desktop/HTB/Academy/Cracking\ with\ Hashcat/blueprints.zip 

```

It is not uncommon to find KeePass files during an assessment, perhaps on a sysadmin's workstation or on an accessible file share. These are often a treasure trove of credentials because systems administrators, network administrators, help desk, etc. may store various passwords in a shared KeePass database. Gaining access may provide local administrator passwords to Windows machines, passwords to infrastructure such as ESXi and vCenter, access to network devices, and more.

We can extract these hashes using the compiled version of the [keepass2john](https://github.com/magnumripper/JohnTheRipper/blob/bleeding-jumbo/src/keepass2john.c) tool or using the Python port done by [HarmJ0y](https://gist.github.com/HarmJ0y), [keepass2john.py](https://gist.githubusercontent.com/HarmJ0y/116fa1b559372804877e604d7d367bbc/raw/c0c6f45ad89310e61ec0363a69913e966fe17633/keepass2john.py). `Hashcat` supports a variety of compressed file formats such as:

`Hashcat` supports the following hash names for KeePass databases, all designated by the same hash mode:

|**Mode**|**Target**|
|---|---|
|`13400`|KeePass 1 AES / without keyfile|
|`13400`|KeePass 2 AES / without keyfile|
|`13400`|KeePass 1 Twofish / with keyfile|
|`13400`|Keepass 2 AES / with keyfile|

We can use `keepass2john.py` to extract the hash:

#### Extract Hash

  Extract Hash

```shell-session
gdxqpardo@htb[/htb]$ python keepass2john.py Master.kdbx 
```

## Example 4 - Cracking Protected PDF Files

The last example in this section focuses on password-protected PDF documents. As with other file types, we often encounter password-protected PDFs on workstations, file shares, or even inside a user's email inbox should we gain access (and perusing users' email for sensitive information is in-scope for your engagement).

We can extract the hash of the passphrase using [pdf2john.py](https://raw.githubusercontent.com/truongkma/ctf-tools/master/John/run/pdf2john.py). The following command will extract the hash into a format that `Hashcat` can use.

`Hashcat` supports a variety of compressed file formats such as:

|**Mode**|**Target**|
|---|---|
|`10400`|PDF 1.1 - 1.3 (Acrobat 2 - 4)|
|`10410`|PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #1|
|`10420`|PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #2|
|`10500`|PDF 1.4 - 1.6 (Acrobat 5 - 8)|
|`10600`|PDF 1.7 Level 3 (Acrobat 9)|
|`10700`|PDF 1.7 Level 8 (Acrobat 10 - 11)|