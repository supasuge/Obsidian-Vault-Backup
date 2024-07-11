## Table of Contents

    - [Nmap Scan](#Nmap\Scan)
        - [Tools:](#Tools:)
    - [Basic Usage](#Basic\Usage)
    - [Commonly Used Options and Commands](#Commonly\Used\Options\and\Commands)
    - [Example Commands](#Example\Commands)



### Nmap Scan
`nmap -p- --min-rate 10000 -Pn IP_ADDR -oN PC.scan1`
*Ports*:
`22 - SSH Server
`50051 - gRPC server`
```bash
blarchey>$ sudo nmap -sCV -sF 10.10.11.214 -p80,50051
\\Starting Nmap 7.94 ( https://nmap.org ) at 2023-11-16 18:34 EST
Stats: 0:00:00 elapsed; 0 hosts completed (0 up), 0 undergoing Script Pre-Scan
NSE Timing: About 0.00% done
Nmap scan report for 10.10.11.214
Host is up (0.093s latency).

PORT      STATE         SERVICE    VERSION
80/tcp    open|filtered tcpwrapped
50051/tcp open          unknown
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port50051-TCP:V=7.94%I=7%D=11/16%Time=6556A71D%P=x86_64-pc-linux-gnu%r(
SF:NULL,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0\x
SF:06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(Generi
SF:cLines,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0
SF:\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(GetR
SF:equest,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0
SF:\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(HTTP
SF:Options,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\
SF:0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(RTS
SF:PRequest,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff
SF:\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(RP
SF:CCheck,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?\xff\xff\0
SF:\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\0")%r(DNSV
SF:ersionBindReqTCP,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\0\?
SF:\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?\0\
SF:0")%r(DNSStatusRequestTCP,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\
SF:0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0
SF:\0\0\?\0\0")%r(Help,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x05\
SF:0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\0\?
SF:\0\0")%r(SSLSessionReq,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xff\xff\0\x
SF:05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0\0\0\0\0\
SF:0\?\0\0")%r(TerminalServerCookie,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\x
SF:ff\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\
SF:0\0\0\0\0\0\?\0\0")%r(TLSSessionReq,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\
SF:?\xff\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x
SF:08\0\0\0\0\0\0\?\0\0")%r(Kerberos,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\
SF:xff\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08
SF:\0\0\0\0\0\0\?\0\0")%r(SMBProgNeg,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\
SF:xff\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08
SF:\0\0\0\0\0\0\?\0\0")%r(X11Probe,2E,"\0\0\x18\x04\0\0\0\0\0\0\x04\0\?\xf
SF:f\xff\0\x05\0\?\xff\xff\0\x06\0\0\x20\0\xfe\x03\0\0\0\x01\0\0\x04\x08\0
SF:\0\0\0\0\0\?\0\0");
```

##### Tools:
[fullstorydev/grpcurl: Like cURL, but for gRPC: Command-line tool for interacting with gRPC servers (github.com)](https://github.com/fullstorydev/grpcurl)

	This tool lets you interact with gRPC servers using cURL like commands and such.

### Basic Usage
- **Syntax**: The basic syntax of `grpcurl` is:
    `grpcurl [options] [address] [list|describe|invoke] [service|method]`
    
- **Options**: These modify how `grpcurl` operates. Common options include `-plaintext`, `-proto`, `-import-path`, etc.
- **Address**: This is the address of the gRPC server you want to interact with (e.g., `localhost:50051`).
- **Commands**: `list`, `describe`, and `invoke` are the primary commands used to interact with a gRPC server.
### Commonly Used Options and Commands

1. **-plaintext**: Use this when connecting to a server that does not use TLS.
2. **-proto**: Specifies a proto file. This is useful when the server does not support server reflection.
3. **-import-path**: Adds a folder to the search path for proto files.
4. **list**: Lists services or methods. Without arguments, it lists all services. With a service name, it lists all methods in that service.
5. **describe**: Shows details about a service or method, such as request and response types.
6. **invoke**: Calls a method. You'll typically provide JSON data for the request.
7. **-d**: Used with `invoke` to pass the request data.
8. **-H**: Adds a header to the request, useful for metadata like authentication tokens.
9. **-v**: Enables verbose output, providing more details about the request and response.
    
### Example Commands

- **List Services**:
    
    `grpcurl -plaintext localhost:50051 list`
    
- **Describe a Service**:
    
    `grpcurl -plaintext localhost:50051 describe my.service.ServiceName`
    
- **Invoke a Method**:
    `grpcurl -plaintext -d '{"myField": "value"}' localhost:50051 my.service.ServiceName/`



