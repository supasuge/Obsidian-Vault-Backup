## Table of Contents

      - [Additional Resources](#Additional\Resources)
- [Reverse Engineering](#reverse\engineering)

Reverse Engineering is the systematic approach to determine the inner workings of an application by inspecting the application's logic flow and responses to specific inputs. 

**Common RE activities**:
- Fuzzing
- Decompilation
- Disassemble
	-  [Binary Ninja](https://binary.ninja/)
	- [Ghidra](https://ghidra-sre.org/)
	- [Radare](https://www.radare.org/r/)
	- [IDAPro](https://www.hex-rays.com/ida-pro/)
- Debugging
	Allows you to perform dynamic analysis and set break points to stepo through the execution of a program. Another option would be to perform active analysis and step through the code with something like `GDB, or GEF`


#### Additional Resources
- [A master list of reverse engineering resources](https://github.com/wtsxDev/reverse-engineering): Books, Courses, Practice, Tools, Binary Format, Analysis, Bytecode Analysis, Dynamic Analysis, Document Analysis, Scripting
- [pwn.college](https://pwn.college/): Free RE exercises and challenges, among many other relevant cybersecurity topics (Assembly (ARM, Intel), Binary Exploitation, Shellcoding)
- [Microcorruption](https://microcorruption.com/): RE exercises and challenges.
- [Challenges.re](https://challenges.re/): RE exercises.
- [Beginners.re](https://beginners.re/): beginner’s guide to RE.


[StackZero - Reverse Engineering](https://www.stackzero.net/reverse-engineering/)

# Reverse Engineering
These challenges are typically quite hard for beginner's. This checklist does not fully cover all things Reverse Engineering; as it's and extensive and difficult topic. However, the goal of this checklist is to give you a foundation to play with reverse engineering a bit and begin to start poking around and asking questions.
1. Whenever you get a file, issuing the `file` command is useful to know what the file really is
2. Use `strings <filename>` command to read the string in the binary to find some clues.
3. Strong knowledge of C, and Assembly Language  computer architecture is required for this category of challenges
