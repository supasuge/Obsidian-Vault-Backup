## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Task Manager](#Task\Manager)

## Table of Contents

  - [Task Manager](#Task\Manager)


## Task Manager 
- Built-in GUI-based Windows Utility that allowss users to see what is running on the Windows System. It also provides information on resource usage, such as CPU and memory. When a program is not responding, Task Manager is used to end(kill) the process.

- **Type** - Each process falls into 1 of 3 categories - (Apps, Background process, or Windows process).
- **Publisher** - Think of this column as the name of the author of the program/file.
- **PID** - This is known as the process identifier number. Windows assigns a unique process identifier each time a program starts. If the same program has multiple running processes, each will have its unique process identifier (PID).
- **Process name** - This is the file name of the process. In the above image, the file name for Task Manager is Taskmrg.exe. 
- **Command line** - The full command used to launch the process. 
- **CPU** - The amount of CPU (processing power) the process uses.
- **Memory** - The amount of physical working memory utilized by the process.

**Process Hacker**

![Process hacker.](https://assets.tryhackme.com/additional/windows-processes/processhacker.png)  

**Process Explorer**

![Process Explorer.](https://assets.tryhackme.com/additional/windows-processes/process-explorer.png)  

Moving forward, we'll use **Process Hacker** and Process Explorer instead of Task Manager to obtain information about each Windows process.   

As always, it's encouraged that you inspect and familiarize yourself with all information available within Task Manager. It's a built-in utility that is available in every Windows system. You might find yourself in a situation where you can't bring your tools to the fight and rely on the tools native to the system.

Aside from Task Manager, it would be best if you also familiarize yourself with the command-line equivalent of obtaining information about the running processes on a Windows system: `tasklist`, `Get-Process` or `ps` (PowerShell), and `wmic`.


The first Windows process on the list is **System**. It was mentioned in a previous section that a PID for any given process is assigned at random, but that is not the case for the System process. The PID for System is always 4. What does this process do exactly?

  
"_The System process (process ID 4) is the home for a special kind of thread that runs only in kernel mode a kernel-mode system thread. System threads have all the attributes and contexts of regular user-mode threads (such as a hardware context, priority, and so on) but are different in that they run only in kernel-mode executing code loaded in system space, whether that is in Ntoskrnl.exe or in any other loaded device driver. In addition, system threads don't have a user process address space and hence must allocate any dynamic storage from operating system memory heaps, such as a paged or nonpaged pool._"

The next process is **smss.exe** (**Session Manager Subsystem**). This process, also known as the **Windows Session Manager**, is responsible for creating new sessions. It is the first user-mode process started by the kernel.

  

This process starts the kernel and user modes of the Windows subsystem (you can read more about the NT Architecture [here](https://en.wikipedia.org/wiki/Architecture_of_Windows_NT)). This subsystem includes win32k.sys (kernel mode), winsrv.dll (user mode), and csrss.exe (user mode). 



Smss.exe starts csrss.exe (Windows subsystem) and wininit.exe in Session 0, an isolated Windows session for the operating system, and csrss.exe and winlogon.exe for Session 1, which is the user session. The first child instance creates child instances in new sessions, done by smss.exe copying itself into the new session and self-terminating. You can read more about this process [here](https://en.wikipedia.org/wiki/Session_Manager_Subsystem).

Any other subsystem listed in the `Required` value of `HKLM\System\CurrentControlSet\Control\Session Manager\Subsystems` is also launched.
![[Pasted image 20231114163049.png]]
**Image Path**:  %SystemRoot%\System32\smss.exe
**Parent Process**:  System
**Number of Instances**:  One master instance and child instance per session. The child instance exits after creating the session.
**User Account**:  Local System
**Start Time**:  Within seconds of boot time for the master instance

What is unusual?

- A different parent process other than System (4)
- The image path is different from C:\Windows\System32
- More than one running process. (children self-terminate and exit after each new session)
- The running User is not the SYSTEM user
- Unexpected registry entries for Subsystem

**Client Server Runtime Subsystem**, or `csrss.exe`, is a component of the [Windows NT](https://en.wikipedia.org/wiki/Windows_NT "Windows NT") family of [operating systems](https://en.wikipedia.org/wiki/Operating_system "Operating system") that provides the [user mode](https://en.wikipedia.org/wiki/User_space "User space") side of the [Win32 subsystem](https://en.wikipedia.org/wiki/Windows_API "Windows API") and is included in [Windows NT 3.1](https://en.wikipedia.org/wiki/Windows_NT_3.1 "Windows NT 3.1") and later.[[1]](https://en.wikipedia.org/wiki/Client/Server_Runtime_Subsystem#cite_note-GDI-1) Because most of the Win32 subsystem operations have been moved to [kernel mode](https://en.wikipedia.org/wiki/Kernel_mode "Kernel mode") [drivers](https://en.wikipedia.org/wiki/Device_driver "Device driver") in [Windows NT 4](https://en.wikipedia.org/wiki/Windows_NT_4 "Windows NT 4") and later, CSRSS is mainly responsible for [Win32 console](https://en.wikipedia.org/wiki/Win32_console "Win32 console") handling and GUI shutdown. It is critical to system operation; therefore, terminating this [process](https://en.wikipedia.org/wiki/Process_(computing) "Process (computing)") will result in system failure. Under normal circumstances, CSRSS cannot be terminated with the _[taskkill](https://en.wikipedia.org/wiki/Kill_(command)#Microsoft_Windows_and_ReactOS "Kill (command)")_ command or with [Windows Task Manager](https://en.wikipedia.org/wiki/Windows_Task_Manager "Windows Task Manager"), although it is possible in [Windows Vista](https://en.wikipedia.org/wiki/Windows_Vista "Windows Vista") if the [Task Manager](https://en.wikipedia.org/wiki/Task_Manager_(Windows) "Task Manager (Windows)") is run in Administrator mode. On [Windows 7](https://en.wikipedia.org/wiki/Windows_7 "Windows 7") and later, Task Manager will inform the user that terminating the process may result in system failure, and prompt if they want to continue. In Windows NT 4.0 however, terminating CSRSS without the [Session Manager Subsystem](https://en.wikipedia.org/wiki/Session_Manager_Subsystem "Session Manager Subsystem") (SMSS) watching will not crash the system.[[2]](https://en.wikipedia.org/wiki/Client/Server_Runtime_Subsystem#cite_note-2) However, in [Windows XP](https://en.wikipedia.org/wiki/Windows_XP "Windows XP"), terminating CSRSS without SMSS watching will crash the system due to the critical bit being set in RAM for csrss.exe.

**Note**: Recall that csrss.exe and winlogon.exe are called from smss.exe at startup for Session 1. 


What is normal?


**Session 0 (PID 392)**

![csrss.exe session 0 image.](https://assets.tryhackme.com/additional/windows-processes/csrss-session0.png)  


**Session 1 (PID 512)**

![csrss.exe session 1 image.](https://assets.tryhackme.com/additional/windows-processes/csrss-session1.png)  
  

Notice what is shown for the parent process for these two processes. Remember, these processes are spawned by smss.exe, which self-terminates itself.  


**Image Path**:  %SystemRoot%\System32\csrss.exe
**Parent Process**:  Created by an instance of smss.exe
**Number of Instances**:  Two or more
**User Account**:  Local System
**Start Time**:  Within seconds of boot time for the first two instances (for Session 0 and 1). Start times for additional instances occur as new sessions are created, although only Sessions 0 and 1 are often created.

  
What is unusual?

- An actual parent process. (smss.exe calls this process and self-terminates)
- Image file path other than C:\Windows\System32
- Subtle misspellings to hide rogue processes masquerading as csrss.exe in plain sight
- The user is not the SYSTEM user.








