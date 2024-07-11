## Table of Contents

- [Setup tor proxy on Arch Linux](#Setup\tor\proxy\on\Arch\Linux)
      - [Installation](#Installation)
    - [Test Tor Network Connection](#Test\Tor\Network\Connection)
    - [Torify your Shell](#Torify\your\Shell)
    - [[### Enable the Tor control port](https://gist.github.com/rayhato/e6d886740f811d4a72947e59827d5b6f#enable-the-tor-control-port)](#[###\Enable\the\Tor\control\port](https://gist.github.com/rayhato/e6d886740f811d4a72947e59827d5b6f#enable-the-tor-control-port))
    - [Enable the Tor control port](#Enable\the\Tor\control\port)
      - [Check the status of port 9051](#Check\the\status\of\port\9051)
    - [Test your `tor` control port](#Test\your\`tor`\control\port)
      - [To request a new circuit (IP address) from Tor, use](#To\request\a\new\circuit\(IP\address)\from\Tor,\use)

### Setup tor proxy on Arch Linux

[Copied from this article](https://linuxconfig.org/install-tor-proxy-on-ubuntu-20-04-linux).
#### Installation

- #### Install `tor`
    
    ```shell
         $ sudo pacman -S tor
         $ ## nyx provides a terminal status monitor for bandwidth usage, connection details and more.
         $ sudo pacman -S nyx
         $ ## torsocks safely torify applications
         $ sudo pacman -S torsocks
    ```
    
    
- #### Start the tor service
    
    ```shell
         $ sudo systemctl enable --now tor.service
    ```
    
    

1. By default, Tor runs on port 9050. Check it
    
```shell
$ systemctl status tor.service
$ ss -nlt
```

### Test Tor Network Connection


- #### Check your current public IP address
    
    ```shell
         $ wget -qO - https://api.ipify.org; echo
    ```
    
    

1. Torify the command through the `torsocks`
    
    ```shell
         $ torsocks wget -qO - https://api.ipify.org; echo
         $ ## must show a different ip address
    ```

### Torify your Shell
- #### torify the shell, issue
    
    ```shell
         $ source torsocks on
         $ wget -qO - https://api.ipify.org; echo
         $ ## must show the ip address of tor node
    ```
    
- #### [To turn on torsocks permanently](https://gist.github.com/rayhato/e6d886740f811d4a72947e59827d5b6f#to-turn-on-torsocks-permanently-for-all-new-shells-add-it-to-bashrc)
    
- #### To turn on `torsocks` permanently for all new shells add it to `.bashrc`
    
```bash
$ echo ". torsocks on" >> ~/.bashrc
```
    
    

If you want to turn `torsocks` off, try
    
```bash
$ source torsocks off
```

### [### Enable the Tor control port](https://gist.github.com/rayhato/e6d886740f811d4a72947e59827d5b6f#enable-the-tor-control-port)

### Enable the Tor control port


- #### Add to your `/etc/tor/torrc`
    
```shell
ControlPort 9051
```
    
    
- #### Set a Tor Control password
    
    Convert your password from plain-text to hash
    
```shell
$ set +o history # unset bash history
$ tor --hash-password your_password
$ set -o history # set bash history
```
    
    
- #### Add that hash to your `/etc/tor/torrc`
    
```shell
HashedControlPassword your_hash
```
    
    
- #### Restart `tor`
    
```shell
$ sudo systemctl restart tor.service
```
    
    

#### Check the status of port 9051
    
```
 
$ ss -nlt
```
### Test your `tor` control port
- #### Install gnu-netcat
    ```shell
$ sudo pacman -S gnu-netcat
    ```
- #### To test your `tor` use
```sbash
echo -e 'PROTOCOLINFO\r\n' | nc 127.0.0.1 9051
```
#### To request a new circuit (IP address) from Tor, use
```bash
$ set +o history
$ echo -e 'AUTHENTICATE "my-tor-password"\r\nsignal NEWNYM\r\nQUIT' | nc 127.0.0.1 9051
$ set -o history
```





