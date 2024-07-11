DNS has many different records. Records are instructions that are located on authoritative DNS servers and contain information about a domain. These entries are written in the DNS syntax that gives the DNS servers appropriate instructions. Here are the most common DNS records that we will come across during our Penetration Tests:

|**Record**|**Description**|
|---|---|
|`A`|IP Version 4 Address records|
|`AAAA`|IP Version 6 Address records|
|`CNAME`|Canonical Name records|
|`HINFO`|Host Information records|
|`ISDN`|Integrated Services Digital Network records|
|`MX`|Mail exchanger record|
|`NS`|Name Server records|
|`PTR`|Reverse-lookup Pointer records|
|`SOA`|Start of Authority records|
|`TXT`|Text records|

There are many tools and resources we can work with to send queries to the DNS servers. For example, we can use tools like:

- `dig`
- `nslookup`

## NS Servers

We want to automate the enumeration process for potential zone transfers on misconfigured DNS servers to check for this vulnerability quickly with the tool we are creating. Let us start by enumerating some NS servers and checking if a DNS zone transfer is possible and, if so, get the information about the subdomains from the zone transfer.


## NS Records

`NS` records stand for `name server`. These records specify which DNS server is the authoritative server for the corresponding domain that contains the actual DNS records. Often a domain has several NS records that can specify primary and secondary name servers for that domain. Now that we know our target domain, we still need DNS servers we will interact with. For this, we have to find out which DNS servers are responsible for the domain, and for this, we can use the tool called `dig`. In this example, we use the domain called: `inlanefreight.com`

#### DIG - NS Queries

DNS Records and Queries

```shell-session
gdxqpardo@htb[/htb]$ dig NS inlanefreight.com

<SNIP>
;; ANSWER SECTION:
inlanefreight.com.	60	IN	NS	ns2.inlanefreight.com.
inlanefreight.com.	60	IN	NS	ns1.inlanefreight.com.

<SNIP>
```

#### DIG - SOA Queries

DNS Records and Queries

```shell-session
gdxqpardo@htb[/htb]$ dig SOA inlanefreight.com

<SNIP>
;; ANSWER SECTION:
inlanefreight.com.	879	IN	SOA	ns-161.awsdns-20.com. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400

<SNIP>
```

#### NSLOOKUP - SPF

DNS Records and Queries

```shell-session
gdxqpardo@htb[/htb]$ nslookup -type=SPF inlanefreight.com

Non-authoritative answer:
inlanefreight.com	rdata_99 = "v=spf1 include:_spf.google.com include:mail1.inlanefreight.com include:google.com ~all"
```

#### NSLOOKUP - DMARC

DNS Records and Queries

```shell-session
gdxqpardo@htb[/htb]$ nslookup -type=txt _dmarc.inlanefreight.com

Non-authoritative answer:
_dmarc.inlanefreight.com	text = "v=DMARC1; p=reject; rua=mailto:master@inlanefreight.com; ruf=mailto:master@inlanefreight.com; fo=1;"
```


