## Table of Contents

- [What is it?](#what\is\it?)
  - [Checklist](#Checklist)
    - [Steghide](#Steghide)
          - [Useful Commands](#Useful\Commands)
    - [Foremost](#Foremost)
          - [Useful Commands](#Useful\Commands)
    - [Exiftool](#Exiftool)
    - [Binwalk](#Binwalk)
    - [Foremost](#Foremost)
    - [zsteg](#zsteg)

# What is it?
Practice of hiding a message within other non-secret text or data. This has been used as a way to protect valuable information in many cultures through history. There are all sorts of facets of of Steganography in cybersecurity.  
## Checklist
1. Verify the file extension is correct
2. Basics first, go for low hanging fruit.
3. Many of the tools used are dependent on file type.
- [ ] File Type - `file>` (check the file information)
- [ ] `strings` - View all strings in the file
	- Ex: `strings -n 7 -t x filename.png` (`-n 7`: Strings of length 7+, `-t x`)
- [ ] `exiftool` - View all the image metadata
	- [Jeffrey's Image Metadata Viewer](http://exif.regex.info/exif.cgi) 
- [ ] `binwalk` - Use `binwalk` to check image's for hidden embedded files.
	- `binwalk -Me filename.png` (`-Me` is used to recursively extract any files)
- [ ] `pngcheck` - Look for optional/correct broken chunks. This is vital if the image appears to be corrupted.
	- `pngcheck -vtp7f filename.png` - View all info (`-V`: Verbose, `t7`: display text chunks, `p`: displays contents of some other optional chunks and `f`: forces continuation after major errors are encountered)
- [ ] Extract LSB Data (`Least Significant Bit`) - `zsteg`
- [ ] Find a password? `steghide extract -sf filename.png`
### Steghide
Hides data in various kinds of images and audio files.
(`JPEG`, `BMP`, `WAV`, `AU`).
- Also useful for extracting embedded/encrypted data from within files.
###### Useful Commands
`steghide info <file>` - Displays info about a file whether it has embedded data or not
`steghide extract -sf <file>` - Extracts embedded data from a file
### Foremost
Recovers files based on their headers, footers and internal data structures.
Useful for `.PNG`
###### Useful Commands
`foremost -i <file>` - extracts data from the given file
### Exiftool
Shows hidden metadata of the image or the file, exiftool is very useful.
`exiftool <file>`
### Binwalk
Tool for searching binary files like images and audio files for embedded files and data. Can be installed with `apt`.
`binwalk <file>` - displays the embedded data in the given file
`binwalk -e <file>` - Displays and extracts the data from the given file
`binwalk --dd ".*" file` - Displays and extracts the data from the given file
### Foremost
Recovers files based on their headers, footers, and internal data structures. It is especially useful when dealing with `png` images. You can select the file that Formost will extract cy changing the config file at `/etc/formost.conf`. It can be installed with `apt`
`foremost -i <file>` - extracts data from the given file

### zsteg
[zsteg](https://github.com/zed-0xff/zsteg)