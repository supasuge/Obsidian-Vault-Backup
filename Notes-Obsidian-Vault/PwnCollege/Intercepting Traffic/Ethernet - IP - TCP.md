# Ethernet Packet Structure
6 bytes - Destination MAC Address
6 Bytes - Source MAC Address
2 Bytes - Type

Broadcast: Send packet to all available hosts


# Internet Protocol
## Packet Structure
4 bits(1 octet) - Version
4 bits(1 octet) - Internet Header Length
1 byte - Differentiated Services Field
2 bytes - Total length
2 bytes - identification
3 bits - Flags
15 bits - Fragment Offset
1 byte - Time To Live
1 byte - Protocol
2 bytes - Header Checksum
4 bytes - Source IP Address
4 bytes - destination IP address
?? bytes - Options

# Transmission Control Protocol
2 bytes - Source port
2 bytes - destination port
4 bytes - sequence number
4 bits - data offset
3 bits - reserved
9 bits - Flags
2 bytes - Window size
2 bytes - Checksum
2 bytes - Urgent pointer
?? bytes - options

### Flags
URG: Urgent pointer field is significant

ACK: Acknowledgement field is significant., All packets after the initial SYN packet sent by the client should have this flag set

PSH: Push function. Asks to push the buffered data to the receiving application

RST: Reset the connection

SYN: Synchronize sequence numbers. Only the first packet sent from each should have this flag set

FIN: Last packet from sender


# Address Resolution Protocol Packet
2 bytes - hardware type
2 bytes - protocol type
1 byte - hardware address length
1 byte - protocol address length
2 bytes - operation
6 bytes - sender hardware address
4 bytes - sender protocol address
6 bytes - target hardware address
4 bytes - target protocol address


###### Level 3
Find and connect to a remote host:
```bash
nmap -p 31337 10.0.0.0/24
nc 10.0.0.2 31337
```

###### Level 4
```bash
root@ip-10-0-0-2:~# nmap -p 31337 --min-rate 10000 --open 10.0.0.0/16

PORT      STATE SERVICE
31337/tcp open  Elite
MAC Address: E6:55:E8:D1:E6:89 (Unknown)

root@ip-10-0-0-2:~# nc 10.0.48.15 31337
pwn.college{MDHO-yYDeMHyVAQQU5tfmS3f0q2.dJjNzMDLzETNyMzW}
```


