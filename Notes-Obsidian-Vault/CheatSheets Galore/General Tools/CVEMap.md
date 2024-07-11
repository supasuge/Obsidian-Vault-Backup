## Table of Contents

- [CVEMap Cheatsheet](#cvemap\cheatsheet)
  - [Description](#Description)
  - [Installation](#Installation)
  - [Usage Examples](#Usage\Examples)
    - [Basic Commands](#Basic\Commands)
    - [Configuring API Key](#Configuring\API\Key)
    - [Searching CVEs](#Searching\CVEs)
    - [Advanced Filtering](#Advanced\Filtering)
    - [Output Options](#Output\Options)
    - [Update and Debug](#Update\and\Debug)
  - [Additional Information](#Additional\Information)
  - [References](#References)

[ProjectDiscovery.io](https://projectdiscovery.io/)
[cvemap - github.com](https://github.com/projectdiscovery/cvemap)

# CVEMap Cheatsheet

## Description
CVEMap, developed by ProjectDiscovery, is a powerful command-line tool designed for cybersecurity professionals. It enables users to efficiently search and analyze Common Vulnerabilities and Exposures (CVEs) data. The tool offers a range of options to filter and display CVEs based on various criteria like ID, vendor, product, severity, and more.
___
## Installation
To install CVEMap, follow these steps:
1. Ensure you have Go installed on your system. If not, download and install it from [Go's official website](https://golang.org/dl/).
2. Install CVEMap using the following Go command:
   ```shell
   go install -v github.com/projectdiscovery/cvemap/cmd/cvemap@latest
   ```
3. Verify the installation by running `cvemap -version` in your terminal.
---
## Usage Examples
---
### Basic Commands
- **Help Menu**: Display help information about the tool.
  ```shell
  cvemap -h
  ```
---
### Configuring API Key
- **Set API Key**: Configure ProjectDiscovery Cloud API key.
  ```shell
  cvemap -auth <your_api_key>
  ```
---
### Searching CVEs
- **By CVE ID**: List CVEs for a given ID.
  ```shell
  cvemap -id CVE-2023-0001
  ```
- **By Vendor**: List CVEs for a specific vendor.
  ```shell
  cvemap -v Microsoft
  ```
- **By Product**: List CVEs for a specific product.
  ```shell
  cvemap -p Windows
  ```
- **By Severity**: List CVEs of a specific severity.
  ```shell
  cvemap -s high
  ```
- **By CVSS Score**: List CVEs for a given CVSS score.
  ```shell
  cvemap -cs 7.0
  ```
---
### Advanced Filtering
- **Exclude Products**: Exclude CVEs based on products.
  ```shell
  cvemap -eproduct Linux
  ```
- **Search in CVE Data**: Perform a search within CVE data.
  ```shell
  cvemap -q "remote code execution"
  ```
- **Limit Results**: Limit the number of results displayed.
  ```shell
  cvemap -l 10
  ```
---
### Output Options
- **JSON Format**: Return output in JSON format.
  ```shell
  cvemap -j
  ```
- **Select Fields**: Choose specific fields to display.
  ```shell
  cvemap -f age,template,poc
  ```
---
### Update and Debug
- **Update CVEMap**: Update the tool to the latest version.
  ```shell
  cvemap -up
  ```

- **Verbose Output**: Enable verbose output for debugging.
  ```shell
  cvemap -verbose
  ```
## Additional Information
---
- CVEMap is regularly updated with the latest CVE data, ensuring you have access to current vulnerability information.
- The tool's flexibility in filtering and output options makes it suitable for a variety of cybersecurity research and analysis tasks.
- Remember to regularly update CVEMap to keep up with new features and improvements.
This cheatsheet provides a concise overview of CVEMap's capabilities and usage. For more detailed information, refer to the official [ProjectDiscovery GitHub repository](https://github.com/projectdiscovery/cvemap).

## References
---
- **[National Vulnerability Database (NVD)](https://nvd.nist.gov/developers)**: Comprehensive CVE vulnerability data.
- **[Known Exploited Vulnerabilities Catalog (KEV)](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)**: Exploited vulnerabilities catalog.
- **[Exploit Prediction Scoring System (EPSS)](https://www.first.org/epss/data_stats)**: Exploit prediction scores.
- **[HackerOne](https://hackerone.com/hacktivity/cve_discovery)**: CVE discoveries disclosure.
- **[Nuclei Templates](https://github.com/projectdiscovery/nuclei-templates)**: Vulnerability validation templates.
- **[Live-Hack-CVE](https://github.com/Live-Hack-CVE/) / [PoC-in-GitHub](https://github.com/nomi-sec/PoC-in-GitHub/)** GitHub Repository: Vulnerability PoCs references.