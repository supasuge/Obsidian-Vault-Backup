## Table of Contents

  - [AutoRuns](#AutoRuns)
  - [ProcDump](#ProcDump)
    - [Process Explorer](#Process\Explorer)
          - [Color Coding](#Color\Coding)
  - [PsExec](#PsExec)
- [Sysinternals Suite Cheatsheet: Process Utilities](#sysinternals\suite\cheatsheet:\process\utilities)
  - [Process Explorer](#Process\Explorer)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [Process Monitor](#Process\Monitor)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [PsKill](#PsKill)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [PsList](#PsList)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [PsExec](#PsExec)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [PsSuspend](#PsSuspend)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [PsService](#PsService)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [ListDLLs](#ListDLLs)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [Handle](#Handle)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)

#sysinternals #autoruns #procdump #processexplorer #windows #blueteam #systemmonitoring #blueteam #defense #forensics 
#sysmon #processutilities #windows

Check out the Sysmon [room](https://tryhackme.com/room/sysmon) to further learn what Sysmon is and how to use it.  

![Screenshot of the Sysmon room card](https://assets.tryhackme.com/additional/sysinternals/sysmon.png)  

Other tools fall under the Security Utilities category. I encourage you to explore these tools at your own leisure.

Link: [https://docs.microsoft.com/en-us/sysinternals/downloads/security-utilities](https://docs.microsoft.com/en-us/sysinternals/downloads/security-utilities)


## AutoRuns
- Comprehensive knowledge of auto-starting locations of any startup monitor, shows you what programs are configured to run during system bootup or login, and when you start various windows applications like internet explorer, explorer, and media players. 
- Includes locations such as: startup foler, Run, RunOne, and other Regirty keys
- Reports Explorer shell extensions, toolbars, browser helper objects, Winlogon notifications, auto-start services, and much more. 
Launch Autoruns.

![Screenshot showing the execution of autoruns on the command prompt](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/fbecf1fad44c2d53a07ef911b2803ff3.png)  

Below is a snapshot of Autoruns, showing the first couple of items from the **Everything** tab. Normally there are a lot of entries within this tab.

![Screenshot showing the Autoruns window](https://assets.tryhackme.com/additional/sysinternals/autoruns.png)  

Notice all the tabs within the application. Click on each tab to inspect the items associated with each.   

The below image is a snapshot of the **Image Hijacks** tab. (At this time there is only 1 item listed)

![Screenshot showing the Autoruns window with the Image Hijacks tab](https://assets.tryhackme.com/additional/sysinternals/autoruns-image-hijacks.png)


## ProcDump
- Purposes is monitoring an application for CPU spikes and generating crash dumps during a spike that an administrator or developer can use to determine the cause of the spike


### Process Explorer
- Does the same as `ProcDump`
- Right-Click on the process to create a Minidump of Full Dump of the process
- 

In the following images, let's look at svchost.exe PID 3636 more closely.

![Screenshot showing svchost.exe in the Process Explorer list](https://assets.tryhackme.com/additional/sysinternals/procexp-svchost.png)  

![Screenshot of the Properties window for svchost.exe showing the Services tab](https://assets.tryhackme.com/additional/sysinternals/procexp-svchost-services.png)  

This process is associated with the WebClient service that is needed to connect to live.sysinternals.com (WebDAV).


There should be web traffic listed in the TCP/IP tab.

![Screenshot of the Properties window for svchost.exe showing the TCP/IP tab](https://assets.tryhackme.com/additional/sysinternals/procexp-svchost-tcpip.png)  

Ideally, it would be wise to check if that IP is what we assume it is.

Various online tools can be utilized to verify the authenticity of an IP address. For this demonstration, I'll use **Talos Reputation Center**.

![Screenshot showing the owner details of an IP address using the Talos Reputation Center](https://assets.tryhackme.com/additional/sysinternals/procexp-svchost-tcpip-2.png)  

[https://talosintelligence.com/reputation_center/lookup?search=52.154.170.73](https://talosintelligence.com/reputation_center/lookup?search=52.154.170.73)

As mentioned in the **ProcExp** description, we can see open handles associated with the process within the bottom window.

![Screenshot showing the open handles associated with a given process](https://assets.tryhackme.com/additional/sysinternals/procexp-svchost-handle.png)

Listed as an open handle is the connection to the remote WebDAV folder.

There is an option within ProcExp to **Verify Signatures**. Once enabled, it shows up as a column within the Process view.

![Screenshot showing ProcExp with the Verify Signatures option enabled](https://assets.tryhackme.com/additional/sysinternals/procexp-verify-sig.png)

Other options to note include **Run at Logon** and **Replace Task Manager**.

You may have noticed that some of the processes within Process Explorer have different colors. Those colors have meaning.

Below is a snippet from MalwareBytes explaining what each of those colors means.



###### Color Coding
Process Explorer uses color coding as extra information about the processes. The colors:
- Purple: Files may be packed
- Red: Process is exiting (stopped)
- Green: Process was freshly spawned (just loaded)
- Light Blue: Run by the same account that started Process Explorer
- Dark Blue: Indicates the process is selected
- Pink: Process is a service (Ex: svchost.exe)
- If you suspend a process it will turn dark grey until you resume it.

 the below snapshot, I set a filter to capture all the events related to PID 3888, notepad.exe. You can see some of the file operations that were captured and the file path or registry path/key the action occurred on, and the operation result.

![Screenshot of the Process Monitor tool showing the operations of the different processes](https://assets.tryhackme.com/additional/sysinternals/procmon.png)  

ProcMon will capture thousands upon thousands of events occurring within the operating system.

The option to capture events can be toggled on and off. 

![Screenshot showing the option to capture events in the Process Monitor](https://assets.tryhackme.com/additional/sysinternals/procmon3.png)  

In this ProcMon example, the session captured events only for a few seconds. Look at how many events were captured in that short space of time!

![Screenshot showing the Process Monitor Filter](https://assets.tryhackme.com/additional/sysinternals/procmon2.png)  

To use ProcMon effectively you **must** use the Filter and **must** configure it properly.

![](https://assets.tryhackme.com/additional/sysinternals/procmon4.png)  

In the above image, a filter was already set to capture events associated with **PID** 3888. Alternatively, a filter could have been set to capture events with the **Process Name** = notepad.exe. 

[Here](https://adamtheautomator.com/procmon/) is a useful guide on configuring ProcMon.

**Note**: To fully understand the output from some of these tools you need to understand some Windows concepts, such as [Processes and Threads](https://docs.microsoft.com/en-us/windows/win32/procthread/about-processes-and-threads) and [Windows API](https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-api-list) calls.


## PsExec
Light-weight telnet replacement that lets you execute processes on other systems, complete with full interactivity for console applications, without having to manually install client software. PsExec's most powerful uses include launching interactive command-prompts on remote systems and remote-enabling tools liek `ipconfig` that otherwise do not have the ability to show information about remote systems.
```powershell
psexec -accepteula
```
PsExec is another tool that is utilized by adversaries. This tool is associated with MITRE techniques [T1570](https://attack.mitre.org/techniques/T1570) (**Lateral Tool Transfer**), [T1021.002](https://attack.mitre.org/techniques/T1021/002) (**Remote Services: SMB/Windows Admin Shares**), and [T1569.002](https://attack.mitre.org/techniques/T1569/002) (**System Services: Service Execution**). It's MITRE ID is [S0029](https://attack.mitre.org/software/S0029/).

You can review this tool more in-depth by visiting its Sysinternals PsExec [page](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec). You can also check out this resource [page](https://adamtheautomator.com/psexec-ultimate-guide/).

Other tools fall under the Process Utilities category. I encourage you to explore these tools at your own leisure.

Link: [https://docs.microsoft.com/en-us/sysinternals/downloads/process-utilities](https://docs.microsoft.com/en-us/sysinternals/downloads/process-utilities)



# Sysinternals Suite Cheatsheet: Process Utilities

This cheatsheet highlights the process utilities in the Sysinternals Suite, offering a guide to their usage and examples.

## Process Explorer

### Description
An advanced tool for monitoring system resources, debugging software, and detecting malware.

### Usage
```
procexp.exe
```

### Examples
- **Launch Process Explorer**: Simply run `procexp.exe`.
- **Identify Process Details**: Hover over a process to see its full path and command line.
- **Check Resource Usage**: View CPU and memory usage in real-time.

## Process Monitor

### Description
Monitors and logs file system, registry, and process/thread activity.

### Usage
```
procmon.exe
```

### Examples
- **Start Logging Activity**: Run `procmon.exe` to begin monitoring.
- **Filter Events**: Use the filter feature to narrow down specific events, processes, or paths.

## PsKill

### Description
Allows you to terminate local or remote processes.

### Usage
```
pskill [-t] [[\\Computer [-u Username [-p Password]]] ProcessName|Pid]
```

### Examples
- **Kill a Process by Name**: `pskill notepad.exe`
- **Kill a Process by PID**: `pskill 1234`
- **Kill a Process Remotely**: `pskill \\RemoteComputer ProcessName`

## PsList

### Description
Provides detailed information about processes running on a system.

### Usage
```
pslist [-t] [-m] [-x] [[\\Computer [-u Username [-p Password]]] [ProcessName|Pid]]
```

### Examples
- **List Processes on Local System**: `pslist`
- **List Processes with CPU and Memory Usage**: `pslist -m`
- **List Processes on a Remote System**: `pslist \\RemoteComputer`

## PsExec

### Description
Executes processes remotely and interactively.

### Usage
```
psexec [\\Computer [-u Username [-p Password]]] [-s] [-i] [-d] [-x] [Executable [Arguments]]
```

### Examples
- **Execute Command Remotely**: `psexec \\RemoteComputer cmd.exe`
- **Run Process Interactively on Remote System**: `psexec -i \\RemoteComputer notepad.exe`

## PsSuspend

### Description
Suspends and resumes processes.

### Usage
```
pssuspend [-r] [[\\Computer [-u Username [-p Password]]] ProcessName|Pid]
```

### Examples
- **Suspend a Process**: `pssuspend notepad.exe`
- **Resume a Suspended Process**: `pssuspend -r notepad.exe`

## PsService

### Description
Views and controls services.

### Usage
```
psservice [\\Computer [-u Username [-p Password]]] command [options] [arguments]
```

### Examples
- **Query Service Information**: `psservice query "ServiceName"`
- **Start a Service**: `psservice start "ServiceName"`
- **Stop a Service**: `psservice stop "ServiceName"`

## ListDLLs

### Description
Shows DLLs loaded into processes.

### Usage
```
listdlls [-d dllname] [processname|pid]
```

### Examples
- **List DLLs in a Process**: `listdlls notepad.exe`
- **Find Processes Loading a DLL**: `listdlls -d kernel32.dll`

## Handle

### Description
Displays information about open handles for any process in the system.

### Usage
```
handle [-p ProcessName|-pid ProcessID] [options]
```

### Examples
- **List Handles of a Process**: `handle -p notepad.exe`
- **Close a Handle**: Use `handle -c HandleId -p ProcessId` (HandleId obtained from handle list).

---
