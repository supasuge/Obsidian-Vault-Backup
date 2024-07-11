## Table of Contents

- [Based-as64](#based-as64)
  - [What is Base64?](#What\is\Base64?)
- [Can-You-XOR-or-NOT](#can-you-xor-or-not)
- [Caesar Salad](#caesar\salad)
- [Vigenere Oh My](#vigenere\oh\my)
- [Params and RSA](#params\and\rsa)
    - [Solution](#Solution)
- [PWN(Binary Exploitation)](#pwn(binary\exploitation))
  - [bb-pwn](#bb-pwn)
- [Forensics](#forensics)
  - [El-Exif](#El-Exif)

# Based-as64
#nofilesprovided
provide the string:
`R3JpenpDVEZ7YmFzZTY0X2lzX3MwX2JhczNkfQ==`
What is the flag provided once base64 decoded?
![[Pasted image 20240117110014.png]]
## What is Base64?
Base64 is an encoding technique commonly used in cryptography and other common web communication protocols. To base64 encode/decode from a bash shell:
`echo 'something' | base64` - Base64 Encode 'something'
`echo 'base64+==' | base64 -d` - Base64 Decode ''

# Can-You-XOR-or-NOT
#`ciphertext.txt` provided
![[Pasted image 20240117110122.png]]
This is an XOR challenge, it was created with `pwntools`. The flag is XOR'd with `grizzly_bears`.
Challenge participant needs to XOR the ciphertext with the given key in this challenge to decrypt the given ciphertext and obtain the flag.
```python
from pwn import *
a = b'blah'
b = b'blah'
c = xor(a,b)
print(c)
```


# Caesar Salad
This is a Caesar Cipher challenge, however it differs from the  traditional caesar cipher because it also operates on the numbers within the flag.
The caesar cipher is simply shifts the full alphabet depending on the keys.
Example:
key = 3
`ABC` -> `DEF`

#`CaesarSalsa.py` provided
#`ciphertext.txt` provided
are both provided to the challenge participant. To solve this challenge you simply shift each character <- 3 (subtract 3, including the numerical characters).




# Vigenere Oh My
![[Pasted image 20240117112125.png]]
For this challenge, resources will be provided to assist in cryptanalysis. For the file provided to the participant, the key will be redacted (`ABC`).

The string `GSKZAETGQUPWOVHLBIRJIHUJESGGSKZANYCGASUIMQVFIRJBZMABFCRTIRJBZMABFCRTCNEETGOALGSNGHBRPZ` will be provided for the challenge, and it will be noted that '{' and '}' have been removed for organization sake.

[Vigenere Cracking Tool + Visualization](https://www.simonsingh.net/The_Black_Chamber/vigenere_cracking_tool.html)
The tool above provides a visualized method of cracking the vigenere cipher. 
The first step in cracking the Vigenere cipher is to find repeated sequences of letters that appear more than once in the ciphertext. The most likely reason for such repetitions is that the same sequence of letters in the plaintext has been encipherd using the same part of the key. 
![[Pasted image 20240118135826.png]]
Show above are the repetitions in the ciphertext, and the spacing between them. The factors of spacing are indicated by crosses. If one factor is common to many of the spacing, then this is probably the length of the keyword.

From the above reptitions analysis, it can be noted that 3 is the most frequently occuring least common factor. The next step is to brute force the cipher using frequency analysis to hint towards the possible key until a readable english plaintext is obtained.
# Params and RSA
In this challenge $p$, $q$, $e$, $n$ are provided to the particpant. Along with an encrypted ciphertext using the RSA algorithm. The goal of this challenge is to simply introduce the RSA algorithm to the challenge participant and to write the code to take the parameters in there raw format, and solve for $d$ the private key which is then used to decrypt the flag.

![[Pasted image 20240117113424.png]]
#`parameters.txt` provided

### Solution
In RSA, 

# PWN(Binary Exploitation)
## bb-pwn
in the source code, it can be clearly seen that a buffer of size `256` is created however the vunction is vulnerable because it reads `512` bytes into the buffer. To exploit this challenge and get the flag, you must first find the address of `print_flag`.
Ex:
![[Pasted image 20240118131616.png]]

![[Pasted image 20240118131720.png]]

The adderss can be seen of `print_flag`:
```bash
0x000000000040119a <+4>:	push   rbp
```
![[Pasted image 20240118131856.png]]




# Forensics
## El-Exif
![[Pasted image 20240122104907.png]]

Above is the photo used for this challenge, the flag is simply hidden in the exifdata of the photo under `Description`.
![[Pasted image 20240122105012.png]]













