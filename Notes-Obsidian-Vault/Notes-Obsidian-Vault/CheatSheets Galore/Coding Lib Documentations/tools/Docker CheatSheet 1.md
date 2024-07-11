## Table of Contents

- [Important Containers](#important\containers)
- [PID and File transfer](#pid\and\file\transfer)
    - [[Lifecycle](https://github.com/wsargent/docker-cheat-sheet#lifecycle)](#[Lifecycle](https://github.com/wsargent/docker-cheat-sheet#lifecycle))
    - [[Starting and Stopping](https://github.com/wsargent/docker-cheat-sheet#starting-and-stopping)](#[Starting\and\Stopping](https://github.com/wsargent/docker-cheat-sheet#starting-and-stopping))


# Important Containers
**Location: ~/Desktop/HTB3/ADMachinePath/sauna/impacket**
```bash
--Impacket--
docker run -it --rm "impacket:latest"
#To Run instance on same network interface as host -->
docker run --network <host/host-interface> --rm -it <image-name>


--Bloodhound --Ingester-- ~/Documents/BloodHound.py
docker build -t bloodhound .
docker run -v ${PWD}:/bloodhound-data -it --network host --rm

```





# PID and File transfer
```bash
#Find container ID or Name
docker ps

#Copy Files fomr docker container to Host
docker cp <container_ID>:/path/to/file/in/container /path/to/destination/on/HostOS

#Copy Files from HostOS to container instance
docker cp /path/to/file/on/Host <Container_ID>:/path/to/detination/in/container
```






curl -sSL https://get.docker.com/ | sh

### [Lifecycle](https://github.com/wsargent/docker-cheat-sheet#lifecycle)

- [`docker create`](https://docs.docker.com/engine/reference/commandline/create) creates a container but does not start it.
- [`docker rename`](https://docs.docker.com/engine/reference/commandline/rename/) allows the container to be renamed.
- [`docker run`](https://docs.docker.com/engine/reference/commandline/run) creates and starts a container in one operation.
- [`docker rm`](https://docs.docker.com/engine/reference/commandline/rm) deletes a container.
- [`docker update`](https://docs.docker.com/engine/reference/commandline/update/) updates a container's resource limits.
Normally if you run a container without options it will start and stop immediately, if you want keep it running you can use the command, `docker run -td container_id` this will use the option `-t` that will allocate a pseudo-TTY session and `-d` that will detach automatically the container (run container in background and print container ID)

### [Starting and Stopping](https://github.com/wsargent/docker-cheat-sheet#starting-and-stopping)

- [`docker start`](https://docs.docker.com/engine/reference/commandline/start) starts a container so it is running.
- [`docker stop`](https://docs.docker.com/engine/reference/commandline/stop) stops a running container.
- [`docker restart`](https://docs.docker.com/engine/reference/commandline/restart) stops and starts a container.
- [`docker pause`](https://docs.docker.com/engine/reference/commandline/pause/) pauses a running container, "freezing" it in place.
- [`docker unpause`](https://docs.docker.com/engine/reference/commandline/unpause/) will unpause a running container.
- [`docker wait`](https://docs.docker.com/engine/reference/commandline/wait) blocks until running container stops.
- [`docker kill`](https://docs.docker.com/engine/reference/commandline/kill) sends a SIGKILL to a running container.
- [`docker attach`](https://docs.docker.com/engine/reference/commandline/attach) will connect to a running container.