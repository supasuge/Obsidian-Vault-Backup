## Table of Contents

- [Docker Cheatsheet](#docker\cheatsheet)
  - [Basic Commands](#Basic\Commands)
      - [Docker Version bash docker --version`](#Docker\Version\bash\docker\--version`)
    - [Docker Information](#Docker\Information)
    - [Docker Help](#Docker\Help)
  - [Container Management](#Container\Management)
    - [List Containers](#List\Containers)
    - [Run Container](#Run\Container)
    - [Stop Container](#Stop\Container)
    - [Remove Container](#Remove\Container)
    - [Execute Command Inside Container](#Execute\Command\Inside\Container)
  - [Image Management](#Image\Management)
    - [List Images](#List\Images)
    - [Pull Image](#Pull\Image)
    - [Remove Image](#Remove\Image)
  - [Docker Compose](#Docker\Compose)
    - [Start Services](#Start\Services)
    - [Stop Services](#Stop\Services)
    - [Validate Compose File](#Validate\Compose\File)
  - [Networking](#Networking)
    - [List Networks](#List\Networks)
    - [Create Network](#Create\Network)
    - [Connect Container to Network](#Connect\Container\to\Network)
    - [Disconnect Container from Network](#Disconnect\Container\from\Network)
  - [Volumes](#Volumes)
    - [List Volumes](#List\Volumes)
    - [Create Volume](#Create\Volume)
    - [Remove Volume](#Remove\Volume)
  - [Miscellaneous](#Miscellaneous)
    - [Docker System Prune](#Docker\System\Prune)
    - [Show Logs](#Show\Logs)
    - [Inspect a Network](#Inspect\a\Network)
    - [Create a User-defined Bridge Network](#Create\a\User-defined\Bridge\Network)
    - [Remove a Network](#Remove\a\Network)
  - [Ports and Port Binding](#Ports\and\Port\Binding)
    - [Run Container with Port Binding](#Run\Container\with\Port\Binding)
    - [Publish All Exposed Ports](#Publish\All\Exposed\Ports)
  - [Bridge Networking](#Bridge\Networking)
    - [Create Bridge Network](#Create\Bridge\Network)
    - [Connect Container to a Bridge Network](#Connect\Container\to\a\Bridge\Network)
    - [Disconnect Container from a Bridge Network](#Disconnect\Container\from\a\Bridge\Network)
  - [Host Networking](#Host\Networking)
    - [Run Container with Host Networking](#Run\Container\with\Host\Networking)
  - [Overlay Networking](#Overlay\Networking)
    - [Create an Overlay Network](#Create\an\Overlay\Network)
  - [Macvlan Networking](#Macvlan\Networking)
    - [Create a Macvlan Network](#Create\a\Macvlan\Network)
  - [IP Translation and Routing](#IP\Translation\and\Routing)
    - [Container to Container](#Container\to\Container)
    - [Container to Host](#Container\to\Host)
    - [Host to Container](#Host\to\Container)
  - [Advanced Features](#Advanced\Features)
    - [Enable Manual Container Attachment](#Enable\Manual\Container\Attachment)
    - [Set Custom IPAM](#Set\Custom\IPAM)
    - [Network Aliases](#Network\Aliases)

# Docker Cheatsheet  
## Basic Commands  

#### Docker Version bash docker --version`

### Docker Information

```


`docker info`
```

### Docker Help
```bash
docker <command> --help
```

---

## Container Management

### List Containers
```

bashCopy code

`docker ps docker ps -a
```

### Run Container

bashCopy code

`docker run <image-name>`

### Stop Container



`docker stop <container-id>`

### Remove Container

bashCopy code

`docker rm <container-id>`

### Execute Command Inside Container

bashCopy code

`docker exec -it <container-id> <command>`

---

## Image Management

### List Images

bashCopy code

`docker images`

### Pull Image

bashCopy code

`docker pull <image-name>`

### Remove Image

bashCopy code

`docker rmi <image-name>`

---

## Docker Compose

### Start Services

bashCopy code

`docker-compose up`

### Stop Services

bashCopy code

`docker-compose down`

### Validate Compose File

bashCopy code

`docker-compose config`

---

## Networking

### List Networks

bashCopy code

`docker network ls`

### Create Network

bashCopy code

`docker network create <network-name>`

### Connect Container to Network

bashCopy code

`docker network connect <network-name> <container-id>`

### Disconnect Container from Network

bashCopy code

`docker network disconnect <network-name> <container-id>`

---

## Volumes

### List Volumes

bashCopy code

`docker volume ls`

### Create Volume

bashCopy code

`docker volume create <volume-name>`

### Remove Volume

bashCopy code

`docker volume rm <volume-name>`

---

## Miscellaneous

### Docker System Prune

bashCopy code

`docker system prune`

### Show Logs

bashCopy code

`docker logs <container-id>`

vbnetCopy code

`Feel free to modify or extend this cheatsheet as you see fit. This is a basic cheatsheet and doesn't cover all Docker commands and options.`

![User](https://chat.openai.com/_next/image?url=https%3A%2F%2Flh3.googleusercontent.com%2Fa%2FACg8ocL3g7XUMR3sXw_Hv7RgmlhULmBnPZdZvbMwG4TdD0TI%3Ds96-c&w=96&q=75)

Please go into networks, netowrking, ports, bridges, IP translations. etc.

ChatGPT

Certainly! Networking is a crucial part of Docker, enabling communication between containers and with the outside world. Here is an extended Markdown cheatsheet that focuses on Docker's networking capabilities, including ports, bridges, and IP translations.

markdownCopy code

`# Docker Networking Cheatsheet  ## Basics  ### List All Networks ```bash docker network ls`

### Inspect a Network

This provides details such as IPAM, subnet, gateway, and connected containers.

bashCopy code

`docker network inspect <network-name-or-id>`

### Create a User-defined Bridge Network

bashCopy code

`docker network create --driver bridge <network-name>`

### Remove a Network

bashCopy code

`docker network rm <network-name-or-id>`

---

## Ports and Port Binding

### Run Container with Port Binding

To bind a container port to a host port.

bashCopy code

`docker run -p <host-port>:<container-port> <image-name>`

### Publish All Exposed Ports

To publish all exposed ports to random ports on the host interfaces.

bashCopy code

`docker run -P <image-name>`

---

## Bridge Networking

### Create Bridge Network

bashCopy code

`docker network create -d bridge <custom-bridge-network-name>`

### Connect Container to a Bridge Network

bashCopy code

`docker run --network=<network-name> <image-name>`

### Disconnect Container from a Bridge Network

bashCopy code

`docker network disconnect <network-name> <container-name-or-id>`

---

## Host Networking

### Run Container with Host Networking

This allows the container to share the host's networking namespace.

bashCopy code

`docker run --network host <image-name>`

---

## Overlay Networking

### Create an Overlay Network

Usually used in Swarm mode.

bashCopy code

`docker network create --driver overlay <network-name>`

---

## Macvlan Networking

### Create a Macvlan Network

bashCopy code

`docker network create -d macvlan \   --subnet=<subnet> \   --gateway=<gateway> \   -o parent=<parent-interface> \   <network-name>`

---

## IP Translation and Routing

### Container to Container

By default, containers can communicate if they are on the same custom bridge network.

### Container to Host

You can use the host's IP or a special DNS name `host.docker.internal`.

### Host to Container

Use port binding or the container's IP address.

---

## Advanced Features

### Enable Manual Container Attachment

Create a network that doesn't automatically connect to containers upon `docker run`.

bashCopy code

`docker network create -o "com.docker.network.bridge.enable_icc=false" <network-name>`

### Set Custom IPAM

bashCopy code

`docker network create --ipam-driver=<driver-name> --ipam-opt=<driver-specific-options> <network-name>`

### Network Aliases

Use network aliases to allow a container to be discoverable under different DNS names.

bashCopy code

`docker network connect --alias <alias> <network-name> <container-name>`
