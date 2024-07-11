## Table of Contents

  - [Subnetting Explained](#Subnetting\Explained)
    - [IP Addresses and Subnet Masks](#IP\Addresses\and\Subnet\Masks)
    - [Example](#Example)
    - [Subnetting Process](#Subnetting\Process)
    - [Detailed Example](#Detailed\Example)
    - [Benefits of Subnetting](#Benefits\of\Subnetting)
    - [Subnetting in Larger Networks](#Subnetting\in\Larger\Networks)

## Subnetting Explained

Subnetting is the practice of dividing a single network into smaller networks (subnets). It's done to improve network efficiency and security. This division is achieved by manipulating the subnet mask of the network.

### IP Addresses and Subnet Masks

- **IP Address**: An IP address is a unique identifier for a device on a network. It consists of two parts: the network part and the host part.
- **Subnet Mask**: A subnet mask separates the IP address into the network and host portions. The mask is a binary number that has all 1s in the network part and 0s in the host part.

### Example

Suppose you have an IP address `192.168.1.0` with a standard subnet mask of `255.255.255.0`. This notation is often written as `192.168.1.0/24`, where `/24` indicates that the first 24 bits are the network part of the address.

### Subnetting Process

1. **Determine the Number of Subnets**: Decide how many subnets you need. This is often based on the logical grouping of devices in your network.
2. **Subnet Mask Modification**: Alter the subnet mask to create the desired number of subnets. This is done by borrowing bits from the host part of the mask.

### Detailed Example

Let's subnet the `192.168.1.0/24` network into four subnets:

1. **Original Network**: `192.168.1.0` with a subnet mask of `255.255.255.0` (`/24`).
2. **Borrow Bits**: Borrow 2 bits from the host part for subnetting (since 2^2 = 4, which gives us four subnets).
3. **New Subnet Mask**: The new subnet mask becomes `255.255.255.192`. In binary, this is `11111111.11111111.11111111.11000000`, where the last two `00` bits are for hosts.
4. **Calculate Subnets**:
    - **Subnet 1**: `192.168.1.0` to `192.168.1.63`
    - **Subnet 2**: `192.168.1.64` to `192.168.1.127`
    - **Subnet 3**: `192.168.1.128` to `192.168.1.191`
    - **Subnet 4**: `192.168.1.192` to `192.168.1.255`

Each subnet now supports up to 62 hosts (64 addresses per subnet minus 2 for the network and broadcast addresses).

### Benefits of Subnetting

- **Improved Network Performance**: Reduces network congestion by segmenting traffic.
- **Enhanced Security**: Isolates parts of the network, limiting access and exposure.
- **Efficient IP Addressing**: More logical use of a limited number of IP addresses.

### Subnetting in Larger Networks

In larger networks, subnetting can become more complex, involving further divisions and varying subnet sizes based on organizational needs.