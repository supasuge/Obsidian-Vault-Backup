## Table of Contents

    - [Footprinting](#Footprinting)
    - [Technical Assessment Methods](#Technical\Assessment\Methods)
    - [Privacy and Confidentiality](#Privacy\and\Confidentiality)
    - [Data Analysis](#Data\Analysis)
    - [MITRE ATT&CK Framework](#MITRE\ATT&CK\Framework)
    - [Tools and Techniques](#Tools\and\Techniques)
      - [Companies](#Companies)
- [](#)
- [ARIN WHOIS data and services are subject to the Terms of Use](#arin\whois\data\and\services\are\subject\to\the\terms\of\use)
- [available at: https://www.arin.net/resources/registry/whois/tou/](#available\at:\https://www.arin.net/resources/registry/whois/tou/)
- [](#)
- [If you see inaccuracies in the results, please report at](#if\you\see\inaccuracies\in\the\results,\please\report\at)
- [https://www.arin.net/resources/registry/whois/inaccuracy_reporting/](#https://www.arin.net/resources/registry/whois/inaccuracy_reporting/)
- [](#)
- [Copyright 1997-2023, American Registry for Internet Numbers, Ltd.](#copyright\1997-2023,\american\registry\for\internet\numbers,\ltd.)
- [](#)
- [start](#start)
      - [People](#People)
      - [Social Networking](#Social\Networking)

The excerpt you've provided outlines several critical concepts and activities in the realm of ethical hacking and cybersecurity, focusing on the initial phase of an engagement known as "footprinting" or reconnaissance. Here's a breakdown of the key terms and activities mentioned:

### Footprinting
Footprinting is the initial phase of cybersecurity reconnaissance where an attacker or ethical hacker gathers as much information as possible about a target system, network, or organization. This information can include domain details, network infrastructure, and employee information.

### Technical Assessment Methods
These include various techniques used to evaluate the security of a system or network. Common methods include:

- **Open Source Intelligence (OSINT)**: Gathering data from publicly available sources to learn more about a target. This includes information from websites, forums, social media, etc.
- **Port Scanning**: Involves using tools like Nmap to scan a system's open ports to infer what services and applications are running.
- **Vulnerability Scanning**: Using tools like Nessus or OpenVAS to automatically scan systems for known vulnerabilities.

### Privacy and Confidentiality
In the context of ethical hacking, it's crucial to maintain the privacy and confidentiality of any information discovered during the reconnaissance phase. Ethical hackers must have permission to probe systems and are obligated to keep findings private.

### Data Analysis
The process of examining data gathered during reconnaissance to identify patterns, correlations, and actionable intelligence. Data analysis helps in making informed decisions about where vulnerabilities may exist.

### MITRE ATT&CK Framework
A globally accessible knowledge base of adversary tactics and techniques based on real-world observations. It can guide ethical hackers during assessments to match their findings with known attacker behavior patterns.

### Tools and Techniques
- **DNS Enumeration**: Extracting information about domain names and associated networks. Tools like `dig` and `nslookup` are commonly used, along with online services to perform DNS lookups and domain history checks.
- **Network Mapping**: Determining the network structure, including devices and interconnections. Tools such as traceroute or mtr can be used.
- **Social Engineering**: Gathering information from human targets through various means without them knowing they are giving up valuable information.
- **Public Records and Databases**: Searching through databases that may contain information about the target organization, such as WHOIS databases for domain registration details.

The overarching goal of these activities is to gather detailed information without alerting the target of the reconnaissance activities. Ethical hackers use this information to understand the attack surface and prepare for further penetration testing phases responsibly and legally.




There are a couple of reasons you may want to use open source intelligence. The first is that you haven't been provided with any details about your target. You may be doing a true red team against the target company and systems. In that case, you need to locate as much information as you can so you know not only what your attack surface looks like but possible ways in. Additionally, you can find a lot of information about individuals within an organization. This is especially useful because social engineering attacks have a good possibility of success. Having contacts to go after is essential.  
  
The second reason is that organizations aren't always aware of the amount of information they are leaking. As noted earlier, attackers can find footholds, as well as potential human targets for social engineering attacks. If you are working hand in hand with the company you are performing testing for (that is, you are doing white‐box testing), you don't need to use open source intelligence for starting points, but you should still gather what information is available for the awareness of the company. They may be able to limit their exposure. Even if there isn't anything that could be pulled back, they could work on awareness so employees and the organization as a whole aren't leaking information unnecessarily.  
  
There are a number of tools that can be used to automate the collection of information, and we'll cover their use as part of looking at how to gather open source intelligence about companies and people. We'll also take a look at social networking sites and some of the ways those websites can be used. Even in cases where privacy settings are locked down, there is still a lot of information that can be gathered. There are also sites that exist to be public, and those can definitely be used.

#### Companies

There are several starting points when it comes to acquiring open source intelligence about your target. The first is to look at the company overall. You'll want to gather information about locations the company has. There are instances where this can be easy. However, increasingly, it can be harder. The reason it can be harder is that companies recognize that the more information they provide, the more that information can be used against them. So, unless they are required to provide that information by regulations, they don't provide it. There are a few resources that can be used to gather information about companies.  
  
Sometimes, these resources can be used to gather information that may be used for social engineering attacks. In some cases, you will be able to gather details about a company's network. Both types of information can be useful. Details about a company and its organizational and governance structure can come from a database maintained by the U.S. government, in the case of businesses registered in the U.S. Details about the business network may come from databases maintained by the organizations that are responsible for governance of the Internet.  
  
**EDGAR**  
Public companies are required to provide information about themselves. There are resources you can use to look up that information. In the process, you may gather information about a company's organizational structure. The organizational structure can tell you who has what position, so when you are working on sending out email messages to gather additional information later, you know who they should appear to be from. You can select the holder of an appropriate position.  
  
The Securities and Exchange Commission (SEC) has a database that stores all public filings associated with a company. The Electronic Data Gathering, Analysis, and Retrieval (EDGAR) system can be used to look up public filings such as the annual report in the form 10‐K. Additionally, the quarterly reports, 10‐Qs, are also submitted to EDGAR and stored there. These reports provide details about a company's finances. The 11‐K, a form including details about employee stock option plans, is also filed with EDGAR. Accessing EDGAR is as easy as going to EDGAR at the SEC website. You can see the search field, part of the page, in Figure 4.1.  
  

![The figure shows a snapshot of the Electronic Data Gathering, Analysis, and Retrieval (EDGAR) site. It shows the company name "EDGAR | Company Filings" at the top. Below it, a search bar is shown to search for a company name. On the right, a search bar is present to do a fast search. At the top right corner, the print, Facebook, Twitter, mail, and plus icons are shown.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f001.png)

Figure 4.1: EDGAR site

  
One of the most useful forms you can find in EDGAR is Schedule 14A on SEC site, which is a proxy statement and will include the annual report to the shareholders, which may include a lot of useful information for you. As an example, Figure 4.2 shows a very small section of the annual report to the shareholders for Microsoft Corporation. Other sections that are not shown include Corporate Governance at Microsoft, Board of Directors, and Audit Committee Matters. While at a high level—what is included in these reports will be the same across all public companies—there may be some companies that present more in the way of specific details than other companies. Some companies will have more to report than others. For instance, the table of contents for the Microsoft report shows the page total in the 80s. The report for John Wiley & Sons shows a page count in the 50s. That's about 30 fewer pages between the two companies.  
  

![The figure shows a portion of Schedule 14A of the annual report to the shareholders for Microsoft Corporation. On the left, number three with "Named executive officer compensation" text is shown. On the right, the annual report is shown.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f002.png)

Figure 4.2: Portion of Schedule 14A for Microsoft

  
**Domain Registrars**  
EDGAR is only for public companies. Not every company is public. You don't get the same level of insight for a private company that you do for a public company. However, EDGAR is not the only resource that can be used to gather information about a company. Another source of information, related to the Internet itself, is the domain registrars. You won't get the same sort of information from the domain registrars as you would from EDGAR, but it's still sometimes a decent source of information. For a start, you can get the address of what is probably the company's headquarters.  
  
This is not a guarantee, however. As mentioned, companies are starting to hide information provided to the registrars. Information is hidden behind the registrar. When you ask for information, you will get what the registrar has been asked to present and not necessarily the real details. There is nothing that says that the registrars have to be provided with real addresses, unless they are checking a billing address on a credit card for payment. In fact, there have been times I have had domains registered with bogus phone numbers and incorrect addresses. Since the data is public, it's important to be careful about what is shared. Anyone can mine the registries for this information and use it for any number of purposes.  
  
Before we get too far down this road, though, it's probably useful for you to understand how the Internet is governed when it comes to domains and addresses. First, there is the Internet Corporation for Assigned Names and Numbers (ICANN). Underneath ICANN is the Internet Assigned Numbers Authority (IANA), which is responsible for managing IP addresses, ports, protocols, and other essential numbers associated with the functioning of the Internet. Prior to the establishment of ICANN in 1998, IANA's functions were managed by one man, Jon Postel, who also maintained the request for comments (RFC) documents.  
  
In addition to ICANN, responsible for names and numbering, are the domain registrars. These organizations store information about addresses they are responsible for as well as contacts. There was a time when registering a domain and other data went through a single entity. Now, though, there are several companies that can perform registrant functions. If you want to register a domain, you go to a registrar company like DomainMonger or GoDaddy. Those companies can then be queried for details about the domains.  
  
To grab information out of the regional Internet registry (RIR), you would use the `whois` program. This is a program that can be used on the command line on most Unix‐like systems, including Linux and macOS. There are also websites that have implementations of `whois` if you don't have a Unix‐like system handy. Here you can see a portion of the output from a `whois` query.  
  

whois **Query of** [wiley.com](http://wiley.com)

  

$ whois wiley.com  
Domain Name: WILEY.COM  
   Registry Domain ID: 936038_DOMAIN_COM-VRSN  
   Registrar WHOIS Server: whois.corporatedomains.com  
   Registrar URL: http://cscdbs.com  
   Updated Date: 2021-08-30T16:27:21Z  
   Creation Date: 1994-10-12T04:00:00Z  
   Registry Expiry Date: 2023-10-11T04:00:00Z  
   Registrar: CSC Corporate Domains, Inc.  
   Registrar IANA ID: 299  
   Registrar Abuse Contact Email: domainabuse@cscglobal.com  
   Registrar Abuse Contact Phone: 8887802723  
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited  
   Name Server: AUS-IBEXTDNS-01.WILEY.COM  
   Name Server: CAR-IBEXTDNS-01.WILEY.COM  
   DNSSEC: unsigned  
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/  
←- SNIP →  
Domain Name: wiley.com  
Registry Domain ID: 936038_DOMAIN_COM-VRSN  
Registrar WHOIS Server: whois.corporatedomains.com  
Registrar URL: www.cscprotectsbrands.com  
Updated Date: 2021-08-30T12:27:21Z  
Creation Date: 1994-10-12T00:00:00Z  
Registrar Registration Expiration Date: 2023-10-11T04:00:00Z  
Registrar: CSC CORPORATE DOMAINS, INC.  
Sponsoring Registrar IANA ID: 299  
Registrar Abuse Contact Email: domainabuse@cscglobal.com  
Registrar Abuse Contact Phone: +1.8887802723  
Domain Status: clientTransferProhibited http://www.icann.org/epp#clientTransferProhibited  
Registry Registrant ID:  
Registrant Name: Domain Administrator  
Registrant Organization: John Wiley & Sons, Inc  
Registrant Street: 111 River Street  
Registrant City: Hoboken  
Registrant State/Province: NJ  
Registrant Postal Code: 07030  
Registrant Country: US  
Registrant Phone: +1.3175723355  
Registrant Phone Ext:  
Registrant Fax: +1.3175724355  
Registrant Fax Ext:  
Registrant Email: domains@wiley.com  
Registry Admin ID:  
Admin Name: Domain Administrator  
Admin Organization: John Wiley & Sons, Inc  
Admin Street: 111 River Street  
Admin City: Hoboken  
Admin State/Province: NJ  
Admin Postal Code: 07030  
Admin Country: US  
Admin Phone: +1.3175723355  
Admin Phone Ext:  
Admin Fax: +1.3175724355  
Admin Fax Ext:  
Admin Email: domains@wiley.com  
Registry Tech ID:  
Tech Name: Electronic Support Services  
Tech Organization: John Wiley & Sons Inc  
Tech Street: 111 River Street  
Tech City: Hoboken  
Tech State/Province: NJ  
Tech Postal Code: 07030  
Tech Country: US  
Tech Phone: +1.3175723100  
Tech Phone Ext:  
Tech Fax: +1.3175724355  
Tech Fax Ext:  
Tech Email: domains@wiley.com  
Name Server: car-ibextdns-01.wiley.com  
Name Server: aus-ibextdns-01.wiley.com

  
DNSSEC: unsigned There is a lot of output there to look through, and I've snipped out a bunch of it to keep it to really relevant information. First, `whois` checks with IANA's `whois` server to figure out who it needs to check with about this specific domain. You can see that happen at the very top of the output. IANA indicates that CSC Corporate Domains is the registrar for this domain. We get the details about the registrar CSC Corporate Domains. After that, and a lot of information being snipped out, we finally get the details about the domain `wiley.com`. What you can see in the output is the address and phone number for the company. Additionally, you get information about a handful of contacts for the company. Registrars expect an administrative contact and a technical contact.  
  
As indicated earlier, not all domains will provide this level of detail. An example of a domain that doesn't include any contact details is [spamhaus.org](http://spamhaus.org). Here you can see that the contact information shows that the data has been redacted for privacy.  
  
**Details About [spamhaus.org](http://spamhaus.org)**  
  

Domain Name: spamhaus.org  
Registry Domain ID: 3438c67fd16d45eca26608d562bb3be6-LROR  
Registrar WHOIS Server: http://whois.comlaude.com  
Registrar URL: https://comlaude.com/whois  
Updated Date: 2022-09-06T23:10:56Z  
Creation Date: 1999-10-01T11:03:57Z  
Registry Expiry Date: 2023-10-01T11:03:57Z  
Registrar: Nom-iq Ltd. dba COM LAUDE  
Registrar IANA ID: 470  
Registrar Abuse Contact Email: abuse@comlaude.com  
Registrar Abuse Contact Phone: +44.2074218250  
Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited  
Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited  
Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited  
Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited  
Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited  
Registry Registrant ID: REDACTED FOR PRIVACY  
Registrant Name: REDACTED FOR PRIVACY  
Registrant Organization: Spamhaus IP Holdings SLU  
Registrant Street: REDACTED FOR PRIVACY  
Registrant City: REDACTED FOR PRIVACY  
Registrant State/Province:  
Registrant Postal Code: REDACTED FOR PRIVACY  
Registrant Country: AD  
Registrant Phone: REDACTED FOR PRIVACY  
Registrant Phone Ext: REDACTED FOR PRIVACY  
Registrant Fax: REDACTED FOR PRIVACY  
Registrant Fax Ext: REDACTED FOR PRIVACY

  
Using a strategy like this will keep information private and out of the hands of the very people [spamhaus.org](http://spamhaus.org) seeks to protect against. The data provided can be used to create a mailing list for spammers. It can also be used to create a physical mailing list for traditional junk mail providers (sometimes called _mail marketing companies_).  
  
**Regional Internet Registries**  
Not all the useful information is stored with the domain registrars, however. There is other data that is important to be kept. Earlier, we discussed IANA. While the IANA server provided information about domain registrars, its purpose has long been to be a central clearinghouse for addresses. This includes not only port numbers for well‐known services but also IP addresses. IANA, at a high level, owns all IP addresses. It hands out those IP addresses, based on need, to the RIRs. The RIRs then hand them out to organizations that fall into their geographic region.  
  
There are five RIRs around the world. They are based in different geographic regions, and an organization would refer to the RIR where they are located for things like IP addresses. The RIRs and the geographic areas they are responsible for are listed here:

- **African Network Information Center (AfriNIC):** Africa
- **American Registry for Internet Numbers (ARIN):** United States and Canada, as well as Antarctica and parts of the Caribbean
- **Asia Pacific Network Information Centre (APNIC):** Asia, Australia, New Zealand, and neighboring countries
- **Latin America and Caribbean Network Information Centre (LACNIC):** Latin America and parts of the Caribbean
- **Réseaux IP Européens Network Coordination Centre (RIPE NCC):** Europe, Russia, Greenland, the Middle East, and parts of Central Asia

All of these RIRs have their own databases that can be queried using `whois`, just as we used `whois` to query information from the domain registrars. Typically, you would use `whois` against the RIRs to find out who owns a particular IP address. For example, in the output for [wiley.com](http://wiley.com) earlier, part of the output indicated which name servers the domain uses to resolve hostnames to IP addresses. One of those name servers is `car-ibextdns-01.wiley.com`. With a minimal amount of effort (we will cover DNS in the section “Domain Name System” later in this lesson), we can discover that the hostname `car-ibextdns-01.wiley.com` resolves to the IP address 63.97.119.2. Using `whois`, we can find out who owns that IP address. You can see the results of that query in the following code.  
  
`whois` **Query for IP Address**  
  

$ whois 63.97.119.2  
  
#  
# ARIN WHOIS data and services are subject to the Terms of Use  
# available at: https://www.arin.net/resources/registry/whois/tou/  
#  
# If you see inaccuracies in the results, please report at  
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/  
#  
# Copyright 1997-2023, American Registry for Internet Numbers, Ltd.  
#  
  
# start  
  
NetRange:       63.64.0.0 - 63.127.255.255  
CIDR:           63.64.0.0/10  
NetName:        UUNET63  
NetHandle:      NET-63-64-0-0-1  
Parent:         NET63 (NET-63-0-0-0-0)  
NetType:        Direct Allocation  
OriginAS:  
Organization:   Verizon Business (MCICS)  
RegDate:        1999-01-22  
Updated:        2022-05-31  
Comment:        ADDRESSES WITHIN THIS BLOCK ARE NON-PORTABLE  
Ref:            https://rdap.arin.net/registry/ip/63.64.0.0  
  
OrgName:        Verizon Business  
OrgId:          MCICS  
Address:        22001 Loudoun County Pkwy  
City:           Ashburn  
StateProv:      VA  
PostalCode:     20147  
Country:        US  
RegDate:        2006-05-30  
Updated:        2022-10-11  
Ref:            https://rdap.arin.net/registry/entity/MCICS  
  

  
NetRange:       63.97.118.0 - 63.97.119.255  
CIDR:           63.97.118.0/23  
NetName:        UU-63-97-118  
NetHandle:      NET-63-97-118-0-1  
Parent:         UUNET63 (NET-63-64-0-0-1)  
NetType:        Reassigned  
OriginAS:  
Customer:       JOHN WILEY & SONS, INC. (C06398648)  
RegDate:        2017-03-21  
Updated:        2017-03-21  
Comment:        Addresses within this block are non-portable.  
Ref:            https://rdap.arin.net/registry/ip/63.97.118.0  
  
CustName:       JOHN WILEY & SONS, INC.  
Address:        1649 W FRANKFORD RD  
City:           CARROLLTON  
StateProv:      TX  
PostalCode:     75007  
Country:        US  
RegDate:        2017-03-21  
Updated:        2017-03-21  
Ref:            https://rdap.arin.net/registry/entity/C06398648  

  
This provides us with a lot of useful information. First, even though we provided a single IP address, addresses are allocated in blocks. The first thing we find is that the parent block was allocated in 1999. Based on the name of the block, Verizon Business was not the original owner, since it is named UUNET63 and UUNET is a network that Verizon obtained through acquisition in the early 2000s. The specific block the IP address provided belongs to, though, is `63.197.118.0` – `63.197.119.255`. That block, unsurprisingly, belongs to John Wiley & Sons. You can see that the address for John Wiley & Sons is in Carrollton, TX, which aligns with the block being registered under ARIN, which shows as the `whois` server used in the first part of the output. The business is located in the United States, so the corresponding regional registry is ARIN.  
  
We've learned a couple of things about the business and the IP addresses that belong to it. Additionally, you can see we have a contact that came out of the response. This gives us a name and email address. If we were going to be testing against this business, we could make use of this information.

#### People

While systems and the IP addresses associated with them make good entry points for technical attacks—those against services that are available—contact information for people can be more useful. There are other places we can go to get lists of people who belong to a target organization. Again, there are utilities we can use to help us gather this information. One of them is theHarvester. This is a script that will search through different sources to locate contact information based on a domain name provided to the program. In the following code, you can see the output from theHarvester run against the domain [wiley.com](http://wiley.com), using Bing as the search source. DuckDuckGo is another search option, but it didn't yield any people results in this search, though it did yield a single host.  
  
**theHarvester Output**  
  

```bash
$  
theHarvester -d wiley.com -b bing  
*******************************************************************  
*  _   _                                            _             *  
* | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *  
* | __|  _ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *  
* | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *  
*  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *  
*                                                                 *  
* theHarvester 4.2.0                                              *  
* Coded by Christian Martorella                                   *  
* Edge-Security Research                                          *  
* cmartorella@edge-security.com                                   *  
*                                                                 *  
*******************************************************************  
  

[*] Target: wiley.com  
  
Searching 0 results.  
[*] Searching Bing.  
  
[*] No IPs found.  
  
[*] No emails found.  
  
[*] Hosts found: 18  
---------------------  
access.wiley.com:63.97.118.255  
agupubs.onlinelibrary.wiley.com:162.159.129.87, 162.159.130.87  
authorservices.wiley.com:108.138.128.128, 108.138.128.52, 108.138.128.31, 108.138.128.89  
bpspsychub.onlinelibrary.wiley.com:162.159.129.87, 162.159.130.87  
bpspubs.onlinelibrary.wiley.com:162.159.130.87, 162.159.129.87  
car-access.wiley.com:63.97.118.255  
careers.wiley.com:34.215.77.72, 52.36.83.75  
chemistry-europe.onlinelibrary.wiley.com:162.159.130.87, 162.159.129.87  
dev.store.wiley.com:104.18.28.249, 104.18.29.249  
ietresearch.onlinelibrary.wiley.com:162.159.129.87, 162.159.130.87  
newsroom.wiley.com:162.159.129.11, 162.159.130.11  
nph.onlinelibrary.wiley.com:162.159.129.87, 162.159.130.87  
onlinelibrary.wiley.com:162.159.130.87, 162.159.129.87  
read.wiley.com:104.18.15.106, 104.18.14.106  
sid.onlinelibrary.wiley.com:162.159.130.87, 162.159.129.87  
support.wiley.com:13.110.24.11, 13.110.24.10, 13.110.24.13  
universityservices.wiley.com:23.185.0.2  
www.wiley.com:104.18.17.99, 104.18.16.99
```

  
Bing is not the only source that can be used with theHarvester. Other search sources include VirusTotal, ThreatCrowd, and Yahoo. Many of these require you to register with the service to obtain an application programming interface (API) key so that you can provide it to theHarvester. Without the key, theHarvester can't issue queries to those search services. This helps to protect them against misuse.  
  

Hands‐on Activity

You should have a lot of experience running these tools before you try to take the test so get as much time working with them as you can. One example is using theHarvester. Select a domain name you have some association with, preferably one you have some rights to, and use theHarvester to look up information about that domain.

While tools like theHarvester are good for identifying information about people at a company automatically, you may want or need to go deeper. This is where you might consider using a people search website, like Spokeo, BeenVerified, Pipl, Wink, or Intelius. These sites can be used to search for people, though searching for people may cost you money.  
  
There are other people search sites that are more focused on looking at social networking presence, and searches can be done using usernames. The website PeekYou **will do people searches using real names**. PeekYou also allows you to look for a username instead, though. This username could be found across multiple social network sites, as well as other locations where a username is exposed to the outside world. A search for a username I have used in the past turned up a few hits, though the information was badly out of date and inconsistent. An online presence, though, can be used for more than just finding people. Searching for me in PeekYou turns up a lot of people who are not me. This is shown in Figure 4.3.  
  

![The figure shows a PeekYou output. It displays the search results for the username 'Richard Messier'. The search results include features like cities, possible relatives, and background check.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f003.png)

Figure 4.3: PeekYou output

  
This example should show you that not only can information online be inaccurate but in some (or many) cases, you need to know something about your target before you start looking.

#### Social Networking

Social networking sites are how people connect. They come in a number of different flavors and have been around for more than two decades, with the first one, [sixdegrees.com](http://sixdegrees.com), launched in 1997. Sites like Myspace have allowed users to share music and personal information with others. Facebook has allowed users to create communities, share news and information, and get back in touch with people they have fallen away from. Twitter is often useful for news and updates, as well as marketing information—making announcements, for example. LinkedIn is useful for all sorts of business purposes. This includes sharing updates about company activities, personal achievements, and moves.  
  
You can spend a lot of time looking for information by hand on these sites. There are also tools that you can use. One of them is one we've already looked at. In addition to using traditional search sites like Google and Bing, theHarvester will also search through some of the social network sites. There are also tools that are specific to some of the sites, such as LinkedIn, where you can use CrossLinked. Finally, we have a tool like Maltego, which is good for open source intelligence in general, though there are ways it can be used to search social network sites for details about people and companies.  
  
**Facebook**  
While sites that may primarily be thought of as personal sites may focus more on individuals, they are still useful for someone doing work as an ethical hacker. A site like Facebook is interesting because people seem to let their guard down there, posting a lot of details that perhaps they shouldn't. What they often fail to realize is how much of what they post is searchable. Over time, Facebook has vastly improved how much information can be acquired, though it's still not great. Several years ago, there was a website, [www.weknowwhatyouredoing.com](http://www.weknowwhatyouredoing.com), that searched through Facebook posts that would fall into one of four categories—who wants to get fired, who is hungover, who is taking drugs, and who has a new phone number. Figure 4.4 shows some of the posts from the site before it got taken down because the API it used is no longer available.  
  

![The figure shows some of the posts from the site: http://www.weknowwhatyouredoing.com. On the left, 'Who's hungover?' text is shown. On the right, 'Who's taking drugs?' text with some posts is shown.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f004.png)

Figure 4.4: www.weknowwhatyouredoing.com

  
This is not to say that Facebook no longer has an API. The Facebook Graph API still exists. It just isn't as open as it once was. There is still a Graph API, and it can still be used in applications. In fact, Facebook provides a Graph API Explorer where queries to the API can be tested. Figure 4.5 shows the Graph API Explorer. Near the top, you can see there is an access token. This token is generated after a number of permissions are selected. The token is based on the user you are logged in as. Once you have a token, you can generate a query. The query shown is the default query, requesting the ID and name for the user. Of course, the name is mine since the access token was based on my login.  
  

![The figure shows a snapshot of the Facebook Graph API Explorer. At the top, an Access Token and the Get Token button are shown. Below it, a search bar and the Submit button are shown.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f005.png)

Figure 4.5: Facebook Graph API

  
You don't have to create an application, though, to generate searches. Searches can be done manually. Facebook is not only used by individuals. It is also, often, used by companies. Many companies create their own page where they can post specifics about their products and services. This is another location where you can gather information about the company. Figure 4.6 shows business details about John Wiley & Sons from its own page on Facebook. Besides the information you can see about its location, there are several other categories of information, such as Reviews, Posts, and Community. The reviews can sometimes provide enlightening information, and of course, there is the contact information provided.  
  

![The figure shows a snapshot of the About page of John Wiley & Sons on Facebook. FIND US, HOURS, PAGE INFO, ADDITIONAL CONTACT INFO, and MORE INFO details are displayed on this page.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f006.png)

Figure 4.6: John Wiley & Sons information

  
You don't have to rely on just looking up business information on Facecourse, though. People regularly post details about their employers on their personal pages. Unfortunately, this is where searching on Facebook can become challenging. You can't, after all, just search for all employees of a particular company using the usual search. However, if you have found some names of employees using other means, you can use those names to find their pages and read their status posts. Often, people will include details of their work situation—companies they do work for and have worked for—as part of their profile. This can help you to better distinguish the employees from other people with the same name.  
  
Much of this relies on how privacy options are set. Often, privacy options are set to public, which means you can probably read the posts of a lot of people you are looking for. You can also likely look at their photos. In an age of social media and the expectation that you can find out just about anything you want about someone, people often don't think about who can potentially see their posts and photos. This gives us an advantage. However, it isn't always the case that you will be able to see what someone is doing and saying. Figure 4.7 shows the different privacy settings available to Facebook users. One of the challenges with this, though, is that your settings only pertain to what you do. Your settings won't prevent other people from sharing one of your posts or photos, which may allow someone beyond who you would like to see the post or photo. Then it comes down to what their permissions are set to.  
  

![The figure shows a snapshot of different privacy settings available to Facebook users. The Privacy checkup heading is shown at the top. Below are options, such as Who can see what you share, How to keep your account secure, How people can find you on Facebook, Your data settings on Facebook, and Your ad preferences on Facebook.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f007.png)

Figure 4.7: Facebook permissions settings

  
Sometimes, people will post a status about their job, including what they may have been doing that day, or they may check in at another job site. Any of this information could potentially be useful to you. However, sites like Facebook are not always the best place to get information about businesses and their employees. There are other social networking sites that can be a bit more productive for you.  
  
**Username Search**  
Credentials are often used by attackers to gain access to resources they wouldn't otherwise have access to. Once you have identified people, getting their usernames may end up being useful. The tool Sherlock may be beneficial to identify people and their usernames. This is a program installed by default on ParrotOS. You provide a list of usernames you want to look for and it searches across hundreds of social networks for those usernames. Of course, finding a username is no guarantee that it is the person you are looking for. Knowing a username, though, does give you help with possible login attacks. Running Sherlock looks like this, having provided three different usernames and indicating a directory name to put the text files that will contain the output. This is a sample of the output from running this command.  
  
**Using Sherlock**  
  

$ sherlock --fo smithsearch jsmith smith johnsmith  
[*] Checking username jsmith on:  
[+] 7Cups: https://www.7cups.com/@jsmith  
[+] 9GAG: https://www.9gag.com/u/jsmith  
[+] About.me: https://about.me/jsmith  
[+] Academia.edu: https://independent.academia.edu/jsmith  
[+] Airbit: https://airbit.com/jsmith  
[+] Airliners: https://www.airliners.net/user/jsmith/profile/photos  
[+] AllMyLinks: https://allmylinks.com/jsmith  
[+] Apple Developer: https://developer.apple.com/forums/profile/jsmith  
[+] Apple Discussions: https://discussions.apple.com/profile/jsmith  
[+] Archive of Our Own: https://archiveofourown.org/users/jsmith  
[+] Archive.org: https://archive.org/details/@jsmith  
[+] Arduino: https://create.arduino.cc/projecthub/jsmith  
[+] Asciinema: https://asciinema.org/~jsmith  
[+] AskFM: https://ask.fm/jsmith  
[+] Audiojungle: https://audiojungle.net/user/jsmith  
[+] BLIP.fm: https://blip.fm/jsmith  
[+] Behance: https://www.behance.net/jsmith  
[+] BiggerPockets: https://www.biggerpockets.com/users/jsmith  
[+] Bikemap: https://www.bikemap.net/en/u/jsmith/routes/created/

  
**LinkedIn**  
LinkedIn has been around for about about two decades as of this writing. In spite of a number of competitors (such as Plaxo) going out of business or just no longer being in the space, LinkedIn is still around. It continues to expand its offerings beyond the very early days, when it was basically a contact manager. These days, LinkedIn is a business networking opportunity, highly useful for those in sales. It is also a great source for identifying jobs you may be interested in. It seems like human resources people, specifically recruiters, commonly use LinkedIn to find people to fill positions, whether it's an internal recruiter or someone who works for a recruiting company.  
  
Because of the amount of business information LinkedIn collects, we can make use of it as a hunting platform. Especially with the paid memberships, LinkedIn does a lot of analytics about the businesses that make use of it, as well as the people who are on the site. If you are looking for information about a target, LinkedIn provides a lot of detail. Just as one example, Figure 4.8 shows some statistics about applicants for a position that is open and advertised through the Jobs section of the website. This information requires a Premium subscription. We can see that the position attracts educated applicants. You can start to get a sense of the workforce by looking at these statistics across multiple positions.  
  

![The figure shows a snapshot of LinkedIn job statistics. At the top left, the Seniority level is shown with different applicants in descending order as 98 Director level applicants, 76 Senior level applicants, 38 Manager level applicants, and 37 VP level applicants. At the top right, Education is shown with the following data: 50% have a Bachelors's Degree, 31% have a Master of Business Administration, 11% have a Master's Degree, and 8% have other degrees. Below it, the Location section is shown with the following data: Greater Denver Area (182 applicants), Greater New York City Area (6-10 applicants), and United States, Other Areas (6-10 applicants).](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f008.png)

Figure 4.8: LinkedIn job statistics

  
Job listings are another place to look for details beyond the statistics about applicants and the workforce overall. What we can see from these listings is technology that may be in place within the organization. A listing for a network security engineer has requirements as shown in Figure 4.9. While some of these requirements are generic, there are also some that are very specific about the technology in use in the network. For example, it appears that the company may be using Check Point on their site and Palo Alto Networks firewalls. Additionally, it has Cisco routers and switches in its network. This is a foothold. We can now start thinking about how to attack that sort of infrastructure. Cisco devices, for sure, have tools that can be run against them for various auditing and attack purposes.  
  

![The figure shows the job requirements for a network security engineer, which are listed as At least 5 years of experience with administration and implementation of IT security infrastructure(mainly CheckPoint, PaloAlto, ForeScout, ISA/TMG, F5); At least 5 years of experience with networking mainly Cisco switches, routers, Wifi as well as F5 load balancers; Knowledge of AWS/ Azure /Google /365 cloud infrastructure is recommended; Knowledge of networking protocols such as TCP/IP,BGP,OSPF,IPSEC, etc.; A deep understanding of monitoring systems and procedures (SolarWinds Orian, PRTG, SNMP); Knowledge of authentication protocols such as OAuth, SAML, RADIUS, etc.; and Unified communication knowledge - an advantage.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f009.png)

Figure 4.9: Job requirements for a network security engineer

  
We don't have to limit ourselves to the web interface, though, since it can be tedious to keep typing things and flicking through pages. Sometimes you want to be able to script a search without writing a program or manually using a web page. Formerly, there was a tool called InSpy that worked well for searching through LinkedIn. Unfortunately, the latest version of InSpy available on Kali, 3.0.0, generates 404 errors trying to search for details. The instructions also require the use of an API key. With a little work, though, you can use another tool to do the LinkedIn searching for you.  
  
Beyond jobs, though, we can use LinkedIn to harvest information about people. There are a couple of reasons to look up people. One is just to get some names and titles that you may use later. Another is that even if job descriptions don't have information about technology, when people post their job responsibilities, they very often do. You can also get details about their certifications. Someone with a load of Cisco certifications, for example, is probably employed at a company that has a lot of Cisco equipment. Don't overlook nontechnical roles either. Anyone may provide a little insight into what you may find in the organization. You may get information about telephone systems and document management systems that the company uses, even if the employee isn't an administrator.  
  
We will take a look at a tool called crosslinked.py here to do some searching for us.  
  
**CrossLinked Results from an Employee Search**  
  

$ git clone https://github.com/m8r0wn/crosslinked  
  
Cloning into 'crosslinked'…  
remote: Enumerating objects: 166, done.  
remote: Counting objects: 100% (27/27), done.  
remote: Compressing objects: 100% (25/25), done.  
remote: Total 166 (delta 15), reused 4 (delta 2), pack-reused 139  
Receiving objects: 100% (166/166), 66.85 KiB | 2.48 MiB/s, done.  
Resolving deltas: 100% (72/72), done.  
$ pip3 install -r requirements.txt  
$ python3 crosslinked.py Wiley -f wiley.com\flast --search google  
  
     _____                    _             _            _  
    /  __ \                  | |   (x)     | |          | |  
    | /  \/_ __ ___  ___ ___ | |    _ _ __ | | _____  __| |  
    | |   | '__/ _ \/ __/ __|| |   | | '_ \| |/ / _ \/ _` |  
    | \__/\ | | (_) \__ \__ \| |___| | | | |   <  __/ (_| | @m8sec  
     \____/_|  \___/|___/___/\_____/_|_| |_|_|\_\___|\__,_| v0.2.1  
  
     
[*] Searching google for valid employee names at "Wiley"  
[*] 99 https://www.google.com/search?q=site:linkedin.com/in+"Wiley"&num=100&start=0 (200)  
[*] 198 https://www.google.com/search?q=site:linkedin.com/in+"Wiley"&num=100&start=99 (200)  
[*] 297 https://www.google.com/search?q=site:linkedin.com/in+"Wiley"&num=100&start=198 (200)  
[*] 298 https://www.google.com/search?q=site:linkedin.com/in+"Wiley"&num=100&start=297 (200)  
[*] 298 https://www.google.com/search?q=site:linkedin.com/in+"Wiley"&num=100&start=298 (200)  
[*] 298 https://www.google.com/search?q=site:linkedin.com/in+"Wiley"&num=100&start=298 (200)  
[*] 298 https://www.google.com/search?q=site:linkedin.com/in+"Wiley"&num=100&start=298 (200)  
[*] 298 names collected  
  

  
The results of the search are placed in a comma‐separated values file named names.csv. This is not a perfect utility, however, since it is not searching for employees at Wiley. Instead, you are getting everything that may have Wiley as part of the results, including people named Wiley. This includes people like a consultant named Calvin Wiley.  
  
**Twitter**  
Another common social networking site or service is Twitter. There is a lot posted to Twitter that can be of some use. Again, programmatic access to Twitter is useful. You can search using the regular interface, but it's sometimes helpful to be able to use other tools. To gain access to Twitter programmatically, you need to get an API key. This means you need to tell Twitter you are creating an application. You can easily tell Twitter you are creating an application without any special credentials through the Twitter developer's website. When you go through the process to create an application, what you are doing is creating identification information that your app needs to interact with the Twitter service. In Figure 4.10, you can see keys and access tokens for an app.  
  

![The figure shows the recon-ng-me app. The Details, Settings, Keys and Access Tokens, and Permissions tabs are shown at the top, where the Keys and Access Tokens tab is selected. On the selected tab, Application Settings are shown with the following information: Consumer Key (API Key), Consumer Secret (API Secret), Access Level, Owner, and Owner ID. At the bottom, Application Actions are shown with Regenerate Consumer Key and Secret and Change App Permissions buttons.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f010.png)

Figure 4.10: Twitter keys and access tokens

  
What you may notice in Figure 4.10 is that the app is named recon‐ng‐me. The reason for this is that I created the app just to get the key and token so I could add it into recon‐ng, a tool used for reconnaissance that includes many plugins. Some of these plugins require API keys or access tokens to be able to interact with the service being queried. That's the case with the Twitter plugin. In the following code, you can see the list of API keys that recon‐ng uses and the API keys set for Twitter. Before you can use the key, you will need to get the module out of the marketplace in recon‐ng using `marketplace install recon/profiles‐profiles/twitter_mentions`.  
  
**recon‐ng Keys**  
  

[recon-ng][default]> keys list  
  
+———————————————————————––––––––––––––––––––––––––––––––––––––––––––––––+  
|       Name       |                       Value                        |  
+—————————————————––––––––––––––––––––––––––––––––––––––––––––––––––––––+  
| bing_api         |                                                    |  
| builtwith_api    |                                                    |  
| censysio_id      |                                                    |  
| censysio_secret  |                                                    |  
| flickr_api       |                                                    |  
| fullcontact_api  |                                                    |  
| github_api       |                                                    |  
| google_api       |                                                    |  
| google_cse       |                                                    |  
| hashes_api       |                                                    |  
| ipinfodb_api     |                                                    |  
| jigsaw_api       |                                                    |  
| jigsaw_password  |                                                    |  
| jigsaw_username  |                                                    |  
| pwnedlist_api    |                                                    |  
| pwnedlist_iv     |                                                    |  
| pwnedlist_secret |                                                    |  
| shodan_api       |                                                    |  
| twitter_api      | 0DE6bQv89M2AApxCvzfX7AIpd                          |  
| twitter_secret   | jxhcaFu9FS8AK9g4m6N9OrhkuCQoP6A5ppgSdckOIf3zhD3cMK |  
+—————————————————————–––––––––––––––––––––––––——–––––––––––––––––––––––+

  
Now that we have the API key in place for Twitter, we can run the module. To run the module, we have to “load” it, meaning we load the module with the `modules load` command. Once it's loaded, we have to set a source. In our case, the source is a text string, so it's in quotes, telling recon‐ng that we are using a text string for the source. The text string expected here is a user handle. The word _recon_ was selected somewhat randomly and it got results. Once that's done, all we need to do is run the module. You can see loading the module, setting the source, and running it in the following code. The results from the module are truncated because there were quite a few of them, and this is just to show you how to use the module.  
  
**Using Twitter Module in recon‐ng**  
  

[recon-ng][default]> modules load recon/profiles-profiles/twitter_mentions  
[recon-ng][default][twitter_mentions]> options list  
  
  Name    Current Value  Required  Description  
  ------  -------------  --------  -----------  
  LIMIT   True           yes       toggle rate limiting  
  SOURCE  default        yes       source of input (see 'show info' for details)  
  
[recon-ng][default][twitter_mentions]> options set SOURCE ‘recon’  
SOURCE => ‘recon’  
[recon-ng][default][twitter_mentions]> run  
  
‘RECON’  
[*] Category: social  
[*] Notes: Pentester Academy  
[*] Resource: Twitter  
[*] Url: https://twitter.com/SecurityTube  
[*] Username: SecurityTube  
[*] --------------------------------------------------  
[*] Category: social  
[*] Notes: Pentester Academy  
[*] Resource: Twitter  
[*] Url: https://twitter.com/SecurityTube  
[*] Username: SecurityTube  
[*] --------------------------------------------------  
[*] Category: social  
[*] Notes: Tony Lazzari  
[*] Resource: Twitter  
[*] Url: https://twitter.com/TonyLazzariXXX  
[*] Username: TonyLazzariXXX  
[*] --------------------------------------------------  
[*] Category: social  
[*] Notes: Michael Lee  
[*] Resource: Twitter  
[*] Url: https://twitter.com/jocko2001  
[*] Username: jocko2001  
[*] --------------------------------------------------  
[*] Category: social  
[*] Notes: Josh Morgerman  
[*] Resource: Twitter  
[*] Url: https://twitter.com/iCyclone  
[*] Username: iCyclone  
[*] --------------------------------------------------  
[*] Category: social  
[*] Notes: JBC Trader  
[*] Resource: Twitter  
[*] Url: https://twitter.com/JbcTrader  
[*] Username: JbcTrader  
[*] --------------------------------------------------  
[*] Category: social  
[*] Notes: wx_fitz2024  
[*] Resource: Twitter  
[*] Url: https://twitter.com/wx_fitz2024  
[*] Username: wx_fitz2024  
[*] --------------------------------------------------

  
Because we are running the twitter‐mentions module, we are using the text string to search for mentions in Twitter. What we get back are profiles from users that were mentioned by a given handle. You could do the reverse of these results with the twitter_mentioned module, which returns profiles that mentioned the specified handle. Finally, we can look for tweets that happened in a given geographic area using the locations‐pushpin/twitter module. We can specify a radius in kilometers within which we want to search using this module.  
  
There is another tool that's useful for reconnaissance overall, but since it has Twitter abilities, we'll take a look at it here. Maltego uses a visual approach by creating graphs from the data that has been collected. It can be useful to have entity relationships identified, like parent‐child relationships between pieces of data. Maltego uses transforms to pivot from one piece of information to another. A collection of transforms is called a machine, and it's a good place to start. Figure 4.11 shows part of the output from Twitter Digger X, which analyzes tweets from the username provided. As you can see, you get a graph that is structured like a tree. This is because every piece of data collected can potentially yield another piece, which would be a child.  
  
Maltego can be a good tool to collect reconnaissance data, especially if you want a visual representation of it. While there are several other tools that can collect the same data Maltego does, Maltego does have the advantage of giving you a quick way to collect additional data by just selecting a node on the graph and running a transform on it to pivot to another data collection tool. You can start with a hostname, for instance, and collect an IP address from it by just running a transform.  
  

![The figure shows the Maltego graph from Twitter. It shows part of the output from Twitter Digger X, which analyzes tweets from the username provided. The graph is structured like a tree.](https://s3.amazonaws.com/jigyaasa_content_static/cehv11img/c04f011.png)

Figure 4.11: Maltego graph from Twitter

  
There are additional Twitter machines and transforms aside from Twitter Digger X. You can also monitor Twitter for the use of hashtags, for example. This will provide a lot of capability, in addition to all of the other capabilities, to search Twitter from inside Maltego.  
  

Hands‐on Activity

Obtain a copy of Maltego Community Edition. It will require you to register to use it. It can be installed from the repositories on Kali Linux and ParrotOS. Use Maltego to locate as much information about yourself as you can using the machines and transforms available.

  
**Job Sites**  
Earlier we looked at LinkedIn as a source of information. One area of information we were able to gather from LinkedIn was job descriptions, leading to some insights about what technology is being used at some organizations. For example, Figure 4.12 shows some qualifications for an open position. This is for a senior DevOps engineer, and the listing was on [indeed.com](http://indeed.com). While the technologies are listed as examples, you certainly have some starting points. You know the company is using relational databases. This isn't surprising, perhaps, since so many companies are using them. It does tell you, though, that they aren't using NoSQL, which includes things like MongoDB and Redis. You also know that they are using Amazon Web Services. Since they are looking for someone certified there, this is a certainty.  
  

![The figure shows a snapshot illustrating some qualifications for an open position. The Preferred Qualifications are as follows: Amazon AWS Certified DevOps Engineer - Professional Certification; Experience with Chef, Puppet, Salt, or Ansible in production environments; Strong scripting skills, i.e., Powershell, Python, Bash, Ruby, etc; Experience with application containerization and orchestration frameworks, i.e., Docker, AWS ECS, Kubernetes; Familiarity with relational database technologies - Oracle, Postgres, MySQL, Aurora; Management of continuous code integration services and tools like Jenkins, Bamboo, TeamCity, and AWS Code Tools; Experience with automated testing tools like Selenium and JMeter; Understanding of Service-Oriented Architecture and REST APIs; Experience building enterprise security strategies for cloud adoption; and Experience leading or working on certification or accreditation of cloud workload(s) to meet industry or regulatory standards such as PCI DSS, ISO 27001, HIPAA, and NIST/DoD frameworks.](https://s3.amazonaws.com/jigyaasa_content_static/SybexCEH1/c04f013.png)

Figure 4.12: Job listing with technologies

  
If you wanted to try to do something through the web application, you know they are using RESTful interfaces for the application. It's not much, but it's a starting point. As you look over job listings, you start to be able to read them with an eye toward picking out potential targets. After you've been reading them for a while, you will start to pick out some of the language of these listings. As an example, the listing uses the word _like_ in several of the lines. While you can't get a complete line on what is used, you can certainly rule some things out.  
  
There are a lot of places to go looking for job listings. While in the old days, we used newspapers, and you'd have to get a newspaper in the region you wanted to look for a job in (or scare up information about a company you were trying to research), now job postings are everywhere online. Some of the big websites you might use are Monster, Indeed, Glassdoor, CareerBuilder, and Dice. You should also keep the company itself in mind. While many companies use the job posting sites, there may be some companies that post jobs on their own site, and they may not be available elsewhere. You may also check specialized sites like USAJobs and ClearanceJobs.  
  
Any of these job postings may provide you with some insight into not only technology but also organizational structure. As I mentioned earlier, don't focus only on technology listings. You can gather additional information about the company using other job listings that aren't about technology.

