## Table of Contents

- [Docker](#docker)
  - [Docker Daemon](#Docker\Daemon)
    - [Container Management](#Container\Management)
      - [Managing Docker Containers](#Managing\Docker\Containers)
        - [Network and Storage](#Network\and\Storage)
    - [Docker Clients](#Docker\Clients)
  - [Docker Privilege Escalation](#Docker\Privilege\Escalation)
      - [Docker Shared Directories](#Docker\Shared\Directories)

# Docker
Docker architecture lies a client-server model, where we have two primary components:
- Docker daemon
- Docker client




The Docker client acts as our interface issuing commands and interacting with the Docker ecosystem, while the Docker daemon is responsible for executing those commands and managing containers.


## Docker Daemon
`Docker Daemon` or the Docker server, is a critical part of the Docker platform that plsy a pivotal role in container managemenet and orchestration. Think of the Docker Daemon as the powerhouse behind Docker. It has several essential responsibilities:
- Running Docker containers
- Interacting with Docker containers
- Managing Docker containers on the host system


### Container Management
- Handles the core containerization functionality
- Ciritical in container management and orchestration. Docker Daemon is the powerhouse behind Docker. It has several essential responsibililites like:
- Docker server, is a critical part of the Docker platform that plays a pivotal role in container management and orchestration. Think of the docker daemon as the powerhouse behind Docker. It has essential responsibilities like:
	- Running Docker containers
	- Interacting with Docker containers
	- Managing Docker containers on the host system

#### Managing Docker Containers
Handles the core containerization functionality. It coordinates the creation, execution, and monitoring/logging of containers. It isolates the containers to operate independently, with their own file systems, processes, and network interfaces. It also handles Docker image management. It pulls images from registries, such as [Docker Hub](https://hub.docker.com/)
**Offers monitoring and logging capabilities**:
- Captures container logs
- Provides insight into container activities, errors, and debugging information.


##### Network and Storage
It facilitates container networking by creating virtual networks and managing network intergaces. it enables containers to communicated with each other and the outside world through network ports, IP addresses, and DNS resolution. The Docker Daemon also plays a critical role in storage management, since it handles Docker volumes, whicha re used to persisit data beyond the lifespan of containers and manages colume creation, attachment, and cleant-up, allowing containers to share or store data independently of each other.


### Docker Clients
When we interact with Docker, we issue commands the the `Docker Client` which communicates with the Docker Daemon (through RESTful API or a Unix socket) and serves as our primary means of interacting with Docker. We also have the ability to create,  start, stop, manage, remove containers, search, and download Docker images.


## Docker Privilege Escalation
- Get access to an environment where users who can manage docker containers. With this, we could look for ways how to use those docker containers to obtain privileges on the target system. We can use several ways and techniques to escalate our privileges



#### Docker Shared Directories
When using Docker, shared directories (volume mounts) can bridge the gap between the host system and the container's filesystem. With shared directories, specific directories or files on the host system can be made accessible within the container. 
**Useful for**:
- Bridge the gap between the host system and the container's filesystem. With shared directories, specific directories or files on the host system can be made accessible within the container. This is incredibly useful for persisting data, sharing code, and facilitating collaboration between devlopment environments and Docker dontainers. However, it always depends on the setup of the environment and the goals that administrators want to achieve. To create 
























