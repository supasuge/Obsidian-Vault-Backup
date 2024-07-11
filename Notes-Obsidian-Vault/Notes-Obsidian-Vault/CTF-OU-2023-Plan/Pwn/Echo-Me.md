## Table of Contents

    - [Tools](#Tools)
  - [Gathering information](#Gathering\information)
      - [Code Analysis](#Code\Analysis)
          - [Vulnerability: **Buffer Overflow**](#Vulnerability:\**Buffer\Overflow**)
          - [Resources](#Resources)

### Tools
- [checksec.sh - Download Link](https://www.trapkit.de/tools/checksec/checksec.sh)
- [GEF](https://github.com/hugsy/gef)
- GDB
	- This tool is reccomended for debugging source code, not stripped binaries without symbols and debugging binaries without symbols and debugging information. 

**Validating Installation**
![[Pasted image 20240103214438.png]]
```plaintext
77b8a7fd9393d10def665658a41176ee745d5c7969a4a0f43cefcc8a4cd90947
```
## Gathering information
When downloading a binary in CTF challenges, the first step is to run `file <FILE_NAME_HERE>` 
**Output example:**
![[Pasted image 20240103204722.png]]
#### Code Analysis
###### Vulnerability: **Buffer Overflow**
A buffer of size `char buffer[256];` is declared that holds 255 characters plus a null terminator.

The vulnerability in this code lies within `read(0, buffer, 256);`. This function reads up to 256 bytes of data from standard input (file descriptor `0`) into the buffer. Since `buffer` can only safely contain 255 characters plus a null terminator, reading 256 bytes can overflow the buffer. The program then outputs whatever is in the `buffer` using `puts(buffer);`

To exploit this challenge in order to print `flag.txt` you would generally need to:
1. Find Offset
2. Craft payload
3. Execute the payload

###### Resources
- [Bug Hunter's Diary - No Starch Press](https://paper.bobylive.com/Security/A%20Bug%20Hunter%20%20039%20s%20Diary%20%20A%20Guided%20Tour%20Through%20the%20Wilds%20of%20Software%20Security.pdf)
