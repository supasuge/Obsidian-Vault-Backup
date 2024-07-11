## Table of Contents

        - [When to use](#When\to\use)
      - [Via Proxy using proxychains](#Via\Proxy\using\proxychains)
        - [Lists usernames if the server allows it](#Lists\usernames\if\the\server\allows\it)
        - [if you have credentials, you can pull a full list of users regardless of the RestrictAnonymous option](#if\you\have\credentials,\you\can\pull\a\full\list\of\users\regardless\of\the\RestrictAnonymous\option)
        - [Pulls usernames from the default RID range (500-550,1000-1050)](#Pulls\usernames\from\the\default\RID\range\(500-550,1000-1050))

##### When to use
```bash
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
```

`enum4linux -u username -p password -a dc-ip-address`

#### Via Proxy using proxychains
```bash
proxychains -q enum4linux -u username -p password -a dc-ip-address
```

##### Lists usernames if the server allows it
```bash
enum4linux -U TARGET_IP
```

##### if you have credentials, you can pull a full list of users regardless of the RestrictAnonymous option
```bash
enum4linux -u administrator -p password -U target-ip
```

##### Pulls usernames from the default RID range (500-550,1000-1050)
```bash



```










