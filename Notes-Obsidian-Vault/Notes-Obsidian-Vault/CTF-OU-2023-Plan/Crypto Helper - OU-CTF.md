## Table of Contents

    - [Challenge 1: Based as 64](#Challenge\1:\Based\as\64)
    - [Challenge 2: Encoding and Boating](#Challenge\2:\Encoding\and\Boating)
    - [Challenge 3: Caesar Salad](#Challenge\3:\Caesar\Salad)
    - [Challenge 4: Vigenere! Oh my](#Challenge\4:\Vigenere!\Oh\my)
          - [What is the Vigenere Cipher?](#What\is\the\Vigenere\Cipher?)
    - [Challenge 5: Can you XOR or not?](#Challenge\5:\Can\you\XOR\or\not?)
    - [Crypto Challenge 6 - RSA Parameters](#Crypto\Challenge\6\-\RSA\Parameters)
      - [A quick RSA Refresher](#A\quick\RSA\Refresher)
    - [Challenge 7: Tiny Wien](#Challenge\7:\Tiny\Wien)
    - [Key Generation in `challenge.py`](#Key\Generation\in\`challenge.py`)
      - [Wiener's Attack Explaination](#Wiener's\Attack\Explaination)
    - [Mathematics Involved](#Mathematics\Involved)
        - [Explaination](#Explaination)
        - [The Mathematical Basis of Wiener's Attack](#The\Mathematical\Basis\of\Wiener's\Attack)
    - [ECC - Elliptic Curve Cryptography](#ECC\-\Elliptic\Curve\Cryptography)
  - [Elliptic Curve Cryptography](#Elliptic\Curve\Cryptography)
          - [Terms](#Terms)
          - [Sources](#Sources)

Modular Exponentiation 
- This operation is used extensively in cryptography and is normally written like so:
$$2^x mod (n)$$
This is simply raising `2` to a certain power `x` then taking the remainder of the division.
Ex:
$2^{10} mod 17$

$2^{10} = 1024$, then the remainder of the division by some other number `7 = 4`
In python this can be use like so:
```python
pow(base, exponent, modulus)
```

Find the solution to the following:
$142^{24} \mod(29943)$`

---
### Challenge 1: Based as 64
In this challenge, the participant is provided with a Base64 string, to get the flag you simply need to decode the given string and you will obtain the flag.
___
### Challenge 2: Encoding and Boating
![[Pasted image 20240117111320.png]]
For the actual challenge, when it is uploaded to the CTFd server, lines 3,4,9-11 are the only lines needed for `challenge.py`

#`ciphertext.txt` is provided.
![[Pasted image 20240117111617.png]]
The flag is first base64 encoded as `honestlynoideawhatimdoing`, then the base64 string is base32 encoded as `geronimo`.
___
### Challenge 3: Caesar Salad
The caesar cipher is a **Substitution cipher** that shifts letters in a message by $N$ alphabetical index values whereas $N$ is the key.
Ex: $k=3$
`ABC`$^k$ -> `DEF`
___
### Challenge 4: Vigenere! Oh my
###### What is the Vigenere Cipher?
This is simply a 26x26 grid version of the caesar cipher (to put it simply).
This cipher works as folows:
(**Vigener Tableu**)
```plaintext
1  ABCDEFGHIJKLMNOPQRSTUVWXYZ
2  BCDEFGHIJKLMNOPQRSTUVWXYZA
3  CDEFGHIJKLMNOPQRSTUVWXYZAB
4  DEFGHIJKLMNOPQRSTUVWXYZABC
5  EFGHIJKLMNOPQRSTUVWXYZABCD
6  FGHIJKLMNOPQRSTUVWXYZABCDE
7  GHIJKLMNOPQRSTUVWXYZABCDEF
8  HIJKLMNOPQRSTUVWXYZABCDEFG
9  IJKLMNOPQRSTUVWXYZABCDEFGH
10 JKLMNOPQRSTUVWXYZABCDEFGHI
11 KLMNOPQRSTUVWXYZABCDEFGHIJ
12 LMNOPQRSTUVWXYZABCDEFGHIJK
13 MNOPQRSTUVWXYZABCDEFGHIJKL
14 NOPQRSTUVWXYZABCDEFGHIJKLM
15 OPQRSTUVWXYZABCDEFGHIJKLMN
16 PQRSTUVWXYZABCDEFGHIJKLMNO
17 QRSTUVWXYZABCDEFGHIJKLMNOP
18 RSTUVWXYZABCDEFGHIJKLMNOPQ
19 STUVWXYZABCDEFGHIJKLMNOPQR
20 TUVWXYZABCDEFGHIJKLMNOPQRS
21 UVWXYZABCDEFGHIJKLMNOPQRST
22 VWXYZABCDEFGHIJKLMNOPQRSTU
23 WXYZABCDEFGHIJKLMNOPQRSTUV
24 XYZABCDEFGHIJKLMNOPQRSTUVW
25 YZABCDEFGHIJKLMNOPQRSTUVWX
26 ZABCDEFGHIJKLMNOPQRSTUVWXY
```
The Vigenere cipher is defined as a periodic **polyalphabetic substitution cipher**. In simpler terms, this simply means that there are different alphabet ordering's used based on the key. To explain how this cipher works, let's replace the characters of the key and the characters of the plaintext by integers, where A=0, B=1, ..., Z=25. The length of the key let's call period or $L$. So the key is just a set of numbers $k^0, k^1, ... k^{L-1}$. Next take the plaintext and express it also as a list of numbers $p^0, p^1, p^2, ...$
The text is encrypted by adding a number from the key $mod (26)$ to a number from the plaintext, where we run through the key over and over again as needed as we run through the plaintext. As an equation, the $i^{th}$ character is encrypted as follows:
$$c_i = (p_i+k_i{ \mod L}) \mod (26)$$
After that the ciphertext is expressed as letters instead of numbers. 
`python` Example:
```python
import strings
uppers = strings.ascii_uppercase
def encrypt(pt, key):
	ct = ''
	for i in range(len(plaintext)):
		p = uppers.index(pt[i])
		k = uppers.index(key[i%len(key)])
		c = (p + k) % 26
		ct += uppers[c]
	return ct
```
**Hint**: There are many online tools to assist with vigenere cryptanalysis. Here are some resources to assist in the solution:
- [Vigenere Solver](https://guballa.de/vigenere-solver)
- [dCode Vigenere Solver](https://www.dcode.fr/vigenere-cipher)
- [Simon Singh - Vigenere Cracker](https://www.simonsingh.net/The_Black_Chamber/vigenere_cracking_tool.html)
___
### Challenge 5: Can you XOR or not?
#`ciphertext.txt` provided
![[Pasted image 20240117110122.png]]
This is an XOR challenge, it was created with `pwntools`. The flag is XOR'd with `grizzly_bears`.
Challenge participant needs to XOR the ciphertext with the given key in this challenge to decrypt the given ciphertext and obtain the flag.
```python
from pwn import *
a = b'blah' # hex
b = b'blah' # Key converted to hex
c = xor(a,b)
print(c)
```
___
### Crypto Challenge 6 - RSA Parameters
#### A quick RSA Refresher
[RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) (Rivest-Shamir-Adleman) is a widely used public key cryptosystem for secure data transmission. It works as follows:
1. **Key Generation**
    - Select two large prime numbers $p$ and $q$
    - Calculate $N=pq$ (RSA modulus).
    - Choose an encryption exponent `e` such that $1<e<ϕ(N)1<e<ϕ(N)$ and $e$ is coprime to $ϕ(N)$, where $ϕ(N)=(p−1)(q−1)$ (*Euler's totient function*).
    - Determine the decryption exponent $d$ as the *modular multiplicative inverse* of $e$ modulo $ϕ(N)$, i.e., $d$ such that $ed≡1 \mod ϕ(N))$.
    - The public key is $(e,N)$ and the private key is $(d,N)$.

2. **Encryption**
3. To encrypt a message `M`, where M < N, compute the ciphertext $C$ as :
$$C = M^e \mod N$$
4. **Decryption**
$$M = C^d \mod N$$
Python example showcasing the calculation of $d$ the private key.
```python
d = pow(e, phi, -1)
```
___

### Challenge 7: Tiny Wien
The `challenge.py` script generates an RSA public/private key pair and encrypts a secret message (flag) using the RSA algorithm. The script's objective is to create an RSA setup vulnerable to Wiener's attack.
### Key Generation in `challenge.py`
1. **Prime Numbers**:
    - Two prime numbers `p` and `q` are generated, ensuring `q < p < 2*q`.
2. **Modulus Calculation**:
    - RSA modulus `N` is calculated as `N = p * q`.
3. **Public and Private Keys**:
    - The script generates a small private exponent `d`.
    - The public exponent `e` is the modular multiplicative inverse of `d` modulo `φ(N) = (p-1)(q-1)`.
    - `d` is chosen to be small enough to make the system susceptible to Wiener's attack (`d < N^(1/4) / 3`).
#### Wiener's Attack Explaination

1. **Continued Fraction Expansion**:
    - The public exponent `e` and the modulus `N` are used to calculate the continued fraction expansion of `e/N`.
2. **Convergents Calculation**:
    - The convergent of the continued fraction expansion are calculated. These convergents give approximations of `e/φ(N)`.
3. **Recovering Private Key `d`**:
    - For each convergent `(pk, pd)` (`pk` - possible `k`, `pd` - possible `d`), the script checks if it leads to the correct private key `d`.
    - The potential `φ(N)` is calculated as `(e * pd - 1) / pk`.
    - Using the quadratic formula, the script solves for the potential `p` and `q`.
    - If the product of these roots equals `N`, the correct factors `p` and `q` have been found, leading to the discovery of `d`.
4. **Decrypting the Message**:
    - Once `d` is found, the encrypted message `C` is decrypted using `M = C^d mod N`.
    - The decrypted message is the secret flag.

### Mathematics Involved
- **Continued Fractions**: Used to approximate `e/N`.
- **Quadratic Equation**: Used to find the potential prime factors `p` and `q`.
- **Modular Arithmetic**: Central to the RSA algorithm and Wiener's attack.
##### Explaination
Wiener's attack is effective when the private key `d` is small relative to the modulus `N`, specifically when `d < N^(1/4) / 3`. The essence of the attack lies in the relationship between `e`, `d`, and `N`, and the mathematical properties of continued fractions.
##### The Mathematical Basis of Wiener's Attack
1. **Key Equation**:
    - In RSA, `ed = 1 (mod φ(N))`. This implies that there's an integer `k` such that `ed - kφ(N) = 1`.
    - This can be rewritten as `ed = kφ(N) + 1`, which suggests a close relationship between the ratio `e/d` and `φ(N)`.
2. **Small `d` Implication**:
    - If `d` is small, then the fraction `k/d` will be a good approximation of `e/φ(N)` since `ed ≈ kφ(N)`.
3. **Continued Fractions**:
    - Continued fractions are a way to approximate real numbers with fractions. Importantly, they can provide the best rational approximations to a real number for a given denominator size.
    - By expanding `e/N` into a continued fraction and computing its convergents, we can find fractions that closely approximate `e/φ(N)`.
4. **Finding the Private Key**:
    - The convergents from the continued fraction expansion are examined to find a fraction `k/d` that satisfies the properties of RSA's `e`, `d`, and `N`.
    - Once a candidate `d` is found, it can be tested to see if it correctly decrypts the RSA ciphertext. If it does, the correct private key `d` has been discovered.
[Weiner Attack - CryptoHack](https://cryptohack.gitbook.io/cryptobook/untitled/low-private-component-attacks/wieners-attack)
[Weiner Attack](https://en.wikipedia.org/wiki/Wiener%27s_attack)
### ECC - Elliptic Curve Cryptography

|  |  |
| ---- | ---- |
| CURVE | the elliptic curve field and equation used |
| $G$ | elliptic curve base point, a point on the curve that generates a [subgroup of large prime order n](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography#Domain_parameters "Elliptic-curve cryptography") |
| $n$ | integer order of _G_, means that n × G = O ![n\times G=O](https://wikimedia.org/api/rest_v1/media/math/render/svg/8340b0e30a45c48c84732ee61306887db59ce2b8) , where O ![O](https://wikimedia.org/api/rest_v1/media/math/render/svg/9d70e1d0d87e2ef1092ea1ffe2923d9933ff18fc) is the identity element. |
| $d_{A}$ | the private key (randomly selected) |
| $Q_{A}$ | the public key d A × G ![{\displaystyle d_{A}\times G}](https://wikimedia.org/api/rest_v1/media/math/render/svg/0675e6c1e166d58e02cb3b0743c0dfd000745fca) (calculated by elliptic curve) |
| $m$ | the message to send |



## Elliptic Curve Cryptography
###### Terms
- *Trap-door function*: Allow a client to keep data secret by performing a mathematical operation which is computationally easy to do, but currently understood to be very expensive to undo. 

**What is it?**:
- Asymmetric cryptographic protocol that, like RSA and Diffie-Hellman (DH), relies on a trapdoor function. 

An **elliptic curve** is a plane curve defined by an equation of the form: $$y^2=x^3+ax+b$$
After a linear change of variables (a and b are real numbers). This is called a *Weierstrass equation*.
- An elliptic curve by definition also requires that the curve is non-singular. Geometrically, this means that the graph has no cusps, self-intersections, or isolated points. Algebraically, this hold if and only if the **discriminant:**
$Δ = -16(4a^3+27b^2)$
is not equal to zero. (Although)
###### Sources
- [A Graduate Course in Applied Cryptography](https://toc.cryptobook.us/)
- [So you want to be a Cryptographer?](https://github.com/SalusaSecondus/CryptoGotchas/blob/master/GettingStarted.md)
- [CryptoHack](https://cryptohack.org/)
- [Caesar Cipher](https://en.wikipedia.org/wiki/Caesar_cipher)
- 

