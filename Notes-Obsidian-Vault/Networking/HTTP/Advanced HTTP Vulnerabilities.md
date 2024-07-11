## Table of Contents

- [Advanced HTTP Vulnerabilities Cheatsheet](#advanced\http\vulnerabilities\cheatsheet)
  - [Expanded Request Methods](#Expanded\Request\Methods)
  - [Detailed Status Codes](#Detailed\Status\Codes)
  - [Enhanced Headers](#Enhanced\Headers)
  - [Advanced Secure Communication](#Advanced\Secure\Communication)
  - [Extended Authentication & Authorization](#Extended\Authentication\&\Authorization)
  - [Additional Common Vulnerabilities](#Additional\Common\Vulnerabilities)
  - [Less Common, but Critical Vulnerabilities](#Less\Common,\but\Critical\Vulnerabilities)

# Advanced HTTP Vulnerabilities Cheatsheet

## Expanded Request Methods
- **PATCH**: Used to apply partial modifications to a resource.
- **CONNECT**: Establishes a tunnel to the server identified by the target resource.
- **TRACE**: Echoes back the received request, used mainly for diagnostic purposes.

## Detailed Status Codes
- **2xx Success**
  - **201 Created**: Indicates that the request has been fulfilled and has resulted in one or more new resources being created.
  - **204 No Content**: The server successfully processed the request, but is not returning any content.
- **3xx Redirection**
  - **301 Moved Permanently**: This and all future requests should be directed to the given URI.
  - **307 Temporary Redirect**: The request should be repeated with another URI, but future requests should still use the original URI.
- **4xx Client Error**
  - **401 Unauthorized**: Authentication is required and has failed or has not yet been provided.
  - **403 Forbidden**: The server understood the request but refuses to authorize it.
- **5xx Server Error**
  - **500 Internal Server Error**: A generic error message when an unexpected condition was encountered.
  - **503 Service Unavailable**: The server is currently unable to handle the request due to a temporary overload or scheduled maintenance.

## Enhanced Headers
- **Entity Headers**
  - **Expires**: Gives the date/time after which the response is considered stale.
  - **Last-Modified**: The date and time at which the origin server believes the resource was last modified.
- **Security Headers**
  - **Content-Security-Policy (CSP)**: Helps to prevent XSS attacks by specifying valid sources of executable scripts.
  - **X-Content-Type-Options**: Prevents MIME-sniffing of the response away from the declared content-type.

## Advanced Secure Communication
- **TLS Pinning**: Ensures the client communicates only with a specified host using a known TLS certificate.
- **Certificate Transparency**: Framework to monitor and audit SSL certificates, ensuring their legitimacy.

## Extended Authentication & Authorization
- **Digest Authentication**: Like basic authentication but uses a challenge-response mechanism for enhanced security.
- **OAuth 2.0**: Authorization framework allowing third-party access to server resources without exposing user credentials.
- **OpenID Connect**: Layer on top of OAuth 2.0 for authentication, providing identity verification.

## Additional Common Vulnerabilities
- **Man-in-the-Middle (MitM) Attack**: Interception of communication between two parties to secretly alter or relay information.
- **Directory Traversal**: Attacker accesses files and directories that are stored outside the web root folder.
- **Remote Code Execution (RCE)**: Allows an attacker to execute arbitrary code on server-side applications.
- **Server-Side Request Forgery (SSRF)**: The server is tricked into making requests to internal services within the server's network or to external third-party systems.
- **Insecure Deserialization**: Where untrusted data is used to abuse the logic of an application, inflict a denial of service (DoS), or execute arbitrary code.
- **API Security**: Inadequately protected APIs may lead to unauthorized access, data breaches, and other attacks.

## Less Common, but Critical Vulnerabilities
- **HTTP Request Smuggling**: Manipulating the interpretation of HTTP requests between two servers, leading to various attacks like web cache poisoning.
- **HTTP Response Splitting**: Where the attacker convinces the server to return a response with arbitrary HTTP headers.
- **Web Parameter Tampering**: Manipulation of parameters exchanged between client and server to modify application data, such as user credentials and permissions.
- **Clickjacking**: Tricks a user into clicking on something different from what the user perceives, potentially revealing confidential information.
