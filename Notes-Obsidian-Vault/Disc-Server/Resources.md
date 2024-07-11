## Table of Contents

    - [CTF Practice](#CTF\Practice)
- [Update and upgrade packages](#update\and\upgrade\packages)
- [Customize the terminal](#customize\the\terminal)
- [Install useful tools](#install\useful\tools)
- [Network configuration](#network\configuration)
- [Security hardening](#security\hardening)
- [Create output directories](#create\output\directories)

### CTF Practice
- [Cryptography](https://github.com/rishitsaiya/CTFlearn-Writeups/tree/master/Cryptography)
- [Binary](https://github.com/rishitsaiya/CTFlearn-Writeups/blob/master/Binary/README.md)
- [Forensics](https://github.com/rishitsaiya/CTFlearn-Writeups/tree/master/Forensics)
- [Programming](https://github.com/rishitsaiya/CTFlearn-Writeups/tree/master/Programming)
- [Reverse Engineering](https://github.com/rishitsaiya/CTFlearn-Writeups/tree/master/Reverse)
- [Web](https://github.com/rishitsaiya/CTFlearn-Writeups/tree/master/Web)
- [Crackme(Reverse Engineering)](https://crackme.re/)
-  [CyberChef](https://gchq.github.io/CyberChef/)
-  [PentestTools](https://pentest-tools.com/)
`Source: https://github.com/rishitsaiya`




Script for configuring Kali:
```bash
#!/bin/bash

# Update and upgrade packages
apt update
apt full-upgrade -y

# Customize the terminal
echo "Set up terminal..."
sed -i 's/bash/zsh/' /etc/passwd # Change to zsh shell
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" # Install Oh My Zsh
sed -i 's/ZSH_THEME=.*/ZSH_THEME="agnoster"/g' ~/.zshrc # Set agnoster theme  
echo 'export TERM="xterm-256color"' >> ~/.zshrc

# Install useful tools
echo "Installing tools..."
apt install -y bloodhound nmap sqlmap john hashcat hydra wfuzz dirbuster

# Network configuration  
echo "Configuring network..."
apt install openvpn # VPN for anonymity
apt install openssh-server # SSH for remote access

# Security hardening
echo "Hardening system..."
ufw enable # Enable firewall
passwd # Change default Kali password 

# Create output directories
echo "Setting up directories..."
mkdir ~/notes ~/scripts ~/tools

echo "Finished setting up Kali system"
```