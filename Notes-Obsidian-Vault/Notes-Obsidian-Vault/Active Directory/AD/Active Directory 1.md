## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Enumeration](#Enumeration)
    - [DNS](#DNS)
      - [Viewdns.info](#Viewdns.info)
    - [Public Data](#Public\Data)
    - [Sharepoint Admin Job Listing](#Sharepoint\Admin\Job\Listing)
      - [Enumeration TTP Ex ->](#Enumeration\TTP\Ex\->)
      - [Goals During 2nd Phase Domain Enumeration](#Goals\During\2nd\Phase\Domain\Enumeration)
          - [Passive Enumeration/Active Enumeration](#Passive\Enumeration/Active\Enumeration)
        - [fping active checks](#fping\active\checks)
- [kerbrute username enumeration](#kerbrute\username\enumeration)
        - [Responder](#Responder)

## Table of Contents

  - [Enumeration](#Enumeration)
    - [DNS](#DNS)
      - [Viewdns.info](#Viewdns.info)
    - [Public Data](#Public\Data)
    - [Sharepoint Admin Job Listing](#Sharepoint\Admin\Job\Listing)
      - [Enumeration TTP Ex ->](#Enumeration\TTP\Ex\->)
      - [Goals During 2nd Phase Domain Enumeration](#Goals\During\2nd\Phase\Domain\Enumeration)
          - [Passive Enumeration/Active Enumeration](#Passive\Enumeration/Active\Enumeration)
        - [fping active checks](#fping\active\checks)
- [kerbrute username enumeration](#kerbrute\username\enumeration)
        - [Responder](#Responder)

5ea7295838103a2f53d82086402f4a37

c04a39ee7d5be4b556468afc01ab5eaa
## Enumeration
- IP Address
- Domain INformation - IP, site information, DNS registrar. Who administers the domain? Are there any subdomains tied to the target?
- Schema Format
- Data Disclosures
- Breach Data


|   |   |
|---|---|
|ASN / IP registrars|[IANA](https://www.iana.org/), [arin](https://www.arin.net/) for searching the Americas, [RIPE](https://www.ripe.net/) for searching in Europe, [BGP Toolkit](https://bgp.he.net/)|
|Domain Registrars & DNS|[Domaintools](https://www.domaintools.com/), [PTRArchive](http://ptrarchive.com/), [ICANN](https://lookup.icann.org/lookup), manual DNS record requests against the domain in question or against well known DNS servers, such as `8.8.8.8`.|
|Social Media|Searching Linkedin, Twitter, Facebook, your region's major social media sites, news articles, and any relevant info you can find about the organization.|
|Public-Facing Company Websites|Often, the public website for a corporation will have relevant info embedded. News articles, embedded documents, and the "About Us" and "Contact Us" pages can also be gold mines.|
|Cloud & Dev Storage Spaces|[GitHub](https://github.com/), [AWS S3 buckets & Azure Blog storage containers](https://grayhatwarfare.com/), [Google searches using "Dorks"](https://www.exploit-db.com/google-hacking-database)|
|Breach Data Sources|[HaveIBeenPwned](https://haveibeenpwned.com/) to determine if any corporate email accounts appear in public breach data, [Dehashed](https://www.dehashed.com/) to search for corporate emails with cleartext passwords or hashes we can try to crack offline. We can then try these passwords against any exposed login portals (Citrix, RDS, OWA, 0365, VPN, VMware Horizon, custom applications, etc.) that may use AD authentication.|


### DNS

DNS is a great way to validate our scope and find out about reachable hosts the customer did not disclose in their scoping document. Sites like [domaintools](https://whois.domaintools.com/), and [viewdns.info](https://viewdns.info/) are great spots to start. We can get back many records and other data ranging from DNS resolution to testing for DNSSEC and if the site is accessible in more restricted countrie


#### Viewdns.info

![image](https://academy.hackthebox.com/storage/modules/143/viewdnsinfo.png)

This is also a great way to validate some of the data found from our IP/ASN searches. Not all information about the domain found will be current, and running checks that can validate what we see is always good practice.

### Public Data

Social media can be a treasure trove of interesting data that can clue us in to how the organization is structured, what kind of equipment they operate, potential software and security implementations, their schema, and more. On top of that list are job-related sites like LinkedIn, Indeed.com, and Glassdoor. Simple job postings often reveal a lot about a company. For example, take a look at the job listing below. It's for a `SharePoint Administrator` and can key us in on many things. We can tell from the listing that the company has been using SharePoint for a while and has a mature program since they are talking about security programs, backup & disaster recovery, and more. What is interesting to us in this posting is that we can see the company likely uses SharePoint 2013 and SharePoint 2016. That means they may have upgraded in place, potentially leaving vulnerabilities in play that may not exist in newer versions. This also means we may run into different versions of SharePoint during our engagements.

### Sharepoint Admin Job Listing

![image](https://academy.hackthebox.com/storage/modules/143/spjob2.png)



#### Enumeration TTP Ex ->
- Check for ASN/IP & Domain Data
- [IANA](https://www.iana.org/), [arin](https://www.arin.net/) for searching the Americas, [RIPE](https://www.ripe.net/) for searching in Europe, [BGP Toolkit](https://bgp.he.net/)
	- [ViewDNS.info]([ViewDNS](https://viewdns.info/)
- nslookup
- check for exposed files/filetypes
	- filetype:xxx inurl:http://github.com
- Search google dorks for email addresses
	- `intext:"@inlanefreight.com" inurl:inlanefreight.com`,
- ([LinkedIn2Username](https://github.com/initstring/linkedin2username))

#### Goals During 2nd Phase Domain Enumeration
- AD Users - trying to enumerate valid user accounts via password spraying
- AD Joined Computers  - Key computers suhc as Domain Controllers, File Servers, SMTP servers, SQL, exchange etc.
- Key Services - Kerberos, NetBIOS, LDAP, DNS
- Vulnerable Hosts and services

###### Passive Enumeration/Active Enumeration
- Put yo ear to the wire with TCPdump, or wireshark
	- Looks for the `MDNS` and `ARP` protocol packets
- Start responder and and listen on for LLMNR and NBT-NS/MDNS requests and responses

##### fping active checks
```bash
fping -asqg 0.0.0.x/24
```


# kerbrute username enumeration
doesnt usualy trigger alerts because of the pre-auth authent
```bash
kerbrute userenum -d DOMAIN.NAME --dc DC.REM.IP.x WORDLIST.txt valid_ad_users
```


|   |   |
|---|---|
|[Responder](https://github.com/lgandx/Responder)|Responder is a purpose-built tool to poison LLMNR, NBT-NS, and MDNS, with many different functions.|
|[Inveigh](https://github.com/Kevin-Robertson/Inveigh)|Inveigh is a cross-platform MITM platform that can be used for spoofing and poisoning attacks.|
|[Metasploit](https://www.metasploit.com/)|Metasploit has several built-in scanners and spoofing modules made to deal with poisoning attacks.|


##### Responder
`-A` - Analyze mode, view LLMNR/NBT-NS requests in the environment without poisoning requests
	must supply interface or IP
`-wf` - start s the WPAD rogue proxy sevrer




