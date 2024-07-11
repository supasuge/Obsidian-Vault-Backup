## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Objectives](#Objectives)
  - [What is SSRF?](#What\is\SSRF?)
  - [How Does SSRF Work?](#How\Does\SSRF\Work?)
    - [Types of SSRF](#Types\of\SSRF)
      - [Prerequisites](#Prerequisites)

## Table of Contents

          - [Objectives](#Objectives)
  - [What is SSRF?](#What\is\SSRF?)
  - [How Does SSRF Work?](#How\Does\SSRF\Work?)
    - [Types of SSRF](#Types\of\SSRF)
      - [Prerequisites](#Prerequisites)

###### Objectives
- Understanding SSRF
- Understanding the different types of SSRF
- Prerequisites for the vulnerability
- Mitigation measures



## What is SSRF?
**Server-Side Request Forgery, is a security vulnerability that occurs when an attacker tricks a web application into making unauthorised requests to internal or external resources on the server's behalf**. This can allow an attacker to interact with internal systems. Potentially leading to *data exposure* or *unauthorised actions*. Leaving web applications vulnerable to SSRF can have profound security implications, potentially leading to unuathorised access to systems, RCE, data breaches, or further compromise.


## How Does SSRF Work?
- Identifying Vulnerable Input: The attacker locates an input field within the application that can be manipulated to trigger server-side requests (URL parameters, web forms, an API endpoint, or request)
- Manipulating the input: The attacker inputs a malicious URL or other payloads that cause the appliation to make unintended requests. This input could be a URL pointing to an internal server, a loopback address, or an external server under the attacker's control
- Requessting Unauthorised Resources: The application server, unaware of the malicious input, makes a request to the specified URL or resource. This request could target internal rsources, sensitive services, or external systems.
### Types of SSRF
- Basic: Attacker sends a crafted request from the vulnerable server to internal or external resources. Ex: may attempt to access files on the local file system, internal services, or databases that are not intended to be publicly accessible
- Blind SSRF: In a blind SSRF attack, the attacker doesn't directly see the response to the request. Instead, they may infer information about the internal network by measuring the time it takes for the server to respond or observing error message changes
- Semi-Blind SSRF: Attacker does not receive direct responses in their browser or application. However they rely on indirect clues, side-channel information, or observable effects within the application to determine the success or failure of their SSRF requests.

#### Prerequisites



