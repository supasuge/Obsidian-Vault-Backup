## Table of Contents

        - [Running a Container](#Running\a\Container)
          - [Listing Containers](#Listing\Containers)
          - [List all Containers on the system](#List\all\Containers\on\the\system)
          - [Show top-level processes](#Show\top-level\processes)
          - [Docker Stop](#Docker\Stop)
          - [Delete a container](#Delete\a\container)
          - [Get the stats of a continer (CPU)](#Get\the\stats\of\a\continer\(CPU))
          - [Attach to a running container](#Attach\to\a\running\container)
          - [Pause the processes in a running container](#Pause\the\processes\in\a\running\container)


##### Running a Container
```bash
sudo docker run -it centos /bin/bash
```
Then hit CTRL + P to return to the `OS` shell.

###### Listing Containers
```bash
docker ps
```

###### List all Containers on the system
```bash
docker ps -a
```

###### Show top-level processes
```bash
sudo docker top 9f215ed0b0d3
sudo docker top <CONTAINER_ID>
```

###### Docker Stop
```bash
sudo docker stop ContainerID
```

###### Delete a container
```bash
sudo docker rm <ContainerID>
```

###### Get the stats of a continer (CPU)
```bash
sudo docker stats <containerid)
```

###### Attach to a running container
```bash
docker attach <ContainerID>
```

###### Pause the processes in a running container
```bash
docker pause ContainerID
```
















