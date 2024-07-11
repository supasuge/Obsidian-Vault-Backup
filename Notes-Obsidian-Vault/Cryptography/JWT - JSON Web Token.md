## Table of Contents

      - [Structure of JWT](#Structure\of\JWT)


compact URL save token format used for transmitting information between parties. The information is digitally signed, ensuring its integrity. A JWT can be signed using a secret(with HMAc) or a pub/priv key paur using RSA or ECDSA

#### Structure of JWT
1. Header: Contains the token type and the signing algorithtm
2. Payload: Contains the claims or the peives ofinformamtion being passed about te user