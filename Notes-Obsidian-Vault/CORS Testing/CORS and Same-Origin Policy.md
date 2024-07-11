**CORS**: Cross-Origin Resource Sharing
- allows web apps to request resources from different domains securely. 
- Prevents malicious scripts on one page from obtaining access to sensitive data.

**SOP**: Same-Origin Policy
- Security measure restricting web pages from interacting with resources from different origins.

## Same-Origin Policy

Same-origin policy or SOP is a policy that instructs how web browsers interact between web pages. According to this policy, a script on one web page can access data on another only if both pages share the same origin. This "origin" is identified by combining the URI scheme, hostname, and port number. The image below shows what a URL looks like with all its features (it does not use all features in every request).

![What URL looks like](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/d721041f6137c9ea50cfa9b661cc1baa.png)

This policy is designed to prevent a malicious script on one page from accessing sensitive data on another web page through the browser.


### Examples of SOP

1. **Same domain, different port**: A script from `https://test.com:80` can access data from `https://test.com:80/about`, as both share the same protocol, domain, and port. However, it cannot access data from `https://test.com:8080` due to a different port.
2. **HTTP/HTTPS interaction**: A script running on `http://test.com` (non-secure HTTP) is not allowed to access resources on `https://test.com` (secure HTTPS), even though they share the same domain because the protocols are different.

### Common Misconceptions

1. **Scope of SOP**: It's commonly misunderstood that SOP only applies to scripts. In reality, it applies to all web page aspects, including embedded images, stylesheets, and frames, restricting how these resources interact based on their origins.
2. **SOP Restricts All Cross-Origin Interactions**: Another misconception is that SOP completely prevents all cross-origin interactions. While SOP does restrict specific interactions, modern web applications often leverage various techniques (like CORS, postMessage, etc.) to enable safe and controlled cross-origin communications.
3. **Same Domain Implies Same Origin**: People often think that if two URLs share the same domain, they are of the same origin. However, SOP also considers protocol and port, so two URLs with the same domain but different protocols or ports are considered different origins.

### SOP Decision Process

![SOP decision process](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/f750c8230ca04378b11f320d4b720640.png)  

The above flowchart illustrates the sequence of checks a browser performs under SOP: it first checks if the protocols match, then the hostnames, and finally the port numbers. If all three match, the resource is allowed; otherwise, it is blocked. This diagram simplifies the concept, making it easier to understand and remember.


**What policy instructs web browsers how they should interact between web pages?**
> `Same-Origin Policy`


## Cross-Origin Resource Sharing

Cross-Origin Resource Sharing (**CORS**) is a mechanism defined by HTTP headers that allows servers to specify how resources can be requested from different origins. While the Same-Origin Policy (**SOP**) restricts web pages by default to making requests to the same domain, **CORS** enables servers to declare exceptions to this policy, allowing web pages to request resources from other domains under controlled conditions.

**CORS** operates through a set of HTTP headers that the server sends as part of its response to a browser. These headers inform the browser about the server's **CORS** policy, such as which origins are allowed to access the resources, which HTTP methods are permitted, and whether credentials can be included with the requests. It's important to note that the server does not block or allow a request based on **CORS**; instead, it processes the request and includes **CORS** headers in the response. The browser then interprets these headers and enforces the **CORS** policy by granting or denying the web page's JavaScript access to the response based on the specified rules.

### Different HTTP Headers Involved in CORS
1. **Access-Control-Allow-Origin**: This header specifies which domains are allowed to access the resources. For example, `Access-Control-Allow-Origin: example.com` allows only requests from `example.com`.
2. **Access-Control-Allow-Methods**: Specifies the HTTP methods (GET, POST, etc.) that can be used during the request.
3. **Access-Control-Allow-Headers**: Indicates which HTTP headers can be used during the actual request.
4. **Access-Control-Max-Age**: Defines how long the results of a preflight request can be cached.
5. **Access-Control-Allow-Credentials**: This header instructs the browser whether to expose the response to the frontend JavaScript code when credentials like cookies, HTTP authentication, or client-side SSL certificates are sent with the request. If Access-Control-Allow-Credentials is set to true, it allows the browser to access the response from the server when credentials are included in the request. It's important to note that when this header is used, Access-Control-Allow-Origin cannot be set to * and must specify an explicit domain to maintain security.


### Common Scenarios Where CORS is Applied
CORS is commonly applied in scenarios such as:

1. **APIs and Web Services**: When a web application from one domain needs to access an API hosted on a different domain, CORS enables this interaction. For instance, a frontend application at `example-client.com` might need to fetch data from `example-api.com`.
2. **Content Delivery Networks (CDNs)**: Many websites use CDNs to load libraries like jQuery or fonts. CORS enables these resources to be securely shared across different domains.
3. **Web Fonts**: For web fonts to be used across different domains, CORS headers must be set, allowing websites to load fonts from a centralized location.
4. **Third-Party Plugins/Widgets:** Enabling features like social media buttons or chatbots from external sources on a website.
5. **Multi-Domain User Authentication:** Services that offer single sign-on (SSO) or use tokens (like OAuth) to authenticate users across multiple domains rely on CORS to exchange authentication data securely.


### Simple Requests vs. Preflight Requests

There are two primary types of requests in CORS: simple requests and preflight requests.

1. **Simple Requests**: These requests meet certain criteria set by CORS that make them "simple". They are treated similarly to same-origin requests, with some restrictions. A request is considered simple if it uses the GET, HEAD, or POST method, and the POST request's `Content-Type` header is one of `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`. Additionally, the request should not include custom headers that aren't CORS-safe listed. Simple requests are sent directly to the server with the `Origin` header, and the response is subject to CORS policy enforcement based on the `Access-Control-Allow-Origin` header. Importantly, cookies and HTTP authentication data are included in simple requests if the site has previously set such credentials, even without the `Access-Control-Allow-Credentials` header being true.
    
2. **Preflight Requests**: These are CORS requests that the browser "preflights" with an OPTIONS request before sending the actual request to ensure that the server is willing to accept the request based on its CORS policy. Preflight is triggered when the request does not qualify as a "simple request", such as when using HTTP methods other than GET, HEAD, or POST, or when POST requests are made with another `Content-Type` other than the allowed values for simple requests, or when custom headers are included. The preflight OPTIONS request includes headers like `Access-Control-Request-Method` and `Access-Control-Request-Headers`, indicating the method and custom headers of the actual request. The server must respond with appropriate CORS headers, such as `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`, and `Access-Control-Allow-Origin` to indicate that the actual request is permitted. If the preflight succeeds, the browser will send the actual request with credentials included if `Access-Control-Allow-Credentials` is set to true.


### Process of a CORS Request

![Process of CORS request](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/9eadd502f90ab196f359029a63494fbe.svg)  

The above flowchart shows the basic process of a CORS request.

1. The browser first sends an HTTP request to the server with  the Origin Header.

3. The server then checks the Origin header against its list of allowed origins.
4. If the origin is allowed, the server responds with the appropriate `Access-Control-Allow-Origin` header.
5. The browser will block the cross-origin request if the origin is not allowed.

**What HTTP header specifies which domains are allowed to access the resources hosted in its server?**
> `Access-Control-Allow-Origin`



CORS was created as a result of **Same-Origin Policy (SOP)** because it doesn't scale well and modern sites/API's need the ability to access resources from external domains for completely legitimate use cases.

There are several HTTP headers related to CORS; however, we are interested in the two related to the commonly seen vulnerabilities — `Access-Control-Allow-Origin` and `Access-Control-Allow-Credentials` Ex:
```
#All domain are allowed  
Access-Control-Allow-Origin: *     
  
  
#comma-separated list of domains  
Access-Control-Allow-Origin: example.com, metrics.com
```

- **Access-Control-Allow-Origin:** This header specifies the allowed domains to read the response contents. The value can be either a wildcard character `*`, which indicates all domains are allowed, or a comma-separated list of domains.
- **Access-Control-Allow-Credentials**: This header determines whether the domain allows for passing credentials — such as cookies or authorization headers in the cross-origin requests.

The value of the header is either True or False. If the header is set to “true,” the domain allows sending credentials, and if it is set to “false,” or not included in the response, then it is not allowed.
  
```
#allow passing credenitals in the requests  
Access-Control-Allow-Credentials: true  
  
#Disallow passing in the requests  
Access-Control-Allow-Credentials: false
```

#### Impact
Data theft, XSS, RCE in some cases.

#### Identification
When testing an application for CORS, check if any of the application's responses contain the CORS headers using BurpSuite.

![](https://miro.medium.com/v2/resize:fit:1271/1*73ksv0ZrBWRf8dQZ7TliOg.png)

![](https://miro.medium.com/v2/resize:fit:1193/1*FVD7mLNMgvsdWa5XVV9MSA.png)

Figures 1 & 2 show the search functionality in Burp Suite to look for CORS headers.

To identify CORS issues, we modify the Origin header in the requests with multiple values and see what response headers we get back from the application. There are four (4) known ways to do so:

## **1. Reflected Origins**

Set the Origin header in the request to an arbitrary domain, such as `[**https://attackersdomain.com**](https://attackersdomain.com.)`, and check the `**Access-Control-Allow-Origin**` header in the response**.** If it reflects the exact domain you supplied in the request, it means the domain doesn’t filter for any origins.
###### Additional Useful Resources
- [CORS Testing Guide](https://medium.com/r3d-buck3t/cross-origin-resource-sharing-cors-testing-guide-29616c225a0a)
- [PortSwigger - CORS](https://portswigger.net/web-security/cors)
- 