## Creating a basic socket connection
```python
import socket


# Create a TCP socket
tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)


# Create a UDP socket
udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
```


### Socket communication methods
- `socket.connect(address)` - Establishes a connection to a remote address
- `socket.bind(address)` - Binds the socket to a specific address and port
- `socket.listen(backlog)` - Listens for incoming connecitons on a TCP socket
- `socket.accept()` accepts and incomding connections and returns a new socket object for communication
- `socket.send(data)`: Sends data over the socket.
- `socket.recv(buffer_size)`: Receives data from the socket.

Here's an example of a basic TCP server that listens for incoming connections:

```python
import socket


# Create a TCP socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)


# Bind the socket to a specific address and port
server_address = ('localhost', 1234)
server_socket.bind(server_address)


# Listen for incoming connections
server_socket.listen(5)


while True:
    # Accept a client connection
    client_socket, client_address = server_socket.accept()


    # Receive and send data
    data = client_socket.recv(1024)
    client_socket.send(b"Received: " + data)


    # Close the client socket
    client_socket.close()
```

### Port Scanner
```python
#!/bin/python3

import sys
import socket
from datetime import datetime

#Define our target
if len(sys.argv) == 2:
	target = socket.gethostbyname(sys.argv[1]) #Translate hostname to IPv4
else:
	print("Invalid amount of arguments.")
	print("Syntax: python3 scanner.py")

#Add a pretty banner
print("-" * 50)
print("Scanning target "+target)
print("Time started: "+str(datetime.now()))
print("-" * 50)

try:
	for port in range(50,85):
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		socket.setdefaulttimeout(1)
		result = s.connect_ex((target,port)) #returns an error indicator - if port is open it throws a 0, otherwise 1
		if result == 0:
			print("Port {} is open".format(port))
		s.close()

except KeyboardInterrupt:
	print("\nExiting program.")
	sys.exit()
	
except socket.gaierror:
	print("Hostname could not be resolved.")
	sys.exit()

except socket.error:
	print("Could not connect to server.")
	sys.exit()
```


### Reading Files
To read from a file, you need to open it in read mode using the `open()` function. Once the file is open, you can use methods like `read()`, `readline()`, or `readlines()` to retrieve the contents of the file.

- `read()`: Reads the entire content of the file as a string.
- `readline()`: Reads a single line from the file.
- `readlines()`: Reads all lines from the file and returns them as a list.

Here's an example of reading from a file:
```python
# Open the file in read mode
file = open("example.txt", "r")


# Read the entire content
content = file.read()
print(content)


# Read a single line
line = file.readline()
print(line)


# Read all lines
lines = file.readlines()
print(lines)


# Close the file
file.close()
```

To append content to an existing file without overwriting its existing contents, you can open the file in append mode (`"a"`) using the `open()` function. Then, you can use the `write()` method to append content to the file.
```python
# Open the file in append mode
file = open("example.txt", "a")


# Append content to the file
file.write("\nThis is appended content.")


# Close the file
file.close()
```

