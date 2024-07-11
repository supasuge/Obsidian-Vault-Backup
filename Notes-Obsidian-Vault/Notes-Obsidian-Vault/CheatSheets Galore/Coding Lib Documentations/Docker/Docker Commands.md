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
- [Docker Networking Cheatsheet](#docker\networking\cheatsheet)
  - [Basics](#Basics)
    - [List All Networks](#List\All\Networks)
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

bashCopy code

`docker info`

### Docker Help

bashCopy code

`docker <command> --help`

---

## Container Management

### List Containers

bashCopy code

`docker ps docker ps -a`

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

`docker-compose up`

### Stop Services

`docker-compose down`

### Validate Compose File


`docker-compose config`

---

## Networking

### List Networks


`docker network ls`

### Create Network


`docker network create <network-name>`

### Connect Container to Network


`docker network connect <network-name> <container-id>`

### Disconnect Container from Network


`docker network disconnect <network-name> <container-id>`

---

## Volumes

### List Volumes


`docker volume ls`

### Create Volume


`docker volume create <volume-name>`

### Remove Volume


`docker volume rm <volume-name>`

---

## Miscellaneous

### Docker System Prune


`docker system prune`

### Show Logs


`docker logs <container-id>`


`Feel free to modify or extend this cheatsheet as you see fit. This is a basic cheatsheet and doesn't cover all Docker commands and options.`


# Docker Networking Cheatsheet  
## Basics  
### List All Networks 
```
bash docker network ls`
```

### Inspect a Network
This provides details such as IPAM, subnet, gateway, and connected containers.
```
docker network inspect <network-name-or-id>
```
### Create a User-defined Bridge Network
```
docker network create --driver bridge <network-name>
```
### Remove a Network

`docker network rm <network-name-or-id>`

---

## Ports and Port Binding

### Run Container with Port Binding

To bind a container port to a host port.

`docker run -p <host-port>:<container-port> <image-name>`

### Publish All Exposed Ports

To publish all exposed ports to random ports on the host interfaces.

`docker run -P <image-name>`

---

## Bridge Networking

### Create Bridge Network

`docker network create -d bridge <custom-bridge-network-name>`

### Connect Container to a Bridge Network

`docker run --network=<network-name> <image-name>`

### Disconnect Container from a Bridge Network


`docker network disconnect <network-name> <container-name-or-id>`

---

## Host Networking

### Run Container with Host Networking

This allows the container to share the host's networking namespace.


`docker run --network host <image-name>`

---

## Overlay Networking

### Create an Overlay Network

Usually used in Swarm mode.

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
