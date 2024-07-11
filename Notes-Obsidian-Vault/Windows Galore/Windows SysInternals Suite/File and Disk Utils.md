## Table of Contents

- [Sysinternals Suite Cheatsheet: File and Disk Utilities](#sysinternals\suite\cheatsheet:\file\and\disk\utilities)
  - [Disk Usage (DU)](#Disk\Usage\(DU))
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [Disk2vhd](#Disk2vhd)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [DiskView](#DiskView)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [DiskExt](#DiskExt)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [Contig](#Contig)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [NTFSInfo](#NTFSInfo)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [FSUtil](#FSUtil)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [Junction](#Junction)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [MoveFile](#MoveFile)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [Streams](#Streams)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)
  - [Sigcheck](#Sigcheck)
    - [Description](#Description)
    - [Usage](#Usage)
    - [Examples](#Examples)

# Sysinternals Suite Cheatsheet: File and Disk Utilities

This cheatsheet covers the File and Disk utilities in the Sysinternals Suite, providing commands and examples for each tool.

## Disk Usage (DU)

### Description
Reports disk usage by directory.

### Usage
```
du [-c] [-l <levels>] [-u] [-q] [<directory>]
```

### Examples
- **Disk Usage of Current Directory**: `du`
- **Disk Usage with Subdirectory Levels**: `du -l 2 C:\path\to\directory`
- **CSV Format Output**: `du -c C:\path\to\directory`

## Disk2vhd

### Description
Converts physical disks into Virtual Hard Disk (VHD/VHDX) files for use in Microsoft Virtual PC or Hyper-V virtual machines.

### Usage
```
disk2vhd [options] <SourceDisk(s)> <VHDFileName>
```

### Examples
- **Convert Physical Disk to VHD**: `disk2vhd C: D: C:\vhd\disk.vhd`
- **Use VHDX Format**: `disk2vhd -x C: C:\vhd\disk.vhdx`

## DiskView

### Description
Graphical tool displaying the physical allocation of disk space.

### Usage
```
diskview
```

### Examples
- **Launch DiskView**: Simply run `diskview` and select a drive to analyze.

## DiskExt

### Description
Displays volume disk-mappings.

### Usage
```
diskext
```

### Examples
- **Show Volume Disk Mappings**: `diskext`

## Contig

### Description
Defragments a single file or all files in a directory.

### Usage
```
contig [-a] [-s] [-q] [-v] [file(s) or directory]
```

### Examples
- **Defragment a Single File**: `contig C:\path\to\file.txt`
- **Defragment All Files in Directory**: `contig -s C:\path\to\directory`

## NTFSInfo

### Description
Displays information about NTFS volumes, including the size and location of the Master File Table (MFT).

### Usage
```
ntfsinfo [-d] <drive letter>
```

### Examples
- **NTFS Volume Information**: `ntfsinfo C:`

## FSUtil

### Description
Displays or configures the file system properties.

### Usage
```
fsutil [behavior|dirty|file|fsinfo|hardlink|objectid|quota|repair|sparse|tiering|transaction|usn|volume] [options]
```

### Examples
- **List Drives**: `fsutil fsinfo drives`
- **Query Free Space**: `fsutil volume diskfree C:`

## Junction

### Description
Creates, lists, or deletes NTFS junction points.

### Usage
```
junction [-s] [-q] <junction directory> [target directory]
```

### Examples
- **Create a Junction Point**: `junction C:\path\to\junction C:\target\directory`
- **List Junction Points**: `junction -s C:\`

## MoveFile

### Description
Schedules file delete and rename operations for the next reboot.

### Usage
```
movefile <source> <destination>
```

### Examples
- **Schedule File Deletion**: `movefile C:\path\to\file.txt ""`
- **Schedule File Rename**: `movefile C:\path\to\oldfile.txt C:\path\to\newfile.txt`

## Streams

### Description
Reveals NTFS alternate data streams.

### Usage
```
streams [-s] [-d] <file or directory>
```

### Examples
- **Reveal File Streams**: `streams C:\path\to\file.txt`
- **Delete Streams from Directory**: `streams -d -s C:\path\to\directory`

## Sigcheck

### Description
Displays file version number, timestamp information, and digital signature details.

### Usage
```
sigcheck [-e] [-u] [-s] [-q] [-v] [file or directory]
```

### Examples
- **Check File Signature**: `sigcheck C:\path\to\file.exe`
- **Check Signatures in Directory**: `sigcheck -e -s C:\path\to\directory`

---

**Note:** Always use these utilities with care, especially when modifying system files or disk structures. For more comprehensive information, consult the [Sysinternals documentation](https://docs.microsoft.com/en-us/sysinternals/).