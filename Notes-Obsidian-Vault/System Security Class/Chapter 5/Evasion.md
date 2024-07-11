## Table of Contents

    - [Hide/Obscure the Data](#Hide/Obscure\the\Data)
    - [Alterations (Polymorphism)](#Alterations\(Polymorphism))
    - [Fragmentation](#Fragmentation)
    - [Overlaps](#Overlaps)
    - [Malformed Data](#Malformed\Data)
    - [Low and Slow Attacks](#Low\and\Slow\Attacks)
    - [Resource Consumption](#Resource\Consumption)
    - [Screen Blindness](#Screen\Blindness)
    - [Tunneling](#Tunneling)
    - [Evasion with nmap](#Evasion\with\nmap)

Evasion techniques are critical for penetration testers to bypass network security measures like firewalls, intrusion detection systems (IDS), and intrusion prevention systems (IPS). These techniques help in conducting security assessments without being detected or blocked. Understanding and applying these techniques can make the difference between a successful penetration test and one that is thwarted early on. Below are detailed explanations and examples of various evasion techniques, including real-world application scenarios and how they can be implemented with tools like `nmap`.
### Hide/Obscure the Data
**Encryption or Obfuscation**: Data encryption is a fundamental method to prevent inspection by network security devices. By encrypting the payload, you ensure that intermediate devices cannot inspect the content.
**Real-World Use Case**: Use TLS for your communication to ensure that data is encrypted end-to-end. For obfuscation, consider encoding payloads in Base64 or implementing custom encoding schemes to disguise the data.
### Alterations (Polymorphism)
**Polymorphic Malware**: By altering the malware code slightly for each distribution, the hash signature changes, making it more difficult for signature-based detection systems to identify the threat.
**Real-World Use Case**: Modify the malware payload for each target system to avoid detection by signature-based IDS/IPS. This requires understanding of malware coding and signature generation mechanisms.
### Fragmentation
**Fragmentation Attacks**: Splitting malicious payloads into smaller fragments can evade detection since some security devices may not reassemble packets to inspect the complete payload.
**Implementation with `fragroute`**: Use `fragroute` to split packets into smaller sizes, forcing IDS/IPS to work harder to reassemble and inspect packets, which can either lead to evasion or resource exhaustion.
### Overlaps
**Sequence Number Overlapping**: Crafting packets with overlapping TCP sequence numbers can confuse the target's operating system and security devices, leading to potential evasion of security checks.
**Real-World Use Case**: Manually crafting packets or using tools that allow manipulation of TCP sequence numbers to create overlapping segments can cause mismatches in packet reassembly, potentially allowing malicious packets to be processed by the target system unnoticed.
### Malformed Data
**Protocol Anomalies**: Using unexpected protocol behavior, such as TCP flags that shouldn't be set together, can sometimes bypass security measures that don't handle anomalies well.
**Implementation with `nmap`**: Utilize `nmap`'s Xmas, FIN, and NULL scans to send packets that don't conform to expected patterns, potentially bypassing firewall rules.
### Low and Slow Attacks
**Throttled Scanning**: Conducting scans or attacks over extended periods reduces the chance of detection by blending in with normal traffic or evading threshold-based detection.
**Implementation with `nmap`**: Use `nmap`'s `-T` (timing) option to control the speed of the scan, setting it to a lower value to make the scan less conspicuous.
### Resource Consumption
**Denial of Service (DoS)**: Overwhelming security devices with a flood of traffic can cause them to fail into a "fail-open" state, allowing subsequent malicious traffic to pass through unchecked.
**Real-World Use Case**: While not recommended due to its disruptive nature and ethical considerations, understanding how DoS attacks can impact security devices is crucial for defense.
### Screen Blindness
**Alert Flooding**: Generating a high volume of benign alerts can desensitize security monitoring, making it more likely for malicious activities to go unnoticed.
**Real-World Use Case**: Conducting a wide range of innocuous network activities that generate alerts to mask the actual malicious traffic. This requires careful planning to avoid causing actual harm or overwhelming network resources.
### Tunneling
**Protocol Tunneling**: Encapsulating malicious traffic within allowed protocols can bypass content inspection by security devices.
**Implementation**: Use SSH, DNS, or HTTP(S) tunneling to encapsulate malicious payloads, making the traffic appear as legitimate communication. Tools like `ssh` for SSH tunneling, or custom scripts for DNS/HTTP(S) tunneling, can be utilized.
### Evasion with nmap
`nmap` offers built-in features for evasion, such as decoy scanning, fragmentation, and spoofing techniques, making it a versatile tool for penetration testing.
**Decoy Scanning**: Use `nmap`'s `-D` option to include decoy IP addresses in the scan, confusing defenders about the scan's origin.

```bash
nmap -D RND:10 [target]
```

**Fragmentation**: The `-f` and `--mtu` options allow `nmap` to fragment packets, potentially bypassing packet inspection.

```bash
nmap -f --mtu 24 [target]
```

**Spoof MAC Address**: `nmap` can spoof the MAC address with `--spoof-mac` to bypass MAC address filters.

```bash
nmap --spoof-mac [MAC or Vendor] [target]
```

