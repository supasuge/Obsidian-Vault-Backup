## Table of Contents

      - [Specific Scripts Category](#Specific\Scripts\Category)
      - [Nmap - Specifying Scripts](#Nmap\-\Specifying\Scripts)
- [Performance](#performance)
      - [Optimized RTT](#Optimized\RTT)
  - [Max Retries](#Max\Retries)
      - [Default Scan](#Default\Scan)
  - [Rates](#Rates)
      - [Default Scan](#Default\Scan)
      - [Optimized Scan](#Optimized\Scan)
- [IPS/IDS evasion](#ips/ids\evasion)
  - [Decoys](#Decoys)
      - [Scan by Using Decoys](#Scan\by\Using\Decoys)
      - [Testing Firewall Rule](#Testing\Firewall\Rule)
      - [Scan by Using Different Source IP](#Scan\by\Using\Different\Source\IP)
  - [DNS Proxying](#DNS\Proxying)
      - [SYN-Scan of a Filtered Port](#SYN-Scan\of\a\Filtered\Port)
      - [SYN-Scan From DNS Port](#SYN-Scan\From\DNS\Port)
      - [Connect To The Filtered Port](#Connect\To\The\Filtered\Port)
    - [DNS Nameserver ID](#DNS\Nameserver\ID)

NSE has 14 total(lua) categories which scripts are divided in:


|**Category**|**Description**|
|---|---|
|`auth`|Determination of authentication credentials.|
|`broadcast`|Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans.|
|`brute`|Executes scripts that try to log in to the respective service by brute-forcing with credentials.|
|`default`|Default scripts executed by using the `-sC` option.|
|`discovery`|Evaluation of accessible services.|
|`dos`|These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services.|
|`exploit`|This category of scripts tries to exploit known vulnerabilities for the scanned port.|
|`external`|Scripts that use external services for further processing.|
|`fuzzer`|This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time.|
|`intrusive`|Intrusive scripts that could negatively affect the target system.|
|`malware`|Checks if some malware infects the target system.|
|`safe`|Defensive scripts that do not perform intrusive and destructive access.|
|`version`|Extension for service detection.|
|`vuln`|Identification of specific vulnerabilities.|
#### Specific Scripts Category
`sudo nmap <target> --script <category>`

Defined scripts: 
```shell-session
sudo nmap <target> --script <script-name>,<script-name>,...
```
#### Nmap - Specifying Scripts

  Nmap - Specifying Scripts

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
```


# Performance
performance plays a large role when we need to scan an extensive network or are dealing with bad netwokr connection(bandwidth). use various options to tell nmap how fast (T<1-5>) with which frequency (--min-parallelism $n$ ), which timeouts (--max-rtt-timeout $n$) and with the number of retries (--max-retries $n$) for the scanned ports the targets should be scanned.

When nmap send a packet it takes some time (Round-Trip-Time-RTT) to receive a response from the scanned port. generally nmap starts with a high timeout (--min-RTT-timeout) of 100ms. Ex:(scanning whole network with 256 hosts, including the top 100 ports):
`sudo nmap IP/24 -F`

#### Optimized RTT

  Optimized RTT

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

## Max Retries

Another way to increase the scans' speed is to specify the retry rate of the sent packets (`--max-retries`). The default value for the retry rate is `10`, so if `Nmap` does not receive a response for a port, it will not send any more packets to the port and will be skipped.

#### Default Scan

  Default Scan

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l

23
```
## Rates

During a white-box penetration test, we may get whitelisted for the security systems to check the systems in the network for vulnerabilities and not only test the protection measures. If we know the network bandwidth, we can work with the rate of packets sent, which significantly speeds up our scans with `Nmap`. When setting the minimum rate (`--min-rate <number>`) for sending packets, we tell `Nmap` to simultaneously send the specified number of packets. It will attempt to maintain the rate accordingly.

#### Default Scan

  Default Scan

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.default
```

#### Optimized Scan

  Optimized Scan

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300
```

# IPS/IDS evasion


Firewalls and there rules: (Errors):
- Net Unreachable
- Net Prohibited
- Host Unreachable
- Host Prohibited
- Port Unreachable
- Proto unreachable


-sA (TCP ACK) scan is much harder to filter for firewalls and IDS/IPS system than regular SYN (-sS) or connect scans (sT) because they only send a TCP packet with only the ACK flag.

## Decoys

There are cases in which administrators block specific subnets from different regions in principle. This prevents any access to the target network. Another example is when IPS should block us. For this reason, the Decoy scanning method (`-D`) is the right choice. With this method, Nmap generates various random IP addresses inserted into the IP header to disguise the origin of the packet sent. With this method, we can generate random (`RND`) a specific number (for example: `5`) of IP addresses separated by a colon (`:`). Our real IP address is then randomly placed between the generated IP addresses. In the next example, our real IP address is therefore placed in the second position. Another critical point is that the decoys must be alive. Otherwise, the service on the target may be unreachable due to SYN-flooding security mechanisms.

#### Scan by Using Decoys

  Scan by Using Decoys

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5

```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 80`|Scans only the specified ports.|
|`-sS`|Performs SYN scan on specified ports.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`-D RND:5`|Generates five random IP addresses that indicates the source IP the connection comes from.|

The spoofed packets are often filtered out by ISPs and routers, even though they come from the same network range. Therefore, we can also specify our VPS servers' IP addresses and use them in combination with "`IP ID`" manipulation in the IP headers to scan the target.

Another scenario would be that only individual subnets would not have access to the server's specific services. So we can also manually specify the source IP address (`-S`) to test if we get better results with this one. Decoys can be used for SYN, ACK, ICMP scans, and OS detection scans. So let us look at such an example and determine which operating system it is most likely to be.

#### Testing Firewall Rule

  Testing Firewall Rule

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.28 -n -Pn -p445 -O

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-22 01:23 CEST
Nmap scan report for 10.129.2.28
Host is up (0.032s latency).

PORT    STATE    SERVICE
445/tcp filtered microsoft-ds
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)
Too many fingerprints match this host to give specific OS details
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.14 seconds
```

#### Scan by Using Different Source IP

  Scan by Using Different Source IP

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-22 01:16 CEST
Nmap scan report for 10.129.2.28
Host is up (0.010s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Linux 2.6.32 - 2.6.35 (94%), Linux 2.6.32 - 3.5 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.11 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-n`|Disables DNS resolution.|
|`-Pn`|Disables ICMP Echo requests.|
|`-p 445`|Scans only the specified ports.|
|`-O`|Performs operation system detection scan.|
|`-S`|Scans the target by using different source IP address.|
|`10.129.2.200`|Specifies the source IP address.|
|`-e tun0`|Sends all requests through the specified interface.|



## DNS Proxying

By default, `Nmap` performs a reverse DNS resolution unless otherwise specified to find more important information about our target. These DNS queries are also passed in most cases because the given web server is supposed to be found and visited. The DNS queries are made over the `UDP port 53`. The `TCP port 53` was previously only used for the so-called "`Zone transfers`" between the DNS servers or data transfer larger than 512 bytes. More and more, this is changing due to IPv6 and DNSSEC expansions. These changes cause many DNS requests to be made via TCP port 53.

However, `Nmap` still gives us a way to specify DNS servers ourselves (`--dns-server <ns>,<ns>`). This method could be fundamental to us if we are in a demilitarized zone (`DMZ`). The company's DNS servers are usually more trusted than those from the Internet. So, for example, we could use them to interact with the hosts of the internal network. As another example, we can use `TCP port 53` as a source port (`--source-port`) for our scans. If the administrator uses the firewall to control this port and does not filter IDS/IPS properly, our TCP packets will be trusted and passed through.

#### SYN-Scan of a Filtered Port

  SYN-Scan of a Filtered Port

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace

```


#### SYN-Scan From DNS Port

  SYN-Scan From DNS Port

```shell-session
gdxqpardo@htb[/htb]$ sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53

SENT (0.0482s) TCP 10.10.14.2:53 > 10.129.2.28:50000 S ttl=58 id=27470 iplen=44  seq=4003923435 win=1024 <mss 1460>
RCVD (0.0608s) TCP 10.129.2.28:50000 > 10.10.14.2:53 SA ttl=64 id=0 iplen=44  seq=540635485 win=64240 <mss 1460>
Nmap scan report for 10.129.2.28
Host is up (0.013s latency).

PORT      STATE SERVICE
50000/tcp open  ibm-db2
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 50000`|Scans only the specified ports.|
|`-sS`|Performs SYN scan on specified ports.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`--source-port 53`|Performs the scans from specified source port.|

Now that we have found out that the firewall accepts `TCP port 53`, it is very likely that IDS/IPS filters might also be configured much weaker than others. We can test this by trying to connect to this port by using `Netcat`.

#### Connect To The Filtered Port

  Connect To The Filtered Port

```shell-session
gdxqpardo@htb[/htb]$ ncat -nv --source-port 53 10.129.2.28 50000

Ncat: Version 7.80 ( https://nmap.org/ncat )
Ncat: Connected to 10.129.2.28:50000.
220 ProFTPd
```hhg


### DNS Nameserver ID
nmap -sSU -g 80 --script dns-nsid MACHINE_IP 







