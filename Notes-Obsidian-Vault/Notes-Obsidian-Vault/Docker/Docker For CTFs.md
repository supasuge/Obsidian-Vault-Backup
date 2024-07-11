## Table of Contents

    - [Docker Cheatsheet for Hosting CTFd and Challenges](#Docker\Cheatsheet\for\Hosting\CTFd\and\Challenges)
      - [Setup Docker for CTFd](#Setup\Docker\for\CTFd)
      - [Docker Basic Commands](#Docker\Basic\Commands)
      - [Docker Compose for CTFd and Challenges](#Docker\Compose\for\CTFd\and\Challenges)
      - [Managing Volumes and Networks (Essential for Persistent Data and Isolation)](#Managing\Volumes\and\Networks\(Essential\for\Persistent\Data\and\Isolation))
      - [Advanced Docker Commands for CTFd and Challenges](#Advanced\Docker\Commands\for\CTFd\and\Challenges)
      - [Tips for Hosting CTFd and Deploying Challenges](#Tips\for\Hosting\CTFd\and\Deploying\Challenges)

Below is a cheatsheet provided from the (Custom) GPT: [Docker and Docker Swarm Assistant](https://chat.openai.com/g/g-Y2azlDdX9-docker-and-docker-swarm-assistant)
### Docker Cheatsheet for Hosting CTFd and Challenges

#### Setup Docker for CTFd

- **Install Docker Engine**
  ```bash
  sudo apt-get update
  sudo apt-get install -y docker-ce docker-ce-cli containerd.io
  ```

- **Start and Enable Docker Service**
  ```bash
  sudo systemctl start docker
  sudo systemctl enable docker
  ```

#### Docker Basic Commands

- **Running a Docker Container**
  ```bash
  sudo docker run -d --name <container_name> -p <host_port>:<container_port> <image_name>
  ```
  `-d` runs the container in detached mode.

- **Listing Docker Containers**
  - Running containers: `sudo docker ps`
  - All containers: `sudo docker ps -a`

- **Stopping and Removing Docker Containers**
  ```bash
  sudo docker stop <container_name_or_id>
  sudo docker rm <container_name_or_id>
  ```

- **Working with Docker Images**
  - Pulling an image: `sudo docker pull <image_name>`
  - Listing all images: `sudo docker images`
  - Removing an image: `sudo docker rmi <image_name>`

#### Docker Compose for CTFd and Challenges

- **Running Services with Docker Compose** (detached mode for CTF challenges)
  ```bash
  sudo docker-compose up -d
  ```
  Use `-d` to run in detached mode.

- **Stopping Services with Docker Compose**
  ```bash
  sudo docker-compose down
  ```

- **Listing Active Services**
  ```bash
  sudo docker-compose ps
  ```

#### Managing Volumes and Networks (Essential for Persistent Data and Isolation)

- **Creating and Managing Volumes**
  - Create a volume: `sudo docker volume create <volume_name>`
  - List volumes: `sudo docker volume ls`
  - Remove a volume: `sudo docker volume rm <volume_name>`

- **Creating and Managing Networks**
  - Create a network: `sudo docker network create <network_name>`
  - List networks: `sudo docker network ls`
  - Remove a network: `sudo docker network rm <network_name>`

#### Advanced Docker Commands for CTFd and Challenges

- **Inspecting Containers and Volumes**
  - Inspect a container: `sudo docker inspect <container_name_or_id>`
  - Inspect a volume: `sudo docker volume inspect <volume_name>`

- **Viewing Logs for Debugging**
  ```bash
  sudo docker logs <container_name_or_id>
  ```
  Very useful for troubleshooting issues with CTFd or challenge instances.

- **Executing Commands Inside Containers**
  ```bash
  sudo docker exec -it <container_name_or_id> <command>
  ```
  For interactive tasks, like accessing a shell inside the container.

#### Tips for Hosting CTFd and Deploying Challenges

- **Isolation**: Use separate Docker networks for each challenge to isolate network traffic.
- **Security**: Regularly update Docker and container images to the latest versions to mitigate vulnerabilities.
- **Performance**: Monitor container and system performance. Adjust resources allocated to containers if necessary.
- **Backup**: Regularly backup volume data, especially for CTFd databases, to avoid data loss.
- **Documentation**: Keep detailed documentation of your setup, including `Dockerfiles`, `docker-compose.yml` files, and custom configurations for easier troubleshooting and maintenance.
