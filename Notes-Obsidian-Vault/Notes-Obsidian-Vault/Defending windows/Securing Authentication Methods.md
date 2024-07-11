## Table of Contents

    - [LAN Manager Hash](#LAN\Manager\Hash)
      - [SMB Signing](#SMB\Signing)
      - [LDAP Signing](#LDAP\Signing)



Topics:
Secure Communications
Data Integrity


`GroupPolicyMangementEditor` is a tool for configuring various security policies.

### LAN Manager Hash
The user account password for Windows ins't stored in clear text, its sstored with two types of hashes.
**If the password for any user account is changed or set with fewer than 15 characters, both LM hash (LAN Manager hash) and NT Hash are used and stored in AD**


#### SMB Signing
	Server Message Block
- Utilized for file and print communication. 
- Allows secure transmission over the network. 
- SMB Signing is crucial to prevent MITM attacks
```

Group Policy Management Editor > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options > double click Microsoft network server: Digitally sign communication (always) > select Enable Digitally Sign Communications
```

#### LDAP Signing
LDAP Enables locating and authenticating resources on the network. Exploited via replay attacks, authentication, MITM, or custom LDAP requests. LDAP signing is a simple Authentication and Security Layer (SASL) propery that only accepts signed LDAP requests and ignores other requests.
```powershell

Group Policy Management Editor > Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options > Domain controller: LDAP server signing requirements > select Require signing from the dropdown
```
