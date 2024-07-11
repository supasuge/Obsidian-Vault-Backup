## Table of Contents

- [Solution + Explaination](#Solution\+\Explaination)
    - [Chinese Remainder Theorem and It's role within the challenge](#Chinese\Remainder\Theorem\and\It's\role\within\the\challenge)
- [Chinese Remainder Theorem in RSA](#chinese\remainder\theorem\in\rsa)
  - [Overview of the Chinese Remainder Theorem (CRT)](#Overview\of\the\Chinese\Remainder\Theorem\(CRT))
- [CTF Challenge Write-Up: Decrypting Repetitive RSA Encryptions](#ctf\challenge\write-up:\decrypting\repetitive\rsa\encryptions)
  - [Challenge Overview](#Challenge\Overview)
    - [Scenario Details:](#Scenario\Details:)
  - [Theoretical Basis](#Theoretical\Basis)
  - [Solution](#Solution)
- [print(f'solution = {solution[0]}')](#print(f'solution\=\{solution[0]}'))
  - [Conclusion](#Conclusion)

#RSACTF #RSA #chineseremaindertheorem

**Challenge source code:**
```python
from Crypto.Util.number import bytes_to_long, getPrime
import socketserver
import json

FLAG = b'HTB{--REDACTED--}'


class TimeCapsule():

    def __init__(self, msg):
        self.msg = msg
        self.bit_size = 1024
        self.e = 5

    def _get_new_pubkey(self):
        while True:
            p = getPrime(self.bit_size // 2)
            q = getPrime(self.bit_size // 2)
            n = p * q
            phi = (p - 1) * (q - 1)
            try:
                pow(self.e, -1, phi)
                break
            except ValueError:
                pass

        return n, self.e

    def get_new_time_capsule(self):
        n, e = self._get_new_pubkey()
        m = bytes_to_long(self.msg)
        m = pow(m, e, n)

        return {"time_capsule": f"{m:X}", "pubkey": [f"{n:X}", f"{e:X}"]}


def challenge(req):
    time_capsule = TimeCapsule(FLAG)
    while True:
        try:
            req.sendall(
                b'Welcome to Qubit Enterprises. Would you like your own time capsule? (Y/n) '
            )
            msg = req.recv(4096).decode().strip().upper()
            if msg == 'Y' or msg == 'YES':
                capsule = time_capsule.get_new_time_capsule()
                req.sendall(json.dumps(capsule).encode() + b'\n')
            elif msg == 'N' or msg == "NO":
                req.sendall(b'Thank you, take care\n')
                break
            else:
                req.sendall(b'I\'m sorry I don\'t understand\n')
        except:
            # Socket closed, bail
            return


class MyTCPRequestHandler(socketserver.BaseRequestHandler):

    def handle(self):
        req = self.request
        challenge(req)


class ThreadingTCPServer(socketserver.ThreadingMixIn, socketserver.TCPServer):
    pass


def main():
    socketserver.TCPServer.allow_reuse_address = True
    server = ThreadingTCPServer(("0.0.0.0", 1337), MyTCPRequestHandler)
    server.serve_forever()


if __name__ == '__main__':
    main()

```

###### Solution + Explaination
- **bit size**: `1024`
- **Constant Public Modulus**: `5`
There is a few ways to go about solving this challenge, however the most straight forward solution is through the Chinese remainder theorem.

### Chinese Remainder Theorem and It's role within the challenge
When generating shares, a new public key is used for encryption using a constant modulus. Remember the public key for RSA is calculated is follows:
$N=pq$ , whereas $N$ is the public key use during modular exponentiation during encryption.
Ex:
$$
C = M^e {mod(N)}
$$

---
# Chinese Remainder Theorem in RSA

## Overview of the Chinese Remainder Theorem (CRT)

The Chinese Remainder Theorem is a theorem of number theory that allows one to solve a system of linear congruences with ease. The theorem states:

> Given a system of simultaneous congruences:


$x≡a^1{mod(n^1)}$
$x≡a^2​{mod(n^2​)}$
$x≡a^3{mod(n^3)}$
$⋮$
$x≡a^k{mod(n^k)}$

where $n1$,$n2$,…,$nk$, are pairwise coprime (i.e., 
where $n1,n2,…,nkn1​,n2​,…,nk$​ are pairwise coprime (i.e., $gcd⁡(ni,nj)=1gcd(ni​,nj​)=1$ for $i≠ji=j$), there exists a unique solution xx modulo N, where $N=n1⋅n2⋅…⋅nk​$.


# CTF Challenge Write-Up: Decrypting Repetitive RSA Encryptions

## Challenge Overview

In this challenge, participants encounter a scenario where the same message, $M$, has been encrypted multiple times using different public keys $N$ but with a common, low public exponent $e$. To solve this task you must first collect the "Limited Messages" from the Safe, then apply the $CRT$ algorithm correctly to decrypt the given messages. The problem here lies within a different (*low-bit*)key being used to encrypt the same message, with a constant public (*low*)Public exponent value.  

### Scenario Details:

- **Common RSA Exponent**: e=5 (e or any value ≥5≥5)
- **Different Public Keys**: Each encryption uses a different $Ni..Ni​$ (product of two distinct primes). (i.e., these are the public keys)
- **Encrypted Messages**: $C1$,$C2$,$…$,$Ce$,  where $Ci=M^e mod  N^i$ 
- $Ci​={{M^e} {mod Ni}}$​

## Theoretical Basis

This situation presents a unique opportunity to apply the Chinese Remainder Theorem (CRT) in combination with the mathematical principle that if a number is a perfect $e$-th power, it can be uniquely determined from its residues modulo several coprime bases.

## Solution 
```python
import json
from Crypto.Util.number import long_to_bytes 
from pwn import *
from sympy.ntheory.modular import crt
from sympy.simplify.simplify import nthroot

connection = remote('209.97.185.157', 32141)
remainder = list()
number = list()
for i in range(3):
    connection.sendline(b'Y')
    response = conn.recvline()
    result = json.loads(response[74:-1].decode())
    message = result['time_capsule']
    modulus = result['pubkey'][0]
    exponent = 5
    message = int(message, 16)
    modulus = int(modulus, 16)
    remainder.append(message)
    number.append(modulus)

solution = crt(number, remainder, check=True)
# print(f'solution = {solution[0]}')
flag = nthroot(solution[0], 5)
print(flag)
print(long_to_bytes(flag))

connection.sendline(b'N')
connection.recvline()
connection.close()
```

## Conclusion

This challenge demonstrates the power of the Chinese Remainder Theorem and number theory in breaking RSA encryption under specific conditions. It highlights the importance of using unique encryption parameters for each message in RSA to maintain cryptographic security.

