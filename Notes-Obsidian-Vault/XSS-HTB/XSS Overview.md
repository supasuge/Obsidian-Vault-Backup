## Table of Contents

- [What is XSS?](#what\is\xss?)
  - [Three main types of XSS vulnerabilities:](#Three\main\types\of\XSS\vulnerabilities:)

# What is XSS?
A typical web application works by receiving the HTML code from the back-end server and rendering it on the client-side internet browser. When a vulnerable web application doesn't properly sanitize user input, a malicious user can inject extra javascript code in an input field. Once another user views the same page, they knowingly execute the malicious code.


## Three main types of XSS vulnerabilities:

|Type|Description|
|---|---|
|`Stored (Persistent) XSS`|The most critical type of XSS, which occurs when user input is stored on the back-end database and then displayed upon retrieval (e.g., posts or comments)|
|`Reflected (Non-Persistent) XSS`|Occurs when user input is displayed on the page after being processed by the backend server, but without being stored (e.g., search result or error message)|
|`DOM-based XSS`|Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is completely processed on the client-side, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)|

