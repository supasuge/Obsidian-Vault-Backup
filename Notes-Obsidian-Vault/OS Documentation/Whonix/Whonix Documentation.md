## Table of Contents

    - [Gateway Common Commmands](#Gateway\Common\Commmands)
      - [Change Keyboard Layout](#Change\Keyboard\Layout)
      - [Circumvent uwt..? Wrapper use](#Circumvent\uwt..?\Wrapper\use)
  - [Enable\\Disable Tor Connection wizard](#Enable\\Disable\Tor\Connection\wizard)
    - [Important Configs](#Important\Configs)
          - [Launch Nyx](#Launch\Nyx)
      - [OS Updates](#OS\Updates)
      - [switch to clearnet user](#switch\to\clearnet\user)
      - [Report the date in UTC:](#Report\the\date\in\UTC:)
      - [Manually set the system clock:](#Manually\set\the\system\clock:)
      - [Randomize the time: [6] [7]](#Randomize\the\time:\[6]\[7])
    - [Tor Commands](#Tor\Commands)
- [Workstation](#workstation)
          - [Connection check](#Connection\check)
          - [GnuPG (OpenPGP)	](#GnuPG\(OpenPGP)	)
          - [Check file signatures (example):](#Check\file\signatures\(example):)
      - [HexChat	](#HexChat	)
          - [Reset HexChat identity:](#Reset\HexChat\identity:)
    - [Important Configuration Folders and Logs	](#Important\Configuration\Folders\and\Logs	)
          - [Important configuration folders:](#Important\configuration\folders:)
          - [Important logs:](#Important\logs:)
          - [Leak Test [8]	](#Leak\Test\[8]	)
    - [OS Updates / Software Installation	](#OS\Updates\/\Software\Installation	)
          - [Install the package-name package.](#Install\the\package-name\package.)
          - [Whonix Version	](#Whonix\Version	)


### Gateway Common Commmands

#### Change Keyboard Layout
```bash
sudo dpkg-reconfigure keyboard-configuration #N/A
sudo dpkg-reconfigure console-data

#Make the reconfigured keyboard-configuration change take effect
sudo setupcon
```

#### Circumvent uwt..? Wrapper use
- `/usr/bin/apt-get.anondist-orig`
- `/usr/bin/wget.anondist-orig`
- `/usr/bin/curl.anondist-orig`
- `/usr/bin/gpg.anondist-orig`
- `/usr/bin/ssh.anondist-orig`

## Enable\\Disable Tor Connection wizard
```bash
lxsudo anon-connection-wizard

sudo setup-wizard-dist
```
### Important Configs
```bash
sudoedit /usr/local/etc/torrc.d/50_user.conf
sudoedit /etc/whonix_firewall.d/50_user.conf

/etc/whonix_firewall.d/ #IMPORTANT
```

###### Launch Nyx
```bash
nyx
```

#### OS Updates 
```bash
upgrade-nonroot
sudo apt update && sudo apt full-upgrade
```

#### switch to clearnet user
```bash
sudo su clearnet

#systemcheck - network time synch and tor connection check
systemcheck
```

#### Report the date in UTC:
```bash
date -u
```

#### Manually set the system clock:
```bash
sudo date -s "17 FEB 2019 24:00:00" && sudo hwclock -w
```

#### Randomize the time: [6] [7]
`clock-random-manual-gui`: a randomized clock setting (in UTC) is entered via a GUI.
`clock-random-manual-cli`: a randomized clock setting (in UTC) is entered on the command line. For example: 
`echo "Wed Dec 04 06:20:13 UTC 2019" | /usr/bin/clock-random-manual-cli`
### Tor Commands
```bash
#Restart Network:
sudo service networking restart

#Restart Tor:
sudo service tor restart

#Stop Tor:
sudo systemctl stop tor@default

#Check the Tor version:
anon-info

#Check the Tor configuration:
anon-verify
 
#sudo -u debian-tor tor --verify-confi
```


# Workstation
###### Connection check
```
nslookup check.torproject.org
```
###### GnuPG (OpenPGP)	
**Retrieve keys (example):**
**Display key fingerprint (example):**
```bash
sudo apt-key adv --fingerprint A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89
```

###### Check file signatures (example):
```bash
gpg --verify tor-browser-linux64-8.5_en-US.tar.xz.asc tor-browser-linux64-8.5_en-US.tar.xz
```


#### HexChat	
###### Reset HexChat identity:
```bash
hexchat-reset
```


### Important Configuration Folders and Logs	
###### Important configuration folders:
```bash
/etc/whonix_firewall.d/
```
###### Important logs:
```bash
sudo tail -f /var/log/syslog
sdwdate-log-viewer
```
###### Leak Test [8]	
```bash
leaktest
Network Restart	
sudo service networking restart
```
### OS Updates / Software Installation	
```bash
upgrade-nonroot
#OR
sudo apt update && sudo apt full-upgrade
```
###### Install the package-name package.
`sudo apt install package-name`

Install the package-name package from Debian backports. Requires enabling backports repository.
```bash
sudo apt -t bookworm-backports install package-name
```

systemcheck	
Network Time Synchronization and Tor Connection Check: [9]
`systemcheck`

Time	
Manually set the system clock:
`sudo date -s "17 FEB 2019 24:00:00" && sudo hwclock -w

Tor Browser	
Note: Tor Browser can only be started when a graphical desktop environment (DE) such as Xfce is running (Whonix Xfce). At time of writing Tor Browser cannot be run without a running DE (Whonix CLI without a custom installed DE). However, graphical applications such as Tor Browser can be started from command line when a DE is running. [10]

Important folders:
``~/.tb/tor-browser
``~/.tb/tor-browser/Browser/TorBrowser/Data/Browser
Tor Browser Launcher:
`torbrowser

Tor Browser in debugging mode:
``~/.tb/tor-browser/Browser/start-tor-browser --debug

Tor Browser Downloader by Whonix developers:
`update-torbrowser

Virtual Consoles	
Text console: Press `Alt + Crtl + F1
Additional text consoles: Press `Alt + Crtl + F2 or F3 and so on.
Graphical console: `Press Alt + Crtl + F7
VM Operations	
Reboot:
`sudo reboot

Power off:
`sudo poweroff

###### Whonix Version	
Whonix version:
`cat /etc/whonix_version


