## Table of Contents

- [Advanced GDB Cheatsheet for Pwn Challenges](#advanced\gdb\cheatsheet\for\pwn\challenges)
  - [Overview](#Overview)
  - [Basic GDB Commands](#Basic\GDB\Commands)
  - [Advanced Breakpoint Management](#Advanced\Breakpoint\Management)
  - [Memory Analysis and Manipulation](#Memory\Analysis\and\Manipulation)
  - [Exploitation Tools](#Exploitation\Tools)
  - [GDB Extensions for Pwn](#GDB\Extensions\for\Pwn)

# Advanced GDB Cheatsheet for Pwn Challenges

**Title:** GDB for Pwn Challenges  
**Date:** 2024-01-31  
**Categories:** Security, Exploitation  
**Tags:** GDB, Pwn, Exploit Development, Debugging

---

## Overview

This cheatsheet covers advanced usage of GDB (GNU Debugger) for tackling pwn challenges, which are typically security exercises focusing on exploiting vulnerabilities in programs. Key aspects include memory analysis, breakpoint management, and exploiting buffer overflows and other vulnerabilities.

---

## Basic GDB Commands

- **Start GDB:**
  ```bash
  gdb ./program
  ```

- **Setting Breakpoints:**
  ```gdb
  break *0xaddress
  break function_name
  ```

- **Running the Program:**
  ```gdb
  run [arguments]
  ```

- **Stepping Through the Program:**
  ```gdb
  next   # Next line of code
  step   # Step into functions
  ```

- **Inspecting Memory:**
  ```gdb
  x/10x $esp  # Examine 10 words at ESP
  ```

- **Continue Execution:**
  ```gdb
  continue
  ```

---

## Advanced Breakpoint Management

- **Conditional Breakpoints:**
  ```gdb
  break *0xaddress if $eax==0xvalue
  ```

- **Temporary Breakpoints:**
  ```gdb
  tbreak *0xaddress
  ```

- **Watchpoints (for monitoring variable changes):**
  ```gdb
  watch variable_name
  ```

---

## Memory Analysis and Manipulation

- **Examine Memory in Different Formats:**
  ```gdb
  x/10x $address  # Hexadecimal format
  x/10s $address  # String format
  x/10i $address  # Instruction format
  ```

- **Setting Memory:**
  ```gdb
  set {int}0xaddress = 0xvalue
  ```

- **Stack Analysis:**
  ```gdb
  info frame
  info registers
  ```

---

## Exploitation Tools

- **Finding Offset for Buffer Overflow:**
  Use external tools like `pattern_create` and `pattern_offset` to find the exact offset where the return address is overwritten.

- **Payload Crafting:**
  - Use Python or other scripting languages to craft payloads.
  - Consider NOP sleds, shellcode, and return-to-libc attacks.

- **Remote Debugging:**
  - Set up GDB server on the target machine: `gdbserver host:port ./program`
  - Connect with GDB from your machine: `gdb ./program`, then `target remote host:port`

---

## GDB Extensions for Pwn

- **PEDA - Python Exploit Development Assistance for GDB:**
  Provides enhanced visualizations and functionalities for exploit development.

- **GEF - GDB Enhanced Features:**
  Similar to PEDA, offers a suite of tools for exploit devs and reverse engineers.

- **Pwngdb:**
  GDB plugin tailored for pwn challenges, with features for heap exploitation, etc.

---
