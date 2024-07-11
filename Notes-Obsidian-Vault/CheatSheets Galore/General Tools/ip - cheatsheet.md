### IP Command Cheat Sheet

The `ip` command is a powerful tool for network configuration in Linux. It can manage and display the status of network interfaces, IP addresses, routing, and tunnels. Below is a detailed cheat sheet with explanations for various options and commands.

---

#### General Syntax

```
ip [OPTIONS] OBJECT {COMMAND | help}
```

- **OBJECT**: Specifies what aspect of the network to operate on (e.g., link, addr, route).
- **OPTIONS**: Modifiers for the command (e.g., `-s`, `-o`, `-r`).

---

### Common Objects and Commands

#### `ip link`
Manage network devices.

- **Set device up/down**
  ```sh
  ip link set dev eth0 up
  ip link set dev eth0 down
  ```

- **Change device attributes**
  ```sh
  ip link set dev eth0 mtu 1400
  ip link set dev eth0 txqueuelen 1000
  ```

- **Display device attributes**
  ```sh
  ip link show
  ip link show dev eth0
  ```

#### `ip addr`
Manage IP addresses on devices.

- **Add/Delete IP address**
  ```sh
  ip addr add 192.168.1.10/24 dev eth0
  ip addr del 192.168.1.10/24 dev eth0
  ```

- **Show/Flush IP addresses**
  ```sh
  ip addr show
  ip addr flush dev eth0
  ```

#### `ip route`
Manage routing tables.

- **Add/Delete route**
  ```sh
  ip route add 192.168.1.0/24 via 192.168.1.1
  ip route del 192.168.1.0/24
  ```

- **Show routing table**
  ```sh
  ip route show
  ```

#### `ip rule`
Manage routing policy database rules.

- **Add/Delete rule**
  ```sh
  ip rule add from 192.168.1.0/24 table 100
  ip rule del from 192.168.1.0/24
  ```

- **Show rules**
  ```sh
  ip rule show
  ```

#### `ip neigh`
Manage neighbor cache (ARP for IPv4, NDP for IPv6).

- **Add/Delete neighbor**
  ```sh
  ip neigh add 192.168.1.1 lladdr 00:11:22:33:44:55 dev eth0
  ip neigh del 192.168.1.1 dev eth0
  ```

- **Show/Flush neighbors**
  ```sh
  ip neigh show
  ip neigh flush dev eth0
  ```

#### `ip tunnel`
Manage tunnels.

- **Add/Delete tunnel**
  ```sh
  ip tunnel add tun0 mode gre remote 192.168.2.1 local 192.168.1.1 ttl 255
  ip tunnel del tun0
  ```

- **Show tunnels**
  ```sh
  ip tunnel show
  ```

#### `ip maddr`
Manage multicast addresses.

- **Add/Delete multicast address**
  ```sh
  ip maddr add 224.0.0.1 dev eth0
  ip maddr del 224.0.0.1 dev eth0
  ```

- **Show multicast addresses**
  ```sh
  ip maddr show
  ```

#### `ip mroute`
Manage multicast routing cache.

- **Show multicast routes**
  ```sh
  ip mroute show
  ```

#### `ip monitor`
Monitor state changes.

- **Monitor devices, addresses, and routes**
  ```sh
  ip monitor all
  ```

#### `ip xfrm`
Manage IPsec policies and states.

- **Add/Delete state**
  ```sh
  ip xfrm state add src 192.168.1.1 dst 192.168.2.1 proto esp spi 0x100 mode transport
  ip xfrm state del src 192.168.1.1 dst 192.168.2.1 proto esp spi 0x100
  ```

- **Add/Delete policy**
  ```sh
  ip xfrm policy add src 192.168.1.0/24 dst 192.168.2.0/24 dir out tmpl src 192.168.1.1 dst 192.168.2.1 proto esp reqid 1 mode transport
  ip xfrm policy del src 192.168.1.0/24 dst 192.168.2.0/24 dir out
  ```

---

### Common Options

- **-V, --version**: Print the version of the `ip` utility.
- **-s, --statistics**: Output more detailed information.
- **-f, --family**: Specify protocol family (`inet`, `inet6`, `link`).
- **-o, --oneline**: Output each record on a single line.
- **-r, --resolve**: Use the system's name resolver to print DNS names.

### Examples

#### Bringing an Interface Up/Down

```sh
# Bring up the interface eth0
ip link set eth0 up

# Bring down the interface eth0
ip link set eth0 down
```

#### Configuring IP Address

```sh
# Add an IP address to the interface eth0
ip addr add 192.168.1.10/24 dev eth0

# Remove an IP address from the interface eth0
ip addr del 192.168.1.10/24 dev eth0

# Show IP addresses assigned to all interfaces
ip addr show
```

#### Managing Routes

```sh
# Add a route to the network 192.168.1.0/24 via the gateway 192.168.1.1
ip route add 192.168.1.0/24 via 192.168.1.1

# Delete the route to the network 192.168.1.0/24
ip route del 192.168.1.0/24

# Show the routing table
ip route show
```

#### Managing Neighbors (ARP)

```sh
# Add a neighbor entry
ip neigh add 192.168.1.1 lladdr 00:11:22:33:44:55 dev eth0

# Delete a neighbor entry
ip neigh del 192.168.1.1 dev eth0

# Show neighbor entries
ip neigh show
```

#### Using Monitor

```sh
# Monitor the state of devices, addresses, and routes
ip monitor all
```

### Tips

- **Batch Operations**: Avoid changing multiple parameters in a single command to prevent unpredictable states if an error occurs.
- **Safety with Flush**: Be cautious with flush commands; mistakes can result in removing all addresses or routes.
