## Table of Contents

- [Overview](#overview)
    - [Enumeration](#Enumeration)
          - [Ports Open](#Ports\Open)
          - [Notes](#Notes)
          - [TO BE CONTINUED](#TO\BE\CONTINUED)

#CVE-2023-46604 #deserialization #vulnerability #apacheactivemq #apache #RCE
#severevuln #unauthenticated

[Apache PoC HackerNew Article](https://thehackernews.com/2023/11/new-poc-exploit-for-apache-activemq.html)
# Overview
Broker
IP = `10.10.11.243`
1. Enumeration
2. 
### Enumeration
Starting out with a simple `nmap` scan as usual.
```bash
└─$ nmap -p- --open -Pn --min-rate 10000 10.10.11.243                  
```
###### Ports Open
```bash
22/tcp    open  ssh
80/tcp    open  http
1883/tcp  open  mqtt
5672/tcp  open  amqp
8161/tcp  open  patrol-snmp
33363/tcp open  unknown
61613/tcp open  unknown
61614/tcp open  unknown
61616/tcp open  unknown
```

**Moving on...**
```bash
nmap -p22,80,1883,5672,8161,33363,61613,61614,61616 -sCV 10.10.11.243 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-12-19 04:44 UTC
Nmap scan report for 10.10.11.243
Host is up (0.075s latency).

PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp    open  http       nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  basic realm=ActiveMQRealm
|_http-title: Error 401 Unauthorized
1883/tcp  open  mqtt
|_mqtt-subscribe: Failed to receive control packet from server.
5672/tcp  open  amqp?
|_amqp-info: ERROR: AQMP:handshake expected header (1) frame, but was 65
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GetRequest, HTTPOptions, RPCCheck, RTSPRequest, SSLSessionReq, TerminalServerCookie: 
|     AMQP
|     AMQP
|     amqp:decode-error
|_    7Connection from client using unsupported AMQP attempted
8161/tcp  open  http       Jetty 9.4.39.v20210325
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  basic realm=ActiveMQRealm
|_http-server-header: Jetty(9.4.39.v20210325)
|_http-title: Error 401 Unauthorized
33363/tcp open  tcpwrapped
61613/tcp open  stomp      Apache ActiveMQ
| fingerprint-strings: 
|   HELP4STOMP: 
|     ERROR
|     content-type:text/plain
|     message:Unknown STOMP action: HELP
|     org.apache.activemq.transport.stomp.ProtocolException: Unknown STOMP action: HELP
|     org.apache.activemq.transport.stomp.ProtocolConverter.onStompCommand(ProtocolConverter.java:258)
|     org.apache.activemq.transport.stomp.StompTransportFilter.onCommand(StompTransportFilter.java:85)
|     org.apache.activemq.transport.TransportSupport.doConsume(TransportSupport.java:83)
|     org.apache.activemq.transport.tcp.TcpTransport.doRun(TcpTransport.java:233)
|     org.apache.activemq.transport.tcp.TcpTransport.run(TcpTransport.java:215)
|_    java.lang.Thread.run(Thread.java:750)
61614/tcp open  http       Jetty 9.4.39.v20210325
|_http-title: Site doesn't have a title.
|_http-server-header: Jetty(9.4.39.v20210325)
| http-methods: 
|_  Potentially risky methods: TRACE
61616/tcp open  apachemq   ActiveMQ OpenWire transport
| fingerprint-strings: 
|   NULL: 
|     ActiveMQ
|     TcpNoDelayEnabled
|     SizePrefixDisabled
|     CacheSize
|     ProviderName 
|     ActiveMQ
|     StackTraceEnabled
|     PlatformDetails 
|     Java
|     CacheEnabled
|     TightEncodingEnabled
|     MaxFrameSize
|     MaxInactivityDuration
|     MaxInactivityDurationInitalDelay
|     ProviderVersion 
|_    *****************{{5.15.15}}****************************************
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port5672-TCP:V=7.94SVN%I=7%D=12/19%Time=65811FAA%P=x86_64-pc-linux-gnu%
SF:r(GetRequest,89,"AMQP\x03\x01\0\0AMQP\0\x01\0\0\0\0\0\x19\x02\0\0\0\0S\
SF:x10\xc0\x0c\x04\xa1\0@p\0\x02\0\0`\x7f\xff\0\0\0`\x02\0\0\0\0S\x18\xc0S
SF:\x01\0S\x1d\xc0M\x02\xa3\x11amqp:decode-error\xa17Connection\x20from\x2
SF:0client\x20using\x20unsupported\x20AMQP\x20attempted")%r(HTTPOptions,89
SF:,"AMQP\x03\x01\0\0AMQP\0\x01\0\0\0\0\0\x19\x02\0\0\0\0S\x10\xc0\x0c\x04
SF:\xa1\0@p\0\x02\0\0`\x7f\xff\0\0\0`\x02\0\0\0\0S\x18\xc0S\x01\0S\x1d\xc0
SF:M\x02\xa3\x11amqp:decode-error\xa17Connection\x20from\x20client\x20usin
SF:g\x20unsupported\x20AMQP\x20attempted")%r(RTSPRequest,89,"AMQP\x03\x01\
SF:0\0AMQP\0\x01\0\0\0\0\0\x19\x02\0\0\0\0S\x10\xc0\x0c\x04\xa1\0@p\0\x02\
SF:0\0`\x7f\xff\0\0\0`\x02\0\0\0\0S\x18\xc0S\x01\0S\x1d\xc0M\x02\xa3\x11am
SF:qp:decode-error\xa17Connection\x20from\x20client\x20using\x20unsupporte
SF:d\x20AMQP\x20attempted")%r(RPCCheck,89,"AMQP\x03\x01\0\0AMQP\0\x01\0\0\
SF:0\0\0\x19\x02\0\0\0\0S\x10\xc0\x0c\x04\xa1\0@p\0\x02\0\0`\x7f\xff\0\0\0
SF:`\x02\0\0\0\0S\x18\xc0S\x01\0S\x1d\xc0M\x02\xa3\x11amqp:decode-error\xa
SF:17Connection\x20from\x20client\x20using\x20unsupported\x20AMQP\x20attem
SF:pted")%r(DNSVersionBindReqTCP,89,"AMQP\x03\x01\0\0AMQP\0\x01\0\0\0\0\0\
SF:x19\x02\0\0\0\0S\x10\xc0\x0c\x04\xa1\0@p\0\x02\0\0`\x7f\xff\0\0\0`\x02\
SF:0\0\0\0S\x18\xc0S\x01\0S\x1d\xc0M\x02\xa3\x11amqp:decode-error\xa17Conn
SF:ection\x20from\x20client\x20using\x20unsupported\x20AMQP\x20attempted")
SF:%r(DNSStatusRequestTCP,89,"AMQP\x03\x01\0\0AMQP\0\x01\0\0\0\0\0\x19\x02
SF:\0\0\0\0S\x10\xc0\x0c\x04\xa1\0@p\0\x02\0\0`\x7f\xff\0\0\0`\x02\0\0\0\0
SF:S\x18\xc0S\x01\0S\x1d\xc0M\x02\xa3\x11amqp:decode-error\xa17Connection\
SF:x20from\x20client\x20using\x20unsupported\x20AMQP\x20attempted")%r(SSLS
SF:essionReq,89,"AMQP\x03\x01\0\0AMQP\0\x01\0\0\0\0\0\x19\x02\0\0\0\0S\x10
SF:\xc0\x0c\x04\xa1\0@p\0\x02\0\0`\x7f\xff\0\0\0`\x02\0\0\0\0S\x18\xc0S\x0
SF:1\0S\x1d\xc0M\x02\xa3\x11amqp:decode-error\xa17Connection\x20from\x20cl
SF:ient\x20using\x20unsupported\x20AMQP\x20attempted")%r(TerminalServerCoo
SF:kie,89,"AMQP\x03\x01\0\0AMQP\0\x01\0\0\0\0\0\x19\x02\0\0\0\0S\x10\xc0\x
SF:0c\x04\xa1\0@p\0\x02\0\0`\x7f\xff\0\0\0`\x02\0\0\0\0S\x18\xc0S\x01\0S\x
SF:1d\xc0M\x02\xa3\x11amqp:decode-error\xa17Connection\x20from\x20client\x
SF:20using\x20unsupported\x20AMQP\x20attempted");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port61613-TCP:V=7.94SVN%I=7%D=12/19%Time=65811FA5%P=x86_64-pc-linux-gnu
SF:%r(HELP4STOMP,27F,"ERROR\ncontent-type:text/plain\nmessage:Unknown\x20S
SF:TOMP\x20action:\x20HELP\n\norg\.apache\.activemq\.transport\.stomp\.Pro
SF:tocolException:\x20Unknown\x20STOMP\x20action:\x20HELP\n\tat\x20org\.ap
SF:ache\.activemq\.transport\.stomp\.ProtocolConverter\.onStompCommand\(Pr
SF:otocolConverter\.java:258\)\n\tat\x20org\.apache\.activemq\.transport\.
SF:stomp\.StompTransportFilter\.onCommand\(StompTransportFilter\.java:85\)
SF:\n\tat\x20org\.apache\.activemq\.transport\.TransportSupport\.doConsume
SF:\(TransportSupport\.java:83\)\n\tat\x20org\.apache\.activemq\.transport
SF:\.tcp\.TcpTransport\.doRun\(TcpTransport\.java:233\)\n\tat\x20org\.apac
SF:he\.activemq\.transport\.tcp\.TcpTransport\.run\(TcpTransport\.java:215
SF:\)\n\tat\x20java\.lang\.Thread\.run\(Thread\.java:750\)\n\0\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port61616-TCP:V=7.94SVN%I=7%D=12/19%Time=65811FA5%P=x86_64-pc-linux-gnu
SF:%r(NULL,140,"\0\0\x01<\x01ActiveMQ\0\0\0\x0c\x01\0\0\x01\*\0\0\0\x0c\0\
SF:x11TcpNoDelayEnabled\x01\x01\0\x12SizePrefixDisabled\x01\0\0\tCacheSize
SF:\x05\0\0\x04\0\0\x0cProviderName\t\0\x08ActiveMQ\0\x11StackTraceEnabled
SF:\x01\x01\0\x0fPlatformDetails\t\0\x04Java\0\x0cCacheEnabled\x01\x01\0\x
SF:14TightEncodingEnabled\x01\x01\0\x0cMaxFrameSize\x06\0\0\0\0\x06@\0\0\0
SF:\x15MaxInactivityDuration\x06\0\0\0\0\0\0u0\0\x20MaxInactivityDurationI
SF:nitalDelay\x06\0\0\0\0\0\0'\x10\0\x0fProviderVersion\t\0\x075\.15\.15");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```



- What is Apacher ActiveMQ? - Open-source message broker written in Java together with a full Java Message Service client(JMS). It provides "Enterprise Features" which in this context means fostering communication between the applications by sending messages between one another. This is commonly known as *(MOM)* **Message-Oriented Middleware**
	- Supports Various communication protocols such as AMQP, MQTT, STOMP, and OpenWire. These protocols allow for different communication patterns, including queue-based (point-to-point) and topic-based (publish/subscribe) messaging


###### Notes
- HTTPBasicAuth
	- admin:admin
- Ubuntu Linux
- SSH: 8.9
- Port `61616` Apache ActiveMQ 5.15.15
	- Vulnerability Requirements:
		- Network Access
		- A manipulated OpenWire Command (used to instantiate a arbitrary class on the classpath with a `String` parameter)/
		- Class on the classpath which can execute arbitrary code simply by instantiatiing it with a `String` parameter


###### TO BE CONTINUED







