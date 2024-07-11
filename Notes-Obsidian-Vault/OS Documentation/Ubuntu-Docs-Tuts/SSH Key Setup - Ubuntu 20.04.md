## Table of Contents

- [SSH](#ssh)
      - [Create Key Pair](#Create\Key\Pair)
        - [Copying the Public Key to Your Ubuntu Server](#Copying\the\Public\Key\to\Your\Ubuntu\Server)

# SSH
(Secure shell), is an encrypted protocol used to administer and communicate with server.
#### Create Key Pair
```bash
ssh-keygen
```
##### Copying the Public Key to Your Ubuntu Server
```bash
ssh-copy-id <username>@<host>
```
Notes:
- If your on a *root* account to set up keys for a user account, it's imptrant `~/.ssh` directory belongs to the user and not to root.