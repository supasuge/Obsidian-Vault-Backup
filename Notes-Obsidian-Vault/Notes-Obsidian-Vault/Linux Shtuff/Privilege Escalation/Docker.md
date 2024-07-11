## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Table of Contents](#Table\of\Contents)
- [Docker Architecture](#docker\architecture)
  - [Docker Daemon](#Docker\Daemon)
    - [Network and Storage](#Network\and\Storage)
  - [Docker Images and Containers](#Docker\Images\and\Containers)
  - [Docker Privilege Escalation](#Docker\Privilege\Escalation)
    - [Shared Directories](#Shared\Directories)
      - [Docker Sockets](#Docker\Sockets)
      - [Docker Group](#Docker\Group)
      - [Docker Socket](#Docker\Socket)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Docker Architecture](#docker\architecture)
  - [Docker Daemon](#Docker\Daemon)
    - [Network and Storage](#Network\and\Storage)
  - [Docker Images and Containers](#Docker\Images\and\Containers)
  - [Docker Privilege Escalation](#Docker\Privilege\Escalation)
    - [Shared Directories](#Shared\Directories)
      - [Docker Sockets](#Docker\Sockets)
      - [Docker Group](#Docker\Group)
      - [Docker Socket](#Docker\Socket)

## Table of Contents

- [Docker Architecture](#docker\architecture)
  - [Docker Daemon](#Docker\Daemon)
    - [Network and Storage](#Network\and\Storage)
  - [Docker Images and Containers](#Docker\Images\and\Containers)
  - [Docker Privilege Escalation](#Docker\Privilege\Escalation)
    - [Shared Directories](#Shared\Directories)
      - [Docker Sockets](#Docker\Sockets)
      - [Docker Group](#Docker\Group)
      - [Docker Socket](#Docker\Socket)


# Docker Architecture
- The docker Daemon: Responsible for executing the commands and managing containers
- Docker client: Acts as out interface for issuing commands and interacting with the Docker ecosystem

## Docker Daemon
Responsible for:
- Running Docker containers
- Interacting with Docker containers
- Managing Docker containers on the host system

Also offers logging capabilities including:
- Capture container logs
- Providing insight into container activities, errors, and debugging information
- Monitors resource utilization, CPU, memory, and network usage


### Network and Storage
Facilitates container networking by creating virtual networks and managing network interfaces.
- Enables containers to communicate with each other and the outside world through network ports, IP Addresses, and DNS resolution.
- Plays a critical role in storage management by controlling docker volumes, which are used to persist data

## Docker Images and Containers

Think of a `Docker image` as a blueprint or a template for creating containers. It encapsulates everything needed to run an application, including the application's code, dependencies, libraries, and configurations. An image is a self-contained, read-only package that ensures consistency and reproducibility across different environments. We can create images using a text file called a `Dockerfile`, which defines the steps and instructions for building the image.

A `Docker container` is an instance of a Docker Image. It's lightweight, isolated, and executable environment that runs applications. When we launch a container, it is created form a specific image, and the container inherits all the properties and configurations defined in that image. Each container operates independently, with it's own filesystem, processes, and network interfaces. This isolation ensures that application within containers remain separate from the underlying host system and other containers, preventing conflicts and interference.

While `images` are immutable and `read-only`, while `containers` are mutable and `can be modified` during runtime. We can interact with containers, execute commands within them, monitor their logs, and even make changes to their filesystem or environment. However, any modifications made to a container's filesystem are not persisted unless explicitly saved as a new image or stored in a persistent volume.



## Docker Privilege Escalation

What can happen is that we get access to an environment where we will find users who can manage docker containers. With this, we could look for ways how to use those docker containers to obtain higher privileges on the target system. We can use several ways and techniques to escalate our privileges or escape the docker container.

### Shared Directories
Shared directories (Volume Mounts) can bridge the gap between the host system and the container's filesystem. With shared directories, files on the host system can be made accessible within the container. This is useful for persisting data, sharing code, and facilitating collaboration between development environments and Docker containers. However, it always depends on the setup of the environment and the goals that administrators want to achieve. To create a shared directory, a path on the host system and a corresponding path within the container is specified, creating a direct link between the two locations.

#### Docker Sockets

A Docker socket or Docker daemon socket is a special file that allows us and processes to communicate with the Docker daemon. This communication occurs either through a Unix socket or a network socket, depending on the configuration of our Docker setup

It acts as a bridge, facilitating communication between the Docker client and the Docker daemon. When we issue a command through the Docker CLI, the Docker client sends the command to the Docker socket, and the Docker daemon, in turn, processes the command and carries out the requested actions.

Nevertheless, Docker sockets require appropriate permissions to ensure secure communication and prevent unauthorized access

From here on, we can use the `docker` to interact with the socket and enumerate what docker containers are already running. If not installed, then we can download it [here](https://master.dockerproject.org/linux/x86_64/docker) and upload it to the Docker container.


We can create our own Docker container that maps the host’s root directory (`/`) to the `/hostsystem` directory on the container. With this, we will get full access to the host system. Therefore, we must map these directories accordingly and use the `main_app` Docker image.

Docker

```shell
htb-student@container:/app$ /tmp/docker -H unix:///app/docker.sock run --rm -d --privileged -v /:/hostsystem main_app
htb-student@container:~/app$ /tmp/docker -H unix:///app/docker.sock ps
```

Now, we can log in to the new privileged Docker container with the ID `7ae3bcc818af` and navigate to the `/hostsystem`.

```shell
htb-student@container:/app$ /tmp/docker -H unix:///app/docker.sock exec -it 7ae3bcc818af /bin/bash
```

#### Docker Group

To gain root privileges through Docker, the user we are logged in with must be in the `docker` group. This allows him to use and control the Docker daemon.

```shell
docker-user@nix02:~$ id

uid=1000(docker-user) gid=1000(docker-user) groups=1000(docker-user),116(docker)
```


Alternatively, Docker may have SUID set, or we are in the Sudoers file, which permits us to run `docker` as root. All three options allow us to work with Docker to escalate our privileges

Most hosts have a direct internet connection because the base images and containers must be downloaded. However many hosts may be disconnected from the internet at night and outside working hours for security reasons.

If these hosts are located in a network there, for example, a web server has to pass through, it can still be reached.


To see which images exist and which we can access, we can use the following command:

```shell
docker-user@nix02:~$ docker image ls
```


#### Docker Socket

A case that can also occur is when the Docker socket is writable. Usually, this socket is located in `/var/run/docker.sock`. However, the location can understandably be different. Because basically, this can only be written by the root or docker group. If we act as a user, not in one of these two groups, and the Docker socket still has the privileges to be writable, then we can still use this case to escalate our privileges.

Docker

```shell
docker-user@nix02:~$ docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash

root@ubuntu:~# ls -l
```


