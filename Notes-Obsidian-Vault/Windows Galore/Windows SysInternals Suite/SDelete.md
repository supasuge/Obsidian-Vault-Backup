## Table of Contents

- [DoD 5220.22-M Wipe Method](#DoD\5220.22-M\Wipe\Method)
    - [SDelete arguments](#SDelete\arguments)
- [SDelete Cheatsheet](#sdelete\cheatsheet)
  - [Basic Command Syntax](#Basic\Command\Syntax)
  - [Examples of Using SDelete](#Examples\of\Using\SDelete)
    - [1. Securely Delete a File](#1.\Securely\Delete\a\File)
    - [2. Securely Delete Multiple Files](#2.\Securely\Delete\Multiple\Files)
    - [3. Securely Delete a Directory](#3.\Securely\Delete\a\Directory)
    - [4. Delete Files Recursively](#4.\Delete\Files\Recursively)
    - [5. Multiple Overwrite Passes](#5.\Multiple\Overwrite\Passes)
    - [6. Securely Delete Files without Confirmation](#6.\Securely\Delete\Files\without\Confirmation)
    - [7. Cleanse Free Space with Zeroes](#7.\Cleanse\Free\Space\with\Zeroes)
    - [8. Cleanse Free Space with Random Data](#8.\Cleanse\Free\Space\with\Random\Data)
    - [9. Use Multiple Options Together](#9.\Use\Multiple\Options\Together)
  - [Notes](#Notes)

#securewipe #deletefiles #windows #sysinternals #wipedata #forensics 
https://learn.microsoft.com/en-us/sysinternals/downloads/file-and-disk-utilities
"SDelete is a cmd line utility that allows you to delete one or more files and or directories to cleanse the free space on a logical disk."

[BlackHills InfoSec Discord](https://discord.com/invite/bhis)

#### DoD 5220.22-M Wipe Method
Data sanitization is as follows:
- 1. Writes a zero and verifies the write.
- 2. Wires a one and verifies the write.
- 3. Writes a random characters and verifies the write.
https://www.lifewire.com/data-sanitization-methods-2626133
https://cmrr.ucsd.edu/resources/secure-erase.html


### SDelete arguments
#datashredding #windows #sdelete #args 
```powershell
-c : Clean Free Space
-f : Force arguments containing only letters to be treated as a file directory rather thana  disk.
-p : Specifies number of overwirte passes (defualt is 1)
-q : quiet mode
-r : Remove read-only attribute
-s : Recurse subdirectories
-z : Zero free space (good for VM Optimization)
-nobanne : do not display banner
```






# SDelete Cheatsheet

SDelete, part of the Sysinternals Suite, is a command-line utility that allows you to securely delete files, directories, and free space on a disk. It overwrites the data to make it unrecoverable.

## Basic Command Syntax
```
sdelete [-p passes] [-r] [-s] [-q] <file or directory>
sdelete [-p passes] [-z|-c] [drive letter]
```

- `-p passes`: Specifies the number of overwrite passes (default is 1).
- `-r`: Recurse subdirectories (applies to directory names).
- `-s`: Recurse subdirectories (applies to file names).
- `-q`: Specifies quiet mode (no confirmation).
- `-z`: Cleanse free space (write zeroes).
- `-c`: Cleanse free space (write random data).

## Examples of Using SDelete

### 1. Securely Delete a File
```
sdelete C:\path\to\file.txt
```
Securely deletes 'file.txt'.

### 2. Securely Delete Multiple Files
```
sdelete C:\path\to\file1.txt C:\path\to\file2.txt
```
Securely deletes 'file1.txt' and 'file2.txt'.

### 3. Securely Delete a Directory
```
sdelete -s C:\path\to\directory\
```
Securely deletes 'directory' and all its contents.

### 4. Delete Files Recursively
```
sdelete -s C:\path\to\directory\*.txt
```
Securely deletes all '.txt' files in 'directory' and its subdirectories.

### 5. Multiple Overwrite Passes
```
sdelete -p 3 C:\path\to\file.txt
```
Securely deletes 'file.txt' with 3 overwrite passes.

### 6. Securely Delete Files without Confirmation
```
sdelete -q C:\path\to\file.txt
```
Securely deletes 'file.txt' without asking for confirmation.

### 7. Cleanse Free Space with Zeroes
```
sdelete -z C:
```
Overwrites all free space on the C: drive with zeroes.

### 8. Cleanse Free Space with Random Data
```
sdelete -c D:
```
Overwrites all free space on the D: drive with random data.

### 9. Use Multiple Options Together
```
sdelete -s -p 2 -q C:\path\to\directory\
```
Securely deletes 'directory' and its contents with 2 overwrite passes without confirmation.

## Notes

- **Data Recovery**: Once data is overwritten by SDelete, it is permanently destroyed and cannot be recovered.
- **System Files**: Be cautious when using SDelete, especially with system files or directories.
- **Free Space Cleansing**: Cleansing free space can take a significant amount of time, depending on the size of the drive.

Always use SDelete with care, as data removed by this tool is unrecoverable. For more detailed information, consult the [SDelete documentation](https://docs.microsoft.com/en-us/sysinternals/downloads/sdelete).
