## Table of Contents

- [Python Network Programming Cheatsheet](#python\network\programming\cheatsheet)
  - [Overview](#Overview)
  - [DNS Resolution](#DNS\Resolution)
  - [Socket Programming](#Socket\Programming)
  - [HTTP Requests](#HTTP\Requests)
  - [Working with WebSockets](#Working\with\WebSockets)

# Python Network Programming Cheatsheet

**Title:** Python Network Programming  
**Date:** 2024-01-31  
**Categories:** Programming, Networking  
**Tags:** Python, DNS, Sockets, WebSockets, TCP/IP, HTTP

---

## Overview

Python network programming involves writing Python scripts that can communicate with other machines over a network. This can involve different protocols and technologies like DNS resolution, socket programming, and using WebSockets.

---

## DNS Resolution

Using `socket` library:

- **Get Hostname from IP:**
  ```python
  import socket
  hostname = socket.gethostbyaddr("8.8.8.8")[0]
  ```

- **Get IP from Hostname:**
  ```python
  ip_address = socket.gethostbyname("www.google.com")
  ```

---

## Socket Programming

- **Creating a TCP Socket:**
  ```python
  import socket
  s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  ```

- **Connecting to a Server:**
  ```python
  s.connect(('hostname', port))
  ```

- **Listening for Connections:**
  ```python
  s.bind(('hostname', port))
  s.listen()
  conn, addr = s.accept()
  ```

- **Sending Data:**
  ```python
  s.sendall(b'Hello, world')
  ```

- **Receiving Data:**
  ```python
  data = s.recv(1024)
  ```

- **Closing a Socket:**
  ```python
  s.close()
  ```

- **UDP Socket:**
  ```python
  udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
  ```

---

## HTTP Requests

Using `requests` library:

- **GET Request:**
  ```python
  import requests
  response = requests.get('http://example.com')
  ```

- **POST Request:**
  ```python
  response = requests.post('http://example.com', data={'key': 'value'})
  ```

---

## Working with WebSockets

Using `websockets` library:

- **Creating a WebSocket Server:**
  ```python
  import asyncio
  import websockets

  async def echo(websocket, path):
      async for message in websocket:
          await websocket.send(message)

  start_server = websockets.serve(echo, "localhost", 8765)
  asyncio.get_event_loop().run_until_complete(start_server)
  asyncio.get_event_loop().run_forever()
  ```

- **Creating a WebSocket Client:**
  ```python
  import asyncio
  import websockets

  async def hello():
      uri = "ws://localhost:8765"
      async with websockets.connect(uri) as websocket:
          await websocket.send("Hello world!")
          response = await websocket.recv()
          print(response)

  asyncio.get_event_loop().run_until_complete(hello())
  ```

---
