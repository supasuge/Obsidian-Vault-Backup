## Table of Contents

- [Syntax](#syntax)
- [Syntax gobuster dir -u [target ip] -w [wordlist] # Example gobuster dir -u http://192.168.0.1:8080 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt # Example which excludes pages with a certain length gobuster dir -u http://192.168.0.1:8080 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt --exclude-length 9265](#syntax\gobuster\dir\-u\[target\ip]\-w\[wordlist]\#\example\gobuster\dir\-u\http://192.168.0.1:8080\-w\/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt\#\example\which\excludes\pages\with\a\certain\length\gobuster\dir\-u\http://192.168.0.1:8080\-w\/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt\--exclude-length\9265)
- [Syntax gobuster dns -d [target site] -w [wordlist] # Standard example gobuster dns -d example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt # Example with show ip gobuster dns -d example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -i # Example of force processing of a domain that has wildcard entries gobuster dns -d 0.0.1.example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt --wildcard](#syntax\gobuster\dns\-d\[target\site]\-w\[wordlist]\#\standard\example\gobuster\dns\-d\example.com\-w\/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt\#\example\with\show\ip\gobuster\dns\-d\example.com\-w\/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt\-i\#\example\of\force\processing\of\a\domain\that\has\wildcard\entries\gobuster\dns\-d\0.0.1.example.com\-w\/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt\--wildcard)
- [Syntax](#syntax)

Nmap


22, 80

Nothin in particular sticks out of the blue... 


Gobuster... /download --> source code!

# Syntax
```bash

gobuster [mode] -u [target ip] -w [wordlist]  

Gobuster can run in multiple scanning modes, at the time of writing these are: dir, dns and vhost.

DIR mode - Used for directory/file bruteforcing

# Syntax gobuster dir -u [target ip] -w [wordlist] # Example gobuster dir -u http://192.168.0.1:8080 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt # Example which excludes pages with a certain length gobuster dir -u http://192.168.0.1:8080 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt --exclude-length 9265  

DNS mode - Used for DNS subdomain bruteforcing

# Syntax gobuster dns -d [target site] -w [wordlist] # Standard example gobuster dns -d example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt # Example with show ip gobuster dns -d example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -i # Example of force processing of a domain that has wildcard entries gobuster dns -d 0.0.1.example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt --wildcard 

VHOST mode - Used for VHOST bruteforcing

# Syntax


gobuster vhost -u [target site] -w [vhost list] # Example gobuster vhost -u https://example.com -w common-vhosts.txt
```
```
```

