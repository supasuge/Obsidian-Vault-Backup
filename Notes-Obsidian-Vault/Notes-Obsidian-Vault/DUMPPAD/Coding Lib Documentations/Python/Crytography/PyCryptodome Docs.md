## Table of Contents

  - [Symmetric ciphers[¶](https://www.pycryptodome.org/src/cipher/cipher#symmetric-ciphers "Permalink to this heading")](#Symmetric\ciphers[¶](https://www.pycryptodome.org/src/cipher/cipher#symmetric-ciphers\"Permalink\to\this\heading"))

|   |   |
|---|---|
|[Crypto.Cipher](https://www.pycryptodome.org/src/cipher/cipher.html)|Modules for protecting **confidentiality** that is, for encrypting and decrypting data (example: AES).|
|[Crypto.Signature](https://www.pycryptodome.org/src/signature/signature.html)|Modules for assuring **authenticity**, that is, for creating and verifying digital signatures of messages (example: PKCS#1 v1.5).|
|[Crypto.Hash](https://www.pycryptodome.org/src/hash/hash.html)|Modules for creating cryptographic **digests** (example: SHA-256).|
|[Crypto.PublicKey](https://www.pycryptodome.org/src/public_key/public_key.html)|Modules for generating, exporting or importing _public keys_ (example: RSA or ECC).|
|[Crypto.Protocol](https://www.pycryptodome.org/src/protocol/protocol.html)|Modules for faciliting secure communications between parties, in most cases by leveraging cryptograpic primitives from other modules (example: Shamir’s Secret Sharing scheme).|
|[Crypto.IO](https://www.pycryptodome.org/src/io/io.html)|Modules for dealing with encodings commonly used for cryptographic data (example: PEM).|
|[Crypto.Random](https://www.pycryptodome.org/src/random/random.html)|Modules for generating random data.|
|[Crypto.Util](https://www.pycryptodome.org/src/util/util.html)|General purpose routines (example: XOR for byte strings).|

- You instantiate a cipher object by calling the `new()` function from the relevant cipher module (e.g. [`Crypto.Cipher.AES.new()`](https://www.pycryptodome.org/src/cipher/aes.html#Crypto.Cipher.AES.new "Crypto.Cipher.AES.new")). The first parameter is always the _cryptographic key_; its length depends on the particular cipher. You can (and sometimes must) pass additional cipher- or mode-specific parameters to `new()` (such as a _nonce_ or a _mode of operation_).
    
- For encrypting data, you call the `encrypt()` method of the cipher object with the plaintext. The method returns the piece of ciphertext. Alternatively, with the `output` parameter you can specify a pre-allocated buffer for the result.

```python
from Crypto.Cipher import Salsa20
key - b'0123456789012345'
cipher = Salsa20.new(key)
ciphertext = cipher
```




## Symmetric ciphers[¶](https://www.pycryptodome.org/src/cipher/cipher#symmetric-ciphers "Permalink to this heading")

There are two types of symmetric ciphers:

- **Stream ciphers**: the most natural kind of ciphers: they encrypt data one byte at a time. See [ChaCha20 and XChaCha20](https://www.pycryptodome.org/src/cipher/chacha20.html) and [Salsa20](https://www.pycryptodome.org/src/cipher/salsa20.html).
    
- **Block ciphers**: ciphers that can only operate on a fixed amount of data. The most important block cipher is [AES](https://www.pycryptodome.org/src/cipher/aes.html), which has a block size of 128 bits (16 bytes).


The widespread consensus is that ciphers that provide only confidentiality, without any form of authentication, are undesirable. Instead, primitives have been defined to integrate symmetric encryption and authentication (MAC). For instance:

- [Modern modes of operation](https://www.pycryptodome.org/src/cipher/modern.html) for block ciphers (like GCM).
    
- Stream ciphers paired with a MAC function, like [ChaCha20-Poly1305 and XChaCha20-Poly1305](https://www.pycryptodome.org/src/cipher/chacha20_poly1305.html).


```python

```













