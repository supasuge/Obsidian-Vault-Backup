## Table of Contents

- [LinuxCommands](#linuxcommands)
          - [Unfinished.](#Unfinished.)

# LinuxCommands
###### Unfinished.
**Change the File Owner to Root**
```bash
sudo chown root:root <file_name>
```

**Set permissions so anyone cna execute, but only root user can read and write**
```bash
sudo chown 711 <file_name>
```
- `7` - Owner permissionsRead, write, and execute for the owner (root, in this case)
- `1` (Group Permissions) – Execute only for the group
- `1` (Others Permissions) – Execute only for others

```bash
#add IP to DNS resolve list
echo 'MACHINE_IP' | sudo tee -a /etc/hosts
```









