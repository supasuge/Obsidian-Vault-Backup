## Table of Contents

- [Getting Started with File Forensics](#getting\started\with\file\forensics)
  - [Metadata](#Metadata)
          - [Image File Analysis](#Image\File\Analysis)
      - [Timestamps](#Timestamps)
  - [Steganography](#Steganography)
    - [File System Analysis](#File\System\Analysis)
      - [Filesystems](#Filesystems)

- Archive Files (ZIP, TGZ)
- Image File Formats (JPG, GIF, BMP, PNG)
- Filesystem images (EXT4)
- Packet Captures (PCAP, PCAPNG)
- PDF
- Video (MP4, WAV, MP3)
- Microsoft Office formats: (RTF, OLE, OOXML)
# Getting Started with File Forensics
When analyzing file formats, a file-format-aware hex-editor can become quite handy. File signatures/extensions are not always the sole way to identify the type of a file. Files have certain leading bytes called `file signatures` or `magic bytes` (as they are often reffered to by some). which are bytes within a file used to identify the format of the file. Generally they are 2-4 bytes long, found at the beginning of a file. File signature analysis is important to identify the format (file type)
## Metadata
The data about data, different types of files have different metadata because the underlying structures are not the same (I.e., file signature). 
###### Image File Analysis
`exiftool -k [file]`
#### Timestamps
Timestamps are data that indicate the time of certain events (MAC)
- Modification: When a file was modified
- Access: When a fiel was read or accessed
- Creation: When a file was created
## Steganography
Art of hiding data in images or audio files. This is quite rare in the real world, steganography is mainly seen in CTF challenges. Although it is not entirely uncommon to see malware or other similar malicious code embedded into a image/file of some sort inconspicously.
**Ex:** (LSB) - Least Significant Bits
- [Stegsolve](https://github.com/zardus/ctf-tools/blob/master/stegsolve/install): used to apply various steganography techniques to image files in an attempt to detect and extract hidden data.
- [Steghide](http://steghide.sourceforge.net/): hide data in various kinds of image- and audio-files.
- [zsteg](https://github.com/zed-0xff/zsteg): detect hidden data in PNG and GMP files.
- [OpenStego](https://www.openstego.com/): free steganography solution.
- [Foremost](https://www.kali.org/tools/foremost/): a forensic program to recover lost files based on their headers, footers, and internal data structures.
- [StegOnline](https://stegonline.georgeom.net/upload): online steganography tool.

### File System Analysis
Forensic CTF's will involve a full disk image. A disk image is a computer file containing the contents and structures of a disk volume. Such as a hard disk drive. 
```bash
mkdir /mnt/challenge
mount -t iso9660 challengefile /mnt/challenge
```
- Searching for a flag in a mounted disk image is difficicult
#### Filesystems
**NTFS**: New Technology File System (NTFS): A modern, well-formed filesystem that's mainly used on windows
**FAT**: A general purpose file syustem that is compatible with all major operating systems
**EXT**(extended): Created to be used with the Linux Kernel. (`ext4`) is the most recent.
**HFS**: Hierarchical File system: A file system developed by Appl for Mac OS X

