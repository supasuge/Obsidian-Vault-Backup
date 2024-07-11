## Table of Contents

        - [Connect to the net work](#Connect\to\the\net\work)


`ip link` - Check network interfaces
- For wireless and WWAN, make sure the card is not blocked with `rfkill`

##### Connect to the net work
- Ethernet
	*Plug in the cable*
- Wi-FI
	`iwctl`
	- `station wlan0 connect <SSID>`
- Mobile Broadband
	`mmcli`

- [DHCP](https://wiki.archlinux.org/title/DHCP "DHCP"): dynamic IP address and DNS server assignment (provided by [systemd-networkd](https://wiki.archlinux.org/title/Systemd-networkd "Systemd-networkd") and [systemd-resolved](https://wiki.archlinux.org/title/Systemd-resolved "Systemd-resolved")) should work out of the box for Ethernet, WLAN, and WWAN network interfaces.
    - Static IP address: follow [Network configuration#Static IP address](https://wiki.archlinux.org/title/Network_configuration#Static_IP_address "Network configuration").
- The connection may be verified with [ping](https://wiki.archlinux.org/title/Ping "Ping"):
    
`ping archlinux.org`


