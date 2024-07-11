**Writing a good report**:
- Descriptive title - Something that explains vuln in a sentence or less
- Clear summary 
- Include Severity/Impacts Assessment
- Give clear steps to reproduce
- Provide a Proof of Concept - screenshots, videos, code snippets/documentation.
- Describe the Impact and Attack Scenarios
- Recommend Possible Mitigations - Don't give recommendations if you aren't 100% sure.
- Validate the Report - Always double check the quality of the report by going over it multiple times.
	- Be clear and concise, and professional.


 **Taking Notes**
- Weird behaviors, new features, misconfigurations, minor bugs, suspicious endpoints etc.
- Progress, the features you've covered and any other's that still need to be checked.
- Jot down information about each vulnerability you learn about.
- Organize yourself and your notes.


###### Reconnaissance
- Google Dorking
	- `site`, `insite`, `inurl`, `intitle`, `link`, `filetype`, `Wildcard(*)`, `Quotes("")`, `Or(|)`, `minues(-)`
	- Look for special extensions: `.php`, `cfm`, `asp`, `.jsp`, `.pl`.
- `whois` and Reverse `whois`: Domain registrar information.
- `IP Addresses`: `nslookup`. Once you find an IP, perform a Reverse IP search. Use `ViewDNS.info`. Also run the `whois` command on it.
**Third Party Hosting**: `awscli`, s3 buckets etc.
- [lazys3](https://github.com/nahamsec/lazys3)

#### Github Recon
Search for any possible public repo's from the organization.
- start with relevant usernames
**Tools**: [gitrob](https://github.com/michenriksen/gitrob/), [TruffleHog](https://github.com/trufflesecurity/truffleHog), [pastehunter(https://github.com/kevthehermit/PasteHunter)
[waybackurls](https://github.com/tomnomnom/waybackurls)


- Check job listings

###### Tech stack fingerprinting
- nmap scanning
- checking heades
- source code
- Dynamic/Static site functionality
- comments
- Certificate enumeration
- Retire.js
- BuiltWith
- Wappalyzer
- StackShare

###### Recon APIs
- Shodan
- Censys
- recon-ng




