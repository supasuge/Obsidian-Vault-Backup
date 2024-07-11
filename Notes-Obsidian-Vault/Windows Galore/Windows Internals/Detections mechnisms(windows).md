## Table of Contents

- [Static/Signature Detection](#Static/Signature\Detection)
    - [Hashing Detection](#Hashing\Detection)
    - [Heuristic Detection](#Heuristic\Detection)
    - [Dynamic Heuristic Analysis (Sandbox Detection)](#Dynamic\Heuristic\Analysis\(Sandbox\Detection))
    - [Behavior-based Detection](#Behavior-based\Detection)
    - [API Hooking](#API\Hooking)
    - [IAT Checking](#IAT\Checking)
    - [Manual Analysis](#Manual\Analysis)
    - [What is a Windows Process?](#What\is\a\Windows\Process?)
    - [Process Threads](#Process\Threads)
    - [Process Memory](#Process\Memory)
    - [Memory Types](#Memory\Types)
    - [Process Environment Block (PEB)](#Process\Environment\Block\(PEB))
    - [PEB Structure](#PEB\Structure)
      - [BeingDebugged](#BeingDebugged)
      - [Ldr](#Ldr)
      - [ProcessParameters](#ProcessParameters)
    - [ProcessParameters](#ProcessParameters)
    - [AtlThunkSListPtr & AtlThunkSListPtr32](#AtlThunkSListPtr\&\AtlThunkSListPtr32)
    - [PostProcessInitRoutine](#PostProcessInitRoutine)
    - [SessionId](#SessionId)
    - [Thread Environment Block (TEB)](#Thread\Environment\Block\(TEB))
      - [TEB Structure](#TEB\Structure)
    - [ProcessEnvironmentBlock (PEB)](#ProcessEnvironmentBlock\(PEB))
    - [TlsSlots](#TlsSlots)
    - [TlsExpansionSlots](#TlsExpansionSlots)
    - [Process And Thread Handles](#Process\And\Thread\Handles)


Security solutions use several techniques to detect malicious software. It's important for one to understand what techniques security solutions use to detect or classify software as being malicious.

### Static/Signature Detection

A signature is a number of bytes or strings within a malware that uniquely identifies it. Other conditions can also be specified such as variable names and imported functions. Once the security solution scans a program, it attempts to match it to a list of known rules. These rules have to be pre-built and pushed to the security solution. [YARA](https://virustotal.github.io/yara/) is one tool that is used by security vendors to build detection rules. For example, if a shellcode contains a byte sequence that begins with `FC 48 83 E4 F0 E8 C0 00 00 00 41 51 41 50 52 51` then this can be used to detect that the payload is a Msfvenom's x64 exec payload. The same detection mechanism can be used against strings within the file.

Signature detection is easy to bypass but can be time-consuming. It's important to avoid hardcoding values in the malware that can be used to uniquely identify the implementation. The code that's presented throughout this course attempts to avoid hardcoding values that could be hardcoded and instead dynamically retrieves or calculates the values.

###  Hashing Detection

- **What is it?** A subset of static/signature detection.
- **How it Works?** Saves hashes (e.g., MD5, SHA256) of known malware in a database and compares file hashes against this database.
- **Evasion**: By altering a single byte in the file, its hash changes, making it undetectable by this method alone.

### Heuristic Detection

- **Purpose**: To detect unknown, modified, or new versions of malware.
- **Types**:
    - **Static Heuristic Analysis**: Decompiles the program and compares code snippets to known malware. If enough code matches, it's flagged.
    - **Dynamic Heuristic Analysis**: Analyzes program behavior in a sandbox environment.

### Dynamic Heuristic Analysis (Sandbox Detection)

- **What is it?** Analyzes file behavior in a sandboxed environment.
- **Detection**: Looks for sequences of suspicious or malicious actions.
- **Evasion**: Malware may have anti-sandbox techniques to detect sandbox environments. If detected, benign code is executed.

### Behavior-based Detection

- **What is it?** Monitors the running process for suspicious behavior.
- **Detection**: Observes actions like loading a DLL, calling Windows APIs, or connecting to the internet. Conducts in-memory scans of suspicious processes.
- **Evasion**: Make the malware behave benignly. Memory scans can be evaded using memory encryption.

### API Hooking

- **What is it?** Technique to monitor real-time code or process execution for malicious behaviors.
- **How it Works?** Intercepts commonly abused APIs and analyzes their parameters in real-time.
- **Advantages**: Can see content passed to the API after decryption or de-obfuscation. Combines real-time and behavior-based detection.
- **Illustration**: A diagram (not provided here) would show the high-level process of API hooking.


### IAT Checking

- **IAT (Import Address Table)**: A component of the PE (Portable Executable) structure.
- **Functionality**:
    - Contains function names used in the PE at runtime.
    - Contains libraries (DLLs) that export these functions.
- **Importance to Security**:
    - Provides information on which WinAPIs the executable uses.
    - Useful in identifying suspicious behaviors, e.g., ransomware might use file management and cryptographic functions.
    - When certain functions like `CreateFileA/W`, `SetFilePointer`, `Read/WriteFile`, `CryptCreateHash`, `CryptHashData`, `CryptGetHashParam` are spotted, the program may be flagged or scrutinized more.
- **Illustration**: A tool like `dumpbin.exe` can be used to inspect a binary's IAT.

### Manual Analysis

- **What is it?** Detailed examination of the malware by experts, often bypassing automated detection mechanisms.
- **Importance**:
    - Despite evading automated checks, manual analysis by knowledgeable defenders can detect malware.
    - Many security solutions send suspicious files to the cloud for more detailed analysis.
- **Evasion Techniques**:
    - Malware can employ anti-reversing techniques to hinder reverse engineering.
    - Techniques can include detecting the presence of a debugger or identifying a virtualized environment.
    - These techniques and their countermeasures are explored in more advanced topics.

### What is a Windows Process?

A Windows process is a program or application that is running on a Windows machine. A process can be started by either a user or by the system itself. The process consumes resources such as memory, disk space, and processor time to complete a task.

### Process Threads

Windows processes are made up of one or more threads that are all running concurrently. A thread is a set of instructions that can be executed independently within a process. Threads within a process can communicate and share data. Threads are scheduled for execution by the operating system and managed in the context of a process.

### Process Memory

Windows processes also use memory to store data and instructions. Memory is allocated to a process when it is created and the amount that is allocated can be set by the process itself. The operating system manages memory using both virtual and physical memory. Virtual memory allows the operating system to use more memory than what is physically available by creating a virtual address space that can be accessed by the applications. These virtual address spaces are divided into "pages" which are then allocated to processes
### Memory Types
Processes can have different types of memory:
- **Private memory** is dedicated to a single process and cannot be shared by other processes. This type of memory is used to store data that is specific to the process.
- **Mapped memory** can be shared between two or more processes. It is used to share data between processes, such as shared libraries, shared memory segments, and shared files. Mapped memory is visible to other processes, but is protected from being modified by other processes.
- **Image memory** contains the code and data of an executable file. It is used to store the code and data that is used by the process, such as the program's code, data, and resources. Image memory is often related to DLL files loaded into a process's address space.
### Process Environment Block (PEB)

The Process Environment Block (PEB) is a data structure in Windows that contains information about a process such as its parameters, startup information, allocated heap information, and loaded DLLs, in addition to others. It is used by the operating system to store information about processes as they are running, and is used by the Windows loader to launch applications. It also stores information about the process such as the process ID (PID) and the path to the executable.

Every process created has its own PEB data structure, that will contain its own set of information about it.
### PEB Structure

The PEB struct in C is shown below. The reserved members of this struct can be ignored.

```c
typedef struct _PEB {
  BYTE                          Reserved1[2];
  BYTE                          BeingDebugged;
  BYTE                          Reserved2[1];
  PVOID                         Reserved3[2];
  PPEB_LDR_DATA                 Ldr;
  PRTL_USER_PROCESS_PARAMETERS  ProcessParameters;
  PVOID                         Reserved4[3];
  PVOID                         AtlThunkSListPtr;
  PVOID                         Reserved5;
  ULONG                         Reserved6;
  PVOID                         Reserved7;
  ULONG                         Reserved8;
  ULONG                         AtlThunkSListPtr32;
  PVOID                         Reserved9[45];
  BYTE                          Reserved10[96];
  PPS_POST_PROCESS_INIT_ROUTINE PostProcessInitRoutine;
  BYTE                          Reserved11[128];
  PVOID                         Reserved12[1];
  ULONG                         SessionId;
} PEB, *PPEB;
```
#### BeingDebugged

BeingDebugged is a flag in the PEB structure that indicates whether the process is being debugged or not. It is set to 1 (TRUE) when the process is being debugged and 0 (FALSE) when it is not. It is used by the Windows loader to determine whether to launch the application with a debugger attached or not.

#### Ldr

Ldr is a pointer to a `PEB_LDR_DATA` structure in the Process Environment Block (PEB). This structure contains information about the process's loaded dynamic link library (DLL) modules. It includes a list of the DLLs loaded in the process, the base address of each DLL, and the size of each module. It is used by the Windows loader to keep track of DLLs loaded in the process. The `PEB_LDR_DATA` struct is shown below.

```c
typedef struct _PEB_LDR_DATA {
  BYTE       Reserved1[8];
  PVOID      Reserved2[3];
  LIST_ENTRY InMemoryOrderModuleList;
} PEB_LDR_DATA, *PPEB_LDR_DATA;
```
`Ldr` can be leveraged to find the base address of a particular DLL, as well as which functions reside within its memory space. This will be used in future modules to build a custom version of [GetModuleHandleA/W](https://learn.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getmodulehandlea) for added stealth.
#### ProcessParameters

ProcessParameters is a data structure in the PEB. It contains the command line parameters passed to the process when created. The Windows loader adds these parameters to the process's PEB structure. ProcessParameters is a pointer to the `RTL_USER_PROCESS_PARAMETERS` struct that's shown below.

```c
typedef struct _RTL_USER_PROCESS_PARAMETERS {
  BYTE           Reserved1[16];
  PVOID          Reserved2[10];
  UNICODE_STRING ImagePathName;
  UNICODE_STRING CommandLine;
} RTL_USER_PROCESS_PARAMETERS, *PRTL_USER_PROCESS_PARAMETERS;
```
### ProcessParameters

- Leveraged in future modules for actions such as command line spoofing.

### AtlThunkSListPtr & AtlThunkSListPtr32

- Used by the ATL (Active Template Library) module.
- **Purpose**: To store a pointer to a linked list of thunking functions.
- **Thunking Functions**: Used to call functions in a different address space, often representing functions exported from a DLL.
- The linked list helps the ATL module manage the thunking process.

### PostProcessInitRoutine

- Field in the PEB structure.
- **Purpose**: Stores a pointer to a function called post TLS (Thread Local Storage) initialization for all threads in a process.
- Can be used for any additional initialization tasks for the process.

### SessionId

- Unique identifier for a single session.
- Tracks user activity during the session.

### Thread Environment Block (TEB)

- A Windows data structure that stores information about a thread.
- Contains thread's environment, security context, and other related details.
- Stored in the thread's stack; used by Windows kernel to manage threads.

#### TEB Structure

cCopy code

`typedef struct _TEB {   PVOID Reserved1[12];   PPEB  ProcessEnvironmentBlock;   PVOID Reserved2[399];   BYTE  Reserved3[1952];   PVOID TlsSlots[64];   BYTE  Reserved4[8];   PVOID Reserved5[26];   PVOID ReservedForOle;   PVOID Reserved6[4];   PVOID TlsExpansionSlots; } TEB, *PTEB;`

### ProcessEnvironmentBlock (PEB)

- Pointer inside the TEB.
- Stores information about the currently running process.

### TlsSlots

- Locations in the TEB used to store thread-specific data.
- Each thread in Windows has its own TEB with a set of TLS slots.
- Used for data like thread-specific variables, handles, states, etc.

### TlsExpansionSlots

- Set of pointers in the TEB used to store thread-local storage data for a thread.
- Reserved for use by system DLLs.

### Process And Thread Handles

- Every process in Windows has a distinct process ID (PID).
- Every thread has a unique ID.
- **WinAPIs**:
    - `OpenProcess`: Opens a process object handle using its identifier.
    - `OpenThread`: Opens a thread object handle using its identifier.
- Opened handle can perform actions on its relative Windows object, like suspending a process/thread.
- Always close handles when not in use using `CloseHandle` to prevent handle leaks.



