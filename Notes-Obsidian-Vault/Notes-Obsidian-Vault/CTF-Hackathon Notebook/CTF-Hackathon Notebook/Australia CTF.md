## Table of Contents

- [Crypto Primitives](#crypto\primitives)
      - [RSA](#RSA)
    - [Known Values:](#Known\Values:)
      - [Logic Flow](#Logic\Flow)



# Crypto Primitives
```python 
PSRG() = from Crypto.Random import getrandbits
RSA Encryption

```
#### RSA
### Known Values:
e=65537.

#### Logic Flow

1. **Initialization**: The code initializes 128-bit numbers mulmul, addadd, and modulusmodulus with the condition mul<modulusmul<modulus and add<modulusadd<modulus.
2. **PRNG Function `gen_num()`**:
    - Takes an argument `bits` and initializes `truncate` and `seed`.
    - Generates a sequence of pseudorandom numbers using a linear congruential generator (LCG) with parameters mulmul, addadd, and modulusmodulus.
    - Returns two lists ��xx and ��yy, where ��yy has "leaky" bits due to right-shifting.
3. **Generating �p and �q**:
    - Uses the last elements �a and �b from the sequences ��ee and ��ff.
    - Computes �p and �q using �a, �b, and a new random number �c.
4. **RSA Encryption**:
    - Calculates �=�×�N=p×q and �=65537e=65537.
    - Encrypts a redacted flag using RSA and prints the ciphertext encenc, �N, ��ee, ��ff, �a, �c, and �m