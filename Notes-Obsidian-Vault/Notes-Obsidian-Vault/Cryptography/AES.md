## Table of Contents


**Bijection** (Bijective Function)
- A function *(f)*: A -> B is bijectice (or is a bijection) if every element of A is associated with exactly one element of B and vice versa. This means:
	- Function is injective (one to one): Different inputs yield different outputs. Not two elements of A map to the same element of B
- The fucntion is surjective (or onto): Every element in B has at least one corresponding element in A.

When talking about AES (or block ciphers in general), the bijection ensures that:

- Every plaintext block corresponds to a unique ciphertext block when encrypted.
- Every ciphertext block corresponds to a unique plaintext block when decrypted.

Hence, bijections are essential for block ciphers to maintain data integrity. The key ensures which specific bijection (out of a vast number of possible permutations) is used, making decryption possible only if you have the correct key.




At a high level, AES-128 begins with a "key schedule" and then runs 10 rounds over a state. The starting state is just the plaintext block that we want to encrypt, represented as a 4x4 matrix of bytes. Over the course of the 10 rounds, the state is repeatedly modified by a number of invertible transformations.

Here's an overview of the phases of AES encryption:  
  
![diagram showing AES rounds](https://cryptohack.org/static/img/aes/Structure.png)  
  
1. **KeyExpansion** or Key Schedule  
  
 From the 128 bit key, 11 separate 128 bit "round keys" are derived: one to be used in each AddRoundKey step.  
  
2. **Initial key addition**  
  
 _AddRoundKey_ - the bytes of the first round key are XOR'd with the bytes of the state.  
  
3. **Round** - this phase is looped 10 times, for 9 main rounds plus one "final round"  
  
 a) _SubBytes_ - each byte of the state is substituted for a different byte according to a lookup table ("S-box").  
  
 b) _ShiftRows_ - the last three rows of the state matrix are transposed—shifted over a column or two or three.  
  
 c) _MixColumns_ - matrix multiplication is performed on the columns of the state, combining the four bytes in each column. This is skipped in the final round.  
  
 d) _AddRoundKey_ - the bytes of the current round key are XOR'd with the bytes of the state.  
  
Included is a `bytes2matrix` function for converting our initial plaintext block into a state matrix. Write a `matrix2bytes` function to turn that matrix back into bytes, and submit the resulting plaintext as the flag.



