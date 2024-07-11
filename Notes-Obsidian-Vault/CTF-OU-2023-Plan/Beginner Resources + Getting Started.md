## Table of Contents

- [Getting Started](#getting\started)
          - [Time for some Linux!](#Time\for\some\Linux!)
- [Beginner CTF Resources](#beginner\ctf\resources)
  - [**CTF Platforms**](#**CTF\Platforms**)
  - [**Learning Resources**](#**Learning\Resources**)
  - [**Tools**](#**Tools**)
  - [**CTF Communities**](#**CTF\Communities**)
  - [**Tips for Beginners**](#**Tips\for\Beginners**)
  - [Reconnaissance](#Reconnaissance)
  - [Web Security](#Web\Security)
  - [Exploitationz](#Exploitationz)
  - [Cryptography tools](#Cryptography\tools)
  - [Forensics](#Forensics)
  - [Languages](#Languages)

# Getting Started
###### Time for some Linux!
- For first time users, I recommend having a command cheatsheet nearby. Especially for kali linux, as many of the tools can only be used come a Command-Line Interface. 
- Learning to control your computer with the command-line can be daunting, however it is highly rewarding and an invaluable skill to have regardless of the operating system your using! 
---
Pretty much any CTF is going to require a working knowledge of [Linux](https://en.wikipedia.org/wiki/Linux). Binary exploitation challenges in particular are almost exclusively limited to the Linux environment. What is linux you ask? Linux is an open source operating system. Because it is open source, developers have the opporunity to create various styles or *flavors* of linux. These styles of linux are referred to as Distributions. For now, we recommend setting up either Kali Linux, or Ubuntu, or Arch Linux(Not reccomended for first time Linux users/beginners).
# Beginner CTF Resources
**Here are some resources to help you get started with Capture the Flag (CTF) competitions:**
## **CTF Platforms**
---
- **PicoCTF:** Designed specifically for middle and high school students, offering fun and educational challenges. [PicoCTF](https://picoctf.org/)
- **Root The Box:** Focuses on realistic scenarios and encourages teamwork. [Root Me](https://www.root-me.org/))
- **CTFlearn:** Offers a wide range of challenges in various categories, with a focus on learning. [CTF Learn](https://ctflearn.com/))
- **CTFTime**: Here you can find more CTF's happening all over the world, as well as read/post writeups for different challenges. This site is very helpful, as many CTF challenges are modified slightly and recycled, meaning there is likely a solution online that could be easily adjusted for your use case.
## **Learning Resources**
---
- **CTF101:** Comprehensive guide covering various CTF categories and techniques.  [CTF 101](https://ctf101.org/))
- **OverTheWire:** Wargames to practice different skills, such as web exploitation, cryptography, and reverse engineering. ([https://overthewire.org/wargames/](https://overthewire.org/wargames/))
- **PentesterLab:** Hands-on exercises to learn about web application security. [Pentester Labs](https://pentesterlab.com/))
- **Trail of Bits CTF Field Guide:** Practical tips and strategies for approaching CTF challenges. [Trail of Bits](https://trailofbits.github.io/ctf/))
- **CTFLearn:** Gentle introduction to simple CTF challenges and more.
## **Tools**
---
- **Burp Suite:** Web application security testing tool. [BurpSuite]([https://portswigger.net/burp](https://portswigger.net/burp)
- **Wireshark:** Network protocol analyzer. ([Wireshark](https://www.wireshark.org/))
- **Kali Linux:** Distribution with pre-installed security tools. [kali linux](https://www.kali.org/))
	- It's recommended that you set this up on a VM (Virtual Box, or VMWare) and give it a shot! As it comes with a majority of the tools needed during CTF's Penetration testing.
- **Ghidra:** Reverse engineering framework from the NSA. ([https://ghidra-sre.org/](https://ghidra-sre.org/))
- [PwnTools](https://github.com/Gallopsled/pwntools#readme): Popular python library for easy exploit developement and interaction with different services/functionalities often found in CTF's.
- [CTF Tools Collections](https://github.com/zardus/ctf-tools) - Below
___

| Category | Source | Tool | Description |
| ---- | ---- | ---- | ---- |
| binary | Directory | [afl](http://lcamtuf.coredump.cx/afl/) | State-of-the-art fuzzer. |
| binary | Directory | [angr](http://angr.io) | Next-generation binary analysis engine from Shellphish. |
| binary | Directory | [barf](https://github.com/programa-stic/barf-project) | Binary Analysis and Reverse-engineering Framework. |
| binary | Directory | [bindead](https://bitbucket.org/mihaila/bindead/wiki/Home) | A static analysis tool for binaries. |
| binary | Library | [capstone](http://www.capstone-engine.org) | Multi-architecture disassembly framework. |
| binary | Directory | [checksec](https://github.com/slimm609/checksec.sh) | Check binary hardening settings. |
| binary | Directory | [codereason](https://github.com/trailofbits/codereason) | Semantic Binary Code Analysis Framework. |
| binary | Directory | [crosstool-ng](http://crosstool-ng.org/) | Cross-compilers and cross-architecture tools. |
| binary | Directory | [cross2](http://kozos.jp/books/asm/asm.html) | A set of cross-compilation tools from a Japanese book on C. |
| binary | Directory | [elfkickers](http://www.muppetlabs.com/~breadbox/software/elfkickers.html) | A set of utilities for working with ELF files. |
| binary | Directory | [elfparser](http://www.elfparser.com/) | Quickly determine the capabilities of an ELF binary through static analysis. |
| binary | Directory | [evilize](http://www.mathstat.dal.ca/~selinger/md5collision/) | Tool to create MD5 colliding binaries |
| binary | Directory | [gdb](http://www.gnu.org/software/gdb/) | Up-to-date gdb with python2 bindings. |
| binary | Directory | [gdb-heap](https://github.com/rogerhu/gdb-heap) | gdb extension for debugging heap issues. |
| binary | Directory | [gef](https://github.com/hugsy/gef) | Enhanced environment for gdb. |
| binary | Directory | [hongfuzz](https://github.com/google/honggfuzz) | A general-purpose, easy-to-use fuzzer with interesting analysis options. |
| binary | Library | [keystone](http://www.keystone-engine.org) | Lightweight multi-architecture assembler framework. |
| binary | Directory | [libheap](https://github.com/cloudburst/libheap) | gdb python library for examining the glibc heap (ptmalloc) |
| binary | Library | [lief](https://lief.quarkslab.com/) | Library to Instrument Executable Formats. |
| binary | Directory | [miasm](https://github.com/cea-sec/miasm) | Reverse engineering framework in Python. |
| binary | Directory | [one_gadget](https://github.com/david942j/one_gadget) | Magic gadget search for libc. |
| binary | Directory | [panda](https://github.com/moyix/panda) | Platform for Architecture-Neutral Dynamic Analysis. |
| binary | Directory | [pathgrind](https://github.com/codelion/pathgrind) | Path-based, symbolically-assisted fuzzer. |
| binary | Directory | [peda](https://github.com/longld/peda) | Enhanced environment for gdb. |
| binary | Directory | [preeny](https://github.com/zardus/preeny) | A collection of helpful preloads (compiled for many architectures!). |
| binary | Directory | [pwndbg](https://github.com/zachriggle/pwndbg) | Enhanced environment for gdb. Especially for pwning. |
| binary | Directory | [pwntools](https://github.com/Gallopsled/pwntools) | Useful CTF utilities. |
| binary | Directory | [python-pin](https://github.com/blankwall/Python_Pin) | Python bindings for pin. |
| binary | Directory | [qemu](http://qemu.org) | Latest version of qemu! |
| binary | Directory | [qira](http://qira.me) | Parallel, timeless debugger. |
| binary | Directory | [radare2](http://www.radare.org/) | Some crazy thing crowell likes. |
| binary | Directory | [rappel](https://github.com/yrp604/rappel) | A linux-based assembly REPL. |
| binary | Directory | [ropper](https://github.com/sashs/Ropper) | Another gadget finder. |
| binary | Directory | [rp++](https://github.com/0vercl0k/rp) | Another gadget finder. |
| binary | Directory | [rr](http://rr-project.org) | Record and Replay Debugging Framework |
| binary | Directory | [scratchabit](https://github.com/pfalcon/ScratchABit) | Easily retargetable and hackable interactive disassembler |
| binary | Directory | [scratchablock](https://github.com/pfalcon/ScratchABlock) | Yet another crippled decompiler project |
| binary | Directory | [seccomp-tools](https://github.com/david942j/seccomp-tools) | Provides powerful tools for seccomp analysis |
| binary | Directory | [shellnoob](https://github.com/reyammer/shellnoob) | Shellcode writing helper. |
| binary | Directory | [shellsploit](https://github.com/b3mb4m/shellsploit-framework) | Shellcode development kit. |
| binary | Directory | [snowman](https://github.com/yegord/snowman) | Cross-architecture decompiler. |
| binary | Directory | [taintgrind](https://github.com/wmkhoo/taintgrind) | A valgrind taint analysis tool. |
| binary | Library | [unicorn](http://www.unicorn-engine.org) | Multi-architecture CPU emulator framework. |
| binary | Directory | [valgrind](http://valgrind.org) | A Dynamic Binary Instrumentation framework with some built-in tools. |
| binary | Directory | [villoc](https://github.com/wapiflapi/villoc) | Visualization of heap operations. |
| binary | Directory | [virtualsocket](https://github.com/antoniobianchi333/virtualsocket) | A nice library to interact with binaries. |
| binary | Directory | [wcc](https://github.com/endrazine/wcc) | The Witchcraft Compiler Collection is a collection of compilation tools to perform binary black magic on the GNU/Linux and other POSIX platforms. |
| binary | Directory | [xrop](https://github.com/acama/xrop) | Gadget finder. |
| binary | Directory | [manticore](https://github.com/trailofbits/manticore) | Manticore is a prototyping tool for dynamic binary analysis, with support for symbolic execution, taint analysis, and binary instrumentation. |
| forensics | Directory | [binwalk](https://github.com/devttys0/binwalk.git) | Firmware (and arbitrary file) analysis tool. |
| forensics | Directory | [dislocker](http://www.hsc.fr/ressources/outils/dislocker/) | Tool for reading Bitlocker encrypted partitions. |
| forensics | Directory | [exetractor](https://github.com/kholia/exetractor-clone) | Unpacker for packed Python executables. Supports PyInstaller and py2exe. |
| forensics | Directory | [firmware-mod-kit](https://code.google.com/p/firmware-mod-kit/) | Tools for firmware packing/unpacking. |
| forensics | apt | [foremost](http://foremost.sourceforge.net/) | File carver. |
| forensics | Directory | [pdf-parser](http://blog.didierstevens.com/programs/pdf-tools/) | Tool for digging in PDF files |
| forensics | Directory | [peepdf](https://github.com/jesparza/peepdf) | Powerful Python tool to analyze PDF documents. |
| forensics | Directory | [scrdec](https://gist.github.com/bcse/1834878) | A decoder for encoded Windows Scripts. |
| forensics | Directory | [testdisk](http://www.cgsecurity.org/wiki/TestDisk) | Testdisk and photorec for file recovery. |
| crypto | Library | [codext](https://github.com/dhondta/python-codext) | Python codecs extension featuring CLI tools for encoding/decoding anything including AI-based guessing mode. |
| crypto | Directory | [cribdrag](https://github.com/SpiderLabs/cribdrag) | Interactive crib dragging tool (for crypto). |
| crypto | Directory | [fastcoll](https://www.win.tue.nl/hashclash/) | An md5sum collision generator. |
| crypto | Directory | [foresight](https://github.com/ALSchwalm/foresight) | A tool for predicting the output of random number generators. To run, launch "foresee". |
| crypto | Directory | [featherduster](https://github.com/nccgroup/featherduster) | An automated, modular cryptanalysis tool. |
| crypto | Directory | [galois](http://web.eecs.utk.edu/~plank/plank/papers/CS-07-593) | A fast galois field arithmetic library/toolkit. |
| crypto | Directory | [hashkill](https://github.com/gat3way/hashkill) | Hash cracker. |
| crypto | Directory | [hashpump](https://github.com/bwall/HashPump) | A tool for performing hash length extension attaacks. |
| crypto | Directory | [hashpump-partialhash](https://github.com/mheistermann/HashPump-partialhash) | Hashpump, supporting partially-unknown hashes. |
| crypto | Directory | [hash-identifier](https://code.google.com/p/hash-identifier/source/checkout) | Simple hash algorithm identifier. |
| crypto | Directory | [libc-database](https://github.com/niklasb/libc-database) | Build a database of libc offsets to simplify exploitation. |
| crypto | Directory | [littleblackbox](https://github.com/devttys0/littleblackbox) | Database of private SSL/SSH keys for embedded devices. |
| crypto | Directory | [msieve](http://sourceforge.net/projects/msieve/) | Msieve is a C library implementing a suite of algorithms to factor large integers. |
| crypto | Directory | [nonce-disrespect](https://github.com/nonce-disrespect/nonce-disrespect) | Nonce-Disrespecting Adversaries: Practical Forgery Attacks on GCM in TLS. |
| crypto | Directory | [pemcrack](https://github.com/robertdavidgraham/pemcrack) | SSL PEM file cracker. |
| crypto | Directory | [pkcrack](https://www.unix-ag.uni-kl.de/~conrad/krypto/pkcrack.html) | PkZip encryption cracker. |
| crypto | Directory | [python-paddingoracle](https://github.com/mwielgoszewski/python-paddingoracle) | Padding oracle attack automation. |
| crypto | Directory | [reveng](http://reveng.sourceforge.net/) | CRC finder. |
| crypto | Directory | [ssh_decoder](https://github.com/jjyg/ssh_decoder) | A tool for decoding ssh traffic. You will need `ruby1.8` from `https://launchpad.net/~brightbox/+archive/ubuntu/ruby-ng` to run this. Run with `ssh_decoder --help` for help, as running it with no arguments causes it to crash. |
| crypto | Directory | [sslsplit](https://github.com/droe/sslsplit) | SSL/TLS MITM. |
| crypto | Directory | [xortool](https://github.com/hellman/xortool) | XOR analysis tool. |
| crypto | Directory | [yafu](http://sourceforge.net/projects/yafu/) | Automated integer factorization. |
| web | Directory | [burpsuite](http://portswigger.net/burp) | Web proxy to do naughty web stuff. |
| web | Directory | [commix](https://github.com/stasinopoulos/commix) | Command injection and exploitation tool. |
| web | Directory | [dirb](http://dirb.sourceforge.net/) | Web path scanner. |
| web | Directory | [dirsearch](https://github.com/maurosoria/dirsearch) | Web path scanner. |
| web | Directory | [mitmproxy](https://mitmproxy.org/) | CLI Web proxy and python library. |
| web | Directory | [sqlmap](http://sqlmap.org/) | SQL injection automation engine. |
| web | Directory | [subbrute](https://github.com/TheRook/subbrute) | A DNS meta-query spider that enumerates DNS records, and subdomains. |
| web | Library | [webgrep](https://github.com/dhondta/webgrep) | `grep` for Web pages, with JS deobfuscation, CSS unminifying and OCR on images. |
| stego | apt | [pngtools](https://launchpad.net/ubuntu/+source/pngtools) | PNG's analysis tool. |
| stego | Directory | [sound-visualizer](http://www.sonicvisualiser.org/) | Audio file visualization. |
| stego | Directory | [steganabara](http://www.caesum.com/handbook/stego.htm) | Another image stenography solver. |
| stego | Directory | [stegano-tools](https://github.com/dhondta/stegano-tools) | A collection of text and image steganography tools (incl LSB, PVD, PIT). |
| stego | Directory | [stegdetect](http://www.outguess.org/) | Stenography detection/breaking tool. |
| stego | Docker | [stego-toolkit](https://github.com/DominicBreuker/stego-toolkit) | A docker image with dozens of steg tools. |
| stego | Directory | [stegsolve](http://www.caesum.com/handbook/stego.htm) | Image stenography solver. |
| stego | Directory | [stegosaurus](https://github.com/AngelKitty/stegosaurus) | A steganography tool for embedding arbitrary payloads in Python bytecode (pyc or pyo) files. |
| stego | Directory | [zsteg](https://github.com/zed-0xff/zsteg) | detect stegano-hidden data in PNG & BMP. |
| dsniff | apt | [dsniff](http://www.monkey.org/~dugsong/dsniff/) | Grabs passwords and other data from pcaps/network streams. |
| android |  Directory | [apktool](https://ibotpeaches.github.io/Apktool/) | Dissect, dis-assemble, and re-pack Android APKs |
| android |  Directory | [android-sdk](http://developer.android.com/sdk) | The android SDK (adb, emulator, etc). |
| misc |  Directory | [xspy](http://git.kali.org/gitweb/?p=packages/xspy.git;a=summary) | Tiny tool to spy on X sessions. |
| misc |  Directory | [z3](https://github.com/Z3Prover/z3) | Theorem prover from Microsoft Research. |
| misc |  Directory | [jdgui](http://jd.benow.ca/) | Java decompiler. |
| misc |  Directory | [veles](https://codisec.com/veles/) | Binary data analysis and visualization tool. |
| misc |  Directory | [youtube-dl](https://yt-dl.org/) | Latest version of the popular youtube downloader. |
| web | apt | [tor-browser](https://www.torproject.org/projects/torbrowser.html.en) | Useful when you need to hit a web challenge from different IPs. |
**Note:** This is not every tool needed for the CTF, nor is it all that is out there. One must first understand the challenge, and how to go about solving it before finding the right tool. A majority of the time, there are already tools pre-made by others. It's recommended to always look for existing tools first before attempting to write your own, although many challenges require you to write you own code at some point in order to get the solution.

## **CTF Communities**
---
- **CTFtime:** Calendar of upcoming CTF events and a global ranking system. [CTFTime](https://ctftime.org/))
- **CTF Discord servers:** Various communities for chatting and collaborating with other CTF players. These are valuable places to learn and converse with others, as no one person can do it all. I enjoy participating in CTF events as I learn new things each time.
## **Tips for Beginners**
---
- **Start with easy challenges:** Build confidence and foundational skills before moving on to more difficult tasks.
- **Don't be afraid to ask for help:** There are many online communities and forums where you can get assistance.
- **ChatGPT and other AI tools**: Although controversial for many, using ChatGPT to assist in solving problems is not cheating... HOWEVER, it's imperative you understand the underlying material. AI cannot and should not do anything and everything regardless of the advanced capabilities. One must first know the vocabulary and underlying concepts to construct better prompts and thus better outputs. 
- **Take Notes**: The more notes the better! As I'm sure just about any college student would know, the amount of information constantly being thrown your way can be overwhelming. This is especially true when it comes to the complex world of IT/Cyber security. Unless you have a photographic memory, it is far too much information for any single person to memorize for long periods. So it is important and highly recommend to develop a organized, and efficient note-taking methodology that is effective and beneficial for you.
- **Practice regularly:** The more you practice, the better you'll become at solving CTF challenges.
- **Collaborate with others:** Form a team with friends or join an online community to learn from others and share knowledge.
- **Create Writeups and read writeups from others!**: There are tons and tons of different sites for write ups of varying detail. It is very helpful to read as many of these as you can,  as these are extremely valuable  
- **Have fun!** CTFs are a great way to learn about cybersecurity and challenge yourself in a gamified environment.

## Reconnaissance
- `Nmap` - Powerful port scanner to enumerate open ports and services
- `netcat` - Read/write TCP/IP connections
- `masscan` - Rapidly scan internet-scale networks
- `shodan` - Very Popular Reconnaissance tool.
- [GoBuster](https://github.com/OJ/gobuster) - Directory/File brute forcing
- [Sublist3r](https://github.com/aboul3la/Sublist3r) - Subdomain enumeration
- [theHarvester](https://github.com/laramies/theHarvester) - Gather subdomain and email data
- [Maltego](https://github.com/laramies/theHarvester) - Visualize networks and track data flows

## Web Security
- `Burp Suite` - Intercept traffic and manipulate requests/responses
- `nikto` - Scan for common web vulnerabilities
- `dirbuster` - Brute force web directories
- `sqlmap` - Automated SQL injection
- [OWASP ZAP]() - Find web app vulnerabilities

## Exploitationz
- `Metasploit Framework` - Exploit code and payloads
- `Immunity Debugger` - Identify memory corruption bugs (Windows only)
- `ghidra` - Reverse engineering framework
- `pwntools` - Popular python library often used for CTF's for it's ease of use, as well as quick ability to develop exploits and similar tools. 
- `Binary Ninja` - Reverse engineering binary programs
- `mimikatz` - Extract credentials on Windows

## Cryptography tools
- `hashcat` - GPU-accelerated password cracking
- `John the Ripper` - Password hash cracker
- [CyberChef]() - A wide array of encoding and encryption functions
- `GPG` - Encrypt and sign data with PGP
- `fcrackzip` - ZIP file password cracker

## Forensics
- `Autopsy` - Analyze disk images
- `binwalk` - Firmware analysis
- [Volatility]() - Memory forensics
- `Wireshark` - Network traffic analysis
- [Steghide]() - Hide data in files
- `ffmpeg` - Audio files
- `gimp` - GNU Image Manipulation Program
- `exiftool` - View the exif data from a file along with other metadata. Can also be used to remove file exifdata
## Languages
- `Python` - Automation, exploits, tools
- `Bash` - Scripting daily tasks
- `C` - Understand buffer overflows and memory corruption


