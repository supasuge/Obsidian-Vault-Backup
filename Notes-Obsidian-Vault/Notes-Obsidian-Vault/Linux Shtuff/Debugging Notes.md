## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [This finally fixed it good fucking lord. Fuck you wireshark, what did I ever do to u??????](#this\finally\fixed\it\good\fucking\lord.\fuck\you\wireshark,\what\did\i\ever\do\to\u??????)

## Table of Contents

- [This finally fixed it good fucking lord. Fuck you wireshark, what did I ever do to u??????](#this\finally\fixed\it\good\fucking\lord.\fuck\you\wireshark,\what\did\i\ever\do\to\u??????)

```
#Mmmmmhm-Kk cool love it lets go
E: The package libwireshark16 needs to be reinstalled, but I can't find an archive for it.
                                                          
┌──(kali㉿kali)-[~/Desktop/HTBAcademy/Assembly]
└─$ sudo dpkg --remove --force-all libwireshark16
dpkg: libwireshark16:amd64: dependency problems, but removing anyway as you requested:
 wireshark-qt depends on libwireshark16 (>= 4.0.0).
 wireshark-common depends on libwireshark16 (>= 4.0.7-1).
 tshark depends on libwireshark16 (>= 4.0.0).
#REEEEEEEEEEEEEEEEEEEEEEEEEEEEE
dpkg: warning: overriding problem because --force enabled:
dpkg: warning: package is in a very bad inconsistent state; you should
 reinstall it before attempting a removal
(Reading database ... 398274 files and directories currently installed.)
Removing libwireshark16:amd64 (4.0.7-1) ...
Processing triggers for libc-bin (2.37-12) ...
ldconfig: File /lib/x86_64-linux-gnu/libwireshark.so.16.0.10.dpkg-new is empty, not checked.
                                                          
┌──(kali㉿kali)-[~/Desktop/HTBAcademy/Assembly]
└─$ sudo apt-get update && sudo apt-get upgrade -y
Get:1 http://mirrors.jevincanders.net/kali kali-rolling InRelease [41.2 kB]
Get:2 http://mirrors.jevincanders.net/kali kali-rolling/main amd64 Packages [19.4 MB]
Get:3 http://mirrors.jevincanders.net/kali kali-rolling/main amd64 Contents (deb) [45.6 MB]
Get:4 http://mirrors.jevincanders.net/kali kali-rolling/contrib amd64 Packages [122 kB]
Get:5 http://mirrors.jevincanders.net/kali kali-rolling/contrib amd64 Contents (deb) [294 kB]
Fetched 65.5 MB in 42s (1576 kB/s)                       
Reading package lists... Done
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 tshark : Depends: libwireshark16 (>= 4.0.0) but it is not installable
 wireshark-common : Depends: libwireshark16 (>= 4.0.7-1) but it is not installable

#WTFOK
 wireshark-qt : Depends: libwireshark16 (>= 4.0.0) but it is not installable
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
                                                          


# This finally fixed it good fucking lord. Fuck you wireshark, what did I ever do to u??????
┌──(kali㉿kali)-[~/Desktop/HTBAcademy/Assembly]
└─$ sudo apt --fix-broken install                 
Reading package lists... Done

```