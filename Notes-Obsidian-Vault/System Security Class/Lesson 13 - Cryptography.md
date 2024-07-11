Asymmetric/Symmetric key Cryptography.

Asymmetric: Public/Private Key pairs
Symmetric: Same key for encryption/Decryption.

Cryptography isn't only about confidentiality. Another property of most cryptosystems is integrity. You want to know not only that the data being sent was protected, but also that it came from the person you expected it to. And you also want to make sure that the data being sent was received for non-repudiation purposes. 

Hashes generates a fixed-length output from a variable--length input. 

Encryption is about privacy and confidentiality.

### Substitution Ciphers
In the rotation cipher, this takes an alphabet, then easily convert it by rotating the letter by some value. Letter's are simply substituted for another in a fixed, known way. 

### Diffie-Hellman
Often enough, the keys are known in advance, often called *pre-shared keys* because they have been shared in advanced of their need. If keys are ot shared ahead of time, there needs to be a way for two parties in an encrypted communication to share keys that aren't shared ahead of time and without being intercepted. A better approach is to derive keys. This means the key is never exchanged.

### Symmetric Encryption
- Block Ciphers - Takes the entire block of data to be encrypted and turn it into fixed-length  blocks. If the total length of the data isn't a multiple of the block size, the last block is padded to get to the size of the block. 
- Stream Ciphers - Encrypts data byte-by-byte

### Asymmetric Encryption
Public key - used for encryption, publicly available. Mathematically connected to private key.
Private Key - kept secret and used for decryption. 
#### Data Encryption Standard
DES is a block cipher that uses a symmetric key. This is long deprecated though 3DES which is also deprecated by AES(128,192,256) which is the current standard.

AES - Rijndael Cipher (basis for AES). Block cipher with multiple key lengths. 


Side Channel Attakcs: Relies on using something other than a weakness in the algorithm. Instead hte implementation becomes the target. Information can be leaked as a result of power consumption or processor utilization, for instance: There may alos be electromagnetic leaks that could provide information to an attacker. 


Attacks on AES are still possible, but unlikely due to the significant amount of computing resources required. Such as a key recovery attack, and other proposed attacks on the key schedule used by AES.

### Certificate Authority
A CA is a repository of certificates. It issues certificates to users, which means it collects information from the user and then generates the key to provide to the user. The certificate is stored in the authority and also provided to the user. Of course, someone has to manage the certificate repository. The CA and related systems is called **PKI** (Public Key Infrastructure). A company that has one of these repositories is sometimes called a **certificate authority** since it is the authority that managers the certificates. This is the case with a piece of software called SimpleAuthority. This software uses the OpenSSL Library utilities to generate certificates and handles the storage, creation, and management through a graphical interface. The root certificate is the certificate that all other certificates in the CA are signed by.

Once the root certificate has been generated, you can create other certificates. Any entity that wants to encrypt messages using public key cryptography needs to have a certificaet. If you wante to send messages to someone, you can get a certificate. Servers, though, also need to have certificates. Any web server that uses encrypted messages, and these days it's rare to find a web server that isn't encrypting traffic, needs to have a certificate. you can see that as one of the options. If you were going to set up a web server that was going to use TLS, you would select SSL server, since TLS is the current implementation of what was once SSL.

Once you have provided the details necessary for the certificate, the CA will create the keys and store the certificate. The CA will then provide the means to obtain the certificate for the user. This is necessary sincee the user ahs to have the private key. The CA can provide the public key to anyone who wants it. but the user who owns the certificate needs to have the private key portion. Once the certificate has been created, you can open it to look at the details. What you will see there are the details provided during the certificate creation as well as information about the key. What you can't see is the key itself. This is just a set of bits that would not be printable, since each byte is not limited to the set of values that are considered printable in the ASCII table. Any byte value is possible, and most of them would just look like gibberish.

Instead of the actual key itself, you can see a fingerprint of the key. The fingerprint is a fixed-length value that is shown in hexadecimal. You could, of course, also show the key in hexadecimal rather than trying to generate characcters from the byte values. A 4906-bit key, though, would be 512 bytes. A byte is represented by two hexadecimal digits. That means showing a 4096 bit key in hexadecimal would be 1024 characters. 

When you use a CA, the authority can revoke certificates. This may because a user is no longer associated with the organization. Enterprises often run their own authorities. Since there is identification information in the certificate, like an email address, if the email address is no longer valid, the certificate may need to be revoked. When a certificate has been revoked, any party validating the certificate against the authority should not accept the certificate. This is  important since there is an element of identity associated with certificates. The certificate can provide a verified identity, which could be used to authenticate someone. If a certificate has been revoked, the user should not be trusted.

Revoked certificates are managed through the use of a CRL(**Certificate Revocation list**). The problem with CRLs historically has been that they are not always requested to validate a certificate. There may also not be a good way to provide the CRL to a system validating a certificate, dependi8ng on the network location of the CA relative to the validating system. And the firewalls that would preclude the communication. As a resut, the Online Certificate Status Protocol (OCSP) can be used to verifiy a certificate with a CA.

### Trusted Third Party
Advantage to a CA is there is a central authority used to store certificates and manage them but also to perform verification of identity. Remember you provide identification information into the certificate. Nonrepudiation doesn't work if the certificate doesn't verifiably belong to anyone. Just providing a name and even an email address is insufficient to verify someone's identity. It could be spoofed, or it could be coming from an attacker who has access to the correct email address. The way to address this is to have someone who actually verifies identity. This may come from checking some identification credential that demonstrates that you are who you say you are. In the case of a third-party providedd like VeriSign, you may be expected to securely provide a photo identification before they will issues your certificaet.

This brings in the idea of a trusted third party. You want to always know that the person you are sending secure messages to or receiving signed messages is from the person you expect them to be. This works with a CA because of the transitive property. The transitive property says, in  this case, that if youj trust the CA and the CA trusts that another person is who they say they are, then youj, too, believe that person is who they say they are. I trust you, you trust Zoey, therefore I trust Zoe. This assumes that the CA is doing a full validation on the person who has requested the certificate.

- Every certificate that is generated by a CA is signed by the CA's root certificate. (CA adds a digital signature.)
- Up to the endpoints to validate the certificate. 
- 

#### Self-Signed Certificate
Sometimes you want to just encrypt messages between two systems in a lab, for instance. You can create you own certificate and use it. This will result in certificate errors because the certificate won't be signed. It will be a bare certificate. 

**Certificate Generation**  
  

```bash
root@quiche:~# openssl req -x509 -newkey rsa:4096 -keyout key.pem  
-out cert.pem -days 365
```
This uses `openssl` to issue a reques for an X.509 certifcate. Thats gets done in the first part of the command line, then provide details about the key that will be embedded into the certificate. It's an RSA key that is 4096 bits in length. The key gets written out to the file `key.out` while the certificate itself is written out to `key.pem`. Then indicate how long the certificate is good for. 

### Hashes
Used to verify data. Always send with the copy of encrypted message in order to verify the data once decrypted. This is done by using a Message Authentication Code. The MAC here is a fixed-length value that is generated by running the entire message through a cryptographic algorithm. The output is often referred to as a **hash**. It can be used for multiple purposes beyond just being able to verify a message that has been sent.

A common hash function, though starting to be deprecated, is Message Digest 5 (MD5). MD5 is a hash algorithm that takes an arbitrary length input and generates a fixed-length output. Generating an MD5 hash will yield 32 hexadecimal characters, which is 128-bits. When it comes to cryptographyic hashes. It's not a linear function. This means event the change of a single bit will generate a completely different value. You can see this in the following code listing where I take the string `"123456789` and get an MD5 hash from it using the `md5sum` command. The second time around, I get a hash from the string `223456789`, which is a change of a single bit. The difference in MD5 output is completely different. One value has no relation to the other in spite of being different by a single bit.
- Used for file integrity systems like Tripwire. Essential files in the file system have a hash computed and stored. The software then goes through periodically to compare what is on the file system to what is in the known good database. If there is a change, it is flagged as a possibility of system compromise. Host based intrustion detection systems will often use elements like this to be able to detect when an attacker gains access to a system.

SHA2 - Uses the Davies-Meyer structure and supports four different digest sizes.
SHA1 - Provides a 160-bit output, which is 20 bytres and yields 40 Hex digits.
SHA3 - Uses the sponge structure and supports arbitrary lengths.
MD5 - Takes arbitrary-length input and generates a fixed-length output, and yields 32 hexadecimal characters.

### PGP and S/MIME
Not everyone appreciates a dentralized management approach, where organizations are trusted. Phil Zimmermann developed another way of managing certificates that does not use a CA for centralized verification. Instead, Pretty Good Privacy (PGP) uses a "web of trust" to perform  verification. The idea is that keys are all uploaded to a web server. Someone who knows the person who the person who has uploaded their key will sign that key as a demonstration that they know the person and are willing to say that key really belongs to the use it purports to belong to.

- Public key is being stored in the public web service
Say you have someone's email address whom already uploaded their key to the server with there email address, You go the PGP key site, and sign her key with your signature. Anyone who knows me but doesn't the other party can be assured that the other key is legitimate. You can trust that zoey's key is legitimate, and you can send encrypted messages to her using her public key.

- Keys are stored in a "keyring". This keyring would typically be signed with your key and also stored with a message authentication code so you can be sure it hasn't been tampered with.
- When it comes to encryption/privacy, ensuring the validity of every aspect of the process and protecting the keys are important. Downsite of PGP is that keys are managed by users. This means that you have to keep track of your key and keep it with you. 


PGP doesn't work well for certificates for servers. Instead, it's used to send messages like email from one user to another. PGP can be used to encrypt an email message that is sent from one user to another. As long as they are running PGP software. PGP is not the only solution for email encryption though.

S/MIME = Secure/Multipurpose Internet Mail Extensions (S/MIME) is another protocol for sending encrypted messages. This standard is generally implemented in email clients. Meaning there is no need for third-party software. S/MIME also used X.509 certificates from certificate authorities. These certificates may commonly be installed inside a Windows Active Directory. In a fully Microsoft environment, users within an enterprise can send encrypted messages to one another and public keys can be retrieved from the Active Directory server. This means there is no need to have other root CA keys in the system or require any other methods to get the public key onto the client system.


13.8

### Summary

Encryption is an important concept because privacy is so important. This is especially the case when attackers are looking for any advantage they can get. If they can intercept messages that are not encrypted, they may be able to make use of the contents of the message. Users will sometimes make the mistake of believing that messages sent to other users within an enterprise are safe because they remain inside the enterprise. These messages are not safe because they can be intercepted and used. The same can be true of disk‐based encryption. You can't assume that a disk that has been encrypted is safe. Once someone has authenticated as a legitimate user, the disk is unencrypted. This means if an attacker can gain authenticated access, even by introducing malware that is run as the primary user, the disk is wide open to the attacker.  
  
In cryptography, there is one more concept known as steganography. Steganography is a technique for concealing secret data in a non-secret file or message to avoid detection; the secret data is then extracted at its destination. The word steganography comes from the Greek words steganos (meaning hidden or covered) and root graph (meaning to write).  
  
There are two types of encryption when you think about the end result. The first is substitution, where one character is substituted for another character. This is common with encryption schemes like a rotation cipher and the Vigenère cipher. The second type is a transformation cipher. This is where the unencrypted message, or plain text, is not replaced one character at a time but the entire message is transformed, generally through a mathematical process. This transformation may be done with fixed‐length chunks of the message, which is a block cipher. It may also be done byte by byte, which is how a stream cipher works. With a block cipher, the data size is expected to be a multiple of the block size. The final block may need to be padded to get it to the right size.  
  
Key management is essential. An important element of that can be key creation. You could use pre‐shared keys, which could be learned or intercepted while they are being shared. If you don't use a pre‐shared key, the key could be derived. This may be done using the Diffie‐Hellman key exchange protocol. Using a common starting point, both parties in the process add a value and pass it to the other party. Once the value has been added to the shared key, you end up with both sides having the common value plus the random value from side A plus the random value from side B. Both sides have the same key and can begin sending encrypted messages.  
  
This process could be used for symmetric keys, where the same key is used for both encryption and decryption. The Advanced Encryption Standard (AES) is a common symmetric key encryption algorithm. AES supports 128, 192, and 256 bits. You might also use an asymmetric key algorithm where different keys are used for encryption and decryption. This is sometimes referred to as _public key cryptography_. A common approach is to use a hybrid cryptosystem where public key cryptography is used to share a session key, which is a symmetric key used to encrypt messages within the session.  
  
Certificates, defined by X.509, which is a subset of the X.500 digital directory standard, are used to store public key information. This includes information about the identity of the certificate holder so verification of the certificate can happen. Certificates may be managed using a CA, which is a trusted third party that verifies the identity of the certificate holder. A CA is not the only way to verify identity, though. PGP uses a web of trust model, where individual users validate identity by signing the public keys of people they know.  
  
A MAC is used to ensure that messages haven't been altered. This is generally a cryptographic hash, which is an algorithm that generates a fixed‐length digest value from variable‐length data. This can be used not only for message authentication but also for verifying that files have not been tampered with or corrupted.  
  
Full disk encryption is a common technique on systems today. Windows systems will typically use BitLocker, though there are third‐party software solutions that will perform the same function. On macOS systems, you would use FileVault, and Linux uses `dm‐crypt`, which is built into the kernel but still requires utilities to be installed on the system to handle setting up volume encryption and then opening encrypted volumes. As with any encryption, key management is essential, and encrypted files or file systems are not protections against everything. Encrypted files may be a good way of losing data if the key or password is lost.


Cryptography is essential for securing data and communications, protecting against unauthorized interception and access. It utilizes two primary encryption methods: 

1. **Substitution Ciphers**: These include simple schemes like the Caesar cipher, a monoalphabetic substitution cipher shifting characters by a fixed number (e.g., rot3), and more complex polyalphabetic substitution ciphers like the Vigenère cipher, which uses multiple substitution alphabets for encryption. 
2. **Transformation Ciphers**: These alter the entire message via mathematical processes. Transformation ciphers can be categorized into block ciphers, which encrypt fixed-size blocks of plaintext (e.g., AES operating in modes like CBC, CFB, and OFB), and stream ciphers, encrypting plaintext digits one at a time in a stream (e.g., XOR operations with a pseudorandom cipher digit stream).

**Key Management** is crucial, involving the creation, distribution, and storage of keys. It supports both symmetric (e.g., AES, using the same key for encryption and decryption) and asymmetric cryptography (using paired public and private keys). The Diffie‐Hellman protocol exemplifies key exchange, allowing two parties to generate a shared secret over an unsecured communication channel.

**Public Key Infrastructure (PKI)**, with X.509 certificates, enables secure public key exchanges, supported by Certificate Authorities (CAs) or trust models like PGP's web of trust for identity verification.

**Integrity Verification** employs Message Authentication Codes (MACs) and cryptographic hashes to ensure data hasn't been altered, using algorithms to produce a fixed-length digest from variable-length data.

**Full Disk Encryption** protects data at rest, with tools like BitLocker (Windows), FileVault (macOS), and `dm-crypt` (Linux), emphasizing the importance of key management to avoid data loss.

**Steganography** complements cryptography by hiding secret data within non-secret files to avoid detection, adding an extra layer of security.

This condensed overview emphasizes the importance of various encryption methods, key management practices, integrity verification techniques, and the role of steganography in ensuring comprehensive data security and privacy.


### Question: What are the main concepts covered in the chapter on cryptography?

### Answer:
The chapter introduces key concepts in cryptography, focusing on encryption methods, key management, integrity verification, and steganography. It distinguishes between substitution and transformation ciphers, discusses symmetric and asymmetric key algorithms, and explains the significance of key management. The chapter also covers public key infrastructure (PKI), message authentication, full disk encryption, and the role of steganography in security.

### Discussion:

**What I Learned:**

- **Encryption Methods:** The chapter elaborates on two primary encryption techniques: substitution and transformation ciphers. I learned that substitution ciphers, such as the Caesar and Vigenère ciphers, replace individual characters based on a cipher algorithm. Transformation ciphers, on the other hand, modify the entire message through mathematical operations, and can be divided into block and stream ciphers, with AES being a prominent example of a block cipher.

- **Key Management:** This section highlighted the importance of securely creating, sharing, and storing cryptographic keys. It introduced me to concepts like symmetric encryption, where a single key encrypts and decrypts data, and asymmetric encryption, which uses a pair of public and private keys. The Diffie‐Hellman key exchange protocol was particularly interesting, demonstrating how two parties can safely establish a shared secret over an insecure channel.

- **Public Key Infrastructure (PKI):** I discovered how PKI uses X.509 certificates to manage public keys and identities, facilitating secure communication over the internet. Certificate authorities (CAs) and alternative trust models, such as PGP's web of trust, play crucial roles in verifying identities and ensuring the authenticity of public keys.

- **Integrity Verification:** The use of Message Authentication Codes (MACs) and cryptographic hashes to verify data integrity and authenticity was an eye-opener. These methods ensure that data has not been tampered with during transmission, providing a layer of security beyond simple encryption.

- **Full Disk Encryption:** Learning about tools like BitLocker, FileVault, and `dm-crypt` revealed how encryption can protect data at rest, emphasizing the necessity of robust key management to prevent unauthorized access and potential data loss.

- **Steganography:** This concept was fascinating, showing how data can be hidden within other non-secret files or messages to avoid detection altogether, adding an extra layer of security to encrypted data.

**Conclusion:**

Through this chapter, I gained a comprehensive understanding of the fundamental principles of cryptography, the practical applications of various encryption techniques, and the critical importance of key management and data integrity in ensuring secure communication and data storage. The exploration of steganography as a supplementary security measure further broadened my perspective on the depth and breadth of methods available to protect information in the digital age.

In the chapter on cryptography, I learned about the essential techniques and concepts for securing digital information. Encryption, the process of converting readable data into a coded format, is categorized into substitution and transformation ciphers. Substitution ciphers, like the Caesar cipher, involve replacing characters based on a specific system, while transformation ciphers, including block and stream ciphers like AES, alter entire messages through mathematical operations.

Key management emerged as a vital aspect, involving the secure creation, sharing, and storage of keys. It encompasses symmetric encryption, where the same key encrypts and decrypts data, and asymmetric encryption, which uses paired public and private keys.

Public Key Infrastructure (PKI) and the use of X.509 certificates facilitate secure public key exchanges and identity verification. Integrity verification through Message Authentication Codes (MACs) and cryptographic hashes ensures data hasn't been tampered with.

Full disk encryption protects data at rest, with tools like BitLocker and FileVault, highlighting the importance of key management. Lastly, steganography, the practice of hiding data within non-secret files, adds an extra layer of security. This chapter provided a foundational understanding of cryptography's role in digital security, emphasizing encryption, key management, and the importance of protecting information integrity.

Encryption is vital for protecting data privacy, even within an enterprise. The two main types are substitution ciphers (e.g., Caesar with ROT3, Vigenère as polyalphabetic) and transformation ciphers (block ciphers like AES with modes of operation, stream ciphers like XOR).

Proper key management is critical. Keys can be pre-shared or derived via protocols like Diffie-Hellman. Symmetric algorithms (e.g., AES) use the same key for encryption and decryption, while asymmetric ones (public key) use different keys. Hybrid systems combine both, using public key crypto to share a symmetric session key.

X.509 certificates manage public keys and identities, verified by Certificate Authorities (CAs) or web of trust (PGP). Message authentication codes (MACs) ensure integrity using cryptographic hash digests.

Full disk encryption (BitLocker, FileVault, dm-crypt) protects data at rest, but not from authenticated users or lost keys. Steganography hides secret data within non-secret files to evade detection.


