## Table of Contents

        - [Objectives/Topics Covered](#Objectives/Topics\Covered)
        - [Some specific objectives we'll cover include:](#Some\specific\objectives\we'll\cover\include:)
  - [Taxonomy of Reconnaissance](#Taxonomy\of\Reconnaissance)
  - [Built-In Tools](#Built-In\Tools)
    - [Advanced Searching](#Advanced\Searching)
      - [Social Media](#Social\Media)
    - [Job Ads](#Job\Ads)
    - [Specialized Search Engines](#Specialized\Search\Engines)
        - [WHOIS and DNS related resources](#WHOIS\and\DNS\related\resources)
    - [Threat Intelligence Platform](#Threat\Intelligence\Platform)
      - [Censys](#Censys)

##### Objectives/Topics Covered
- Types of reconnaissance activities
- WHOIS and DNS-based reconnaissance
- Advanced searching
- Searching by image
- Google Hacking
- Specialized search engines
- Recon-ng
- Maltego

##### Some specific objectives we'll cover include:

- Discovering subdomains related to our target company
- Gathering publicly available information about a host and IP addresses
- Finding email addresses related to the target
- Discovering login credentials and leaked passwords
- Locating leaked documents and spreadsheets



## Taxonomy of Reconnaissance
1. **Passive Recon**: Can be carried out by watching passively
2. **Active Recon**: Requires interacting with the target to provoke it in order to observe its response


Example information that we might collect includes domain names, IP address blocks, email addresses, employee names, and job posts. In the upcoming task, we'll see how to query DNS records and expand on the topics from the [Passive Reconnaissance](https://tryhackme.com/room/passiverecon) room and introduce advanced tooling to aid in your recon.

**Active recon** requires interacting with the target by sending requests and packets and observing if and how it responds. The responses collected - or lack of responses - would enable us to expand on the picture we started developing using passive recon
Ex: Using `nmap` to scan target subnets and live hosts. 


**Active recon** can be classified as:

1. **External Recon**: Conducted outside the target's network and focuses on the externally facing assets assessable from the Internet. One example is running Nikto from outside the company network.
2. **Internal Recon**: Conducted from within the target company's network. In other words, the pentester or red teamer might be physically located inside the company building. In this scenario, they might be using an exploited host on the target's network. An example would be using Nessus to scan the internal network using one of the target’s computers.

## Built-In Tools
- `whois`
- `dig`, `nslookup`, `host`
- `traceroute`, `tracert`

WHOIS is a request and response protocol that follows the RFC 3912 specification
- Listens on TCP port 43 for incoming requests. The domain registrar is responsible for maintaining the WHOIS records for the domain names it is leasing. `whois` will query the WHOIS server to provide all saved records

`whois` provides us with:

- Registrar WHOIS server
- Registrar URL
- Record creation date
- Record update date
- Registrant contact info and address (unless withheld for privacy)
- Admin contact info and address (unless withheld for privacy)
- Tech contact info and address (unless withheld for privacy)


### Advanced Searching
Being able to use a search engine efficiently is a crucial skill. The following table shows some popular search modifiers that work with many popular search engines.  

| Symbol / Syntax                  | Function                                            |
| -------------------------------- | --------------------------------------------------- |
| `"search phrase"`                | Find results with exact search phrase               |
| `OSINT filetype:pdf`             | Find files of type `PDF` related to a certain term. |
| `salary site:blog.tryhackme.com` | Limit search results to a specific site.            |
| `pentest -site:example.com`      | Exclude a specific site from results                |
| `walkthrough intitle:TryHackMe`  | Find pages with a specific term in the page title.  |
| `challenge inurl:tryhackme`      | Find pages with a specific term in the page URL.    |

Note: in addition to `pdf`, other filetypes to consider are `doc`, `docx`, `ppt`, `pptx`, `xls`, `xlsx`

Each search engine might have a slightly varied set of rules and syntax. To learn about the specific syntax for the different search engines, you will 
https://www.google.com/advanced_search
https://duckduckgo.com/duckduckgo-help-pages/results/syntax/
https://help.bing.microsoft.com/apex/index/18/en-US/10002
https://support.google.com/websearch/answer/2466433



Search engines crawl the world wide web day and night to index new web pages and files. Sometimes this can lead to indexing confidential infomation. Examples of confidential information include:
- Documents for internal company use
- Confidential speadsheets with usernames, email addresses, and even passwords sometimes
- Files containing usernames
- Sensitive directories
- Service version number
- Error Messages

Combining advanced Google searches with specific terms, documents containing sensitive information or vulnerable web servers can be found. Websites such as [Google Hacking Database](https://www.exploit-db.com/google-hacking-database) (GHDB) collect such search terms and are publicly available. Let's take a look at some of the GHDB queries to see if our client has any confidential information exposed via search engines. GHDB contains queries under the following categories:

- **Footholds**  
	Consider [GHDB-ID: 6364](https://www.exploit-db.com/ghdb/6364) as it uses the query `intitle:"index of" "nginx.log"` to discover Nginx logs and might reveal server misconfigurations that can be exploited.


- **File Containing Usernames**
	For example, [GHDB-ID: 7047](https://www.exploit-db.com/ghdb/7047) uses the search term `intitle:"index of" "contacts.txt"` to discover files that leak juicy information.

- **Sensitive Directories**  
    For example, consider [GHDB-ID: 6768](https://www.exploit-db.com/ghdb/6768), which uses the search term `inurl:/certs/server.key` to find out if a private RSA key is exposed.

- **Web Server Detection**  
    Consider [GHDB-ID: 6876](https://www.exploit-db.com/ghdb/6876), which detects GlassFish Server information using the query `intitle:"GlassFish Server - Server Running"`.

- **Vulnerable Files**  
    For example, we can try to locate PHP files using the query `intitle:"index of" "*.php"`, as provided by [GHDB-ID: 7786](https://www.exploit-db.com/ghdb/7786).

- **Vulnerable Servers**  
    For instance, to discover SolarWinds Orion web consoles, [GHDB-ID: 6728](https://www.exploit-db.com/ghdb/6728) uses the query `intext:"user name" intext:"orion core" -solarwinds.com`.

- **Error Messages**  
    Plenty of useful information can be extracted from error messages. One example is [GHDB-ID: 5963](https://www.exploit-db.com/ghdb/5963), which uses the query `intitle:"index of" errors.log` to find log files related to errors.

Now we'll explore two additional sources that can provide valuable information without interacting with our target:

- Social Media
- Job ads

#### Social Media
- LinkedIn
- Twitter
- Facebook
- Instagram

### Job Ads

Job advertisements can also tell you a lot about a company. In addition to revealing names and email addresses, job posts for technical positions could give insight into the target company’s systems and infrastructure

Note that the [Wayback Machine](https://archive.org/web/) can be helpful to retrieve previous versions of a job opening page on your client’s site.


### Specialized Search Engines
##### WHOIS and DNS related resources
There are a handful of websites that offer advanced DNS services that are free to use. Some of these websites offer rich functionality and could have a complete room dedicated to exploring one domain. For now, we'll focus on key DNS related aspects. We will consider the following:

- [ViewDNS.info](https://viewdns.info/)
- [Threat Intelligence Platform](https://threatintelligenceplatform.com/)

With shared hosting, one IP address is shared among many different web servers with different domain names. With reverse IP lookup, starting from a domain name or an IP address, you can find the other domain names using a specific IP address(es).

In the figure below, we used reverse IP lookup to find other servers sharing the same IP addresses used by `cafe.thmredteam.com`. Therefore, it is important to note that knowing the IP address does not necessarily lead to a single website.
![[a6a57e946b742bf9439430c1669382a5.png]]

### Threat Intelligence Platform

[Threat Intelligence Platform](https://threatintelligenceplatform.com/) requires you to provide a domain name or an IP address, and it will launch a series of tests from malware checks to WHOIS and DNS queries. The WHOIS and DNS results are similar to the results we would get using `whois` and `dig`, but Threat Intelligence Platform presents them in a more readable and visually appealing way. There is extra information that we get with our report. For instance, after we look up `thmredteam.com`, we see that Name Server (NS) records were resolved to their respective IPv4 and IPv6 addresses, as shown in the figure below.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/8ba0fec5aad89cfb7c3d242ed92c9da4.png)

On the other hand, when we searched for `cafe.thmredteam.com`, we could also get a list of other domains on the same IP address. The result we see in the figure below is similar to the results we obtained using [ViewDNS.info](https://viewdns.info/).

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/c920bcf224185c47c4ba54b300079e48.png)


#### Censys
[Censys Search](https://search.censys.io) can provide a lot of information about IP addresses and domains
