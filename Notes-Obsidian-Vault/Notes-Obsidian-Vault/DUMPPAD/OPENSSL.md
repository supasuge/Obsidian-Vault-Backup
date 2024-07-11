## Table of Contents

- [list only TLSv1 ciphers](#list\only\tlsv1\ciphers)
- [list only high encryption ciphers (keys larger than 128 bits)](#list\only\high\encryption\ciphers\(keys\larger\than\128\bits))
- [list only high encryption ciphers using the AES algorithm](#list\only\high\encryption\ciphers\using\the\aes\algorithm)
- [test rsa speeds](#test\rsa\speeds)
    - [How to test with an SSL-enabled](#How\to\test\with\an\SSL-enabled)
- [on one host, set up the server (using default port 4433)](#on\one\host,\set\up\the\server\(using\default\port\4433))
- [on second host (or even the same one), run s_time](#on\second\host\(or\even\the\same\one),\run\s_time)
  - [How do I generate a self-signed certificate?](#How\do\I\generate\a\self-signed\certificate?)

`version -a` - version information

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

You’ll first need to decide whether or not you want to encrypt your key. Doing so means that the key is protected by a passphrase.
```sh
openssl req \
	-x509 -nodes -days 365 -sha256 \
	-newkey rsa:SIZE mycert.pem -out mycert.pem

```

Using this command-line invocation, you’ll have to answer a lot of questions: Country Name, State, City, and so on. The tricky question is “Common Name.” You’ll want to answer with the _hostname or CNAME by which people will address the server_. This is very important







