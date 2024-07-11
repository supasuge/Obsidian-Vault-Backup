## Table of Contents

  - [**1. Enable the optional feature from an Administrator PowerShell first**](#**1.\Enable\the\optional\feature\from\an\Administrator\PowerShell\first**)
  - [2. Set up user and packages](#2.\Set\up\user\and\packages)
- [Setting up your dev environment to use WSL](#setting\up\your\dev\environment\to\use\wsl)
- [install and confiugre firewall](#install\and\confiugre\firewall)
  - [3. User Management](#3.\User\Management)
  - [4. Configure VS Code (or IDE of your choice) to use bash or wsl for interactive terminal](#4.\Configure\VS\Code\(or\IDE\of\your\choice)\to\use\bash\or\wsl\for\interactive\terminal)
  - [5. Create a link from your WSL to your C drive](#5.\Create\a\link\from\your\WSL\to\your\C\drive)
          - [6. Verify](#6.\Verify)
    - [Installing Git](#Installing\Git)

[WSL Documentation](https://learn.microsoft.com/en-us/windows/wsl/)

## **1. Enable the optional feature from an Administrator PowerShell first**

Open PowerShell as Administrator (search for PowerShell from the start menu, right click instead of clicking to open, and choose to open as Administrator). Type the following:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
- Restart Computer
## 2. Set up user and packages
# Setting up your dev environment to use WSL
```bash
sudo apt update && sudo apt upgrade
# install and confiugre firewall
sudo apt install ufw
    # Enable
sudu ufw enabled
sudo default deny incoming
sudo default allow outgoing
sudo ufw allow ssh
```

## 3. User Management
- Use strong passwords for all user accoutns
- Always consider creating non-root users for daily operations
- ```bash
```bash
sudo adduser [name] #addsuser
sudo usermod -aG suod [username]
```
## 4. Configure VS Code (or IDE of your choice) to use bash or wsl for interactive terminal

In VS Code, search settings for “integrated.shell”, and replace the line with Powershell with this:

`“terminal.integrated.shell.windows”: “C:\\WINDOWS\\System32\\wsl.exe”`

If you are using 32-bit VS Code, you will have to reference bash.exe in sysnative instead of wsl:

`“terminal.integrated.shell.windows”: “C:\\WINDOWS\\sysnative\\bash.exe”
`


## 5. Create a link from your WSL to your C drive
`ln` - User to create a link to your C drive from your Ubuntu home directory. Device where you want to put your devlopment project files in your C drive. For this example, let's assume you want 
1. got to home dir(`~`) - `cd`
2. Create the "C:\\IDK" directory fron the WSL:
```bash
mkdir /mnt/c/projects
```

###### 6. Verify
Verify this change was made completely by running:
`ls -l /mnt`

### Installing Git
```bash
sudo apt-get install git

#GitConfigSetup
git config --global user.name "yaditydo"
git config --global user.email "epardo1742@proton.me"
```





