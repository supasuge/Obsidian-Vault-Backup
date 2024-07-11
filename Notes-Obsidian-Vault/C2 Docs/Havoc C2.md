## Table of Contents

      - [Now Can set up the teamserver and the client](#Now\Can\set\up\the\teamserver\and\the\client)
        - [Next:](#Next:)
    - [Yay!](#Yay!)


```bash
sudo apt update && sudo apt upgrade -y 

#install Havoc C2
cd /opt && git clone https://github.com/HavocFramework/Havoc.git
```

Then setup the dependencies:
```bash
sudo apt install -y git build-essential apt-utils cmake libfontconfig1 libglu1-mesa-dev libgtest-dev libspdlog-dev libboost-all-dev libncurses5-dev libgdbm-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev libbz2-dev mesa-common-dev qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools libqt5websockets5 libqt5websockets5-dev qtdeclarative5-dev golang-go qtbase5-dev libqt5websockets5-dev libspdlog-dev python3-dev libboost-all-dev mingw-w64 nasm
```


#### Now Can set up the teamserver and the client
`cd /opt`

`cd Teamserver`

`bash Install.sh`

`make`

`./teamserver server --profile profiles/havoc.yaotl`

*Server should start here*



To start the Client:
`cd /opt/Havoc/Client/`

`make`

`./Havoc`

##### Next:
- Click "new Profile" and login with the default credentials:
  ``"5spider:password1234"`

### Yay!
We should now have out Havoc C2 up and running. Click "View" then click on "Listeners" to set up listener

At bottom for the screen click "Add"

In Listener menu click "Save" and yur new listener will appear






