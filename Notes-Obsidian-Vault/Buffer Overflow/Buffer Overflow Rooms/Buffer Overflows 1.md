## Table of Contents

      - [ERC](#ERC)
  - [Debugging a Program](#Debugging\a\Program)
- [Fuzzing Parameters](#fuzzing\parameters)
  - [Fuzzing Text Fields](#Fuzzing\Text\Fields)
- [Fuzzing Opened File](#fuzzing\opened\file)
- [Controlling EIP](#controlling\eip)
  - [Creating Unique Pattern](#Creating\Unique\Pattern)
  - [Controlling EIP](#Controlling\EIP)
- [Identifying Bad Characters](#identifying\bad\characters)
  - [Updating our exploit](#Updating\our\exploit)
  - [Comparing our Input](#Comparing\our\Input)
  - [Eliminating Bad Characters](#Eliminating\Bad\Characters)
  - [Using ESP Address](#Using\ESP\Address)
  - [Using JMP ESP](#Using\JMP\ESP)
      - [Locating Modules](#Locating\Modules)
      - [Searching for JMP ESP](#Searching\for\JMP\ESP)
    - [Handling Instruction Addresses in Payloads](#Handling\Instruction\Addresses\in\Payloads)
      - [Address Validation](#Address\Validation)
      - [Identifying Instructions](#Identifying\Instructions)
      - [Checking Other Resources](#Checking\Other\Resources)
      - [Efficient Search](#Efficient\Search)
      - [Caution](#Caution)
      - [Searching for Patterns](#Searching\for\Patterns)
  - [Summary](#Summary)

To successfully identify and exploit buffer overflows in Windows programs, we need to debug the program to follow its execution flow and its data in memory. There are many tools we can use for debugging, like [Immunity Debugger](https://www.immunityinc.com/products/debugger/index.html), [OllyDBG](http://www.ollydbg.de/), [WinGDB](http://wingdb.com/), or [IDA Pro](https://www.hex-rays.com/products/ida/). However, these debuggers are either outdated (Immunity/OllyDBG) or need a pro license to use (WinGDB/IDA).

In this module, we will be using [x64dbg](https://github.com/x64dbg/x64dbg), which is an excellent Windows debugger aimed specifically at binary exploitation and reverse engineering. `x64dbg` is an open-source tool developed and maintained by the community and also supports x64 debugging (unlike Immunity), so we can keep using it when we want to move to Windows x64 buffer overflows.

In addition to the debugger itself, we will utilize a binary exploitation plugin to efficiently carry out many tasks required for identifying and exploiting buffer overflows. One popular plugin is [mona.py](https://github.com/x64dbg/mona), which is an excellent binary exploitation plugin, though it is no longer maintained, does not support x64, and runs on Python2 instead of Python3.  
So instead, we will be using [ERC.Xdbg](https://github.com/Andy53/ERC.Xdbg), which is an open-source binary exploitation plugin for x64dbg.

To install `x64dbg`, we can follow the instructions as shown in its [GitHub Page](https://github.com/x64dbg/x64dbg), and go to the [latest release](https://github.com/x64dbg/x64dbg/releases/tag/snapshot) page, and download the `snapshot_<SNIP>.zip` file. Once we download it in our Windows VM, we can extract the `zip` archive content, rename the `release` folder to something like `x64dbg`, and move it to our `C:\Program Files` folder, or keep it in any folder we want.

Finally, we can double-click on `C:\Program Files\x64dbg\x96dbg.exe` to register the shell extension and add a shortcut to our Desktop.

#### ERC

To install the `ERC` plugin, we can go to the [release page](https://github.com/Andy53/ERC.Xdbg/releases), and download the `zip` archive that matches our VM (`x64` or `x32`), which in our case is `ERC.Xdbg_32-<SNIP>.zip`. Once we download it to our Windows VM, we can extract its content into `x32dbg` plugins folder located in `C:\Program Files\x64dbg\x32\plugins`.


When that's complete, the plugin should be ready for use. So, once we run `x32dbg`, we can type `ERC --help` in the command bar at the bottom to view `ERC`'s help menu.

To view the `ERC`'s output, we must switch to the `Log` tab by clicking on it or by clicking `Alt+L`, as we can see below:

We can also set a default working directory to have all output files saved to, using the following command:

  ERC

```powershell-session
ERC --config SetWorkingDirectory C:\Users\htb-student\Desktop\
```


## Debugging a Program

Whenever we want to debug a program, we can either run it through `x32dbg`, or run it separately and then attach to its process through `x32dbg`.

To open a program with `x32dbg`, we can select `File>Open` or press `F3`, which will prompt us to select the program to be debugged. If we wanted to attach to a process/program that is already running, we could select `File>Attach` or press `Alt+A`, and it will present us with various running processes accessible by our user: ![Attach Process](https://academy.hackthebox.com/storage/modules/89/win32bof_attach_process.jpg)

We can select the process we want to debug and click on `Attach` to start debugging it.

Tip: If we wanted to debug a process and it was not shown in the "Attach Window", we can try running x32dbg as an admin, by clicking on `File > Restart as Admin`, and then we will have access to all running processes on our VM.

# Fuzzing Parameters
For stack-based buffer overflow exploitation, we usually follow four main steps to identify and exploit buffer overflow vulnerablity:
- Fuzzing parameters
- Controlling EIP
- Identifying Bad characters
- Finding a return instruction
- Jumping to shellcode

Depending on the program's size, potential input fields include:

|**Field**|**Example**|
|---|---|
|`Text Input Fields`|- Program's "license registration" field.  <br>- Various text fields found in the program's preferences.|
|`Opened Files`|Any file that the program can open.|
|`Program Arguments`|Various arguments accepted by the program during runtime.|
|`Remote Resources`|Any files or resources loaded by the program on run time or on a certain condition.|

As any program may have many of these types of parameters, and each may have to be fuzzed with various kinds of inputs, we should attempt to select parameters with the highest possibilities of overflows and start fuzzing them. We should look for a field that expects a short input, like a field that sets the date, as the date is usually short so that the developers may expect a short input only.

Another common thing we should look for is fields that are expected to be processed somehow, like the license number registration field, as it will probably be run on a specific function to test whether it is a correct license number. License numbers also tend to have a specific length so that developers may be expecting a certain length only, and if we provide a long enough input, it may overflow the input field.

The same applies to opened files, as opened files tend to be processed after being opened. While developers may keep a very long buffer for opened files, certain files are expected to be shorter, like configuration files, and if we provide a long input, it may overflow the buffer. Certain file types tend to cause overflow vulnerabilities, like `.wav` files or `.m3u` files, due to the vulnerabilities in the libraries that process these types of files.

With that in mind, let's start fuzzing some fields in our program.


## Fuzzing Text Fields

```powershell
python -c "print('A'*10000)"
```


# Fuzzing Opened File
Both the progam's File menu and clicking encode button seem to accept .wav files.


# Controlling EIP

Another method of finding `EIP`'s offset is by using a unique pattern as our input and then seeing which values fill `EIP` to calculate precisely how far away it is from the beginning of our pattern. For example, we can send a pattern of sequential numbers, 'i.e. `12345678...`', and see which numbers would fill `EIP`. However, this is not a very practical method, as once numbers start getting larger, it would be difficult to know which number it is since it may be part of one number and part of another number. Furthermore, as numbers start getting 2 or 3 digits long, they would no longer indicate the actual offset since each number would fill multiple bytes. As we can see, using numbers as our pattern would not work.

The best way to calculate the exact offset of `EIP` is through sending a unique, non-repeating pattern of characters, such that we can view the characters that fill `EIP` and search for them in our unique pattern. Since it's a unique non-repeating pattern, we will only find one match, which would give us the exact offset of `EIP`.

## Creating Unique Pattern

We can generate a unique pattern with `pattern_create` either in our `PwnBox` instance or right within our debugger `x32dbg` with the `ERC` plugin. To do so in `PwnBox`, we can use the following command:

```shell-session
gdxqpardo@htb[/htb]$ /usr/bin/msf-pattern_create -l 5000
```

As we can see, we can use `ERC --pattern c 5000` to get our pattern


We can start by creating a new function with `def eip_offset():`, and then create our `payload` variable as a `bytes` object and paste between the parenthesis the `Ascii:` output from `Pattern_Create_1.txt`. So, we can click on the Windows Search bar at the bottom and write `IDLE`, which would open the Python3 editor, and then click `ctrl+N` to start writing a new python script where we can start writing our code. Our initial code should look as follows:

Code: python

```python
def eip_offset():
    payload = bytes("Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac"
                    ...SNIP...
                    "Gi3Gi4Gi5Gi6Gi7Gi8Gi9Gj0Gj1Gj2Gj3Gj4Gj5Gj6Gj7Gj8Gj9Gk0Gk1Gk2Gk3Gk4Gk5Gk",
					"utf-8")
```


Next, under the same `eip_offset()` function, we will write `payload` to a file called `pattern.wav`, by adding the following lines:

```python
  with open('pattern.wav', 'wb') as f:
        f.write(payload)
```

Now that we have our pattern saved into a `.wav` file, we can load it into our program. We should ensure that the program is running and is attached to `x32dbg`, and then we can open our file as we did in the previous section. We can click on the `restart` button in `x32dbg` to restart our program if our previous input had crashed it:  
![](https://academy.hackthebox.com/storage/modules/89/win32bof_x32dbg_restart.jpg)

Once we do, we should see that our program crashes due to the long input. Most importantly, we should see that the `EIP` register got overwritten with part of our unique pattern: ![Pattern EIP](https://academy.hackthebox.com/storage/modules/89/win32bof_pattern_eip.jpg)

Now we can use the value of `EIP` to calculate the offset. We can once again do it in our `PwnBox` with `msf-pattern_offset` (the counterpart of `msf-pattern_create`), by using the hex value in `EIP`, as follows:

```shell-session
gdxqpardo@htb[/htb]$ /usr/bin/msf-pattern_offset -q 31684630

[*] Exact match at offset 4112
```

As we can see, it tells us that our `EIP` offset is `4112` bytes. We can also stay in the `Windows` VM and use `ERC` to calculate the same offset. First, we should get the ASCII value of the hex bytes found in `EIP`, by right-clicking on `EIP` and selecting `Modify Value`, or by clicking on `EIP` and then clicking Enter. Once we do, we will see various representations of the `EIP` value, with ASCII being the last one: ![ASCII EIP](https://academy.hackthebox.com/storage/modules/89/win32bof_pattern_eip_ascii.jpg)

The hex value found in `EIP` represents the string `1hF0`. Now, we can use `ERC --pattern o 1hF0` to get the pattern offset: ![Pattern Offset](https://academy.hackthebox.com/storage/modules/89/win32bof_pattern_offset.jpg)

We once again get `4112` bytes as our `EIP` offset.


## Controlling EIP

Our final step is to ensure we can control what value goes into `EIP`. Knowing the offset, we know exactly how far our `EIP` is from the start of the buffer. So, if we send `4112` bytes, the next 4 bytes would be the ones that fill `EIP`.

Let's add another function, `eip_control()`, to our `win32bof_exploit.py` and create an `offset` variable with the offset we found. Then, we'll create a `buffer` variable with a string of `A` bytes as long as our offset to fill the buffer space, and an `eip` variable with the value we want `EIP` to be, which we will use as `4` bytes of `B`. Finally, we'll add both to a `payload` variable and write it to `control.wav`, as follows:

Code: python

```python
def eip_control():
    offset = 4112
    buffer = b"A"*offset
    eip = b"B"*4
    payload = buffer + eip
    
    with open('control.wav', 'wb') as f:
        f.write(payload)

eip_control()
```


# Identifying Bad Characters

To identify bad characters, we have to send all characters after filling the `EIP` address, which is after `4112` + `4` bytes. We then check whether any of the characters got removed by the program or if our input got truncated prematurely after a specific character.

To do this, we would need two files:

1. A `.wav` file with all characters to load into the program
2. A `.bin` file to compare with our input in memory

We can use `ERC` to generate the `.bin` file and generate a list of all characters to create our `.wav` file. To do so, we can use the `ERC --bytearray` command:

![ERC Byte Array](https://academy.hackthebox.com/storage/modules/89/win32bof_erc_bytearry.jpg)

This also creates two files on our Desktop:

- `ByteArray_1.txt`: Which contains the string of all characters we can use in our python exploit
- `ByteArray_1.bin`: Which we can use with `ERC` later to compare with our input in memory
## Updating our exploit

The next step would be to generate a `.wav` file with the characters string generated by `ERC`. We will once again write a new function `bad_chars()`, and use a similar code to the `eip_control()` function, but will use the characters under `C#` in `ByteArray_1.txt`. We will create a new list of bytes `all_chars = bytes([])`, and paste the characters between the brackets. We will then write to `chars.wav` the same `payload` from `eip_control()`, and add after it `all_chars`. The final function would look as follows:


```python
def bad_chars():
    all_chars = bytes([
        0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07,
        ...SNIP...
        0xF8, 0xF9, 0xFA, 0xFB, 0xFC, 0xFD, 0xFE, 0xFF
    ])
    
    offset = 4112
    buffer = b"A"*offset
    eip = b"B"*4
    payload = buffer + eip + all_chars
    
    with open('chars.wav', 'wb') as f:
        f.write(payload)

bad_chars()
```


Note how we added a `b` before `A` and `B` to turn them into bytes

We can now run our exploit with `F5` to generate the `chars.wav` file.

---

## Comparing our Input

Now we can restart our program in `x32dbg` and load `chars.wav` to it. Once we do, we can start comparing our input in memory and seeing whether any characters are missing. To do so, we can check the Stack pane on the bottom right of `x32dbg`, which should be aligned exactly at the beginning of our input: ![Byte Stack](https://academy.hackthebox.com/storage/modules/89/win32bof_bytes_stack.jpg)

We can now manually go through the stack line by line from right to left and ensure that all hex values are present, from `0x00` to `0xff`. As this may take a while, and we would entirely rely on our eyes, we may miss a character or two. So, we will once again utilize `ERC` to make the comparison for us. It will easily compare our input in memory to all characters.

We must first copy the address of `ESP` since this is where our input is located. We can do this by right-clicking on it and selecting `Copy value`, or clicking `[Ctrl + C]`: ![Byte ESP](https://academy.hackthebox.com/storage/modules/89/win32bof_bytes_esp.jpg)

Once we have the value of `ESP`, we can use `ERC --compare` and give it the `ESP` address and the location of the `.bin` file that contains all characters, as follows:

```cmd-session
ERC --compare 0014F974 C:\Users\htb-student\Desktop\ByteArray_1.bin
```


What this command will do is compare byte-by-byte both our input in `ESP` and all characters that we generated earlier in `ByteArray_1.bin`: ![Byte Compare 1](https://academy.hackthebox.com/storage/modules/89/win32bof_bytes_compare.jpg)

As we can see, this places each byte from both locations next to each other to quickly spot any issues. The output we seek is where all bytes from both locations are the same, with no differences whatsoever. However, we see that after the first character, `00`, all remaining bytes are different. `This indicates that 0x00 truncated the remaining input, and hence it should be considered a bad character.`

---

## Eliminating Bad Characters

Now that we have identified the first bad character, we should use `--bytearray` again to generate a list of all characters without the bad characters, which we can specify with `-bytes 0x00,0x0a,0x0d...etc.`. So, we will use the following command:

```cmd-session
ERC --bytearray -bytes 0x00
```

Now, let's use this command with `ERC` again to generate the new file and use it to update our exploit:

![ERC Byte Array 2](https://academy.hackthebox.com/storage/modules/89/win32bof_erc_bytearry_2.jpg)

As we can see, this time, it said `excluding: 00`, and the array table does not include `00` at the beginning. So, let's go to the generated output file `ByteArray_2.txt`, copy the new bytes under `C#`, and place them in our exploit, which should now look as follows:

```python
def bad_chars():
    all_chars = bytes([
        0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08
```

---

## Using ESP Address

Let's first try the most basic method of writing the address of the top of the stack `ESP`. Once we write an address to `EIP` and the program crashes on the return instruction `ret`, the debugger would stop at that point, and the `ESP` address at the point would match the beginning of our shellcode, similarly to how we saw our characters on the stack when looking for bad characters. We can take note of the `ESP` address at this point, which in this case is `0014F974`:

![Pattern EIP](https://academy.hackthebox.com/storage/modules/89/win32bof_pattern_eip.jpg)

Tip: We can also look at the stack on the bottom right pane to find the same details.

This method may work for this particular program, but it is not a very reliable method on Windows machines. First, the input we are attacking here is an audio file, so we see that all characters are allowed without bad characters. However, in many cases, we may be attacking a string input or a program argument, in which case `0x00` would be a bad character, and we would not use the address of `ESP` since it begins with `00`.

Another reason is that the address of `ESP` may not be the same on all machines. So, it may work throughout the debugging and exploit development process but may not be the same address when we fire the exploit at a real target, as it may have a different `ESP` address, at which point our exploit would fail.

Nevertheless, let's note this address and continue, and we'll test both methods in the next section.

---

## Using JMP ESP

The more reliable way of executing shellcode loaded on the stack is to find an instruction used by the program that directs the program's execution flow to the stack. We can use several such instructions, but we will be using the most basic one, `JMP ESP`, that jumps to the top of the stack and continues the execution.

#### Locating Modules

To find this instruction, we must look through executables and libraries loaded by our program. This includes:

1. The program's `.exe` file
2. The program's own `.dll` libraries
3. Any Windows `.dll` libraries used by the program


To find a list of all loaded files by the program, we can use `ERC --ModuleInfo`, as follows: ![Module Info](https://academy.hackthebox.com/storage/modules/89/win32bof_module_info.jpg)

We find many modules loaded by the program. However, we can skip any files with:

- `NXCompat`: As we are looking for a `JMP ESP` instruction, so the file should not have stack execution protection.
- `Rebase` or `ASLR`: Since these protections would cause the addresses to change between runs

As for `OS DLL`, if we are running on a newer Windows version like Windows 10, we can expect all OS DLL files to have all memory protections present, so we would not use any of them. If we were attacking an older Windows version like Windows XP, many of the loaded OS DLLs likely have no protections so that we can look for `JMP ESP` instructions in them as well.

 `The best option is to use an instruction from the program itself, as we'll be sure that this address will exist regardless of the version of Windows running the program`.


#### Searching for JMP ESP

Now that we have a list of loaded files that may include the instruction we are looking for, we can search them for usable instructions. To access any of these files, we can go to the `Symbols` tab by clicking on it or hitting `alt+e`:

![Module Symbols](https://academy.hackthebox.com/storage/modules/89/win32bof_module_symbols.jpg)

We can start with `cdextract.exe` and double-click it to open view and search its instructions. To search for the `JMP ESP` instruction within the instructions of this file, we can click `ctrl+f`, which allows us to search for any instruction within the opened file `cdextract.exe`: ![Find Command](https://academy.hackthebox.com/storage/modules/89/win32bof_find_command.jpg)

We can enter `jmp esp`, and it should show us if this file contains any of the instructions we searched for: ![Find JMP ESP](https://academy.hackthebox.com/storage/modules/89/win32bof_find_jmp_esp.jpg)

As we can see, we found the following matches:

Code: shell

```shell
Address  Disassembly
00419D0B jmp esp
00463B91 jmp esp
00477A8B jmp esp
0047E58B jmp esp
004979F4 jmp esp
```


### Handling Instruction Addresses in Payloads

#### Address Validation

- When using an ESP address in payloads, ensure the instruction address lacks bad characters. Bad characters can truncate the payload, causing the attack to fail.

#### Identifying Instructions

- Double-click on results in the disassembler to confirm they are the desired instructions, e.g., `JMP ESP`.

#### Checking Other Resources

- In addition to the main file, examine loaded `.dll` files for useful instructions.
    - Navigate to the Symbols tab.
    - Double-click the desired file.
    - Search for instructions like `JMP ESP`.

#### Efficient Search

- For a large list of loaded modules, use the "Search For > All Modules > Command" option.
    - Input the instruction (`jmp esp`) to search across all modules.

#### Caution

- Bulk searching may yield unusable results due to binary protections or inaccessibility from your program.
- It's advisable to search within individual files first for better reliability.

#### Searching for Patterns

Another example of a basic command to jump to the stack is `PUSH ESP` followed by `RET`. Since we are searching for two instructions, in this case, we should search using the machine code rather than the assembly instructions. We can use [Online Assemblers](https://defuse.ca/online-x86-assembler.htm), or the `msf-nasm_shell` tool found in `PwnBox` to convert any assembly instructions to machine code. Both of these take an assembly instruction and give us the corresponding machine code.

After using one of these, we would find that the machine code for `JMP ESP` is `FFE4`, and for `PUSH ESP; RET` is `54C3`. Now we can search using this pattern by clicking `ctrl+b` in the `CPU` pane and entering the pattern `54C3`:

![Find Pattern](https://academy.hackthebox.com/storage/modules/89/win32bof_find_pattern.jpg)

Once we do, we would find a few other addresses we can use as well: ![Find Pattern PUSH ESP](https://academy.hackthebox.com/storage/modules/89/win32bof_find_pattern_push_esp.jpg)

We can double-click any of them and confirm that it is indeed a `PUSH ESP` instruction followed by a `RET` instruction:

![PUSH ESP](https://academy.hackthebox.com/storage/modules/89/win32bof_pattern_push_esp.jpg)

---

## Summary

We have discussed many methods to find an instruction that would execute the shellcode we load on the stack:

1. We can use the `ESP` address
2. We can search loaded modules with disabled security for `JMP ESP` instructions
3. We can search for Assembly Instructions or search for machine code patterns
4. Any address we pick must not contain any bad characters



















