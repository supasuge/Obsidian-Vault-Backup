## Table of Contents

  - [Install Docker](#Install\Docker)
- [Docker requirements](#docker\requirements)
- [Docker GPG key](#docker\gpg\key)
- [Docker setup of the stable repository](#docker\setup\of\the\stable\repository)
- [Docker installation](#docker\installation)

 ip:
`ssh root@172.232.23.195 -i id_rsa_linode`

```bash
    1  id
    2  id
    3  sudo apt update && sudo apt upgrade
    4  adduser ctfd
    5  usermod -aG sudo ctfd
    6  apt install ufw
    7  ufw allow openssh
    8  ufw allow http
    9  ufw allow https
   10  ufw enable
   11  apt install -y python3-pip python3-dev build-essential libssl-dev libffi-dev python3-setuptools nginx git
   12  ifconfig
   13  ip a
   14  history
```

## Install Docker
```bash
#!/bin/bash
# Docker requirements
sudo apt install ca-certificates curl gnupg lsb-release

# Docker GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Docker setup of the stable repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/nul

# Docker installation
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io
```