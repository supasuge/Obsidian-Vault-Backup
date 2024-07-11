## File Share Services
Provides, mediates, and monitors the transfer of files. 
- FTP, SMB, NFS, TFTP, SFTP

## Server Message Block (SMB)
We will often find share folders in a Windows network. We can interact with SMB using the GUI, CLI, or tools. Let us cover some common ways of interacting with SMB using Windows/Linux


#### Windows

There are different ways we can interact with a shared folder using Windows, and we will explore a couple of them. On Windows GUI, we can press `[WINKEY] + [R]` to open the Run dialog box and type the file share location, e.g.: `\\192.168.220.129\Finance\`

![text](https://academy.hackthebox.com/storage/modules/116/windows_run_sharefolder2.jpg)

Suppose the shared folder allows anonymous authentication, or we are authenticated with a user who has privilege over that shared folder. In that case, we will not receive any form of authentication request, and it will display the content of the shared folder.

![image](https://academy.hackthebox.com/storage/modules/116/finance_share_folder2.jpg)

If we do not have access, we will receive an authentication request.

![text](https://academy.hackthebox.com/storage/modules/116/auth_request_share_folder2.jpg)

Windows has two command-line shells: the [Command shell](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands) and [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview). Each shell is a software program that provides direct communication between us and the operating system or application, providing an environment to automate IT operations.

Let's discuss some commands to interact with file share using Command Shell (`CMD`) and `PowerShell`. The command [dir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir) displays a list of a directory's files and subdirectories.

#### Windows CMD - DIR

Interacting with Common Services

```cmd-session
C:\htb> dir \\192.168.220.129\Finance\

Volume in drive \\192.168.220.129\Finance has no label.
Volume Serial Number is ABCD-EFAA

Directory of \\192.168.220.129\Finance

02/23/2022  11:35 AM    <DIR>          Contracts
               0 File(s)          4,096 bytes
               1 Dir(s)  15,207,469,056 bytes free
```

The command [net use](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/gg651155(v=ws.11)) connects a computer to or disconnects a computer from a shared resource or displays information about computer connections. We can connect to a file share with the following command and map its content to the drive letter `n`.

#### Windows CMD - Net Use

Interacting with Common Services

```cmd-session
C:\htb> net use n: \\192.168.220.129\Finance

The command completed successfully.
```

We can also provide a username and password to authenticate to the share.

Interacting with Common Services

```cmd-session
C:\htb> net use n: \\192.168.220.129\Finance /user:plaintext Password123

The command completed successfully.
```

With the shared folder mapped as the `n` drive, we can execute Windows commands as if this shared folder is on our local computer. Let's find how many files the shared folder and its subdirectories contain.

#### Windows CMD - DIR

Interacting with Common Services

```cmd-session
C:\htb> dir n: /a-d /s /b | find /c ":\"

29302
```

We found 29,302 files. Let's walk through the command:

Interacting with Common Services

```shell-session
dir n: /a-d /s /b | find /c ":\"
```

|**Syntax**|**Description**|
|---|---|
|`dir`|Application|
|`n:`|Directory or drive to search|
|`/a-d`|`/a` is the attribute and `-d` means not directories|
|`/s`|Displays files in a specified directory and all subdirectories|
|`/b`|Uses bare format (no heading information or summary)|

The following command `| find /c ":\\"` process the output of `dir n: /a-d /s /b` to count how many files exist in the directory and subdirectories. You can use `dir /?` to see the full help. Searching througth 29,302 files is time comsuming, scripting and command line utilities can help us speed up the search. With `dir` we can search for specific names in files such as:

- cred
- password
- users
- secrets
- key
- Common File Extensions for source code such as: .cs, .c, .go, .java, .php, .asp, .aspx, .html.

Interacting with Common Services

```cmd-session
C:\htb>dir n:\*cred* /s /b

n:\Contracts\private\credentials.txt


C:\htb>dir n:\*secret* /s /b

n:\Contracts\private\secret.txt
```

If we want to search for a specific word within a text file, we can use [findstr](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr).

#### Windows CMD - Findstr

Interacting with Common Services

```cmd-session
c:\htb>findstr /s /i cred n:\*.*

n:\Contracts\private\secret.txt:file with all credentials
n:\Contracts\private\credentials.txt:admin:SecureCredentials!
```

We can find more `findstr` examples [here](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr#examples).

#### Windows PowerShell

PowerShell was designed to extend the capabilities of the Command shell to run PowerShell commands called `cmdlets`. Cmdlets are similar to Windows commands but provide a more extensible scripting language. We can run both Windows commands and PowerShell cmdlets in PowerShell, but the Command shell can only run Windows commands and not PowerShell cmdlets. Let's replicate the same commands now using Powershell.

#### Windows PowerShell

Interacting with Common Services

```powershell-session
PS C:\htb> Get-ChildItem \\192.168.220.129\Finance\

    Directory: \\192.168.220.129\Finance

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2/23/2022   3:27 PM                Contracts
```

Instead of `net use`, we can use `New-PSDrive` in PowerShell.

Interacting with Common Services

```powershell-session
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem"

Name           Used (GB)     Free (GB) Provider      Root                                               CurrentLocation
----           ---------     --------- --------      ----                                               ---------------
N                                      FileSystem    \\192.168.220.129\Finance
```

To provide a username and password with Powershell, we need to create a [PSCredential object](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.pscredential). It offers a centralized way to manage usernames, passwords, and credentials.

#### Windows PowerShell - PSCredential Object

Interacting with Common Services

```powershell-session
PS C:\htb> $username = 'plaintext'
PS C:\htb> $password = 'Password123'
PS C:\htb> $secpassword = ConvertTo-SecureString $password -AsPlainText -Force
PS C:\htb> $cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred

Name           Used (GB)     Free (GB) Provider      Root                                                              CurrentLocation
----           ---------     --------- --------      ----                                                              ---------------
N                                      FileSystem    \\192.168.220.129\Finance
```

In PowerShell, we can use the command `Get-ChildItem` or the short variant `gci` instead of the command `dir`.

#### Windows PowerShell - GCI

Interacting with Common Services

```powershell-session
PS C:\htb> N:
PS N:\> (Get-ChildItem -File -Recurse | Measure-Object).Count

29302
```

We can use the property `-Include` to find specific items from the directory specified by the Path parameter.

Interacting with Common Services

```powershell-session
PS C:\htb> Get-ChildItem -Recurse -Path N:\ -Include *cred* -File

    Directory: N:\Contracts\private

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2/23/2022   4:36 PM             25 credentials.txt
```

The `Select-String` cmdlet uses regular expression matching to search for text patterns in input strings and files. We can use `Select-String` similar to `grep` in UNIX or `findstr.exe` in Windows.

#### Windows PowerShell - Select-String

Interacting with Common Services

```powershell-session
PS C:\htb> Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List

N:\Contracts\private\secret.txt:1:file with all credentials
N:\Contracts\private\credentials.txt:1:admin:SecureCredentials!
```

CLI enables IT operations to automate routine tasks like user account management, nightly backups, or interaction with many files. We can perform operations more efficiently by using scripts than the user interface or GUI.

#### Linux

Linux (UNIX) machines can also be used to browse and mount SMB shares. Note that this can be done whether the target server is a Windows machine or a Samba server. Even though some Linux distributions support a GUI, we will focus on Linux command-line utilities and tools to interact with SMB. Let's cover how to mount SMB shares to interact with directories and files locally.

#### Linux - Mount

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ sudo mkdir /mnt/Finance
gdxqpardo@htb[/htb]$ sudo mount -t cifs -o username=plaintext,password=Password123,domain=. //192.168.220.129/Finance /mnt/Finance
```

As an alternative, we can use a credential file.

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ mount -t cifs //192.168.220.129/Finance /mnt/Finance -o credentials=/path/credentialfile
```

The file `credentialfile` has to be structured like this:

#### CredentialFile

Code: txt

```txt
username=plaintext
password=Password123
domain=.
```

Note: We need to install `cifs-utils` to connect to an SMB share folder. To install it we can execute from the command line `sudo apt install cifs-utils`.

Once a shared folder is mounted, you can use common Linux tools such as `find` or `grep` to interact with the file structure. Let's hunt for a filename that contains the string `cred`:

#### Linux - Find

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ find /mnt/Finance/ -name *cred*

/mnt/Finance/Contracts/private/credentials.txt
```

Next, let's find files that contain the string `cred`:

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ grep -rn /mnt/Finance/ -ie cred

/mnt/Finance/Contracts/private/credentials.txt:1:admin:SecureCredentials!
/mnt/Finance/Contracts/private/secret.txt:1:file with all credentials
```

---

## Other Services

There are other file-sharing services such as FTP, TFTP, and NFS that we can attach (mount) using different tools and commands. However, once we mount a file-sharing service, we must understand that we can use the available tools in Linux or Windows to interact with files and directories. As we discover new file-sharing services, we will need to investigate how they work and what tools we can use to interact with them.

#### Email

We typically need two protocols to send and receive messages, one for sending and another for receiving. The Simple Mail Transfer Protocol (SMTP) is an email delivery protocol used to send mail over the internet. Likewise, a supporting protocol must be used to retrieve an email from a service. There are two main protocols we can use POP3 and IMAP.

We can use a mail client such as [Evolution](https://wiki.gnome.org/Apps/Evolution), the official personal information manager, and mail client for the GNOME Desktop Environment. We can interact with an email server to send or receive messages with a mail client. To install Evolution, we can use the following command:

#### Linux - Install Evolution

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ sudo apt-get install evolution
...SNIP...
```

Note: If an error appears when starting evolution indicating "bwrap: Can't create file at ...", use this command to start evolution `export WEBKIT_FORCE_SANDBOX=0 && evolution`.

#### Video - Connecting to IMAP and SMTP using Evolution

Click on the image below to see a short video demonstration.

[![Evolution](https://academy.hackthebox.com/storage/modules/116/ConnectToIMAPandSMTP.jpg)](https://www.youtube.com/watch?v=xelO2CiaSVs)

We can use the domain name or IP address of the mail server. If the server uses SMTPS or IMAPS, we'll need the appropriate encryption method (TLS on a dedicated port or STARTTLS after connecting). We can use the `Check for Supported Types` option under authentication to confirm if the server supports our selected method.

#### Databases

Databases are typically used in enterprises, and most companies use them to store and manage information. There are different types of databases, such as Hierarchical databases, NoSQL (or non-relational) databases, and SQL relational databases. We will focus on SQL relational databases and the two most common relational databases called MySQL & MSSQL. We have three common ways to interact with databases:

|||
|---|---|
|`1.`|Command Line Utilities (`mysql` or `sqsh`)|
|`2.`|A GUI application to interact with databases such as HeidiSQL, MySQL Workbench, or SQL Server Management Studio.|
|`3.`|Programming Languages|

#### MySQL example

![text](https://academy.hackthebox.com/storage/modules/116/3_way_to_interact_with_MySQL.png)

Let's explore command-line utilities and a GUI application.

---

## Command Line Utilities

#### MSSQL

To interact with [MSSQL (Microsoft SQL Server)](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) with Linux we can use [sqsh](https://en.wikipedia.org/wiki/Sqsh) or [sqlcmd](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility) if you are using Windows. `Sqsh` is much more than a friendly prompt. It is intended to provide much of the functionality provided by a command shell, such as variables, aliasing, redirection, pipes, back-grounding, job control, history, command substitution, and dynamic configuration. We can start an interactive SQL session as follows:

#### Linux - SQSH

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ sqsh -S 10.129.20.13 -U username -P Password123
```

The `sqlcmd` utility lets you enter Transact-SQL statements, system procedures, and script files through a variety of available modes:

- At the command prompt.
- In Query Editor in SQLCMD mode.
- In a Windows script file.
- In an operating system (Cmd.exe) job step of a SQL Server Agent job.

#### Windows - SQLCMD

Interacting with Common Services

```cmd-session
C:\htb> sqlcmd -S 10.129.20.13 -U username -P Password123
```

To learn more about `sqlcmd` usage, you can see [Microsoft documentation](https://docs.microsoft.com/en-us/sql/ssms/scripting/sqlcmd-use-the-utility).

#### MySQL

To interact with [MySQL](https://en.wikipedia.org/wiki/MySQL), we can use MySQL binaries for Linux (`mysql`) or Windows (`mysql.exe`). MySQL comes pre-installed on some Linux distributions, but we can install MySQL binaries for Linux or Windows using this [guide](https://dev.mysql.com/doc/mysql-getting-started/en/#mysql-getting-started-installing). Start an interactive SQL Session using Linux:

#### Linux - MySQL

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ mysql -u username -pPassword123 -h 10.129.20.13
```

We can easily start an interactive SQL Session using Windows:

#### Windows - MySQL

Interacting with Common Services

```cmd-session
C:\htb> mysql.exe -u username -pPassword123 -h 10.129.20.13
```

#### GUI Application

Database engines commonly have their own GUI application. MySQL has [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) and MSSQL has [SQL Server Management Studio or SSMS](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms), we can install those tools in our attack host and connect to the database. SSMS is only supported in Windows. An alternative is to use community tools such as [dbeaver](https://github.com/dbeaver/dbeaver). [dbeaver](https://github.com/dbeaver/dbeaver) is a multi-platform database tool for Linux, macOS, and Windows that supports connecting to multiple database engines such as MSSQL, MySQL, PostgreSQL, among others, making it easy for us, as an attacker, to interact with common database servers.

To install [dbeaver](https://github.com/dbeaver/dbeaver) using a Debian package we can download the release .deb package from [https://github.com/dbeaver/dbeaver/releases](https://github.com/dbeaver/dbeaver/releases) and execute the following command:

#### Install dbeaver

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ sudo dpkg -i dbeaver-<version>.deb
```

To start the application use:

#### Run dbeaver

Interacting with Common Services

```shell-session
gdxqpardo@htb[/htb]$ dbeaver &
```

To connect to a database, we will need a set of credentials, the target IP and port number of the database, and the database engine we are trying to connect to (MySQL, MSSQL, or another).

#### Video - Connecting to MSSQL DB using dbeaver

Click on the image below for a short video demonstration of connecting to an MSSQL database using `dbeaver`.

[![MSSQL](https://academy.hackthebox.com/storage/modules/116/ConnectToMSSQL.jpg)](https://www.youtube.com/watch?v=gU6iQP5rFMw)

Click on the image below for a short video demonstration of connecting to a MySQL database using `dbeaver`.

#### Video - Connecting to MySQL DB using dbeaver

[![MYSQL](https://academy.hackthebox.com/storage/modules/116/ConnectToMYSQL.jpg)](https://www.youtube.com/watch?v=PeuWmz8S6G8)

Once we have access to the database using a command-line utility or a GUI application, we can use common [Transact-SQL statements](https://docs.microsoft.com/en-us/sql/t-sql/statements/statements?view=sql-server-ver15) to enumerate databases and tables containing sensitive information such as usernames and passwords. If we have the correct privileges, we could potentially execute commands as the MSSQL service account. Later in this module, we will discuss common Transact-SQL statements and attacks for MSSQL & MySQL databases.

#### Tools

It is crucial to get familiar with the default command-line utilities available to interact with different services. However, as we move forward in the field, we will find tools that can help us be more efficient. The community commonly creates those tools. Although, eventually, we will have ideas on how a tool can be improved or for creating our own tools, even if we are not full-time developers, the more we get familiar with hacking. The more we learn, the more we find ourselves looking for a tool that does not exist, which may be an opportunity to learn and create our tools.

#### Tools to Interact with Common Services

|**SMB**|**FTP**|**Email**|**Databases**|
|---|---|---|---|
|[smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)|[ftp](https://linux.die.net/man/1/ftp)|[Thunderbird](https://www.thunderbird.net/en-US/)|[mssql-cli](https://github.com/dbcli/mssql-cli)|
|[CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)|[lftp](https://lftp.yar.ru/)|[Claws](https://www.claws-mail.org/)|[mycli](https://github.com/dbcli/mycli)|
|[SMBMap](https://github.com/ShawnDEvans/smbmap)|[ncftp](https://www.ncftp.com/)|[Geary](https://wiki.gnome.org/Apps/Geary)|[mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py)|
|[Impacket](https://github.com/SecureAuthCorp/impacket)|[filezilla](https://filezilla-project.org/)|[MailSpring](https://getmailspring.com)|[dbeaver](https://github.com/dbeaver/dbeaver)|
|[psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py)|[crossftp](http://www.crossftp.com/)|[mutt](http://www.mutt.org/)|[MySQL Workbench](https://dev.mysql.com/downloads/workbench/)|
|[smbexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py)||[mailutils](https://mailutils.org/)|[SQL Server Management Studio or SSMS](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)|
|||[sendEmail](https://github.com/mogaal/sendemail)||
|||[swaks](http://www.jetmore.org/john/code/swaks/)||
|||[sendmail](https://en.wikipedia.org/wiki/Sendmail)||

---

## General Troubleshooting

Depending on the Windows or Linux version we are working with or targetting, we may encounter different problems when attempting to connect to a service.

Some reasons why we may not have access to a resource:

- Authentication
- Privileges
- Network Connection
- Firewall Rules
- Protocol Support