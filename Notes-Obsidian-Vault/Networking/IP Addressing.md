## Table of Contents

- [IP Addressing Cheatsheet](#ip\addressing\cheatsheet)
  - [Loopback Addresses](#Loopback\Addresses)
  - [Subnetting & Supernetting](#Subnetting\&\Supernetting)
  - [Public/Private IP Addresses](#Public/Private\IP\Addresses)
  - [Reasons for Allocation Strategy](#Reasons\for\Allocation\Strategy)

# IP Addressing Cheatsheet

## Loopback Addresses
- **Definition**: Reserved IP address that loops back to the host's network interface.
- **IPv4 Range**: `127.0.0.0` to `127.255.255.255`.
- **IPv6 Address**: `::1`.
- **Purpose**: For testing network software, diagnostics, and inter-process communication.
- **Characteristics**:
  - Does not represent physical hardware.
  - Used for IPC and network stack testing.
  - Does not leave the host or require network access.

## Subnetting & Supernetting
- **Subnetting**:
  - **Process**: Dividing a larger network into smaller, efficient subnetworks.
  - **Method**: Altering the subnet mask to increase the network portion of the IP address.
  - **CIDR**: Allows variable-length subnet masking for flexibility.
  - **Use Cases**: Enhancing network security, improving management, and reducing congestion.
- **Supernetting**:
  - **Process**: Combining smaller networks into a larger network.
  - **CIDR Aggregation**: Used for routing efficiency.
  - **Impact**: Simplifies routing tables and reduces network complexity.

## Public/Private IP Addresses
- **Public IPs**:
  - **Uniqueness**: Globally unique.
  - **Allocation**: Managed by IANA and distributed through RIRs.
  - **Usage**: For devices needing direct internet access.
- **Private IPs**:
  - **Scope**: Unique within a private network.
  - **Defined In**: RFC 1918 (IPv4), RFC 4193 (IPv6).
  - **Usage**: For internal network devices, protected by NAT for internet access.
- **NAT (Network Address Translation)**:
  - **Role**: Translates private IPs to a public IP for external communication.
  - **Importance**: Conserves public IPv4 addresses, adds a layer of security.

## Reasons for Allocation Strategy
- **Global Uniqueness**: Ensures each device on the internet can be uniquely identified.
- **IP Conservation**: Private IPs and NAT delay IPv4 exhaustion.
- **Network Security**: Private addressing isolates internal networks from the internet.
- **Efficient Network Management**: Allows structured traffic routing and network control.
