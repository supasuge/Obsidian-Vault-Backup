## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Back In time](#Back\In\time)
- [File Signatures/Magic bytes](#file\signatures/magic\bytes)

## Table of Contents

  - [Back In time](#Back\In\time)
- [File Signatures/Magic bytes](#file\signatures/magic\bytes)

8410138**Common DOS commands and Utilities:  
**

|   |   |
|:-:|:-:|
|CD|Change Directory|
|DIR|Lists all files and directories in the current directory|
|TYPE|Displays the contents of a text file|
|CLS|Clears the screen|
|HELP|Provides help information for DOS commands|
|EDIT|The MS-DOS Editor|

## Back In time
- Goal: Restore the `AC2023.BAK` file found in the root directory using the backup tool found in the `C:\Tools\BACKUP` directory. navigate to this directory and run the command `bumaster.exe C:\AC2023.BAK`

# File Signatures/Magic bytes
`File Signatures`, often referred to as the `magic bytes` is the specific byte sequence at the beginning of a file that identify or verify its content type and format. These bytes often have corresponding ASCII characters, allowing for easier human readability. 




```bash
PNG: 89 50 4E 47 0D 0A 1A 0A

GIF: 47 49 46 38

Windows and DOS Executables: 4D 5A

Linux ELF Executables: 7F 45 4C 46

MP3 Audio File: 49 44 33

```

Let's see this in action by creating our own ﻿DOS executable.

Navigate to the `C:\DEV\HELLO` directory. Here, you will see `HELLO.C`, which is a simple program that we will be compiling into a DOS executable.









