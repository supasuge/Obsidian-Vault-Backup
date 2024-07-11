## Table of Contents

- [Enumerating Processes](#Enumerating\Processes)
    - [CreateToolhelp32Snapshot](#CreateToolhelp32Snapshot)
    - [PROCESSENTRY32 struct](#PROCESSENTRY32\struct)



### Enumerating Processes

*Before* being able to inject a DLL into a process, a destination/target process ust be chosed. 

Fiest step to remote process injection is usally ot enumerate the running processes on the machine to know of potential target processes that can be injected. The `process ID (or PID)` is required to open a handle to the target

Module that creates a function that performs process enumeration to determine all the running pocesses:`GetRemoteHandle` will be used to perform an enumeration of all running processes. Opening a handle on the target process and returning both PID and handle to the process.

### CreateToolhelp32Snapshot
use ^^ with the `TH32cs_SNAPPROCESS` flag for its first parameter, which takes a snapshot of all processes funning on the sys at the moment of execution

### PROCESSENTRY32 struct
Once snapshot is taken, Process32First is used to get info for the first process in snap shot. 

Both Process32First and process32Next require a PROCESSENTRY32 structure to be passed in for their second parameter. after the struct is passed in, the functions will populate the strut with information about the prcess. The `PROCESSENTRY32` 

```c
typedef struct tagPROCESSENTRY32 {
  DWORD     dwSize;
  DWORD     cntUsage;
  DWORD     th32ProcessID;              // The process ID
  ULONG_PTR th32DefaultHeapID;
  DWORD     th32ModuleID;
  DWORD     cntThreads;
  DWORD     th32ParentProcessID;        // Process ID of the parent process
  LONG      pcPriClassBase;
  DWORD     dwFlags;
  CHAR      szExeFile[MAX_PATH];        // The name of the executable file for the process
} PROCESSENTRY32;
```


After `Process32First` or `Process32Next` populate the struct, the data can be extracted from the struct by using the dot operator. For example, to extract the PID use `PROCESSENTRY32.th32ProcessID`.

