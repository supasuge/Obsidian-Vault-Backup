
## Enumeration
- `nmap`
Ports open: `22`(SSH), `5000`(upnp)
```bash
nmap -p- --min-rate 10000 -Pn 10.10.11.8 -oN headless.ports
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-03-25 01:53 EDT
Nmap scan report for 10.10.11.8
Host is up (0.081s latency).
Not shown: 62185 closed tcp ports (conn-refused), 3348 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
5000/tcp open  upnp

Nmap done: 1 IP address (1 host up) scanned in 13.86 seconds
```

#### UPNP
I have never heard of this, so I had to look it up... https://www.ionos.ca/digitalguide/server/know-how/what-is-upnp/

UPnP: Pluyg and plan everywhere. It is a protocol developed by microsoft. It includes file access capabilities across devices. It includes multicast addresses and protocols such as IP, UDP, HTTP, XML, TCP, and SOAP.



Moving on with a more aggressive comprehensive scan:
