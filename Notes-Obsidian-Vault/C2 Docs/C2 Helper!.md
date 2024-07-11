## Table of Contents

- [Havoc](#havoc)

https://howto.thec2matrix.com/



# Havoc
#havoc #c2 #beacon #listener

**installHavoc.sh**
```bash
#!/bin/bash
sudo apt-get update && apt-get upgrade  

sudo apt install -y git build-essential apt-utils cmake libfontconfig1 libglu1-mesa-dev libgtest-dev libspdlog-dev libboost-all-dev libncurses5-dev libgdbm-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev libbz2-dev mesa-common-dev qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools libqt5websockets5 libqt5websockets5-dev qtdeclarative5-dev golang-go qtbase5-dev libqt5websockets5-dev python3-dev libboost-all-dev mingw-w64 nasm 

```

https://github.com/HavocFramework/Havoc
```bash
#Next
git clone https://github.com/HavocFramework/Havoc
cd havoc
```

Now we need to set up the bookworm repo for Python 3.10 (For that change to the root user) 
```bash

echo 'deb http://ftp.de.debian.org/debian bookworm main'>> /etc/apt/sources.list sudo apt update sudo apt install python3-dev python3.10-dev libpython3.10 libpython3.10-dev python3.10
```
