### Setup tor proxy on Arch Linux

[](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#setup-tor-proxy-on-arch-linux)

[Copied from this article](https://linuxconfig.org/install-tor-proxy-on-ubuntu-20-04-linux).

#### Installation

[](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#installation)

- #### Install `tor`
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#install-tor)
    
- ```shell
         $ sudo pacman -S tor
         $ ## nyx provides a terminal status monitor for bandwidth usage, connection details and more.
         $ sudo pacman -S nyx
         $ ## torsocks safely torify applications
         $ sudo pacman -S torsocks
    ```
    
- #### Start the tor service
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#start-the-tor-service)
    
- ```shell
         $ sudo systemctl enable --now tor.service
    ```
    
- #### By default, Tor runs on port 9050. Check it
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#by-default-tor-runs-on-port-9050-check-it)
    

1. ```shell
         $ systemctl status tor.service
         $ ss -nlt
    ```
    

### Test Tor Network Connection

[](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#test-tor-network-connection)

- #### Check your current public IP address
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#check-your-current-public-ip-address)
    
- ```shell
         $ wget -qO - https://api.ipify.org; echo
    ```
    
- #### Torify the command through the `torsocks`
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#torify-the-command-through-the-torsocks)
    

1. ```shell
         $ torsocks wget -qO - https://api.ipify.org; echo
         $ ## must show a different ip address
    ```
    

### Torify your Shell

[](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#torify-your-shell)

- #### torify the shell, issue
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#torify-the-shell-issue)
    
- ```shell
         $ source torsocks on
         $ wget -qO - https://api.ipify.org; echo
         $ ## must show the ip address of tor node
    ```
    
- #### To turn on `torsocks` permanently for all new shells add it to `.bashrc`
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#to-turn-on-torsocks-permanently-for-all-new-shells-add-it-to-bashrc)
    
- ```shell
         $ echo ". torsocks on" >> ~/.bashrc
    ```
    
- #### If you want to turn `torsocks` off, try
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#if-you-want-to-turn-torsocks-off-try)
    

1. ```shell
         $ source torsocks off
    ```
    

### Enable the Tor control port

[](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#enable-the-tor-control-port)

- #### Add to your `/etc/tor/torrc`
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#add-to-your-etctortorrc)
    
- ```shell
          ControlPort 9051
    ```
    
- #### Set a Tor Control password
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#set-a-tor-control-password)
    
- Convert your password from plain-text to hash
    
    ```shell
         $ set +o history # unset bash history
         $ tor --hash-password your_password
         $ set -o history # set bash history
    ```
    
- #### Add that hash to your `/etc/tor/torrc`
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#add-that-hash-to-your-etctortorrc)
    
- ```shell
         HashedControlPassword your_hash
    ```
    
- #### Restart `tor`
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#restart-tor)
    
- ```shell
        $ sudo systemctl restart tor.service
    ```
    
- #### Check the status of port 9051
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#check-the-status-of-port-9051)
    

1. ```shell
        $ ss -nlt
    ```
    

### Test your `tor` control port

[](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#test-your-tor-control-port)

- #### Install gnu-netcat
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#install-gnu-netcat)
    
- ```shell
          $ sudo pacman -S gnu-netcat
    ```
    
- #### To test your `tor` use
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#to-test-your-tor-use)
    
- ```shell
        $ echo -e 'PROTOCOLINFO\r\n' | nc 127.0.0.1 9051
    ```
    
- #### To request a new circuit (IP address) from Tor, use
    
    [](https://gist.github.com/valyakuttan/ce4afb62288120cd5ecef0fde4ea63c4#to-request-a-new-circuit-ip-address-from-tor-use)
    

```shell
    $ set +o history
    $ echo -e 'AUTHENTICATE "my-tor-password"\r\nsignal NEWNYM\r\nQUIT' | nc 127.0.0.1 9051
    $ set -o history
```