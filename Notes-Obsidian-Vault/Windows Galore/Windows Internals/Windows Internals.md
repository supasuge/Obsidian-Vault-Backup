## Table of Contents

  - [Threads](#Threads)
- [Virtual Memory](#virtual\memory)
- [Portable Executable Format](#portable\executable\format)
- [Interacting with Windows Internals](#interacting\with\windows\internals)


A process maintains and represents the execution of a program; an application can contain one or more processes. A process has many components that it gets broken down into to be stored and interacted with. https://docs.microsoft.com/en-us/windows/win32/procthread/about-processes-and-threads

As previously mentioned, processes are created from the execution of an application. Processes are core to how Windows functions, most functionality of Windows can be encompassed as an application and has a corresponding process. Below are a few examples of default applications that start processes.

- MsMpEng (Microsoft Defender)
- wininit (keyboard and mouse)
- lsass (credential storage)

Attackers can target processes to evade detections and hide malware as legitimate processes. Below is a small list of potential attack vectors attackers could employ against processes:
- Process Injection
- Process Hollowing
- Process Masquerading

Processes have many components; they can be split into key characteristics that we can use to describe processes at a high level. The table below describes each critical component of processes and their purpose.

|   |   |
|---|---|
|**Process Component  <br>**|**Purpose**|
|Private Virtual Address Space|Virtual memory addresses that the process is allocated.|
|Executable Program|Defines code and data stored in the virtual address space.|
|Open Handles|Defines handles to system resources accessible to the process.|
|Security Context|The access token defines the user, security groups, privileges, and other security information.|
|Process ID|Unique numerical identifier of the process.|
|Threads|Section of a process scheduled for execution.|

We can also explain a process at a lower level as it resides in the virtual address space. The table and diagram below depict what a process looks like in memory.

|   |   |
|---|---|
|**Component  <br>**|**Purpose**|
|Code|Code to be executed by the process.|
|Global Variables|Stored variables.|
|Process Heap|Defines the heap where data is stored.|
|Process Resources|Defines further resources of the process.|
|Environment Block|Data structure to define process information.|


We can make the process tangible by observing them in the _Windows Task Manager_. The task manager can report on many components and information about a process. Below is a table with a brief list of essential process details.



|   |   |   |
|---|---|---|
|**Value/Component  <br>**|**Purpose  <br>**|**Example**|
|Name|Define the name of the process, typically inherited from the application|conhost.exe|
|PID|Unique numerical value to identify the process|7408|
|Status|Determines how the process is running (running, suspended, etc.)|Running|
|User name|User that initiated the process. Can denote privilege of the process|SYSTEM|


Utilities available that make obeserving processes earier: https://github.com/processhacker/processhacker 
https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer
https://docs.microsoft.com/en-us/sysinternals/downloads/procmon


## Threads
A thread is an executable unit employed by a process and scheduled based on device factors.

Device factors can vary based on CPU and memory specifications, priority and logical factors, and others.
;"Controlling the execution of a process"

Since threads control execution, this is a commonly targeted component. Thread abuse can be used on its own to aid in code execution, or it is more wisely used to chain with other API calls as part of other techniques.

Threads share the same details and resources as their parent process, such as code, global variables, etc. threads also have their unique values and data:
|   |   |
|---|---|
|**Component**|**Purpose**|
|Stack|All data relevant and specific to the thread (exceptions, procedure calls, etc.)|
|Thread Local Storage|Pointers for allocating storage to a unique data environment|
|Stack Argument|Unique value assigned to each thread|
|Context Structure|Holds machine register values maintained by the kernel|


# Virtual Memory
Virtual Memory is a critical componenet of how windows internals work and interact with each other. Virtual memory allows other internal components to ineract with memory as if it was physical memory without the risk of collisions between applications.

Virtual memory provides each process with a [private virtual address space](https://docs.microsoft.com/en-us/windows/win32/memory/virtual-address-space). A memory manager is used to translate virtual addresses to physical addresses. By having a private virtual address space and not directly writing to physical memory, processes have less risk of causing damage.

Virtual memory provides each process with a [private virtual address space](https://docs.microsoft.com/en-us/windows/win32/memory/virtual-address-space). A memory manager is used to translate virtual addresses to physical addresses. By having a private virtual address space and not directly writing to physical memory, processes have less risk of causing damage.

The memory manager will also use pages or transfers to handle memory. Applications may use more virtual memory than physical memory allocated; the memory manager will transfer or page virtual memory to the disk to solve this problem. 



The [Microsoft docs](https://docs.microsoft.com/en-us/troubleshoot/windows-client/deployment/dynamic-link-library#:~:text=A%20DLL%20is%20a%20library,common%20dialog%20box%20related%20functions.) describe a DLL as "a library that contains code and data that can be used by more than one program at the same time."

DLLs are used as one of the core functionalities behind application execution in Windows. From the [Windows documentation](https://docs.microsoft.com/en-us/troubleshoot/windows-client/deployment/dynamic-link-library#:~:text=A%20DLL%20is%20a%20library,common%20dialog%20box%20related%20functions.), "The use of DLLs helps promote modularization of code, code reuse, efficient memory usage, and reduced disk space. So, the operating system and the programs load faster, run faster, and take less disk space on the computer."

When a DLL is loaded as a function in a program, the DLL is assigned as a dependency. Since a program is dependent on a DLL, attackers can target the DLLs rather than the applications to control some aspect of execution or functionality.

- DLL Hijacking ([T1574.001](https://attack.mitre.org/techniques/T1574/001/))
- DLL Side-Loading ([T1574.002](https://attack.mitre.org/techniques/T1574/002/))
- DLL Injection ([T1055.001](https://attack.mitre.org/techniques/T1055/001/))

DLLs are created no different than any other project/application; they only require slight syntax modification to work. Below is an example of a DLL from the Visual C++ Win32 Dynamic-Link Library project.
```
#include "stdafx.h"
#define EXPORTING_DLL
#include "sampleDLL.h"
BOOL APIENTRY DllMain( HANDLE hModule, DWORD ul_reason_for_call, LPVOID lpReserved)
{
	return TRUE;
}
void HelloWorld()
{
	MessageBox(NULL, TEXT("Hello World"), TEXT("In a DLL"), MB_OK);
}
```

below is the header file for the DLL; it will define what functions are imported and exported. 
```cpp
#ifndef INDLL_H
    #define INDLL_H
    #ifdef EXPORTING_DLL
        extern __declspec(dllexport) void HelloWorld();
    #else
        extern __declspec(dllimport) void HelloWorld();
    #endif

#endif
```

The DLL has been created, but that still leaves the question of how are they used in an application?

DLLs can be loaded in a program using _load-time dynamic linking_ or _run-time dynamic linking_.

When loaded using _load-time dynamic linking_, explicit calls to the DLL functions are made from the application. You can only achieve this type of linking by providing a header (_.h_) and import library (_.lib_) file. Below is an example of calling an exported DLL function from an application.

```cpp
#include "stdafx.h"
#include "sampleDLL.h"
int APIENTRY WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow)
{
    HelloWorld();
    return 0;
}
```

When loaded using _run-time dynamic linking_, a separate function (`LoadLibrary` or `LoadLibraryEx`) is used to load the DLL at run time. Once loaded, you need to use `GetProcAddress` to identify the exported DLL function to call. Below is an example of loading and importing a DLL function in an application.

```cpp
...
typedef VOID (*DLLPROC) (LPTSTR);
...
HINSTANCE hinstDLL;
DLLPROC HelloWorld;
BOOL fFreeDLL;

hinstDLL = LoadLibrary("sampleDLL.dll");
if (hinstDLL != NULL)
{
    HelloWorld = (DLLPROC) GetProcAddress(hinstDLL, "HelloWorld");
    if (HelloWorld != NULL)
        (HelloWorld);
    fFreeDLL = FreeLibrary(hinstDLL);
}
...
```

In malicious code, threat actors will often use run-time dynamic linking more than load-time dynamic linking. This is because a malicious program may need to transfer files between memory regions, and transferring a single DLL is more manageable than importing using other file requirements.

# Portable Executable Format
Executables and applications are a large portion of how Windows internals operate at a higher level. The PE(portable executable) format defines the information about the executable and stored data. The PE format also defines the structure of how data components are stored. 

The PE (Portable Executable) format is an overarching structure for executable and object files. The PE (Portable Executable) and COFF (Common Object File Format) files make up the PE format.

PE data is most commonly seen in the hex dump of an executable file. Below we will break down a hex dump of calc.exe into the sections of PE data.

The structure of PE data is broken up into seven components,

The **DOS Header** defines the type of file

The `MZ` DOS header defines the file format as `.exe`. The DOS header can be seen in the hex dump section below.


The MZ DOS header degines the fule format as .exe


The **DOS Stub** is a program run by default at the beginning of a file that prints a compatibility message. This does not affect any functionality of the file for most users.

The DOS stub prints the message `This program cannot be run in DOS mode`. The DOS stub can be seen in the hex dump section below.


The PE File Header provides PE header information of the binary. Defines the format of the file, contains the signature and image file header, and other information headers.

The PE file header is the section with the least human-readable output. You can identify the start of the PE file header from the PE stub in the hex dump 


The **Image Optional Header** has a deceiving name and is an important part of the **PE File Header**

The **Data Dictionaries** are part of the image optional header. They point to the image data directory structure.

The **Section Table** will define the available sections and information in the image. As previously discussed, sections store the contents of the file, such as code, imports, and data. You can identify each section definition from the table in the hex dump section below.



Now that the headers have defined the format and function of the file, the sections can define the contents and data of the file.

|   |   |
|---|---|
|**Section  <br>**|**Purpose**|
|.text|Contains executable code and entry point|
|.data|Contains initialized data (strings, variables, etc.)|
|.rdata or .idata|Contains imports (Windows API) and DLLs.|
|.reloc|Contains relocation information|
|.rsrc|Contains application resources (images, etc.)|
|.debug|Contains debug information|


# Interacting with Windows Internals
Interacting with Windows internals may seem daunting, but it has been dramatically simplified. The most accessible and researched option to interact with windows internals is to interface through windows API calls. The windows API provides native functionality to interact with the windows operating system. The API contains the win32 api and, less commonly, the win64 API.


Most Windows internals components require interacting with physical hardware and memory.

The Windows kernel will control all programs and processes and bridge all software and hardware interactions. This is especially important since many Windows internals require interaction with memory in some form.

An application by default normally cannot interact with the kernel or modify physical hardware and requires an interface. This problem is solved through the use of processor modes and access levels.

A Windows processor has a _user_ and _kernel_ mode. The processor will switch between these modes depending on access and requested mode.

The switch between user mode and kernel mode is often facilitated by system and API calls. In documentation, this point is sometimes referred to as the "_Switching Point_."

|   |   |
|---|---|
|**User mode**|**Kernel Mode**|
|No direct hardware access|Direct hardware access|
|Creates a process in a private virtual address space|Ran in a single shared virtual address space|
|Access to "owned memory locations"|Access to entire physical memory|

Applications started  in user mode or "_userland"_ will stay in that mode until a system call is made or interfaced through an API. When a system call is made, the application will switch modes. Pictured right is a flow chart describing this process.

When looking at how languages interact with the Win32 API, this process can become further warped; the application will go through the language runtime before going through the API. The most common example is C# executing through the CLR before interacting with the Win32 API and making system calls.

We will inject a message box into our local process to demonstrate a proof-of-concept to interact with memory.

1. Allocate local process memory for the message box.
2. Write/copy the message box to allocated memory.
3. Execute the message box from local process memory.

At step one, we can use `OpenProcess` to obtain the handle of the specified process.

```cpp
HANDLE hProcess = OpenProcess(
	PROCESS_ALL_ACCESS, // Defines access rights
	FALSE, // Target handle will not be inhereted
	DWORD(atoi(argv[1])) // Local process supplied by command-line arguments 
);
```

at step two, we can use VirtualAllocEx to allocate a region of memory with the payload buffer

```cpp
remoteBuffer = VirtualAllocEx(
	hProcess, // Opened target process
	NULL, 
	sizeof payload, // Region size of memory allocation
	(MEM_RESERVE | MEM_COMMIT), // Reserves and commits pages
	PAGE_EXECUTE_READWRITE // Enables execution and read/write access to the commited pages
);
```

At step three, we can use `WriteProcessMemory` to write the payload to the allocated region of memory.

```cpp
WriteProcessMemory(
	hProcess, // Opened target process
	remoteBuffer, // Allocated memory region
	payload, // Data to write
	sizeof payload, // byte size of data
	NULL
);
```
At step four, we can use CreateRemoteThread to execute our payload from memory
```cpp
remoteThread = CreateRemoteThread(
	hProcess, // Opened target process
	NULL, 
	0, // Default size of the stack
	(LPTHREAD_START_ROUTINE)remoteBuffer, // Pointer to the starting address of the thread
	NULL, 
	0, // Ran immediately after creation
	NULL
); 
```




























