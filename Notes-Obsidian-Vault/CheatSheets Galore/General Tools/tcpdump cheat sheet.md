
Output the content's of a `pcap` packet capture file in a human readable format:
```bash
tcpdump -qns 0 -A -r <file.pcap> 
```
### Tcpdump Cheatsheet

#### Basic Command
- **Capture packets on a network interface:**
```sh
tcpdump [options] [expression]
```

#### General Options
- **-A**: Print each packet (minus its link-level header) in ASCII.
- **-B**: Set the capture buffer size.
```sh
tcpdump -B buffer_size
```
- **-c**: Exit after capturing a specified number of packets.
```sh
tcpdump -c count
```
- **-C**: Rotate the dump file when it reaches a certain size.
```sh
tcpdump -C file_size
```
- **-d**: Dump the compiled packet-matching code.
- **-dd**: Dump packet-matching code as a C program fragment.
- **-ddd**: Dump packet-matching code as decimal numbers.
- **-D**: List available network interfaces.
```sh
tcpdump -D
```
- **-e**: Print the link-level header on each dump line.
- **-E**: Decrypt IPsec ESP packets.
```sh
tcpdump -E spi@ipaddr algo:secret
```
- **-f**: Print foreign IPv4 addresses numerically.
- **-F**: Read the filter expression from a file.
```sh
tcpdump -F file
```
- **-G**: Rotate the dump file every specified seconds.
```sh
tcpdump -G rotate_seconds -w file
```
- **-i**: Listen on a specific network interface.
```sh
tcpdump -i interface
```
- **-I**: Put the interface in "monitor mode" (for Wi-Fi).
- **-K**: Don't attempt to verify checksums.
- **-l**: Make stdout line buffered.
- **-L**: List known data link types for the interface.
- **-m**: Load SMI MIB module definitions.
- **-M**: Use a shared secret for validating TCP-MD5 digests.
- **-n**: Don't convert host addresses to names.
- **-nn**: Don't convert protocol and port numbers to names.
- **-N**: Don't print domain name qualification.
- **-O**: Do not run the packet-matching code optimizer.
- **-p**: Don't put the interface into promiscuous mode.
- **-q**: Quick output (less protocol information).
- **-R**: Assume ESP/AH packets are based on old specification.
- **-r**: Read packets from a file.
```sh
tcpdump -r file
```
- **-S**: Print absolute TCP sequence numbers.
- **-s**: Set the snapshot length (number of bytes captured).
```sh
tcpdump -s snaplen
```
- **-T**: Force interpretation of packets as a specific type.
```sh
tcpdump -T type
```
- **-t**: Don't print a timestamp on each dump line.
- **-tt**: Print an unformatted timestamp.
- **-ttt**: Print a delta between current and previous line.
- **-tttt**: Print timestamp with date.
- **-ttttt**: Print delta between current and first line.
- **-u**: Print undecoded NFS handles.
- **-U**: Make output packet-buffered.
- **-v**: Verbose output.
- **-vv**: More verbose output.
- **-vvv**: Even more verbose output.
- **-w**: Write raw packets to a file.
```sh
tcpdump -w file
```
- **-W**: Limit the number of created files and overwrite old ones.
```sh
tcpdump -C file_size -W filecount -w file
```
- **-x**: Print packet data in hex.
- **-xx**: Print packet data with link-level header in hex.
- **-X**: Print packet data in hex and ASCII.
- **-XX**: Print packet data with link-level header in hex and ASCII.
- **-y**: Set the data link type.
```sh
tcpdump -y datalinktype
```
- **-z**: Execute a post-rotate command on a savefile.
```sh
tcpdump -C file_size -W filecount -w file -z postrotate-command
```
- **-Z**: Drop privileges to user.
```sh
tcpdump -Z user
```

#### Expressions
- **host**: Capture packets to/from a specific host.
```sh
tcpdump host hostname
```
- **net**: Capture packets to/from a specific network.
```sh
tcpdump net network
```
- **port**: Capture packets to/from a specific port.
```sh
tcpdump port portnumber
```
- **portrange**: Capture packets within a specific port range.
```sh
tcpdump portrange port1-port2
```
- **src**: Capture packets from a specific source.
```sh
tcpdump src host
```
- **dst**: Capture packets to a specific destination.
```sh
tcpdump dst host
```
- **proto**: Capture specific protocols (e.g., tcp, udp, icmp).
```sh
tcpdump proto protocol
```
- **gateway**: Capture packets that use a specific gateway.
```sh
tcpdump gateway host
```
- **broadcast**: Capture broadcast packets.
```sh
tcpdump broadcast
```
- **multicast**: Capture multicast packets.
```sh
tcpdump multicast
```
- **less**: Capture packets smaller than a specified size.
```sh
tcpdump less size
```
- **greater**: Capture packets larger than a specified size.
```sh
tcpdump greater size
```
- **expr**: Capture packets that match a boolean expression.
```sh
tcpdump 'expr'
```

#### Examples
- **Capture all packets:**
```sh
tcpdump
```
- **Capture packets to/from a specific host:**
```sh
tcpdump host example.com
```
- **Capture packets to/from a specific network:**
  ```sh
tcpdump net 192.168.1.0/24
  ```
- **Capture packets to/from a specific port:**
  ```sh
tcpdump port 80
  ```
- **Capture TCP packets with the SYN flag set:**
```sh
tcpdump 'tcp[tcpflags] & tcp-syn != 0'
```
- **Capture packets and write them to a file:**
  ```sh
tcpdump -w capture.pcap
  ```
- **Read packets from a file:**
```sh
tcpdump -r capture.pcap
```
- **Capture packets with a specific filter expression:**
```sh
tcpdump 'src host 192.168.1.1 and tcp port 80'
```

#### Output Format
- **Link Level Headers (-e option):**
```sh
tcpdump -e
  ```
- **ARP/RARP Packets:**
  ```sh
arp who-has host tell host
arp reply host is-at MAC
  ```
- **TCP Packets:**
  ```sh
src > dst: flags data-seqno ack window urgent options
  ```

###### Additional Resources
- https://serverfault.com/questions/38626/how-can-i-read-pcap-files-in-a-friendly-format
- https://jarryshaw.github.io/PyPCAPKit/
- - https://www.wireshark.org/docs/wsug_html_chunked

