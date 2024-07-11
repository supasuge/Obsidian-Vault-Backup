## Table of Contents

- [Minimal Hyprland installation on a fresh Archlinux](#minimal\hyprland\installation\on\a\fresh\archlinux)
- [What is [Hyprland](https://hyprland.org/)](#what\is\[hyprland](https://hyprland.org/))
  - [Install yay](#Install\yay)
        - [About AUR helper](#About\AUR\helper)
- [Install the following for installation with DKMS](#install\the\following\for\installation\with\dkms)

# Minimal Hyprland installation on a fresh Archlinux

Simple guide to setup Hyprland on Linux (Archlinux) machine 💻 #1

March 30, 2023 [Hyprland](https://blog.cschad.com/tags/hyprland/)[wayland](https://blog.cschad.com/tags/wayland/)

[Source](https://blog.cschad.com/de/posts/hyprland_installation_archlinux/)

![hypr1](https://blog.cschad.com/img/hypr1.webp)

**Table of Contents**
- [What is](https://blog.cschad.com/posts/hyprland_installation_archlinux/#what-is-hyprlandhttpshyprlandorg) [Hyprland](https://hyprland.org/)
    - [Install yay](https://blog.cschad.com/posts/hyprland_installation_archlinux/#install-yay)
        - [About AUR helper](https://blog.cschad.com/posts/hyprland_installation_archlinux/#about-aur-helper)
- [Auto start Hyprland](https://blog.cschad.com/posts/hyprland_installation_archlinux/#auto-start-hyprland)
    - [](https://blog.cschad.com/posts/hyprland_installation_archlinux/#--configure-hyprlandhttpscschadcompostshyprland_configuration)[⚒️ Configure Hyprland](https://cschad.com/posts/hyprland_configuration/)
    
# What is [Hyprland](https://hyprland.org/)
Hyprland is a dynamic tiling wayland compositor written in `C++` that offers unique features like smooth animations, dynamic tiling and rounded corners.

## Install yay
##### About AUR helper
Aur Helper is a tool which help you download apps and binaries stored and uploaded in Arch User Repository thus, giving you access to a large in no packages which are not available in official pacman repo’s

```sh
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

Now let’s install Hyprland from the `AUR`

```sh
yay -S hyprland-git 
```

## V4
```bash
git clone https://github.com/SolDoesTech/HyprV4.git
cd HyprV4
./set_hypr
y

> Password
> Disable Wifi powersave: Y
> Install Packages: Y
> Copy config files: Y
> Activate Starship shell: Y
> Install Asus BOG Software: N

```


`sudo pacman -S linux-headers base-devel lm_sensors git dmidecode python-pyqt5 python-yaml python-argcomplete python-darkdetect`
# Install the following for installation with DKMS

`sudo pacman -S dkms openssl mokutil`

Troubleshooting:

- Got error `ERROR: Kernel configuration is invalid.`. Just reinstall kernel headers, e.g. in Debian:

```shell
sudo apt install --reinstall linux-headers-$(uname -r)
```



| Key Combination | Action |
| ---- | ---- |
| `Windows + Q` | Open the terminal (Kitty). |
| `Windows + F4` | Close the active window. |
| `Windows + L` | Lock the screen using swaylock. |
| `Windows + M` | Show the logout window using wlogout with `--protocol layer-shell`. |
| `Windows + SHIFT + M` | Exit Hyprland altogether (force quit Hyprland). |
| `Windows + E` | Open the graphical file browser (Thunar). |
| `Windows + V` | Toggle floating mode for a window. |
| `Windows + SPACE` | Show the graphical app launcher (wofi). |
| `Windows + P` | Set the window layout to pseudo/dwindle. |
| `Windows + J` | Toggle the split orientation in dwindle mode. |
| `Windows + S` | Take a screenshot with grim and slurp, and edit with swappy. |
| `ALT + V` | Open the clipboard manager, select an entry with wofi, and paste with `cliphist decode`. |
| `Windows + T` | Execute a custom script (`~/.config/HyprV/hyprv_util vswitch`) to switch HyprV version. |
| `Windows + Left Arrow` | Move focus left |
| `Windows + Right Arrow` | Move focus right |
| `Windows + Up Arrow` | Move focus up |
| `Windows + Down Arrow` | Move focus down |
| `Windows + 1` | Switch to workspace 1 |
| `Windows + 2` | Switch to workspace 2 |
| `Windows + 3` | Switch to workspace 3 |
| `Windows + 4` | Switch to workspace 4 |
| `Windows + 5` | Switch to workspace 5 |
| `Windows + 6` | Switch to workspace 6 |
| `Windows + 7` | Switch to workspace 7 |
| `Windows + 8` | Switch to workspace 8 |
| `Windows + 9` | Switch to workspace 9 |
| `Windows + 0` | Switch to workspace 10 |
| `Windows + Shift + 1` | Move active window to workspace 1 |
| `Windows + Shift + 2` | Move active window to workspace 2 |
| `Windows + Shift + 3` | Move active window to workspace 3 |
| `Windows + Shift + 4` | Move active window to workspace 4 |
| `Windows + Shift + 5` | Move active window to workspace 5 |
| `Windows + Shift + 6` | Move active window to workspace 6 |
| `Windows + Shift + 7` | Move active window to workspace 7 |
| `Windows + Shift + 8` | Move active window to workspace 8 |
| `Windows + Shift + 9` | Move active window to workspace 9 |
| `Windows + Shift + 0` | Move active window to workspace 10 |
| `Windows + Mouse Scroll Down` | Scroll to next workspace (down) |
| `Windows + Mouse Scroll Up` | Scroll to previous workspace (up) |
| `Windows + Left Mouse Button` | Move window (with left mouse button) |
| `Windows + Right Mouse Button` | Resize window (with right mouse button) |
|  |  |
