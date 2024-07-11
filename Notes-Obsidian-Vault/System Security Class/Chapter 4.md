## Table of Contents

- [Cybersecurity and Networking Test Cheat Sheet](#cybersecurity\and\networking\test\cheat\sheet)
  - [p0f - Passive Traffic Analysis Tool](#p0f\-\Passive\Traffic\Analysis\Tool)
  - [Regional Internet Registries (RIRs)](#Regional\Internet\Registries\(RIRs))
  - [Financial Information and Filings](#Financial\Information\and\Filings)
  - [DNS Records](#DNS\Records)
  - [Online Tools for Information Gathering](#Online\Tools\for\Information\Gathering)
  - [Google Dork Queries](#Google\Dork\Queries)
  - [WHOIS Queries](#WHOIS\Queries)

The tool Sherlock is used for:

B. Looking for potential usernames
**Explanation:**
- **Sherlock**: This tool is designed to search for usernames across various social networks and websites. It's useful in cybersecurity and ethical hacking for reconnaissance, allowing researchers to find social media accounts and other online presences associated with a specific username. This can be part of an effort to gather information about individuals or entities during a security assessment or investigation. Sherlock is not primarily used for searching domain registrars, looking up job information, or searching for fingerprints.
---
> If you were looking for detailed financial information on a target company, the resource you would have the most success with is:
> **D. EDGAR**
> *Explanation:*
- **EDGAR (Electronic Data Gathering, Analysis, and Retrieval system)**: This is the system used by the U.S. Securities and Exchange Commission (SEC) for companies to submit their required filings. EDGAR is a primary source for detailed financial information on public companies, including annual and quarterly reports (10-K and 10-Q), as well as other filings that provide a comprehensive view of a company's financial performance and business operations. LinkedIn and Facebook are social networking platforms and generally do not provide detailed financial reports of companies.



# Cybersecurity and Networking Test Cheat Sheet

## p0f - Passive Traffic Analysis Tool
- **Operating System Detection**: Analyzes TCP/IP packet headers to determine OS versions.
- **Network Device Type**: Infers device types like routers, firewalls, or IoT devices.
- **Software Version Detection**: Determines versions of software, especially network stacks.
- **Connection Type**: Analyzes direct connections or through NAT/proxy.
- **Link Type Detection**: Infers network link types (Ethernet, DSL, OC3).
- **Network Distance**: Estimates number of hops between host and target.
- **Uptime Guessing**: Estimates system uptime using TCP timestamps.
- **Host Role in Network**: Identifies if a host is a client or server.
- **MTU Size**: Determines Maximum Transmission Unit size.
- **Fragmentation Detection**: Detects packet fragmentation.
- **Quirks in Packet Construction**: Identifies unusual packet behaviors.

## Regional Internet Registries (RIRs)
- **APNIC**: Asia-Pacific Network Information Centre.
- **RIPE NCC**: Réseaux IP Européens Network Coordination Centre.
- **AfriNIC**: African Network Information Centre.
- **LACNIC**: Latin America and Caribbean Network Information Centre.

## Financial Information and Filings
- **EDGAR**: Electronic Data Gathering, Analysis, and Retrieval system for U.S. SEC filings.

## DNS Records
- **CNAME (Canonical Name)**: Maps an alias name to a true domain name.
- **A (Address)**: Maps a domain name to its IPv4 address.
- **NS (Name Server)**: Specifies the name servers for a domain.

## Online Tools for Information Gathering
- **theHarvester**: Gathers emails, subdomains, hosts from various public sources.
- **Sherlock**: Searches for usernames across social networks and websites.
- **PeekYou**: People search engine for finding social media profiles, web links, public records.

## Google Dork Queries
- `site:pastebin.com intext:password.txt`: Searches for password files on Pastebin.

## WHOIS Queries
- **Expected Info**: Address block owner, technical contact, IP address block.
- **Not Expected**: Domain association.


