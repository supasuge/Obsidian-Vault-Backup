## Table of Contents

- [Kali](#kali)
- [Ubuntu](#ubuntu)
  - [BLE Support](#BLE\Support)


# Kali
```bash
apt-get install python3-pip
git clone https://www.github.com/threat9/routersploit
cd routersploit
python3 -m pip install -r requirements.txt
python3 rsf.py
```
Bluetooth Low Energy support:

```bash 
sudo apt-get install libglib2.0-dev
python3 -m pip install bluepy
python3 rsf.py
```



# Ubuntu
```bash
sudo add-apt-repository universe
sudo apt-get install git python3-pip
git clone https://www.github.com/threat9/routersploit
cd routersploit
python3 -m pip install setuptools
python3 -m pip install -r requirements.txt
python3 rsf.py
```
## BLE Support
```bash
apt-get install libglib2.0-dev
python3 -m pip install bluepy
python3 rsf.py
```