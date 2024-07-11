A CSRF token is a unique, unpredictable value associated with a user's session, ensuring each request comes from a legitamate source. One effective implementation is the Double submit cookies tehcnique, where a cookie value corresponds to a value in a hidden from field. When the server receives a request, it checks that the cookie value mathces the form field value, providing additional layer of verification.

How it works

- **Token Generation**: When a user logs in or initiates a session, the server generates a unique CSRF token.![how double submit cookie attack works](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/63340a0ec9d725fcb640fbc3eedcbcf3.svg) This token is sent to the user's browser both as a cookie (CSRF-Token cookie) and embedded in hidden form fields of web forms where actions are performed (like money transfers).
- **User Action**: Suppose the user wants to transfer money. They fill out the transfer form on the website, which includes the hidden CSRF token.
- **Form Submission**: Upon submitting the form, two versions of the CSRF token are sent to the server: one in the cookie and the other as part of the form data.
- **Server Validation**: The server then checks if the CSRF token in the cookie matches the one sent in the form data. If they match, the request is considered legitimate and processed; if not, the request is rejected.

### Possible Vulnerable Scenarios

Despite its effectiveness, it's crucial to acknowledge that hackers are persistent and have identified various methods to bypass Double Submit Cookies:

- **Session Cookie Hijacking (Man in the Middle Attack)**: If the CSRF token isn't appropriately isolated and safeguarded from the session, an attacker may also be able to access it by other means. 
- **Subverting the Same-Origin Policy (Attacker Controlled Subdomain)**: An attacker can set up a situation where the browser's same-origin policy is broken. Browser vulnerabilities or deceiving the user into sending a request through an attacker-controlled subdomain with permission to set cookies for its parent domain could be used.
- **Exploiting XSS Vulnerabilities**: An attacker may be able to obtain the CSRF token from the cookie or the page itself if the web application is susceptible to Cross-Site Scripting (XSS). By creating fraudulent requests with the double-submitted cookie CSRF token, the attacker can get around the defence once they have the CSRF token.
- **Predicting or Interfering with Token Generation**: An attacker may be able to guess or modify the CSRF token if the tokens are not generated securely and are predictable or if they can tamper with the token generation process.
- **Subdomain Cookie Injection**: Injecting cookies into a user's browser from a related subdomain is another potentially sophisticated technique that might be used. This could fool the server's CSRF protection system by appearing authentic to the main domain.


### How it Works

Now, we will see how the attack works, connecting it to the previous example. This technique will chain two vulnerable scenarios by reversing token generation and injecting a cookie through an attacker-controlled subdomain. 

- The attacker has successfully transferred the amount from Josh's bank account but seems more greedy and wants to take complete control of the bank account.
- To exploit the account, the attacker must access the bank account. But how? This requires the password of the Josh account.
- Exploiting a vulnerability like CSRF requires a lot of code and functionality and understanding the website logic from the developer's point of view. The attacker logged into his bank account and noticed that the form for updating passwords is protected through a CSRF- token.


