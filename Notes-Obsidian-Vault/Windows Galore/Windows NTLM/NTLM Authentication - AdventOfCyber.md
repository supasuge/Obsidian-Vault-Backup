## Table of Contents

- [NTLM Authentication](#ntlm\authentication)
    - [File Shares](#File\Shares)
      - [NetNTLM/NTLM Authentication](#NetNTLM/NTLM\Authentication)
      - [Responding to the Race](#Responding\to\the\Race)
- [NTLM Authentication and Responder Tool](#ntlm\authentication\and\responder\tool)
  - [NTLM Authentication Overview](#NTLM\Authentication\Overview)
    - [Key Points](#Key\Points)
  - [NetNTLM/NTLM Authentication Process](#NetNTLM/NTLM\Authentication\Process)
    - [Authentication Mechanism](#Authentication\Mechanism)
  - [Potential Security Threats](#Potential\Security\Threats)
  - [Responder Tool](#Responder\Tool)
    - [Responder in Action](#Responder\in\Action)

# NTLM Authentication
- File shares are often used on servers and workstation connected to an AD domain.
	- Easier access management of the file share
- Once connected, it's not only local users on the host who will have access to the file share; all AD users on the host will have access to the file share that have the permissions to.

### File Shares
- Easy access to resources in complex environments
	- Easy segmentation
- *Comes at a cost..* 
- **Security Controls are not applied to standard shares, allowing any authenticated user to access their contents.**

#### NetNTLM/NTLM Authentication
**SMB**: Server Message Block
	- Allows clientes (workstations) to communicate with a server (like a file share). In network that use Microsoft AD, SMB governs everything from inter-network file-sharing to remote administration. Even the "out of paper" alert on your computer is handled via SMB. 

When a user authenticates to a server, the server responds with a challenge. The user can encrypt the challenge using their password (not their actual password, but the hash derived from the password) to create a response that is sent back to the server. The server then passes both the challenge and response to the domain controller since it known the user's password hash, it can verify the response. If the response is correct, the domain controller can notify the server that the user has been successfully authenticated and that the server can provide access. This prevents the application or server from having to store the user's credentials, which are now securely and exclusively stored on the domain controller. 
- If we intercept these authentication requests and challenges, we could leverage them to gain unauthorised access, 

#### Responding to the Race
There are lots of authentication requests and challenges flying around the network at all times. 
- [Responder](https://github.com/lgandx/Responder) is a popular tool that is used to perform man-in-the-middle attack by poisoning the respones during NetNTLM authentication, tricking the client into talking to you instead of the actual server they want to connect to. 

On a real network, responder will attempt to poison any Link-Local Multicast name Resolution (LLMNR), NetBIOS Name Service (NBT-NS), and Web Proxy Auto-Discovery (**WPAD**) requests that are detected. On large networks, these protocols allow hosts to perform their own local DNS resolution to the attacker's machine running responder. 

___
# NTLM Authentication and Responder Tool

## NTLM Authentication Overview

- NTLM (NT LAN Manager) is a suite of Microsoft security protocols.
- Primarily used for authentication in networks that include systems connected to an Active Directory (AD) domain.
- Useful for managing access to file shares on servers and workstations.

### Key Points

- **File Shares and AD Domain**: Eases the access management of file shares across the network.
- **Access to File Shares**: Available not just to local users on the host, but to all AD users with appropriate permissions.

## NetNTLM/NTLM Authentication Process

- **Server Message Block (SMB)**:
    - A protocol allowing clients to communicate with servers for tasks like file sharing.
    - In Microsoft AD environments, SMB is crucial for network file-sharing and remote administration.

### Authentication Mechanism

- When a user authenticates to a server, the server issues a challenge.
- The user encrypts this challenge using a hash derived from their password, creating a response.
- The server forwards the challenge and response to the domain controller.
- The domain controller, knowing the user's password hash, verifies the response.
- Successful verification leads to user authentication without the server needing to store user credentials.

## Potential Security Threats

- **Interception of Authentication Requests**:
    - Unauthorized access can be gained by intercepting and manipulating these authentication requests and challenges.

## Responder Tool

- **Purpose**: Used for conducting man-in-the-middle attacks during NetNTLM authentication.
- **Functionality**:
    - Poisons responses during NetNTLM authentication.
    - Tricks clients into communicating with the attacker's machine instead of the actual server.
- **Network Protocols Targeted**:
    - Link-Local Multicast Name Resolution (LLMNR).
    - NetBIOS Name Service (NBT-NS).
    - Web Proxy Auto-Discovery (WPAD).

### Responder in Action

- In a network, Responder attempts to poison LLMNR, NBT-NS, and WPAD requests.
- This allows hosts to resolve DNS locally to the attacker’s machine.
- Particularly effective on large networks.






n