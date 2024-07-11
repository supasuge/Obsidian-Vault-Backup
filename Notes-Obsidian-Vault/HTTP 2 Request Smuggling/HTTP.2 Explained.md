## Table of Contents

    - [Request Smuggling and HTTP/2](#Request\Smuggling\and\HTTP/2)
- [HTTP/2 Downgrading](#http/2\downgrading)
  - [Expected Behaviour](#Expected\Behaviour)
  - [H2.CL](#H2.CL)
  - [H2.TE](#H2.TE)
  - [CRLF Injection](#CRLF\Injection)
- [HTTP/2 Request Tunneling](#http/2\request\tunneling)
    - [HTTP/2 Request Tunneling: Leaking Internal Headers](#HTTP/2\Request\Tunneling:\Leaking\Internal\Headers)
      - [Leaking Internal Headers](#Leaking\Internal\Headers)

The second version of the HTTP protocol proposes several changes over the original specifications. The new protocol intends to overcome the problems inherent to HTTP/1.1 by changing the message format and how the client and server communicate. One of the significant differences is that HTTP/2 requests and responses use a completely binary protocol, unlike HTTP/1.1, which is humanly readable. This is a massive improvement over the older version since it allows any binary information to be sent in a way that is easier for machines to parse without making mistakes.

While the HTTP/2 binary format is difficult to read for humans, we will use a simplified representation of requests throughout the room. Here's a visual representation of HTTP/2 requests compared with an HTTP/1.1 request: 
![[b62f90be37525447e4a9118b906f1291.svg]]

The HTTP/2 request has the following components:

- **Pseudo-headers:** HTTP/2 defines some headers that start with a colon `:`. Those headers are the minimum required for a valid HTTP/2 request. In our image above, we can see the `:method`, `:path`, `:scheme` and `:authority` pseudo-headers.
- **Headers:** After the pseudo-headers, we have regular headers like `user-agent` and `content-length`. Note that HTTP/2 uses lowercase for header names.
- **Request Body:** Like in HTTP/1.1, this contains any additional information sent with the request, like POST parameters, uploaded files and other data.

### Request Smuggling and HTTP/2

One of the main reasons HTTP request smuggling is possible in HTTP/1 scenarios is the existence of several ways to define the size of a request body. This ambiguity in the protocol leads to different proxies having their own interpretation of where a request ends and the next one begins, ultimately ending in request smuggling scenarios.

The second version of the HTTP protocol was built to improve on many of the characteristics of the first version. The one we most notably care about in the context of HTTP request smuggling is the clear definition of sizes for each component of an HTTP request. To avoid the ambiguities in HTTP/1, HTTP/2 prefixes each request component with a field that contains its size. For example, each header is prefixed with its size, so parsers know precisely how much information to expect. To understand this better, let's take a look at a captured request in Wireshark, looking specifically at the request headers:

![HTTP 2 Binary Format](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/25f541a57c89cb198f879c7fefee15ec.png)


In the image, we are looking at the `:method` pseudo-header. As we can see, both the header name and value are prefixed with their corresponding lengths. The header name has a length of 7, corresponding to `:method` and the header value has a length of 3, corresponding to the string `GET`


The request's body also includes a length indicator, rendering headers like `Content-Length` and `Transfer-Encoding: chunked` meaningless in pure HTTP/2 environments.

Even though `Content-Length` headers aren't directly used by HTTP/2, modern browsers will still include them for a specific scenario where HTTP downgrades may occur. This is very important for our specific scenario and we will discuss it in more detail in the following sections.

With such clear boundaries for each part of a request, one would expect request smuggling to be impossible, and to a certain extent, it is in implementations that rely solely on HTTP/2. However, as with any protocol version, not all devices can be upgraded to it directly. This results in implementations of load balancers or reverse proxies that support HTTP/2, serving content from server farms that still use HTTP/1.

Which version of the HTTP protocol uses \r\n to separate headers in a request?
> `HTTP/1.1`

Which version of the HTTP protocol uses a binary format and clearly defines boundaries for elements in requests/responses?
> `HTTP/2`

# HTTP/2 Downgrading

When a reverse proxy serves content to the end user with HTTP/2 (frontend connection) but requests it from the backend servers by using HTTP/1.1 (backend connection), we talk about HTTP/2 downgrading. This type of implementation is still common nowadays, making it possible to reintroduce HTTP request smuggling in the context of HTTP/2, but only where downgrades to HTTP/1.1 occur.

![HTTP 2 Downgrading](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/9d4081b19fc689e4c7d5dc74908cf2fa.svg)  

Instead of dealing directly with HTTP/2, we send HTTP/2 requests in the frontend connection to influence the corresponding HTTP/1.1 request generated in the backend connection so that it causes an HTTP desync condition.

Ideally, the proxy should safely convert a single HTTP/2 request to a single HTTP/1.1 equivalent. This is only sometimes true in practice. Each proxy implementation may handle the conversion slightly differently, making introducing a malicious HTTP/1.1 request in the backend connection possible, leading to any of the typical cases of HTTP desync.

## Expected Behaviour
Before getting into request smuggling, let's understand how a request would be translated from HTTP/2 to HTTP/1.1. Take the following POST request as an example:

![How HTTP 2 Downgrading Works](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/5558498a8ce3e4e55944215fc0da7236.svg)  

The process is straightforward. The headers and the body from the HTTP/2 request are directly passed into the HTTP/1.1 request. Notice that the HTTP/2 request includes a `content-length` header. Remember that HTTP/2 doesn't use such a header, but HTTP/1.1 requires one to delimit the request body correctly, so any decent browser will include content-length in HTTP/2 requests to preemptively deal with HTTP downgrades. In the case of the proxies we will be using, the `Host` header is added after all the other headers based on the content of the `:authority` pseudo-header. Other proxy implementations may have the host header appear before the rest of custom headers.


## H2.CL
As mentioned before, the Content-Length header has no meaning for HTTP/2, since the length of the request body is specified unambigously. But nothing stops us from adding a `Content-Length` header to an HTTP/2 request. If HTTP downgrades occur, the proxy will pass the added `content-length` header from HTTP/2 to the HTTP/1.1 connection, enabling desync. To better understand this, consider what would happeng with the following HTTP/2 request:

![H2.CL Case](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/8f01b7f9b691452f47a11b6363d5711b.svg)  

The proxy receives the HTTP/2 request on the frontend connection. **When translating the request to HTTP/1.1, it simply passes the Content-Length header to the backend connection**. When the backend web server reads the request, it acknowledges the injected Content-Length as valid. Since the injected Content-Length in our example is 0, the backend is tricked into believing this is a POST request without a body. Whatever comes after the headers (the original body of the HTTP/2 request) will be interpreted as the start of a new request. Since the word `HELLO` is not a complete HTTP/1.1 request, the backend server will wait until more data arrives to complete it.

The backend connection is now desynced. If another user sends a request, it will be concatenated to the `HELLO` value lingering in the backend connection. If for example, another user makes a request right after, this is what would happen:

![H2.CL Victim](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/60cc684893cd19e782fb0178285e93fd.svg)

Note how the request line of the following request gets merged with the lingering HELLO. This effectively alters the request of the victim user, which can be abused by the attacker in many ways we'll cover later.


## H2.TE
We can also add a "Transfer-Encoding: chunked" header to the frontend HTTP/2 request, and the proxy might also pass it to the backend HTTP/1.1 connection untouched. If the backend web server prioritizes this header to determine the request body size, we can desync the backend connection once again. Here's how our HTTP/2 request would look:
![[62d1c36e7c84315008fdfb09519c218f.svg]]
The effect would be the same as with the H2.CL case. The first request is now a chunked request. The first chunk is of size 0, so the backend believes that's where it ends. The rest of the HTTP/2 request body will poison the backend connection, affecting the next upcoming request.

## CRLF Injection
**CRLF** is the shorthand notation for a newline. **CR** stands for **Carriage Return**, equivalent to the character with ASCII code point `0xD`, also represented as the `\r` character. **LF** stands for **Line Feed**, the ASCII character with `0xA`, often represented as `\n`. CRLF is simply the sequence of both those characted `\r\n`, one after the other, and is used in HTTP/1.1 as a delimited between headers, and also to separate the headers from the body (by using a double `\r\n`).

Since HTTP/2 packets can handle binary information, inserting character in any request field is possible. This poses a problem when translating requests to HTTP/1.1, as some characters like `\r\n` represent delimiters between headers. If we can inject `\r\n` in an HTTP/2 header, it might get translated by the proxy into HTTP/1.1 directly, which will be interpreted as a header separator, thus allowing us to smuggle requests.

To understand this, look at what would happened if we sent the following HTTP/2 request:

![CRLF Injection](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/b07be99d2c0be4b83525a9b52f2e6e33.svg)  

The resulting HTTP/1.1 request now has an additional header. Note that we aren't limited to injecting headers, but we can also smuggle entire requests in this way:

CRLF injection is not restricted to HTTP/2 headers only. Any place where you send a `\r\n` that potentially ends up in the HTTP/1.1 request could potentially achieve the same results. Note that each proxy will try to sanitize the requests differently, so your mileage may vary depending on your target.


# HTTP/2 Request Tunneling
Request Tunneling vs Desync

So far, the attack vectors we have looked at depend on the backend server to reuse a single HTTP connection to serve all users. In certain proxy implementations, each user will get its own backend connection to separate their request from others. Whenever this happens, an attacker won't be able to influence the requests of other users. At first sight, it would appear that we can't do much if confined to our own connection, but we can still smuggle requests through the frontend proxy and achieve some results. Since we can only smuggle requests to our connection, this scenario is often called request tunnelling.

![Per-user backend connections](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/a2ae4b71b8a024459c2cb4bc37dfcb1d.svg)  

In the following three tasks, we will use an old version of HAProxy, vulnerable to [CVE-2019-19330](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-19330) as our frontend proxy. This version allows request smuggling by using the CRLF injection technique. The vulnerable backend application will be accessible through the proxy at [https://10.10.172.45:8100](https://10.10.172.45:8100/).

### HTTP/2 Request Tunneling: Leaking Internal Headers
#### Leaking Internal Headers
The simplest way to abuse request tunnelling is to get some information on how the backend requests look. In some scenarios, the frontend proxy may add headers to the requests before sending them to the backend. If we want to smuggle a specific request to the backend, we may need to add such headers for the request to go through.

To leak such headers, we can abuse any functionality in the backend application that reflects a parameter from the request into the response. In our case, the application reflects whatever data is sent to `/hello` through the `q` POST parameter. Here's how the request would look like:

![Search Engine Request](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/8b293f1a35622264fdd6a7166f40fa52.svg)  

Notice the existence of a `content-length` header despite being ignored by HTTP/2. Most browsers will add this header to all HTTP/2 requests so that the backend will still receive a valid `Content-Length` header if an HTTP downgrade occurs. In the backend, the request would be converted into HTTP/1.1. This particular proxy will insert the `Host:` header after the headers sent by the client (right after content-length). If needed, the proxy could also add any additional headers (represented as X-Internal in the image). The final backend request would look like this:

![Search Engine HTTP 1.1 Request](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/929da265f2a2e96245964dbd3893be4e.svg)  

We will take advantage of the vulnerability in HAProxy that allows us to inject CRLFs via headers to leak the backend headers successfully. We will add a custom Foo header and send our attack payload through it. This is how our request would look:

![Abusing CRLF Injection to Leak Internal Headers](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/fea04890f95f87e73a433538916e34a5.svg)  

There's quite a bit to unpack here:

- This will be a normal request for the frontend since HTTP/2 doesn't care about binary information in its headers.
- The `Content-Length: 0` header injected through the Foo header will make the backend think the first POST request has no body. Whatever comes after the headers will be interpreted as a second request.
- Since the `Host` header and any other internal headers are inserted by the proxy after `Foo`, the first POST request will have no `Host` header unless we provide one. This is why we injected a `Host` header for the first request. This is required, as the HTTP/1.1 specification requires a `Host` header for each request.
- The second POST request will trigger a search on the website. Notice how the internal headers are now part of the `q` parameter in the body of the request. This will cause the website to reflect the headers back to us.
- The second POST request we have injected has a `Content-Length: 300`. This number is just an initial guess of how much space we will require for the Internal headers. You will need to play a bit with it until you get the right answer. If it's set too high, the connection will hang as the backend waits for that many bytes to be transferred. If you set it too low, you may only get a part of the internal headers.

Now let's try sending this Burp. First, capture the request that is sent by the website when performing a search. You should be able to identify a POST request being sent to `/hello`. Right-click the request and send it to Repeater:

![Sending request to repeater](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/fa150fe0acf8e8eac8649f226989afa8.png)

**Note:** Be sure to send an HTTP/2 request to repeater. Under certain circumstances, your browser may send an HTTP/1.1 request the first time you request a resource. In that case, simply refresh the website, and it should send an HTTP/2 request the second time.

Once our request is in the Repeater tab, we'll do two modifications to it:

1. Delete the body content.
2. Set the `Content-Length` header to 0. We do this for the same reason as before. We want the first request to be a POST with no body. Remember we will need to disable the `Update Content-Length` setting on Repeater to avoid Burp overwriting our custom value.

![Deleting request body](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/4fa11b2e366568fa088aec13a4c47f87.png)  

If Burp is giving you a hard time with the above steps, here's a simplified version of the same modified request that you can copy directly into Repeater instead to continue:

```shell
POST /hello HTTP/2
Host: 10.10.172.45:8100
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 0

```

Let's add our custom `Foo` header with an initial content of bar. Notice that Burp allows us to edit the HTTP/2 request as if it were an HTTP/1 request. This is somewhat convenient as long as you don't need to insert binary characters in the request. Since we will be adding CRLFs to the request, editing the request as text won't be possible. Instead, we will use the Inspector pane at the right, since it allows for a much more precise editing of the request:

![Sending request to repeater](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/fa150fe0acf8e8eac8649f226989afa8.png)

**Note:** Be sure to send an HTTP/2 request to repeater. Under certain circumstances, your browser may send an HTTP/1.1 request the first time you request a resource. In that case, simply refresh the website, and it should send an HTTP/2 request the second time.

Once our request is in the Repeater tab, we'll do two modifications to it:

1. Delete the body content.
2. Set the `Content-Length` header to 0. We do this for the same reason as before. We want the first request to be a POST with no body. Remember we will need to disable the `Update Content-Length` setting on Repeater to avoid Burp overwriting our custom value.

![Deleting request body](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/4fa11b2e366568fa088aec13a4c47f87.png)  

If Burp is giving you a hard time with the above steps, here's a simplified version of the same modified request that you can copy directly into Repeater instead to continue:

```shell
POST /hello HTTP/2
Host: 10.10.172.45:8100
User-Agent: Mozilla/5.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 0

```

Let's add our custom `Foo` header with an initial content of bar. Notice that Burp allows us to edit the HTTP/2 request as if it were an HTTP/1 request. This is somewhat convenient as long as you don't need to insert binary characters in the request. Since we will be adding CRLFs to the request, editing the request as text won't be possible. Instead, we will use the Inspector pane at the right, since it allows for a much more precise editing of the request: