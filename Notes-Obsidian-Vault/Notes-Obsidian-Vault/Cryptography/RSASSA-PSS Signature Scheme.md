## Table of Contents

      - [Probabilistic Padding](#Probabilistic\Padding)
- [RSAES-OAEP](#rsaes-oaep)
    - [Two-Stage process](#Two-Stage\process)

**RSA Signature Scheme-with Appendix - Probabilistic Signature Scheme**


- Enhanced Security Features over PKCS#1 v1.5
- Difference between other schemes lies within the use of randomized padding. 


#### Probabilistic Padding
**Probabilistic Padding**: Unlike PKCS#1 v1.5, which uses deterministic padding, RSASSA-PSS incorporates random elements in its padding. This randomness ensures that even when the same message is signed multiple times,

**Standardization and Compliance**: RSASSA-PSS is recommended by several standards, such as PKCS#1 v2.1




# RSAES-OAEP
**RSA-Optimal Asymmetric Encryption Padding**
- Uses a pair of hash functions and a masking feature to process plaintext before encryption.
### Two-Stage process
- **OAEP** First adds randomness to the plaintext through a process involving a hash function and arandom seend (nonce). 
- OAEP first adds randomness to the plaintext through a process involving a hash function and a randomly generated seed. Then, it masks this output with another hash function. This process ensures that the same plaintext encrypted twice will result in different ciphertexts.
