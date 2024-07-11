Processes are a common PrivEsc vector. Most common example is discoveriung a web server like IIS or XAMPP running on the box, placing an `aspx/php` shell on the box, and gaining a shell as the user running the web server. Generally, this is not an administrator but will often `SeImpersonate` token, allowing for the `Rogue/JuicyPotatoe` exploit. 


## Access Tokens
In Windows, [access tokens](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-tokens) are used to describe the security context (security attributes or rules) of a process or thread. The token includes information about the user account's identity and privileges related to a specific process or thread. When a user authenticates to a system, their password is verified against a security database, and if properly authenticated, they will be assigned an access token. Every time a user interacts with a process, a copy of this token will be presented to determine their privilege level.

---

## Enumerating Network Services
#### Display Active Network Connections
```cmd
netstat -ano
```
- Look for Active Network Connection entries (listening on loopback address (`127.0.0.1`, and `::1`)) that are not listening on the IP Address (`10.129.43.8` or broadcast: `0.0.0.0`, `::/0`). 
	- Network sockets on localhost are often insecure due to the thouygh that they arent accessible to the network. 

#### More Examples
`Splunk Universal Forwarder`: Installed on endpoints to send logs into Splunk. 
- Default configuration didn't have any authentication on the software and allowed anyone to deploy applications.... leads to code execution
- Again, the default configuration of Splunk was to run it as SYSTEM$ and not a low privilege user. For more information, check out [Splunk Universal Forwarder Hijacking](https://airman604.medium.com/splunk-universal-forwarder-hijacking-5899c3e0e6b2) and [SplunkWhisperer2](https://clement.notin.org/blog/2019/02/25/Splunk-Universal-Forwarder-Hijacking-2-SplunkWhisperer2/).



## Named Pipes
The other way processes communicate with each other is through Named Pipes. Pipes are essentially files stored in memory that get cleared out after being read. Cobalt strikie uses Named Pipes for every command. (Exlcuding https://www.cobaltstrike.com/help-beacon-object-files ), The workflow is as follow:
1. Beacon starts a named pipe of `\.\pipe\msagent_12`
2. Beacon starts a new process and injects command into that process directing output to `\.\pipe\msagent_12`
3. Server displays what was written into `\.\pipe\msagent_12`
- This is so that any command being ran that got flagged by antivirus or crashed, would not affect the beacon (process running command). Often, Cobalt Strike users will change their named pipes to masquerade as another program. I.e., `mojo` in place of `msagent`. 

### More on Named Pipes
Pipes are used for communication between two applications or processes using shared memory. There are two types of pipes, [named pipes](https://docs.microsoft.com/en-us/windows/win32/ipc/named-pipes) and anonymous pipes. An example of a named pipe is `\\.\PipeName\\ExampleNamedPipeServer`. Windows systems use a client-server implementation for pipe communication. In this type of implementation, the process that creates a named pipe is the server, and the process communicating with the named pipe is the client.

Named pipes can communicate using `half-duplex`, or a one-way channel with the client only being able to write data to the server, or `duplex`, which is a two-way communication channel that allows the client to write data over the pipe. and the server to respoind back with data over the pipe. Every active connection to a named pipe server results in the creation of a new named pipe. 


We can use the tool [PipeList](https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist) from the Sysinternals Suite to enumerate instances of named pipes.

### Listing Named Pipes with Pipelist

```cmd
C:\htb> pipelist.exe /accepteula
```

Additionally, we can use PowerShell to list named pipes using `gci` (`Get-ChildItem`).

#### Listing Named Pipes with PowerShell
```cmd
gci \\.\pipe\
Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 InitShutdown
------       12/31/1600   4:00 PM              4 lsass
------       12/31/1600   4:00 PM              3 ntsvcs
------       12/31/1600   4:00 PM              3 scerpc


    Directory: \\.\pipe\Winsock2


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              1 Winsock2\CatalogChangeListener-34c-0


    Directory: \\.\pipe


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
------       12/31/1600   4:00 PM              3 epmapper

<SNIP>
```

After getting a listing of named pipes, we can use [Accesschk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) to enumerate the permissions assigned to a specific named pipe by reviewing the Discretionary Access List (DACL), which shows us who has the permissions to modify, write, read, or execute a resource. Let's take a look at the `LSASS` process. We can also review the **DACLs** of all named pipes using the command `.\accesschk.exe /accepteula \pipe\`.


#### Reviewing LSASS Named Pipe Permissions
Communication with Processes
```cmd
C:\htb> accesschk.exe /accepteula \\.\Pipe\lsass -v

Accesschk v6.12 - Reports effective permissions for securable objects
Copyright (C) 2006-2017 Mark Russinovich
Sysinternals - www.sysinternals.com

\\.\Pipe\lsass
  Untrusted Mandatory Level [No-Write-Up]
  RW Everyone
        FILE_READ_ATTRIBUTES
        FILE_READ_DATA
        FILE_READ_EA
        FILE_WRITE_ATTRIBUTES
        FILE_WRITE_DATA
        FILE_WRITE_EA
        SYNCHRONIZE
        READ_CONTROL
  RW NT AUTHORITY\ANONYMOUS LOGON
        FILE_READ_ATTRIBUTES
        FILE_READ_DATA
        FILE_READ_EA
        FILE_WRITE_ATTRIBUTES
        FILE_WRITE_DATA
        FILE_WRITE_EA
        SYNCHRONIZE
        READ_CONTROL
  RW APPLICATION PACKAGE AUTHORITY\Your Windows credentials
        FILE_READ_ATTRIBUTES
        FILE_READ_DATA
        FILE_READ_EA
        FILE_WRITE_ATTRIBUTES
        FILE_WRITE_DATA
        FILE_WRITE_EA
        SYNCHRONIZE
        READ_CONTROL
  RW BUILTIN\Administrators
        FILE_ALL_ACCESS
```

From above, we can not that only administrators have full access to the LSASS process as expected. 

### Named Pipes Attack Example
Let's walk through an example of taking advantage of an exposed named pipe to escalate privileges. This [WindscribeService Named Pipe Privilege Escalation](https://www.exploit-db.com/exploits/48021) is a great example. Using `accesschk` we can search for all named pipes that allow write access with a command such as `accesschk.exe -w \pipe\* -v` and notice that the `WindscribeService` named pipe allows `READ` and `WRITE` access to the `Everyone` group, meaning all authenticated users.

```cmd
accesschk.exe -accepteula -w \pipe\WindscribeService -v
Accesschk v6.13 - Reports effective permissions for securable objects
Copyright ⌐ 2006-2020 Mark Russinovich
Sysinternals - www.sysinternals.com

\\.\Pipe\WindscribeService
  Medium Mandatory Level (Default) [No-Write-Up]
  RW Everyone
        FILE_ALL_ACCESS
```

##### Summary
In Windows, access tokens define the security context of processes and threads, detailing the user account's identity and privileges. These tokens are crucial in determining privilege levels during user interactions with processes. When enumerating network services and active connections, commands like `netstat -ano` help identify potential vulnerable services running on local addresses. Named pipes, used for inter-process communication, can also be a privilege escalation vector. Tools like [PipeList](https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist) and PowerShell's `Get-ChildItem` (`gci`) can list named pipes, while [AccessChk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) reveals the permissions on these pipes. For example, `accesschk.exe -c \pipe\SQLLocal\SQLEXPRESS01` checks permissions on the `\pipe\SQLLocal\SQLEXPRESS01` named pipe. Such analysis can reveal misconfigured permissions, allowing attackers to exploit processes with high privileges. This was illustrated with the WindscribeService named pipe, which had `WRITE` access for `Everyone`, potentially leading to privilege escalation.

#### Key Commands and Tools
- **Enumerate network services**: `netstat -ano`
- **List named pipes**: 
  - `pipelist.exe /accepteula`
  - `gci \\.\pipe\` (PowerShell)
- **Check named pipe permissions**: 
  - `accesschk.exe /accepteula \pipe\`
  - `accesschk.exe -w \pipe\* -v`
  - `accesschk.exe -c \pipe\SQLLocal\SQLEXPRESS01`

**Which account has WRITE_DAC privileges over the \pipe\SQLLocal\SQLEXPRESS01 named pipe?**
> `NT SERVICE\MSSQL$SQLEXPRESS01`
1. `.\pipelist.exe`
2. `accesschk64.exe \pipe\SQLLocal\SQLEXPRESS01 -v`
3. 
