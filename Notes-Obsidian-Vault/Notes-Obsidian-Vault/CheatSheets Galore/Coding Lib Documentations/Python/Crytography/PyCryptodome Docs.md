## Table of Contents

- [PyCryptodome Cheatsheet: Official Implementations](#PyCryptodome\Cheatsheet:\Official\Implementations)
      - [RSA Encryption (Asymmetric Cipher)](#RSA\Encryption\(Asymmetric\Cipher))
- [Encrypt with public key](#encrypt\with\public\key)
- [Decrypt with private key](#decrypt\with\private\key)
      - [SHA-256 Hashing](#SHA-256\Hashing)
      - [Digital Signature with PKCS#1 v1.5](#Digital\Signature\with\PKCS#1\v1.5)
- [Signing](#signing)
- [Verifying](#verifying)
      - [Shamir's Secret Sharing](#Shamir's\Secret\Sharing)
- [Combining shares to retrieve the secret](#combining\shares\to\retrieve\the\secret)
      - [PEM Encoding/Decoding](#PEM\Encoding/Decoding)
- [PEM encoded keys](#pem\encoded\keys)
      - [Generating Random Data](#Generating\Random\Data)
      - [Utility Example: XOR for Byte Strings](#Utility\Example:\XOR\for\Byte\Strings)
      - [1) AES Modes of Operation](#1)\AES\Modes\of\Operation)
        - [AES Encryption (Symmetric Cipher)](#AES\Encryption\(Symmetric\Cipher))
      - [2) ECC Implementations in PyCryptodome](#2)\ECC\Implementations\in\PyCryptodome)
      - [3) Stream Cipher Implementations with Authentication](#3)\Stream\Cipher\Implementations\with\Authentication)

[Cryptobook - Examples](https://cryptobook.nakov.com/)

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




### PyCryptodome Cheatsheet: Official Implementations

#### RSA Encryption (Asymmetric Cipher)
```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP
import binascii

key = RSA.generate(2048)
private_key = key.export_key()
public_key = key.publickey().export_key()

# Encrypt with public key
recipient_key = RSA.import_key(public_key)
cipher_rsa = PKCS1_OAEP.new(recipient_key)
enc_data = cipher_rsa.encrypt(b'Your Data Here')

# Decrypt with private key
private_key = RSA.import_key(private_key)
cipher_rsa = PKCS1_OAEP.new(private_key)
data = cipher_rsa.decrypt(enc_data)
```

#### SHA-256 Hashing
```python
from Crypto.Hash import SHA256

hash = SHA256.new()
hash.update(b'Your Data Here')
print(hash.digest())
```

#### Digital Signature with PKCS#1 v1.5
```python
from Crypto.PublicKey import RSA
from Crypto.Signature import pkcs1_15
from Crypto.Hash import SHA256

key = RSA.generate(2048)
private_key = key.export_key()
public_key = key.publickey().export_key()

# Signing
message = b'To be signed'
key = RSA.import_key(private_key)
h = SHA256.new(message)
signature = pkcs1_15.new(key).sign(h)

# Verifying
key = RSA.import_key(public_key)
h = SHA256.new(message)
try:
    pkcs1_15.new(key).verify(h, signature)
    print("The signature is valid.")
except (ValueError, TypeError):
    print("The signature is not valid.")
```

#### Shamir's Secret Sharing
```python
from Crypto.Protocol.SecretSharing import Shamir
from Crypto.Random import get_random_bytes

secret = get_random_bytes(16)
shares = Shamir.split(2, 5, secret)  # 2-out-of-5 sharing

# Combining shares to retrieve the secret
secret_recovered = Shamir.combine(shares[:2])
```

#### PEM Encoding/Decoding
```python
from Crypto.PublicKey import RSA

key = RSA.generate(2048)
private_key = key.export_key()
public_key = key.publickey().export_key()

# PEM encoded keys
print(private_key.decode('utf-8'))
print(public_key.decode('utf-8'))
```

#### Generating Random Data
```python
from Crypto.Random import get_random_bytes

random_data = get_random_bytes(16)
print(random_data)
```

#### Utility Example: XOR for Byte Strings
```python
from Crypto.Util.strxor import strxor

data1 = b'ByteString1'
data2 = b'ByteString2'
result = strxor(data1, data2)
```


#### 1) AES Modes of Operation
##### AES Encryption (Symmetric Cipher)
- **EAX Mode**:
```python
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes

data = b"Your Data Here"
key = get_random_bytes(16)  # AES key must be either 16, 24, or 32 bytes long
cipher = AES.new(key, AES.MODE_EAX)
nonce = cipher.nonce
ciphertext, tag = cipher.encrypt_and_digest(data)
```

- **Electronic Codebook (ECB)**:
  ```python
  from Crypto.Cipher import AES
  key = b'Sixteen byte key'
  cipher = AES.new(key, AES.MODE_ECB)
  ```

- **Cipher Block Chaining (CBC)**:
  ```python
  from Crypto.Cipher import AES
  from Crypto.Random import get_random_bytes
  key = get_random_bytes(16)
  iv = get_random_bytes(16)
  cipher = AES.new(key, AES.MODE_CBC, iv)
  ```

- **Cipher Feedback (CFB)**:
  ```python
  from Crypto.Cipher import AES
  key = get_random_bytes(16)
  iv = get_random_bytes(16)
  cipher = AES.new(key, AES.MODE_CFB, iv)
  ```

- **Output Feedback (OFB)**:
  ```python
  from Crypto.Cipher import AES
  key = get_random_bytes(16)
  iv = get_random_bytes(16)
  cipher = AES.new(key, AES.MODE_OFB, iv)
  ```

- **Counter (CTR)**:
  ```python
  from Crypto.Cipher import AES
  from Crypto.Util import Counter
  key = get_random_bytes(16)
  ctr = Counter.new(128)
  cipher = AES.new(key, AES.MODE_CTR, counter=ctr)
  ```

- **Galois/Counter Mode (GCM)**:
  ```python
  from Crypto.Cipher import AES
  key = get_random_bytes(16)
  cipher = AES.new(key, AES.MODE_GCM)
  ```

- **EAX Mode**:
  ```python
  from Crypto.Cipher import AES
  key = get_random_bytes(16)
  cipher = AES.new(key, AES.MODE_EAX)
  ```

- **SIV Mode**:
  ```python
  from Crypto.Cipher import AES
  key = get_random_bytes(32)  # Key must be 32 or 64 bytes
  cipher = AES.new(key, AES.MODE_SIV)
  ```

- **CCM Mode**:
  ```python
  from Crypto.Cipher import AES
  key = get_random_bytes(16)
  cipher = AES.new(key, AES.MODE_CCM)
  ```

- **OCB Mode**:
  ```python
  from Crypto.Cipher import AES
  key = get_random_bytes(16)
  cipher = AES.new(key, AES.MODE_OCB)
  ```

#### 2) ECC Implementations in PyCryptodome

PyCryptodome supports various elliptic curves for ECC. Here are the key curves and examples for signatures and encryption:

- **Supported Curves**: P-256, P-384, P-521, SECP256k1.

- **ECC Signature and Verification**:
  ```python
  from Crypto.PublicKey import ECC
  from Crypto.Signature import DSS
  from Crypto.Hash import SHA256
  
  key = ECC.generate(curve='P-256')
  hasher = SHA256.new(b'Message')
  signer = DSS.new(key, 'fips-186-3')
  signature = signer.sign(hasher)
  
  pub_key = key.public_key()
  verifier = DSS.new(pub_key, 'fips-186-3')
  try:
      verifier.verify(hasher, signature)
      print("The signature is valid.")
  except ValueError:
      print("The signature is not valid.")
  ```

- **ECC Encryption (ECDH)**:
  ```python
  from Crypto.PublicKey import ECC
  from Crypto.Cipher import AES
  from Crypto.Random import get_random_bytes
  
  # Key generation
  key1 = ECC.generate(curve='P-256')
  key2 = ECC.generate(curve='P-256')

  # ECDH
  secret1 = key1._export_key(format='DER')
  secret2 = key2._export_key(format='DER')
  
  # Encrypt using derived secret (example with AES)
  cipher = AES.new(secret1, AES.MODE_EAX)
  ciphertext, tag = cipher.encrypt_and_digest(b'Hello')
  ```

#### 3) Stream Cipher Implementations with Authentication

- **ChaCha20-Poly1305**:
  ```python
  from Crypto.Cipher import ChaCha20_Poly1305
  from Crypto.Random import get_random_bytes
  
  key = get_random_bytes(32)
  cipher = ChaCha20_Poly1305.new(key=key)
  data = b"Secret Message"
  ciphertext, tag = cipher.encrypt_and_digest(data)
  ```

- **Salsa20 (with MAC for Authentication)**:
  ```python
from Crypto.Cipher

import Salsa20
from Crypto.Hash import HMAC, SHA256
  
key = get_random_bytes(32)
cipher = Salsa20.new(key=key)
msg = cipher.nonce + cipher.encrypt(b'Hello')
h = HMAC.new(key, digestmod=SHA256)
h.update(msg)
mac = h.digest()
  ```











