## Table of Contents


Pigeonhole effect- There are a set number of different output values for the hash function, but you can give it any size input. As there are more inputs that outputs, some of the inputs must give the same output. If you have 128 pigeons and 96 pigeonholes, some of the pigeons are going to have to share.

On Linux, password hashes are stored in /etc/shadow. This file is normally only readable by root. They used to be stored in /etc/passwd, and were readable by everyone.

On Windows, password hashes are stored in the SAM. Windows tries to prevent normal users from dumping them, but tools like mimikatz exist for this. Importantly, the hashes found there are split into NT hashes and LM hashes.

Here's a quick table of the most Unix style password prefixes that you'll see.



|   |   |
|---|---|
|Prefix|Algorithm|
|$1$|md5crypt, used in Cisco stuff and older Linux/Unix systems|
|$2$, $2a$, $2b$, $2x$, $2y$|Bcrypt (Popular for web applications)|
|$6$|sha512crypt (Default for most Linux/Unix systems)|


