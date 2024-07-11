## Table of Contents

    - [System Memory Management](#System\Memory\Management)
    - [Memory locations](#Memory\locations)
        - [Linux Filesystems](#Linux\Filesystems)
- [Reconstructed via infocmp from file: /usr/share/terminfo/x/xterm-256color](#reconstructed\via\infocmp\from\file:\/usr/share/terminfo/x/xterm-256color)
      - [xterm Command Line Patterns](#xterm\Command\Line\Patterns)
    - [Monitoring Programs/Processes](#Monitoring\Programs/Processes)

- Linux Kernel
- GNU Utilities
- Graphical Desktop environment
- Application Software

![[Pasted image 20240107070056.png]]
The kernel is primarily responsible for four main functions:
- System memory management
- Software program management
- Hardware management
- Filesystem Management

### System Memory Management
![[Pasted image 20240107070639.png]]

### Memory locations 
- Grouped into  blocks called *pages*. The kernel locates each page of memory either in the physical memory or the swap space. The kernel then maintains a table of the memory pages that indicates which pages are in physical memory, and which pages are swapped out to disk.

- Kernel keeps track of which memory *pages* are in use and automatically copies memory pages that have not been accessed for a period of time to the swap space area (called *swapping out*) 
- When a program wants access to a memory page that has been swapper out the kernel must make room for it in physical memiry by swapping out a different memory page, and swap in the required

You can see the current status of the virtual memory on your linux system by viewing the special `/proc/meminfo` file. 

Ex: 
![[Pasted image 20240107071907.png]]

- Each process running on the linux system has its own private memory pages. `ipcs` command allows to view the current shared memory pages on the system. The output froma  sample `ipcs`:
- To facilitate fata sharing, you can create shared memory pages. Multiple processes can read and write to and from a common shared memory area. The kernel maintains and administers the shared memory ares and follow individual processes access to the shared area.

![[Pasted image 20240107073159.png]]

- Some linux implementations contain a table of processes to start automatically on bootup. On Linux systems this table is usually located in the special file `/etc/inittabs`
- 

##### Linux Filesystems
`etx` - Linux Extended file system - The OG
`ext2` - Second extended filesystem, provided advanced features over ext
`ext3` - Third extended filesystem, provided advanced features over ext
`hpfs` - OS/2 high performance filesystem
`jfs` - IBM's journaling file system
`iso9660` - ISO 9660 filesystem (CD-ROMs)
`minix` - MINIX filesystem
`msdos` - Microsoft FAT16
`ncp` - Netware filesystem
`nfs` - Network File System
`ntfs` - Support for Microsoft NT filesystem
`proc` - Access to system information
`ReiserFS` - Advanced Linux file system for better performance and disk recovery
`smb` - Samba SMB filesystem for network access
`sysv` - Olver Unix filesystem
`ufs` - BSD filesystem
`umsdos` - Unix-Like filesystem that resides on top of MSDOS
`vfat` - Windows 95 filesystem (FAT32)
`XFS` - High~performance 64-bit journaling filesystem

`/usr/share/terminfo`
`/etc/terminfo`
`/lib/terminfo`

```
<reeeeeeYeper>$ infocmp
#       Reconstructed via infocmp from file: /usr/share/terminfo/x/xterm-256color
xterm-256color|xterm with 256 colors,
        am, bce, ccc, km, mc5i, mir, msgr, npc, xenl,
        colors#0x100, cols#80, it#8, lines#24, pairs#0x10000,
        acsc=``aaffggiijjkkllmmnnooppqqrrssttuuvvwwxxyyzz{{||}}~~,
        bel=^G, blink=\E[5m, bold=\E[1m, cbt=\E[Z, civis=\E[?25l,
        clear=\E[H\E[2J, cnorm=\E[?12l\E[?25h, cr=\r,
        csr=\E[%i%p1%d;%p2%dr, cub=\E[%p1%dD, cub1=^H,
        cud=\E[%p1%dB, cud1=\n, cuf=\E[%p1%dC, cuf1=\E[C,
        cup=\E[%i%p1%d;%p2%dH, cuu=\E[%p1%dA, cuu1=\E[A,
        cvvis=\E[?12;25h, dch=\E[%p1%dP, dch1=\E[P, dim=\E[2m,
        dl=\E[%p1%dM, dl1=\E[M, ech=\E[%p1%dX, ed=\E[J, el=\E[K,
        el1=\E[1K, flash=\E[?5h$<100/>\E[?5l, home=\E[H,
        hpa=\E[%i%p1%dG, ht=^I, hts=\EH, ich=\E[%p1%d@,
        il=\E[%p1%dL, il1=\E[L, ind=\n, indn=\E[%p1%dS,
        initc=\E]4;%p1%d;rgb:%p2%{255}%*%{1000}%/%2.2X/%p3%{255}%*%{1000}%/%2.2X/%p4%{255}%*%{1000}%/%2.2X\E\\,
        invis=\E[8m, is2=\E[!p\E[?3;4l\E[4l\E>, kDC=\E[3;2~,
        kEND=\E[1;2F, kHOM=\E[1;2H, kIC=\E[2;2~, kLFT=\E[1;2D,
        kNXT=\E[6;2~, kPRV=\E[5;2~, kRIT=\E[1;2C, ka1=\EOw,
        ka3=\EOy, kb2=\EOu, kbeg=\EOE, kbs=^?, kc1=\EOq, kc3=\EOs,
        kcbt=\E[Z, kcub1=\EOD, kcud1=\EOB, kcuf1=\EOC, kcuu1=\EOA,
        kdch1=\E[3~, kend=\EOF, kent=\EOM, kf1=\EOP, kf10=\E[21~,
        kf11=\E[23~, kf12=\E[24~, kf13=\E[1;2P, kf14=\E[1;2Q,
        kf15=\E[1;2R, kf16=\E[1;2S, kf17=\E[15;2~, kf18=\E[17;2~,
        kf19=\E[18;2~, kf2=\EOQ, kf20=\E[19;2~, kf21=\E[20;2~,
        kf22=\E[21;2~, kf23=\E[23;2~, kf24=\E[24;2~,
        kf25=\E[1;5P, kf26=\E[1;5Q, kf27=\E[1;5R, kf28=\E[1;5S,
        kf29=\E[15;5~, kf3=\EOR, kf30=\E[17;5~, kf31=\E[18;5~,
        kf32=\E[19;5~, kf33=\E[20;5~, kf34=\E[21;5~,
        kf35=\E[23;5~, kf36=\E[24;5~, kf37=\E[1;6P, kf38=\E[1;6Q,
        kf39=\E[1;6R, kf4=\EOS, kf40=\E[1;6S, kf41=\E[15;6~,
        kf42=\E[17;6~, kf43=\E[18;6~, kf44=\E[19;6~,
        kf45=\E[20;6~, kf46=\E[21;6~, kf47=\E[23;6~,
        kf48=\E[24;6~, kf49=\E[1;3P, kf5=\E[15~, kf50=\E[1;3Q,
        kf51=\E[1;3R, kf52=\E[1;3S, kf53=\E[15;3~, kf54=\E[17;3~,
        kf55=\E[18;3~, kf56=\E[19;3~, kf57=\E[20;3~,
        kf58=\E[21;3~, kf59=\E[23;3~, kf6=\E[17~, kf60=\E[24;3~,
        kf61=\E[1;4P, kf62=\E[1;4Q, kf63=\E[1;4R, kf7=\E[18~,
        kf8=\E[19~, kf9=\E[20~, khome=\EOH, kich1=\E[2~,
        kind=\E[1;2B, kmous=\E[<, knp=\E[6~, kpp=\E[5~,
        kri=\E[1;2A, mc0=\E[i, mc4=\E[4i, mc5=\E[5i, meml=\El,
        memu=\Em, mgc=\E[?69l, nel=\EE, oc=\E]104\007,
        op=\E[39;49m, rc=\E8, rep=%p1%c\E[%p2%{1}%-%db,
        rev=\E[7m, ri=\EM, rin=\E[%p1%dT, ritm=\E[23m, rmacs=\E(B,
        rmam=\E[?7l, rmcup=\E[?1049l\E[23;0;0t, rmir=\E[4l,
        rmkx=\E[?1l\E>, rmm=\E[?1034l, rmso=\E[27m, rmul=\E[24m,
        rs1=\Ec\E]104\007, rs2=\E[!p\E[?3;4l\E[4l\E>, sc=\E7,
        setab=\E[%?%p1%{8}%<%t4%p1%d%e%p1%{16}%<%t10%p1%{8}%-%d%e48;5;%p1%d%;m,
        setaf=\E[%?%p1%{8}%<%t3%p1%d%e%p1%{16}%<%t9%p1%{8}%-%d%e38;5;%p1%d%;m,
        sgr=%?%p9%t\E(0%e\E(B%;\E[0%?%p6%t;1%;%?%p5%t;2%;%?%p2%t;4%;%?%p1%p3%|%t;7%;%?%p4%t;5%;%?%p7%t;8%;m,
        sgr0=\E(B\E[m, sitm=\E[3m, smacs=\E(0, smam=\E[?7h,
        smcup=\E[?1049h\E[22;0;0t, smglp=\E[?69h\E[%i%p1%ds,
        smglr=\E[?69h\E[%i%p1%d;%p2%ds,
        smgrp=\E[?69h\E[%i;%p1%ds, smir=\E[4h, smkx=\E[?1h\E=,
        smm=\E[?1034h, smso=\E[7m, smul=\E[4m, tbc=\E[3g,
        u6=\E[%i%d;%dR, u7=\E[6n, u8=\E[?%[;0123456789]c,
        u9=\E[c, vpa=\E[%i%p1%dd,
```


#### xterm Command Line Patterns
`132` - Default xterm does not allow 132 characters per line mode
`ah` - always highligh the text
`aw` - auto-line-wrap is enabled
`bc` - Enables text cursor blinking
`bg color` - Specify the color to use for the background
`cm` - Disables recognition of ANSI color change control codes
`fb font` - Specify the font to use for bold text
`fg color` - Specify the color to use for the foreground text
`fn font` - Specify the font to use for wide text
`hc color` - Specify the color to use for highlighted text
`j` - Use jump scrolling, scrolling multiple lines at a time
`l` - Enable logging screen data to a log file
`lf filename` - Specify the file name to use for screen logging
`mb` - Ring a margin bell when the cursor reaches


![[Pasted image 20240107075849.png]]

### Monitoring Programs/Processes
When a program(binary) runs on the system, it's referred to as a **process**. To examine these processes, you'll need to become familiar with the `ps` command, the Swiss Army knife of utilities. It can produce lots of information about all the programs running on your system.

![[Pasted image 20240107080546.png]]



















































































































































































































































