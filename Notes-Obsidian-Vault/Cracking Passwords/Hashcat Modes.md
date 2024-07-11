## Table of Contents

      - [Generate SHA1 Hashes](#Generate\SHA1\Hashes)
      - [Root Password in Ubuntu Linux](#Root\Password\in\Ubuntu\Linux)
      - [NetNTLMv2](#NetNTLMv2)
      - [Responder - NTLMv2](#Responder\-\NTLMv2)



|   |   |   |
|---|---|---|
|0|MD5|8743b52063cd84097a65d1633f5c74f5|
|100|SHA1|b89eaac7e61417341b710b727768294d0e6a277b|
|1000|NTLM|b4b9b02e6f09a9bd760f388b67351e2b|
|1800|sha512crypt $6$, SHA512 (Unix)|$6$52450745$k5ka2p8bFuSmoVT1tzOyyuaREkkKBcCNqoDKzYiJL9RaE8yMnPgh2XzzF0NDrUhgrcLwg78xs1w5pJiypEdFX/|
|3200|bcrypt $2*$, Blowfish (Unix)|$2a$05$LhayLxezLhK1LhWvKxCyLOj0j1u.Kj0jZ0pEmm134uzrQlFvQJLF6|
|5500|NetNTLMv1 / NetNTLMv1+ESS|u4-netntlm::kNS:338d08f8e26de93300000000000000000000000000000000:9526fb8c23a90751cdd619b6cea564742e1e4bf33006ba41:cb8086049ec4736c|
|5600|NetNTLMv2|admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030|
|13100|Kerberos 5 TGS-REP etype 23|$krb5tgs$23$_user$realm$test/spn_$63386d22d359fe42230300d56852c9eb$ < SNIP >|

#### Generate SHA1 Hashes

  Generate SHA1 Hashes

```shell-session
gdxqpardo@htb[/htb]$ for i in $(cat words); do echo -n $i | sha1sum | tr -d ' -';done
```


#### Root Password in Ubuntu Linux

  Root Password in Ubuntu Linux

```shell
root:$6$tOA0cyybhb/Hr7DN$htr2vffCWiPGnyFOicJiXJVMbk1muPORR.eRGYfBYUnNPUjWABGPFiphjIjJC5xPfFUASIbVKDAHS3vTW1qU.1:18285:0:99999:7:::
```

The hash contains nine fields separated by colons. The first two fields contain the username and its encrypted hash. The rest of the fields contain various attributes such as password creation time, last change time, and expiry.
  
Root Password in Ubuntu Linux

```shell-session
gdxqpardo@htb[/htb]$ hashcat -m 1800 nix_hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```

#### NetNTLMv2

During a penetration test it is common to run tools such as [Responder](https://github.com/lgandx/Responder) to perform MITM attacks to attempt to "steal" credentials. These types of attacks are covered in-depth in other modules. In busy corporate networks it is common to retrieve many NetNTLMv2 password hashes using this method. These can often be cracked and leveraged to establish a foothold in the Active Directory environment or sometimes even gain full administrative access to many or all systems depending on the privileges granted to the user account associated with the password hash. Consider the password hash below retrieved using `Responder` at the beginning of an assessment:

#### Responder - NTLMv2

  Responder - NTLMv2

```shell-session
sqladmin::INLANEFREIGHT:f54d6f198a7a47d4:7FECABAE13101DAAA20F1B09F7F7A4EA:0101000000000000C0653150DE09D20126F3F71DF13C1FD8000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C0003003400570049004E002D00500052004800340039003200520051004100460056002E0053004D00420033002E006C006F00630061006C000500140053004D00420033002E006C006F00630061006C0007000800C0653150DE09D201060004000200000008003000300000000000000000000000003000001A67637962F2B7BF297745E6074934196D5F4371B6BA3E796F2997306FD4C1C00A001000000000000000000000000000000000000900280063006900660073002F003100390032002E003100360038002E003100390035002E00310037003000000000000000000000000000
```
NetNTLMv2 hash, or mode `5600` in `Hashcat`.

|**Mode**|**Target**|
|---|---|
|`11600`|7-Zip|
|`13600`|WinZip|
|`17200`|PKZIP (Compressed)|
|`17210`|PKZIP (Uncompressed)|
|`17220`|PKZIP (Compressed Multi-File)|
|`17225`|PKZIP (Mixed Multi-File)|
|`17230`|PKZIP (Compressed Multi-File Checksum-Only)|
|`23001`|SecureZIP AES-128|
|`23002`|SecureZIP AES-192|
|`23003`|SecureZIP AES-256|


`Hashcat` supports a variety of compressed file formats such as:

|**Mode**|**Target**|
|---|---|
|`10400`|PDF 1.1 - 1.3 (Acrobat 2 - 4)|
|`10410`|PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #1|
|`10420`|PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #2|
|`10500`|PDF 1.4 - 1.6 (Acrobat 5 - 8)|
|`10600`|PDF 1.7 Level 3 (Acrobat 9)|
|`10700`|PDF 1.7 Level 8 (Acrobat 10 - 11)|

