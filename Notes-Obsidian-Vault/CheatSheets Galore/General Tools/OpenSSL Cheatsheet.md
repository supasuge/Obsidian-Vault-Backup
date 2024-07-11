## Table of Contents

- [OpenSSL Cheatsheet](#openssl\cheatsheet)
- [list only TLSv1 ciphers](#list\only\tlsv1\ciphers)
- [list only high encryption ciphers (keys larger than 128 bits)](#list\only\high\encryption\ciphers\(keys\larger\than\128\bits))
- [list only high encryption ciphers using the AES algorithm](#list\only\high\encryption\ciphers\using\the\aes\algorithm)
- [test rsa speeds](#test\rsa\speeds)
    - [How to test with an SSL-enabled](#How\to\test\with\an\SSL-enabled)
- [on one host, set up the server (using default port 4433)](#on\one\host,\set\up\the\server\(using\default\port\4433))
- [on second host (or even the same one), run s_time](#on\second\host\(or\even\the\same\one),\run\s_time)
  - [How do I generate a self-signed certificate?](#How\do\I\generate\a\self-signed\certificate?)
  - [General Commands](#General\Commands)
    - [Check OpenSSL Version](#Check\OpenSSL\Version)
    - [Get Basic Help](#Get\Basic\Help)
  - [Performance Testing](#Performance\Testing)
    - [Test Algorithm Performance](#Test\Algorithm\Performance)
  - [Random Number Generation](#Random\Number\Generation)
    - [Generate Random Bytes](#Generate\Random\Bytes)
  - [Encoding and Decoding](#Encoding\and\Decoding)
    - [Base64 Encoding](#Base64\Encoding)
    - [Base64 Decoding](#Base64\Decoding)
  - [Working with Hashes](#Working\with\Hashes)
    - [List Digest Algorithms](#List\Digest\Algorithms)
    - [Hashing Files](#Hashing\Files)
    - [Hashing with Binary Output](#Hashing\with\Binary\Output)
    - [Hashing Text](#Hashing\Text)
    - [Creating HMAC](#Creating\HMAC)
  - [Asymmetric Encryption](#Asymmetric\Encryption)
    - [List Available Elliptic Curves](#List\Available\Elliptic\Curves)
    - [Generate RSA Key Pair](#Generate\RSA\Key\Pair)
    - [Display RSA Key Information](#Display\RSA\Key\Information)
    - [Encrypt RSA Key Pair](#Encrypt\RSA\Key\Pair)
    - [Convert RSA Private Key](#Convert\RSA\Private\Key)
    - [Extract Public Key from RSA Key Pair](#Extract\Public\Key\from\RSA\Key\Pair)
    - [Encrypt File using RSA Public Key](#Encrypt\File\using\RSA\Public\Key)
    - [Decrypt File using RSA Private Key](#Decrypt\File\using\RSA\Private\Key)
    - [Generate EC Private Key](#Generate\EC\Private\Key)
    - [Encrypt EC Private Key](#Encrypt\EC\Private\Key)
  - [Symmetric Encryption](#Symmetric\Encryption)
    - [List Symmetric Ciphers](#List\Symmetric\Ciphers)
    - [Encrypt File with AES](#Encrypt\File\with\AES)
    - [Decrypt File with AES](#Decrypt\File\with\AES)
    - [Encrypt with Specific Key and IV](#Encrypt\with\Specific\Key\and\IV)
  - [Digital Signatures](#Digital\Signatures)
    - [Generate DSA Parameters](#Generate\DSA\Parameters)
    - [Generate DSA Key Pair](#Generate\DSA\Key\Pair)
    - [Extract DSA Public Key](#Extract\DSA\Public\Key)
    - [Sign File using RSA](#Sign\File\using\RSA)
    - [Verify Signature with RSA Public Key](#Verify\Signature\with\RSA\Public\Key)
  - [Working with TLS Protocol](#Working\with\TLS\Protocol)
    - [List Cipher Suites](#List\Cipher\Suites)
    - [TLS Connection to Server](#TLS\Connection\to\Server)
    - [Display Certificates from TLS Connection](#Display\Certificates\from\TLS\Connection)
    - [Set up TLS Server](#Set\up\TLS\Server)
  - [Digital Certificates](#Digital\Certificates)
    - [Generate CSR and RSA Key Pair](#Generate\CSR\and\RSA\Key\Pair)
    - [Display CSR Information](#Display\CSR\Information)
    - [Create Self-Signed Certificate](#Create\Self-Signed\Certificate)
    - [Sign a CSR with Your CA](#Sign\a\CSR\with\Your\CA)
    - [Verify a Certificate Chain](#Verify\a\Certificate\Chain)
  - [Advanced TLS/SSL Testing](#Advanced\TLS/SSL\Testing)
    - [Test SSL/TLS Handshake](#Test\SSL/TLS\Handshake)
    - [Display Specific Cipher Suite Information](#Display\Specific\Cipher\Suite\Information)
    - [Test Server for Specific Cipher Suite](#Test\Server\for\Specific\Cipher\Suite)
    - [Extract TLS Certificate from Server](#Extract\TLS\Certificate\from\Server)
    - [Test TLS Connection with SNI Support](#Test\TLS\Connection\with\SNI\Support)
  - [PKCS#12 and Key Management](#PKCS#12\and\Key\Management)
    - [Convert Certificate and Key to PKCS#12](#Convert\Certificate\and\Key\to\PKCS#12)
    - [Extract Certificate and Key from PKCS#12](#Extract\Certificate\and\Key\from\PKCS#12)
    - [Convert PEM to DER Format](#Convert\PEM\to\DER\Format)
  - [Advanced Configuration](#Advanced\Configuration)
    - [Custom OpenSSL Configuration File](#Custom\OpenSSL\Configuration\File)
    - [Create a Simple CA Using OpenSSL](#Create\a\Simple\CA\Using\OpenSSL)
    - [Convert CRL to PEM Format](#Convert\CRL\to\PEM\Format)
    - [Create ECDSA Keys and Certificates](#Create\ECDSA\Keys\and\Certificates)
    - [Advanced CSR with SAN (Subject Alternative Name)](#Advanced\CSR\with\SAN\(Subject\Alternative\Name))

# OpenSSL Cheatsheet
#openssl #crypto #cryptography #rsa #aes #tools #linux #cryptography #PKI
___
**Version Information**
```bash
openssl version -a - version information
```
**Standard Commands** are the main top-level options.
___
```sh
-c              to output the digest with separating colons
-r              to output the digest in coreutils format


-d              to output debug info

-hex            output as hex dump

-binary         output in binary form

-sign   [file]    sign digest using private key in file

-verify [file]    verify a signature using public key in file

-prverify file  verify a signature using private key in file

-keyform arg    key file format (PEM or ENGINE)

-out filename   output to filename rather than stdout

-signature file signature to verify

-sigopt nm:v    signature parameter

-hmac key       create hashed MAC with key

-mac algorithm  create MAC (not neccessarily HMAC)

-macopt nm:v    MAC algorithm parameters or key

-engine e       use engine e, possibly a hardware device.

-md4            to use the md4 message digest algorithm

-md5            to use the md5 message digest algorithm

-ripemd160      to use the ripemd160 message digest algorithm

-sha            to use the sha message digest algorithm

-sha1           to use the sha1 message digest algorithm

-sha224         to use the sha224 message digest algorithm

-sha256         to use the sha256 message digest algorithm

-sha384         to use the sha384 message digest algorithm

-sha512         to use the sha512 message digest algorithm

-whirlpool      to use the whirlpool message digest algorithm
```


**How Do I get a list of available ciphers?**
```bash
openssl ciphers -v

# list only TLSv1 ciphers
openssl ciphers -v -tls1

# list only high encryption ciphers (keys larger than 128 bits)
openssl ciphers -v 'HIGH'

# list only high encryption ciphers using the AES algorithm
openssl ciphers -v 'AES+HIGH'

To run a catchall benchmark, run it without any further options.

openssl speed

# test rsa speeds
openssl speed rsa


```

### How to test with an SSL-enabled
```sh
# on one host, set up the server (using default port 4433)
openssl s_server -cert mycert.pem -www

# on second host (or even the same one), run s_time
openssl s_time -connect myhost:4433 -www / -new -ssl3

```


## How do I generate a self-signed certificate?
#self-signed-certificate
You’ll first need to decide whether or not you want to encrypt your key. Doing so means that the key is protected by a passphrase.
```sh
openssl req \
	-x509 -nodes -days 365 -sha256 \
	-newkey rsa:SIZE mycert.pem -out mycert.pem

```

## General Commands

### Check OpenSSL Version
- Displays the OpenSSL version along with build and configuration information.
  ```bash
  openssl version -a
  ```

### Get Basic Help
- Provides a list of standard commands.
  ```bash
  openssl help
  ```

## Performance Testing

### Test Algorithm Performance
- Tests the performance of cryptographic algorithms using multiple CPU cores.
- Example: Using 4 CPU cores to test RSA algorithm speed.
  ```bash
  openssl speed -multi 4 rsa
  ```
#opensslperformance
## Random Number Generation
#RNG
### Generate Random Bytes
- Generates a specified number of random bytes.
- Example: Generating 20 random bytes in hexadecimal format.
  ```bash
  openssl rand -hex 20
  ```

## Encoding and Decoding

### Base64 Encoding
- Encode files or text using Base64 encoding.

  - **File Encoding**
    ```bash
    openssl base64 -in file.data
    ```
  
  - **Text Encoding**
    ```bash
    echo -n "some text" | openssl base64
    ```

### Base64 Decoding
- Decode Base64 encoded files.
- Example: Decoding a Base64 encoded file.
  ```bash
  openssl base64 -d -in encoded.data -out decoded.data
  ```
#opensslbase64
## Working with Hashes

### List Digest Algorithms
- Lists all available digest algorithms.
  ```bash
  openssl list -digest-algorithms
  ```
#opensslhash
### Hashing Files
- Computes the hash of a file using a specific algorithm.
- Example: Hashing a file using SHA256.
  ```bash
  openssl dgst -sha256 file.data
  ```

### Hashing with Binary Output
- Generates a hash with binary output (useful for piping into other commands or files).
- Example: SHA256 hash of a file with binary output.
  ```bash
  openssl dgst -binary -sha256 file.data | xxd
  ```
#opensslhashbin
### Hashing Text
- Hashes given text using a specified algorithm.
- Example: Hashing text using SHA3-512.
  ```bash
  echo -n "some text" | openssl dgst -sha3-512
  ```

### Creating HMAC
- Creates a Hash-based Message Authentication Code (HMAC) using a given key.
- Example: Create HMAC-SHA384 of a file using a specific key.
  ```bash
  openssl dgst -SHA384 -mac HMAC -macopt hexkey:369bd7d655 file.data
  ```
#opensslhmac

---
## Asymmetric Encryption

### List Available Elliptic Curves
- Lists all elliptic curves supported by OpenSSL.
  ```bash
  openssl ecparam -list_curves
  ```
#opensslECC
### Generate RSA Key Pair
- Generates an RSA key pair of a specified size.
- Example: Generating a 4096-bit RSA key pair.
  ```bash
  openssl genrsa -out pub_priv.key 4096
  ```
#opensslRSA
### Display RSA Key Information
- Displays detailed information about an RSA private key.
  ```bash
  openssl rsa -text -in pub_priv.key -noout
  ```

### Encrypt RSA Key Pair
- Encrypts an RSA private key using a specified algorithm (e.g., AES-256).
  ```bash
  openssl rsa -in pub_priv.key -out encrypted.key -aes256
  ```
#opensslRSAENC
### Convert RSA Private Key
- Converts an encrypted RSA private key into a non-encrypted form.
  ```bash
  openssl rsa -in encrypted.key -out cleartext.key
  ```
#opensslRSAPriv
### Extract Public Key from RSA Key Pair
- Extracts the public key from an RSA key pair.
  ```bash
  openssl rsa -in pub_priv.key -pubout -out pubkey.key
  ```
#opensslRSAExtractPublicKey
### Encrypt File using RSA Public Key
- Encrypts a file using an RSA public key.
  ```bash
  openssl rsautl -encrypt -inkey pubkey.key -pubin -in cleartext.file -out ciphertext.file
  ```
#OpenSSLRSAEncryptFile
### Decrypt File using RSA Private Key
- Decrypts a file using an RSA private key.
  ```bash
  openssl rsautl -decrypt -inkey pub_priv.key -in ciphertext.file -out decrypted.file
  ```

### Generate EC Private Key
- Generates a private key using a specified elliptic curve.
- Example: Using the P-224 curve.
  ```bash
  openssl ecparam -name secp224k1 -genkey -out ecpriv.key
  ```
#OpenSSLGenECCKey
### Encrypt EC Private Key
- Encrypts an EC private key using a specified algorithm (e.g., 3DES).
  ```bash
  openssl ec -in ecpriv.key -des3 -out ecpriv_enc.key
  ```
#OpenSSLECCENC
## Symmetric Encryption

### List Symmetric Ciphers
- Lists all supported symmetric encryption ciphers.
  ```bash
  openssl enc -list
  ```
#OpensslListCiphers
### Encrypt File with AES
- Encrypts a file using AES and a specified mode (e.g., AES-128-ECB).
- Example: Using an ASCII password.
  ```bash
  openssl enc -aes-128-ecb -in cleartext.file -out ciphertext.file -pass pass:thisisthepassword
  ```
#OpenSSLAESEncFile
### Decrypt File with AES
- Decrypts a file using AES and a specified mode (e.g., AES-256-CBC).
- Example: Using a key file.
  ```bash
  openssl enc -d -aes-256-cbc -in ciphertext.file -out cleartext.file -pass file:./key.file
  ```
#OpenSSLAESDecFile
### Encrypt with Specific Key and IV
- Encrypts a file using a specific key and IV in hexadecimal format.
- Example: Using ARIA-256-CBC.
  ```bash
  openssl enc -aria-256-cbc -in cleartext.file -out ciphertext.file -K [Hex Key] -iv [Hex IV] -nosalt
  ```

## Digital Signatures

### Generate DSA Parameters
- Generates DSA parameters for key generation.
- Example: 2048-bit length.
  ```bash
  openssl dsaparam -out dsaparam.pem 2048
  ```

### Generate DSA Key Pair
- Generates a DSA key pair for signing, optionally encrypted with a specified algorithm.
  ```bash
  openssl gendsa -out dsaprivatekey.pem -aes-128-cbc dsaparam.pem
  ```

### Extract DSA Public Key
- Extracts the public key from a DSA key pair.
  ```bash
  openssl dsa -in dsaprivatekey.pem -pubout -out dsapublickey.pem
  ```

### Sign File using RSA
- Signs a file using an RSA private key and a specified hash algorithm.
- Example: SHA-256.
  ```bash
  openssl dgst -sha256 -sign rsakey.key -out signature.data document.pdf
  ```

### Verify Signature with RSA Public Key
- Verifies a file signature using an RSA public key.
  ```bash
  openssl dgst -sha256 -verify publickey.pem -signature signature.data original.file
  ```

## Working with TLS Protocol

### List Cipher Suites
- Lists all or

 specific cipher suites supported by OpenSSL.
- Example: Listing all cipher suites.
  ```bash
  openssl ciphers -V 'ALL'
  ```

### TLS Connection to Server
- Establishes a TLS connection to a server on a specified port.
- Example: Connecting to a server using TLS v1.2.
  ```bash
  openssl s_client -tls1_2 -connect domain.com:443
  ```

### Display Certificates from TLS Connection
- Connects to a server and displays the certificates provided by the server.
  ```bash
  openssl s_client -showcerts domain.com:443
  ```

### Set up TLS Server
- Sets up a listening port to accept TLS connections using a specified certificate and private key.
- Example: Supporting only TLS 1.2.
  ```bash
  openssl s_server -port 443 -cert cert.crt -key priv.key -tls1_2
  ```

---
## Digital Certificates

### Generate CSR and RSA Key Pair
- Generates a Certificate Signing Request (CSR) and an RSA key pair.
  ```bash
  openssl req -newkey rsa:4096 -keyout private.key -out request.csr
  ```

### Display CSR Information
- Shows the content of a CSR file.
  ```bash
  openssl req -text -noout -in request.csr
  ```

### Create Self-Signed Certificate
- Generates a self-signed certificate with a new RSA key pair.
  ```bash
  openssl req -newkey rsa:2048 -nodes -keyout priv.key -x509 -days 365 -out cert.crt
  ```

### Sign a CSR with Your CA
- Creates and signs a certificate using a CSR and your CA's private key (requires `openssl.cnf` setup).
  ```bash
  openssl ca -in request.csr -out certificate.crt -config ./CA/config/openssl.cnf
  ```

### Verify a Certificate Chain
- Checks the validity of a certificate chain file.
  ```bash
  openssl verify -CAfile ca-chain.cert.pem server.cert.pem
  ```

## Advanced TLS/SSL Testing

### Test SSL/TLS Handshake
- Performs an SSL/TLS handshake test with a server.
  ```bash
  openssl s_client -connect domain.com:443 -tls1_2
  ```

### Display Specific Cipher Suite Information
- Lists detailed information about specific cipher suites.
  ```bash
  openssl ciphers -V 'ECDHE-RSA-AES256-GCM-SHA384'
  ```

### Test Server for Specific Cipher Suite
- Tests if a server supports a specific cipher suite.
  ```bash
  openssl s_client -cipher 'ECDHE-RSA-AES256-GCM-SHA384' -connect domain.com:443
  ```

### Extract TLS Certificate from Server
- Retrieves the TLS certificate from a server.
  ```bash
  openssl s_client -connect domain.com:443 | openssl x509 -out certificate.crt
  ```

### Test TLS Connection with SNI Support
- Tests a TLS connection to a server that requires Server Name Indication (SNI).
  ```bash
  openssl s_client -servername domain.com -connect domain.com:443
  ```

## PKCS#12 and Key Management

### Convert Certificate and Key to PKCS#12
- Combines certificate and private key into a PKCS#12 file (`.p12`/`.pfx` format).
  ```bash
  openssl pkcs12 -export -out certificate.pfx -inkey private.key -in certificate.crt
  ```

### Extract Certificate and Key from PKCS#12
- Extracts the certificate and key from a PKCS#12 file.
  ```bash
  openssl pkcs12 -in certificate.pfx -out certificate.crt -nodes
  ```

### Convert PEM to DER Format
- Converts a certificate from PEM to DER format.
  ```bash
  openssl x509 -inform PEM -outform DER -in certificate.pem -out certificate.der
  ```

## Advanced Configuration

### Custom OpenSSL Configuration File
- Utilize a custom `openssl.cnf` file for complex CA and certificate operations.
  ```bash
  openssl req -new -config custom_openssl.cnf
  ```

### Create a Simple CA Using OpenSSL
- Set up a basic Certificate Authority using OpenSSL (see `openssl.cnf` example in the previous cheat sheet).

### Convert CRL to PEM Format
- Converts a Certificate Revocation List (CRL) from DER to PEM format.
  ```bash
  openssl crl -inform DER -in crl.der -outform PEM -out crl.pem
  ```

### Create ECDSA Keys and Certificates
- Generate an ECDSA key pair and certificate.
  ```bash
  openssl ecparam -name prime256v1 -genkey -out ecdsa.key
  openssl req -new -key ecdsa.key -out ecdsa.csr
  openssl x509 -req -in ecdsa.csr -signkey ecdsa.key -out ecdsa.crt
  ```

### Advanced CSR with SAN (Subject Alternative Name)
- Generate a CSR with SAN for multi-domain certificates.
  ```bash
  openssl req -new -key server.key -out server.csr -config san_openssl.cnf
  ```

____