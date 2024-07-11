## Table of Contents

- [N00bzCTF 2023](#n00bzctf\2023)
    - [Source Code](#Source\Code)
    - [Attack Explained](#Attack\Explained)
    - [Solution](#Solution)
        - [Summary](#Summary)

# N00bzCTF 2023
Simple crypto challenge~ 

### Source Code
```Python
from Crypto.Util.number import *
import time
flag = bytes_to_long(b'n00bz{***********************}')
e = 17
p = getPrime(1024)
q = getPrime(1024)
n = p*q
ct = pow(flag,e,n)
time.sleep(0.5)
print(f'{e = }')
print(f'{ct = }')
print(f'{n = }')

```
- e = 17 (Public exponent)
- p,q = 1024-bit prime numbers
- n = p*q (RSA Public Modulus)
- ct = pow(flag, e, n) ~ Flag is encrypted using RSA.


### Attack Explained
Hashtad's Broadcast Attack:
That attack takes advantage of the fact the same message (flag) is encrypted with the same *e* but different *n* values. Normally, RSA is secure against this, but when *e* is small (17), and you have *e* different ciphertexts with *e* different modulus, Hashtad's attack can be used to recover the original message.
Steps:
- 1. crt(ct, n) ~ Chinese Remainder Theorem (CRT). Computers M such that M mod n[i] = ct[i] for all i. Essentially, combining the 17 different ciphertext and modulus into a single equation.
- 2. iroot(M,e) ~ Takes the *e*-th root of *M* to recover the original message. this is possible because the CRT combined the equations in a way that effectively removed encryption.    


### Solution
```Python
from pwn import *
from sage.all import *
from Crypto.Util.number import *
from gmpy2 import iroot
n = []
ct = []
e = 17
for i in range(17):
	io = process('../src/chall.py')
	io.readuntil(b'ct = ')
	ct.append(int(io.readline().strip()))
	io.readuntil(b'n = ')
	n.append(int(io.readline().strip()))
M = crt(ct,n)
flag = long_to_bytes(iroot(M,e)[0])
print(flag)
```

##### Summary
- This CTF challenge is a classic example of why one should not use the same small public exponent
*e* for encrypting the same message with different moduli *n*. The Håstad's Broadcast Attack can defeat this weak configuration.
