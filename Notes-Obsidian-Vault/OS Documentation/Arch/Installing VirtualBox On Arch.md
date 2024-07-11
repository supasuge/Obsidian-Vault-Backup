## Table of Contents

- [Installing VirtualBox on Arch Linux](#Installing\VirtualBox\on\Arch\Linux)
      - [Step 1)  Install VirtualBox package](#Step\1) \Install\VirtualBox\package)

### Installing VirtualBox on Arch Linux

To get VirtualBox on your system, follow the steps below.

#### Step 1)  Install VirtualBox package

Installing VirtualBox is as easy as it gets. Just launch the terminal and a s a sudo user, simply run the command:

```bash
sudo pacman -S virtualbox virtualbox-guest-iso
```

Next, add the current user to the **vboxusers** group by running the command:

```bash
$ sudo gpasswd -a $USER vboxusers
```


Next, load the virtualbox kernel module using the command:

```bash
sudo modprobe vboxdrv
```

Update your system
```bash
yay -Syy
```


Install the virtulabox extension pack using the command:
```bash
yay -S virtualbox-ext-oracle
```


You also need to enable vboxweb for it to start on boot and start the service
```bash
sudo systemctl enable vboxweb.service
sudo systemctl start vboxweb.service
```

Perfect, Service also got started successfully. Let’s verify the VirtualBox Kernel module is loaded or not, run below **[lsmod command](https://www.linuxtechi.com/how-to-manage-kernel-modules-in-linux/)**
```bash
lsmod | grep -i vbox
```
- [Resources](https://linuxiac.com/how-to-install-virtualbox-on-arch-linux/)








