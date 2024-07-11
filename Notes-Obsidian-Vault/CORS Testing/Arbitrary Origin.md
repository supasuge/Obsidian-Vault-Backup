#### Arbitrary Origin

Exploiting an Arbitrary Origin vulnerability is relatively easy compared to other CORS vulnerabilities since the application accepts cross-origin requests from any domain name. For example, below is the vulnerable code of [http://corssop.thm/arbitrary.php](http://corssop.thm/arbitrary.php):

```php
if (isset($_SERVER['HTTP_ORIGIN'])){
    header("Access-Control-Allow-Origin: ".$_SERVER['HTTP_ORIGIN']."");
    header('Access-Control-Allow-Credentials: true');
}
```

The code above implements a flawed CORS policy since it echoes back the `Origin` header from the client request in the `Access-Control-Allow-Origin` header without proper validation. An attacker might use an origin like `http://evilcors.thm` and the server will echo it back.

![Server echo the supplied origin](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/abf822675dac1bfbafd2bbbb02594aba.png)  

To exploit the vulnerable code above, go to [http://exploit.evilcors.thm](http://exploit.evilcors.thm). The exploit server has an existing JavaScript code that makes cross-origin requests to the target application. The sample exploit code can be found at [http://corssop.thm/exploits/data_exfil.html](http://corssop.thm/exploits/arbitrary.html). The exploit code uses `XMLHttpRequest` to send requests to the vulnerable application and process the response. The processed response will be sent to the web server with the receiver.php file.

