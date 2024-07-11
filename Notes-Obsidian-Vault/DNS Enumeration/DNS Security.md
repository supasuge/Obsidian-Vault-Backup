Many companies/organization's already have dedicated DNS server's in place and additional DNSSEC measures in place. Such as regular scans, vulnerability assessment software, and more. Companies recognize the value of the DNS as an active `line of defense`, embedded in an in-depth and comprehensive security concept.

DNS is part of every network connection. The DNS is uniquely positioned in the network to act as a cental control point to decide whether a benign or malicious request is received.

`DNS Threat Intelligence` can be integrated with other open-source and other threat intelligence feeds. Analytics systems such as `EDR` (`Endpoint Detection and Response`) and `SIEM` (`Security Information and Event Management`) can provide a holistic and situation-based picture of the security situation. DNS Security Services support the coordination of incident response by sharing `IOC`s (`Indicators of Compromise`) and `IOA`s (`Indicators of Attacks`) with other security technologies such as firewalls, network proxies, endpoint security, Network Access Control (NACs), and vulnerability scanners, providing them with information.

## DNSSEC
Another feed used for the security of DNS servers is `Domain Name System Security Extensions` (`DNSSEC`), designed to ensure the authenticity and integrity of data transmitted through the Domain Name System by securing resource records with digital certificates. `DNSSEC` ensures that the DNS data has not been manipulated and does not originate from any other source. `Private keys` are used to sign the `Resource Records` digitally. `Resource Records` can be signed several times with different private keys, for example, to replace keys that expire in time.

#### Private Key
The DNS server that manages a zone to be secured signs its sent resource recores using its only known `private key`. Each zone has its zone keys, each consisting of a `private` and a `public key`. DNSSEC specifies a new resource record type with the `RRSIG`. It contains the signature of the respective DNS record, and these used keys have a specific validity period and are provided with a `start` and `end date`.

#### Public Key

The public key can be used to verify the signature of the recipients of the data. For the `DNSSEC` security mechanisms, it must be supported by the provider of the DNS information and the requesting client system. The requesting clients verify the signatures using the generally known public key of the DNS zone. If a check is successful, manipulating the response is impossible, and the information comes from the requested source.


## Enumeration
As with any service we work with and want to find out information, it is essential to understand how DNS works to create a clear structure with the appropriate information. Since DNS can provide much information about the company's infrastructure, we can divide this information into the following categories:

- `DNS Records`
- `Subdomains`/`Hosts`
- `DNS Security`

There is a variety of techniques that can be used for this. These include:

- OSINT
- Certificate Transparency
- Zone transfer

---

## OSINT

The OSINT is an information procurement from publicly available and open sources. In the simplest case, we can also use search engines like `Bing`, `Yahoo`, and `Google` with the corresponding [Google Dorks](https://securitytrails.com/blog/google-hacking-techniques) of Google to filter out our results.

![[osint-dns2.webp]]

Also, we can use public services such as [VirusTotal](https://www.virustotal.com), [DNSdumpster](https://dnsdumpster.com/), [Netcraft](https://searchdns.netcraft.com), and others to read known entries for the corresponding domain. We can use "Right-Click on the image -> View Image" to make it bigger.

#### VirusTotal

![](https://academy.hackthebox.com/storage/modules/27/virustotal.png)

#### DNSdumpster

![](https://academy.hackthebox.com/storage/modules/27/dnsdumpster.png)

#### Netcraft

![](https://academy.hackthebox.com/storage/modules/27/netcraft.png)

---

## Certificate Transparency

`Certificate Transparency` (`CT`) logs contain all certificates issued by a participating `Certificate Authority` (`CA`) for a specific domain. Therefore, `SSL/TLS certificates` from web servers include `domain names`, `subdomain names`, and `email addresses`. Since these logs are public and accessible to everyone, it is a valuable source for understanding the target company's infrastructure better. We can use a tool that outputs all the `CT logs` for our target domain from different sources and filtered is [ctfr.py](https://github.com/UnaPibaGeek/ctfr).

#### CTFR.py
```shell-session
gdxqpardo@htb[/htb]$ python3 ctfr.py -d inlanefreight.com

          ____ _____ _____ ____  
         / ___|_   _|  ___|  _ \ 
        | |     | | | |_  | |_) |
        | |___  | | |  _| |  _ < 
         \____| |_| |_|   |_| \_\
	
    Made by Sheila A. Berta (UnaPibaGeek)
	
inlanefreight.com
vc.inlanefreight.com
wlan.inlanefreight.com
afdc0102.inlanefreight.com
autodiscover.inlanefreight.com
kfdcex07.inlanefreight.com
kfdcex08.inlanefreight.com
videoconf.inlanefreight.com
<SNIP>
```

---

## Zone Transfer

`Zone transfer` in DNS refers to the transfer of zones to other DNS servers. This procedure is called the `Asynchronous Full Transfer Zone` (`AXFR`), as we have already learned. Since a DNS failure usually has severe consequences for a company, the `zone files` are almost without exception kept identical on several name servers. In the event of changes, it must be ensured that all servers have the same data stock. `Zone transfer` involves the mere transfer of files or records and the detection of discrepancies in the databases of the servers involved. DNS servers' configuration requires a great deal of attention because many administrators cannot always fully understand the operating principle from the technical configuration and troubleshooting. This leads to error rate and vulnerability, leading to misconfigurations and endanger the system or even the entire infrastructure by allowing the entire content of the zones to be viewed.

#### Performing DNS Zone Transfer

DNS Enumeration

```shell-session
gdxqpardo@htb[/htb]$ dig axfr inlanefreight.com @10.129.2.67

; <<>> DiG 9.16.1-Ubuntu <<>> axfr inlanefreight.com @10.129.2.67
;; global options: +cmd
inlanefreight.com.	    3600	IN	SOA	ns1.inlanefreight.com. adm.inlanefreight.com. 8 3600 300 86400 600
inlanefreight.com.	    3600	IN	A	178.128.39.165
inlanefreight.com.   	3600	IN	A	206.189.119.186
inlanefreight.com.	    3600	IN	NS	ns1.inlanefreight.com.
inlanefreight.com.	    3600	IN	NS	ns2.inlanefreight.com.
adm.inlanefreight.com.	3600	IN	A	134.209.24.248
blog.inlanefreight.com.	3600	IN	A	134.209.24.248
<SNIP>
```