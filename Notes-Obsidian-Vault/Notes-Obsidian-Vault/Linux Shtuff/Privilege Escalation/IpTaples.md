## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [iptables Cheatsheet](#iptables\cheatsheet)
  - [Introduction](#Introduction)
    - [Basics](#Basics)
    - [Creating and Deleting Chains](#Creating\and\Deleting\Chains)
    - [Adding Rules](#Adding\Rules)
    - [Advanced Rules](#Advanced\Rules)
    - [Special Cases](#Special\Cases)
    - [Chain Management](#Chain\Management)
    - [Monitoring and Logging](#Monitoring\and\Logging)
    - [iptables with IPv6 (`ip6tables`)](#iptables\with\IPv6\(`ip6tables`))
    - [Best Practices](#Best\Practices)
    - [Note](#Note)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [iptables Cheatsheet](#iptables\cheatsheet)
  - [Introduction](#Introduction)
    - [Basics](#Basics)
    - [Creating and Deleting Chains](#Creating\and\Deleting\Chains)
    - [Adding Rules](#Adding\Rules)
    - [Advanced Rules](#Advanced\Rules)
    - [Special Cases](#Special\Cases)
    - [Chain Management](#Chain\Management)
    - [Monitoring and Logging](#Monitoring\and\Logging)
    - [iptables with IPv6 (`ip6tables`)](#iptables\with\IPv6\(`ip6tables`))
    - [Best Practices](#Best\Practices)
    - [Note](#Note)

# iptables Cheatsheet

## Introduction

`iptables` is a user-space utility program that allows a system administrator to configure the IP packet filter rules of the Linux kernel firewall, implemented as different Netfilter modules. Below is a comprehensive cheatsheet for `iptables` usage.

### Basics

- **List all Rules**: `iptables -L -n -v`
    
    - `-L`: List rules
    - `-n`: Numeric output (don't resolve names)
    - `-v`: Verbose (more detailed output)
- **Set Default Policy**:
    
    - `iptables -P INPUT DROP` (Drop all incoming packets by default)
    - `iptables -P FORWARD DROP` (Drop all forwarded packets by default)
    - `iptables -P OUTPUT ACCEPT` (Accept all outgoing packets by default)
- **Delete All Existing Rules**: `iptables -F`
    
    - `-F`: Flush (delete) all rules
- **Save iptables Rules**:
    
    - `iptables-save > /path/to/iptables.rules`
    - To restore: `iptables-restore < /path/to/iptables.rules`

### Creating and Deleting Chains

- **Create New Chain**: `iptables -N MY_CHAIN`
- **Delete Chain**: `iptables -X MY_CHAIN`

### Adding Rules

- **Allow/Deny Specific IP Address**:
    
    - Allow: `iptables -A INPUT -s 192.168.1.2 -j ACCEPT`
    - Deny: `iptables -A INPUT -s 192.168.1.2 -j DROP`
- **Allow/Deny Specific Port**:
    
    - Allow: `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`
    - Deny: `iptables -A INPUT -p tcp --dport 22 -j DROP`
- **Allow Loopback Access**: `iptables -A INPUT -i lo -j ACCEPT`
    

### Advanced Rules

- **Stateful Firewall Rules**:
    
    - Allow Established and Related Connections: `iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT`
    - Drop Invalid Packets: `iptables -A INPUT -m state --state INVALID -j DROP`
- **Port Forwarding**:
    
    - `iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080`
- **Log Dropped Packets**:
    
    - `iptables -A INPUT -j LOG --log-prefix "IPTables-Dropped: " --log-level 4`

### Special Cases

- **Prevent DoS Attack**:
    
    - `iptables -A INPUT -p tcp --dport 80 -m limit --limit 25/minute --limit-burst 100 -j ACCEPT`
- **Block Null Packets**:
    
    - `iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP`
- **Syn-Flood Protection**:
    
    - `iptables -A INPUT -p tcp ! --syn -m state --state NEW -j DROP`
- **XMAS Packets**:
    
    - `iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP`

### Chain Management

- **Insert Rule at Specific Position**: `iptables -I INPUT 2 -s 192.168.1.2 -j DROP`
- **Replace Rule at Specific Position**: `iptables -R INPUT 2 -s 192.168.1.2 -j ACCEPT`
- **Delete Rule by Specification**: `iptables -D INPUT -s 192.168.1.2 -j DROP`
- **Delete Rule by Position**: `iptables -D INPUT 2`

### Monitoring and Logging

- **Packet and Byte Counters**:
    
    - Show: `iptables -L -v`
    - Zero counters: `iptables -Z`
- **Logging**:
    
    - `iptables -A INPUT -j LOG --log-prefix "INPUT DROP: " --log-level info`

### iptables with IPv6 (`ip6tables`)

- `ip6tables` are used similarly to `iptables` but for IPv6 traffic.
    - Example: `ip6tables -A INPUT -p tcp --dport 80 -j ACCEPT`

### Best Practices

- Always backup current rules before making changes.
- Test new rules with `-j LOG` before applying.
- Be cautious with default DROP policies to avoid locking yourself out.
- Apply changes incrementally and test as you go.

### Note

- iptables rules are not persistent across reboots by default. Use `iptables-save` and `iptables-restore` or other mechanisms depending on your distribution (like `netfilter-persistent` on Debian-based systems) for persistence.
- Always ensure that you have physical or out-of-band access to the server when configuring iptables remotely, in case you get locked out due to misconfiguration.