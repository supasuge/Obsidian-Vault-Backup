## Table of Contents

- [Socat Cheat Sheet](#socat\cheat\sheet)
  - [Differences Between `socat` and `netcat`](#Differences\Between\`socat`\and\`netcat`)
  - [Common Flags/Options and Use Cases](#Common\Flags/Options\and\Use\Cases)
    - [Basic Syntax](#Basic\Syntax)
    - [Addresses](#Addresses)
    - [Common Options](#Common\Options)
    - [SSL/TLS Options](#SSL/TLS\Options)
    - [Examples](#Examples)
- [Advanced Usage](#advanced\usage)
  - [Advanced Options and Arguments](#Advanced\Options\and\Arguments)
    - [Multipurpose Relay (Chaining)](#Multipurpose\Relay\(Chaining))
    - [File Descriptors](#File\Descriptors)
    - [Conditional Execution](#Conditional\Execution)
    - [Advanced Socket Options](#Advanced\Socket\Options)
    - [Tunnelling and Proxy](#Tunnelling\and\Proxy)
    - [Data Processing](#Data\Processing)
    - [Special Addresses](#Special\Addresses)
    - [SSL/TLS Advanced Options](#SSL/TLS\Advanced\Options)
  - [Advanced Examples](#Advanced\Examples)

# Socat Cheat Sheet

`socat` (SOcket CAT) is a powerful and versatile utility for establishing and transferring data between network sockets. It can create bi-directional byte streams between two data channels, including files, pipes, devices (terminal or modem, etc.), or sockets (Unix, TCP, UDP, IPv4, IPv6, raw, etc.), thereby offering more features than `netcat`.

## Differences Between `socat` and `netcat`
- **Versatility**: `socat` can handle more types of connections and has more options.
- **Bidirectional Data Transfer**: `socat` can create more complex data flow scenarios.
- **Encryption**: `socat` supports SSL and TLS encryption natively.

## Common Flags/Options and Use Cases

### Basic Syntax
```bash
socat [options] <address1> <address2>
```

### Addresses
- **`-` (stdin/stdout)**: Use standard input/output.
- **`TCP:<host>:<port>`**: TCP connection to given host and port.
- **`TCP-LISTEN:<port>,reuseaddr`**: TCP port listening.
- **`UDP:<host>:<port>`**: UDP connection to given host and port.
- **`EXEC:<command>`**: Execute a command.
- **`PIPE:<pipename>`**: Named pipe.
- **`UNIX:<path>`**: Unix domain socket.
- **`FILE:<path>`**: Regular file.

### Common Options
- **`-d`**: Enables debugging, printing more verbose information.
- **`-D`**: Enables address-dependent debugging.
- **`-u`**: UDP mode (SOCK_DGRAM).
- **`-l`**: Creates a loop for continuous listening.
- **`-L`**: Use line mode for input.
- **`-t <timeout>`**: Sets timeout for the connection.
- **`-v`**: Verbose output, prints traffic on the console.
- **`-V`**: Print version and exit.
- **`-h`**: Display help.

### SSL/TLS Options
- **`OPENSSL:<host>:<port>,verify=<n>`**: SSL/TLS client connection.
- **`openssl-listen:<port>,cert=<cert.pem>,key=<key.pem>`**: SSL/TLS server.

### Examples
1. **TCP Port Forwarding**
   ```bash
   socat TCP-LISTEN:8000,fork TCP:192.168.1.15:80
   ```
   Forwards TCP port 8000 to 192.168.1.15 on port 80.

2. **File Transfer**
   ```bash
   # Receiver
   socat TCP-LISTEN:8000,reuseaddr OPEN:/path/to/file
   # Sender
   socat -u OPEN:/path/to/file TCP:<receiver-ip>:8000
   ```
   Transfers a file over TCP.

3. **Create a Virtual Serial Port**
   ```bash
   socat PTY,link=/tmp/virtual-serial,raw,echo=0 PTY,link=/tmp/virtual-serial2,raw,echo=0
   ```
   Creates two linked virtual serial ports.

4. **Execute a Command and Return Output Over TCP**
   ```bash
   socat TCP-LISTEN:8000 EXEC:'uname -a'
   ```
   Listens on TCP port 8000 and returns the output of `uname -a`.

5. **Encrypt Data with SSL/TLS**
   ```bash
   # Server
   socat openssl-listen:443,cert=server.pem,key=server.key,verify=0 -
   # Client
   socat - OPENSSL:server:443
   ```
   Secure communication using SSL/TLS.

6. **Unix Socket to TCP**
   ```bash
   socat UNIX-LISTEN:/tmp/socket,fork TCP:localhost:8000
   ```
   Forwards a UNIX socket to a TCP port.

7. **Proxy with Fork**
   ```bash
   socat TCP-LISTEN:8000,fork TCP:www.google.com:80
   ```
   Acts as a simple TCP proxy to google.com.

# Advanced Usage

`socat` is highly versatile and can be used for complex networking tasks. This section of the cheat sheet focuses on advanced usage, detailing more sophisticated command line arguments and providing examples of their application.

## Advanced Options and Arguments

### Multipurpose Relay (Chaining)
- **`socat` allows chaining multiple addresses**, enabling complex data relay setups.

### File Descriptors
- **`FD:<n>`**: Uses the file descriptor `n`.
- **`STDERR`**: Uses the standard error.

### Conditional Execution
- **`SYSTEM:<command>`**: Executes a command in a subshell.
- **`EXEC:<command>,fdin=<n>,fdout=<n>`**: Executes a command with specific file descriptors for input and output.

### Advanced Socket Options
- **`TCP-LISTEN:<port>,bind=<address>,range=<ip-range>`**: Listen on a specific IP address or IP range.
- **`TCP-WRAP:<service>,exec=<command>`**: Wraps TCP service around a command.

### Tunnelling and Proxy
- **`PROXY:<proxy-address>:<target-address>,proxyport=<port>`**: Connects via a proxy server.

### Data Processing
- **`READLINE`**: Offers basic line editing and history features.

### Special Addresses
- **`TUN:<tun-device>`**: Creates or opens a TUN/TAP device.
- **`PTY,link=<path>`**: Creates a pseudoterminal.

### SSL/TLS Advanced Options
- **`verify=<n>`**: Sets the level of certificate verification.
- **`cafile=<file.pem>`**: Specifies the CA file for SSL/TLS.

## Advanced Examples

8. **Chaining Addresses for Complex Relay**
   ```bash
   socat - TCP-LISTEN:8000,fork SYSTEM:"tee log.txt",TCP:example.com:80
   ```
- **`-`**: This indicates that `socat` should use standard input (stdin).
- **`TCP-LISTEN:8000,fork`**: `socat` listens on TCP port 8000. The `fork` option allows it to handle multiple connections by creating a new process for each incoming connection.
- **`SYSTEM:"tee log.txt"`**: This is an intermediate step where `socat` uses the `tee` command to write the incoming data to a file (`log.txt`) while also passing the data to the next address in the chain.
- **`TCP:example.com:80`**: The final destination of the data stream is `example.com` on port 80.

9. **Combine File Descriptor with Network Socket**
   ```bash
   socat -d -d FD:0 TCP:localhost:8000
   ```
   This links the standard input (file descriptor 0) to a TCP connection.

10. **Using TUN/TAP Device for Networking**
   ```bash
   socat TUN:192.168.0.1/24,up TCP:localhost:8000
   ```
- **`TUN:192.168.0.1/24,up`**: A TUN/TAP device is a virtual network kernel device. `TUN` (network TUNnel) simulates a network layer device and it operates with layer 3 packets like IP packets. `TAP` (network tap) simulates a link layer device and it operates with layer 2 packets like Ethernet frames. In this command, `socat` creates a TUN device with the IP address `192.168.0.1` and a subnet mask of `255.255.255.0` (indicated by `/24`). The `up` option brings the device up.
- **`TCP:localhost:8000`**: The TUN device is connected to a TCP server running on localhost port 8000.
		- TUN/TAP devices: Software based virtual network devices. They allow user-space programs to simulate network devices. Ex: Creating a network interface that looks like a real network card but is actually managed by a program.
			- Uses in VPN's.
1. **SSL Encrypted Chat Server**
   ```bash
   socat OPENSSL-LISTEN:8000,cert=server.pem,key=server.key,verify=0,fork SYSTEM:"$SHELL -li"
   ```
   Creates an SSL-encrypted chat server.

12. **Proxy Connection**
   ```bash
   socat TCP-LISTEN:8000,fork PROXY:proxyserver:www.example.com:80,proxyport=8080
   ```
   Forwards connections from local port 8000 to `www.example.com` through a proxy server on port 8080.

13. **Combining socat with sed for Data Manipulation**
   ```bash
   socat TCP-LISTEN:8000 SYSTEM:"sed 's/old/new/'"
   ```
   Listens on TCP port 8000 and uses `sed` to replace 'old' with 'new' in the transferred data.

14. **Interactive Shell Over TCP**
   ```bash
   socat TCP-LISTEN:8000 EXEC:'/bin/bash',pty,stderr,setsid,sigint,sane
   ```
   Provides a bash shell to a remote user connecting to port 8000.

15. **Using socat for Port Scanning**
   ```bash
   socat - TCP4:www.example.com:80,range=20-30
   ```
   Tries to establish a TCP connection to `www.example.com` on ports 20 to 30.

16. **Creating a Serial Port Bridge**
   ```bash
   socat PTY,link=/dev/virtualcom0,raw PTY,link=/dev/virtualcom1,raw
   ```
   Creates two interconnected virtual serial ports.

17. **Data Transfer with Readline Support**
    ```bash
    socat READLINE TCP:localhost:8000
    ```
    Connects to TCP port 8000 with readline capabilities for user input.
