## Table of Contents

  - [Table of Contents](#Table\of\Contents)
        - [Interfaces and Configuration](#Interfaces\and\Configuration)
          - [Interface Configuration - ifconfig](#Interface\Configuration\-\ifconfig)
          - [Internet Protocol - ip](#Internet\Protocol\-\ip)
          - [Netstat - netstat](#Netstat\-\netstat)
          - [Managing Routing Tables - route](#Managing\Routing\Tables\-\route)
          - [Traceroute](#Traceroute)
          - [Configuring Firewall Rules - iptables](#Configuring\Firewall\Rules\-\iptables)
          - [Socket Statistics - ss](#Socket\Statistics\-\ss)
          - [My Traceroute](#My\Traceroute)
          - [Analyzing and Capturing Network Traffic - tcpdump](#Analyzing\and\Capturing\Network\Traffic\-\tcpdump)
          - [Monitor Network Bandwidth Usage - iftop](#Monitor\Network\Bandwidth\Usage\-\iftop)
          - [Internet Protocol Performance - iperf](#Internet\Protocol\Performance\-\iperf)

## Table of Contents

- [Interfaces and Configuration](#Interfaces\and\Configuration)
          - [Interface Configuration - ifconfig](#Interface\Configuration\-\ifconfig)
          - [Internet Protocol - ip](#Internet\Protocol\-\ip)
          - [Netstat - netstat](#Netstat\-\netstat)
          - [Managing Routing Tables - route](#Managing\Routing\Tables\-\route)
          - [Traceroute](#Traceroute)
          - [Configuring Firewall Rules - iptables](#Configuring\Firewall\Rules\-\iptables)
          - [Socket Statistics - ss](#Socket\Statistics\-\ss)
          - [My Traceroute](#My\Traceroute)
          - [Analyzing and Capturing Network Traffic - tcpdump](#Analyzing\and\Capturing\Network\Traffic\-\tcpdump)
          - [Monitor Network Bandwidth Usage - iftop](#Monitor\Network\Bandwidth\Usage\-\iftop)
          - [Internet Protocol Performance - iperf](#Internet\Protocol\Performance\-\iperf)

##### Interfaces and Configuration
Network interfaces are the physical or virtual components through which a Linux system communicates with the network. Network interfaces provide a means for devices to send and receive data packets over a network.
- Ethernet: Most common type of network interface used for wired connections. They have unique MAC addresses and are configured with IP addresses, netmasks, and gateways.
- Wireless Interfaces: User for wireless (WiFi) connections. They require SSID's (Service Set Identifiers), encryption keys, and authentication settings
- Virtual Interfaces: Virtual network interfaces, such as VLANS (Virtual Local Area Networks) and virtual tunnels (like VPN connections), allow for segmentation and secure communication over networks.


Linux systems store network configuration details in files like `/etc/network/interfaces`
or
`/etc/sysconfig/network-scripts/ifcfg-<interface>`
- These files define IP addresses, netmasks, gateway, DNS Servers, and more.
`/etc/hosts` - This contains DNS Server information (I.e., specific servers to point to etc.)

`ping` - test connectivity
	`-c` - count
	`-s` packet size
	`-i` interval
	`-t` TTL
###### Interface Configuration - ifconfig
#ifconfig #linuxcmd #linuxnetworking
`ifconfig` - displays/enables the configuration of network interfaces. Provides functionality to assign IP parameters, and modifying a network interface
`ifconfig <INTEFACE> [options]`
	`up,down,add,del,netmask`
	`-v`: Verbose
___
###### Internet Protocol - ip
#ip #linuxcmd
`ip` - preferred for configuring network interfaces on modern Linux distributions
	`ip [OPTIONS] OBJECT {Command|help}`
	`ip link`
	`ip address`
	`ip route`
	`ip rule`
	`ip monitor`
*(Example)*: Remove an IP address from a network interface
```bash
ip address del 192.168.82.100/24 dev enp2s0
```
This command removes the specified IP address (`192.168.82.100/24`) from the interface `enp2s0`
___
###### Netstat - netstat
Displays comprehensive information on network connections, routing tables, and network statistics. It is useful for monitoring and troubleshooting.
`netstat [OPTIONS]`
	`-l` - Listening Sockets
	`-p` - Program
	`-t` - TCP Connections
	`-s` - Statistics
(Example): Network traffic in real-time
```bash
netstat -c
```
___
###### Managing Routing Tables - route
#route #tables #
Lets you view and manipulate the IP routing table.
The IP routing table determines how network traffic is directed and forwarded between different networks or hosts.
`route [OPTIONS] [command] <destination>`
	`add` - add
	`del` - delete
	`-net` - networkinterface
	`-v` - Verbose
	`-n`  - Numeric Values
*(Example)*: Add a route to a network
```bash
route add -net 198.166.0.0/24 gw 10.0.0.1
```
- Configures the routing table to direct traffic destined for `198.166.0.0/24` network to the gateway `10.0.0.1`
	- This ensures that network packets intended for the specified are correctly forwarded through the appropriate gateway

###### Traceroute
This command traces the route packets from a source to a destination over a network. It provides information about the number of hops between the source and desitnations
`traceroute [options] destination`
	`-n` - Numeric addresses
	`-p` - port
	`--debug`
	`-m` - Max TTL
___
###### Configuring Firewall Rules - iptables
`iptables` is a firewall utility that allows you to configure and manage the netfiler firewall rules. it provides a flexible framework for filtering and manipulating network traffic, enabling network administrators to set up rules to control incoming and outgoing connections.
`iptables [options] [table] <command> [chain] [rulespec]`
	`--append`
	`--delete`
	`--source`
	`--destination`
	`--jump`
- **Command**: Delete a rule from the firewall.
```
iptables --delete INPUT -s 192.168.0.10 -j DROP
```
This command removes the rules that DROPs incoming traffic from the specified source IP address `192.168.0.10` in the target chain (`INPUT`)
___
###### Socket Statistics - ss
`ss` displays information about **active** network connections, sockets, and network statistics. It provides a more detailed and comprehensive view of network connections than the traditional `netstat`
`ss [options] [filer]`
	`-t` - TCP Sockets
	`-u` - UDP Sockets
	`-n` - Bandwidth values
	`-l` - Listening Sockets
	`-a` - All Sockets
- **Command**: Display established TCP connections + the state of the connections
```bash
ss -t state established
```
___
###### My Traceroute
`mtr` - combines `ping` and `traceroute` functionalities. Provides network monitoring and troubleshooting capabilities. The `mtr` command sends packets to a specific destination and displays detailed information about the network path, including latency (round-trip time) and packet loss for each hop along the route
	`mtr [OPTIONS] destination`
	`-c` - Count
	`-r` - Report mode
	`-n` - Numeric Addreses
	`-s` - Packet size
- **Command**: `mtr` with a specific packet count
```bash
mtr -c 10 google.com
```
Send packets to goole.com and dispays detailed information about each hop along with the network path. the `-c` option sets the number of pings sent. The output helps in understanding the network route
___
###### Analyzing and Capturing Network Traffic - tcpdump
Monitor traffic similar to wireshark or `tshark`.
`tcpdump [options] [expressions]`
	`-i` - Interface
	`-n` - Numeric Address
	`-c` - Count
	`-s` - Snapshot length
___
###### Monitor Network Bandwidth Usage - iftop
Allows you to monitor real-time network bandwidth usage on selected interfaces by displaying a continously updated list of connections and their corresponding bandwidth usage.
`iftop [options]`
	`-i` - Interface
	`-f` - Filter code
	`-n` - Numeric addresses
	`-p` - Promiscuous mode
```bash
iftop -i wlp1s0
```
___
###### Internet Protocol Performance - iperf
`iperf` is used for measuring network performance. It lets you test the network bandwidth between two networked devices by generating TCP/UDP traffic.
`iperf [options] [server] [client]`
	`-s`: Server Mode
	`-c`: Client Mode
	`-u`: UDP
	`-P`: Parallel Connections
	`-i`: Interval
Ex: Run an `iperf` client with UDP traffic
```bash
iperf -c 192.168.1.100 -u
```


















































































































































































































































































































































