#### Bad Regex in Origin

Exploiting bad regular expressions (regex) in CORS origin handling is a technique that involves taking advantage of poorly implemented regex patterns used by web applications to validate origins in CORS headers. For example, below is the vulnerable code of [http://corssop.thm/badregex.php](http://corssop.thm/badregex.php):

```php
if (isset($_SERVER['HTTP_ORIGIN']) && preg_match('#corssop.thm#', $_SERVER['HTTP_ORIGIN'])) {
    header("Access-Control-Allow-Origin: ".$_SERVER['HTTP_ORIGIN']."");
    header('Access-Control-Allow-Credentials: true');
}
```

The code above implements a flawed CORS policy since it validates domains that contain the word `corssop.thm`. An attacker might use an origin like `http://corssop.thm.evilcors.thm` that technically matches the pattern.
![[6815765c9ea5422bf078964c9245f3b9.png]]

We can reuse the exploit code we used earlier to exploit the vulnerable code above. Just change the target URL to [http://corssop.thm/badregex.php](http://corssop.thm/badregex.php). The exploit code can bypass the CORS since it's hosted in [http://corssop.thm.evilcors.thm](http://corssop.thm.evilcors.thm).

![[09dd9972280f1ce4c80ec50951435e63.png]]


Once done with the updates, click the Save button again. To verify if the exploit is working, click the View exploit button. This will open a new tab containing the exploit code saved in the hosting server.

In the newly open tab, open Developer tools > Network. There should be two XHR connections. The first request is sent to the target website (badregex.php), while the second is sent to the exfiltrating server.


You can then send the exploit code to the victim by clicking the Send to victim button on the Exploit server's homepage.

Note: The victim will automatically go to the page containing the exploit code. However, in this scenario, the victim visits the domain name ([http://corssop.thm.evilcors.thm](http://corssop.thm.evilcors.thm)) instead of the previous domain name in Task 7.

To check if the victim has successfully executed the exploit code, check the exploit server's logs by clicking the Logs button in the navigation bar. The logs should have a request from IP 10.10.39.12 since this is the victim's IP address. 

![Server logs with victim interaction](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/3386f7278f365d64da5c4a704ab2696c.png)  

In your exfiltrator server, you should receive a POST request from the victim. This POST request contains the whole webpage response of the first XHR request in our exploit.


Note: The IP differs from the previous image since the victim is simulated in an internal network. So, an outbound connection will use the IP of the machine instead.  

Once the request is saved, you can check the contents of the exfiltrated data in the text file.

![Exfiltrated data from the victim](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/01d3c77786fc09dd73995d3a0a8b0dfd.png)  

So, in a nutshell, below is the entire process of the exploitation once the user clicks or visits the hosted exploit code:

![Entire process of exploitation](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/0451e7a3fc7b3cf9259fcfff90846915.png)


