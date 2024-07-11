## Table of Contents

        - [Keys](#Keys)
      - [Generator Point](#Generator\Point)
      - [ECC Vs RSA](#ECC\Vs\RSA)



##### Keys
*Private Key*:  ECC cryptography’s private key creation is as simple as safely producing a random integer in a specific range, making it highly quick. Any integer in the field represents a valid ECC private key.

 *Public keys*: Public keys within ECC are EC points, which are pairs of integer coordinates x, and y that lie on a curve. Because of its unique features, EC points can be compressed to a single coordinate + 1 bit (odd or even). As a result, the compressed public key corresponds to a 256-bit ECC.

#### Generator Point
ECC establish pre=defined EC point called generator point G (BASE POINT) for elliptic curves over finite fields. WHich can generate any other position in it subgroup over the elliptic curve by multiplying G some integer in the range `[0....R]` when `R` is referred to as the "ordering" of the cyclic group. Subgroups typcially contain numerous generator points, careful



- **Elliptic Curve Digital Signature Algorithm. (ECDSA):** ECDSA, or Elliptic Curve Digital Signature Algorithm, is a more highly complicated public-key cryptography encryption algorithm. Elliptic curve cryptography is a type of public key cryptography that uses the algebraic structure of elliptic curves with finite fields as its foundation. Elliptic curve cryptography is primarily used to generate pseudo-random numbers, digital signatures, and other data.
- **Edwards-curve Digital Signature Algorithm (EdDSA):** The Edwards-curve Digital Signature Algorithm (EdDSA) was proposed as a replacement for the Elliptic Curve Digital Signature Algorithm for performing fast public-key digital signatures (ECDSA). Its primary benefits for embedded devices are higher performance and simple, secure implementations. During a signature, no branch or lookup operations based on the secret values are performed. Many side-channel attacks are foiled by these properties.



#### ECC Vs RSA
  
Working algorithmECC is a cryptography technique that works just on a mathematical model of elliptic curves.RSA cryptography algorithm is primarily based on the prime factorization approach.Bandwidth savingsECC gives significant bandwidth savings over RSA.RSA provides much lesser bandwidth saving than ECC.Encryption processThe encryption process takes less time in ECC.The encryption process takes more time in RSA.Decryption processThe decryption process takes more time.Decryption is faster than ECC.SecurityECC is much safer than RSA and is currently in the process of adapting. RSA is heading toward the end of its tenure.



**ECC vs RSA: Key Length Comparison:**

|Security(in Bits)|RSA key length required|ECC key length required|
|---|---|---|
|80|1024|160-223|
|112|2048|224-255|
|128|3072|256-383|
|192|7680|384-511|
|256|15360|512+|








