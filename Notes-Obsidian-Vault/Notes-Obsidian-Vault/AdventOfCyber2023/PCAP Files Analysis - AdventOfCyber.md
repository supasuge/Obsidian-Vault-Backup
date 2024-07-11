## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Key Data Files of PCAP format](#Key\Data\Files\of\PCAP\format)
    - [Key data fiels of Network Flow format](#Key\data\fiels\of\Network\Flow\format)
    - [How to Collect and Process Network Data](#How\to\Collect\and\Process\Network\Data)

## Table of Contents

    - [Key Data Files of PCAP format](#Key\Data\Files\of\PCAP\format)
    - [Key data fiels of Network Flow format](#Key\data\fiels\of\Network\Flow\format)
    - [How to Collect and Process Network Data](#How\to\Collect\and\Process\Network\Data)


### Key Data Files of PCAP format
- Link layer information
- Timestamp
- Packet length
- MAC addresses source and destination
- IP and port information, Source, Destination, IP addresses etc.
- TCP/UDP info
- Application layer protocol

### Key data fiels of Network Flow format
- IP and port information
- IP Protocol
- Volume details in byte and packet metrics
- TCP flags
- Time details
	- Start time, duration
- Application layer protocol

### How to Collect and Process Network Data
Involves using network monitoring and analysis tools (I.e., Wireshark, tshark, tcpdump) to collect information about the traffic on a network and then analyse that data to gain insight, troubleshoot, or conduct blue and purple team operations. System-based solutions will  help collect network data in flow format. The specific tools and methods you use will depend on the size and complexity of your network and objectives.


You can collect network flows from endpoints and network devices in the same way as you can collect full packet captures. It's also possible to convert PCAPs to network flows if you need a quick look at network statistics on a pre-recorded file. Many open-source tools can help you read network flows or convert PCAPs to network flow data, and SiLK is one of the most popular choices.


SiLK mainly works on a data repo, but it can also process data sources not in the base data repository. By default, the data repository resides under the `/var/silk/data`

Flow File Properties with SilK Suite: rwfileinfo

One of the top five actions in packet and flow analysis is overviewing the file info. SiLK suite has a tool `rwfileinfo` that makes this possible. Now, let's start working with the artefacts provided. We'll need to view the details of binary flow files using the command below:  

- `rwfileinfo FILENAME`

File info

           `user@tryhackme:~/Desktop$ rwfileinfo suspicious-flows.silk`
 






