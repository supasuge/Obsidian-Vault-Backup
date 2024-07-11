## Table of Contents

  - [What is an X.509 Certificate Used For?](#What\is\an\X.509\Certificate\Used\For?)
    - [Certificate Authority](#Certificate\Authority)
      - [Certificate Transparency](#Certificate\Transparency)

## What is an X.509 Certificate Used For?
- Email Security
- Online Document Signature's
- TLS (Transport Layer Security)
- SSL (Secure Sockets Layer)
Standard format for public key certificates.
- Different versions
- Not all certificates require public trust

Each **Certificate** includes:
- Public Key
- Digital Signature
- Issuing CA
- Additional Information About the certificate


Click the (lock) icon in your browser, click on the Certificate > Details
- **Version:** The iteration of the X.509 certificate being issued to a user.
- **Serial Number:** A unique number assigned to each X.509 certificate by the CA.
- **Signature Algorithm ID:** The specific mathematical algorithm used to create and encrypt the CA’s private key.
- **Issuer:** The name of the CA who issued the X.509 certificate.
- **Validity Period:** The timeframe in which the X.509 certificate can be used before it expires and becomes obsolete. It includes the start and end dates that the certificate is viable.
- **Subject:** The user’s name or the type of device that receives the X.509 certificate from the CA.
- **Subject Public Key Information:** This includes the algorithm used to generate the public key attached to the X.509 certificate, the public key itself, and additional data such as the key’s size and unique function.
- **Certificate Signature Algorithm:** The type of algorithm involved in signing and encrypting the X.509 certificate.
- **Certificate Signature:** A long, alphanumerical string unique to the identity of the CA issuing the X.509 certificate.
### Certificate Authority
Company or organization that acts to validate the identities of entities and bind them to cryptographic keys through the issuance of digital certificates.
[Source](https://courses.cs.washington.edu/courses/cse484/21wi/sections/slides/section_05.pdf)
![[Pasted image 20231220055333.png]]

#### Certificate Transparency
Used for monitoring and auditing digital certificates
- 


