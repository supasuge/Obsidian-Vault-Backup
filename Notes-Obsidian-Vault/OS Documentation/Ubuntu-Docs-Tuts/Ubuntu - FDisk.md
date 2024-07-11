## Table of Contents

- [Fdisk Command Guide for Ubuntu](#fdisk\command\guide\for\ubuntu)
  - [Basic Information](#Basic\Information)
  - [Common Operations](#Common\Operations)
    - [Listing Partitions](#Listing\Partitions)
    - [Viewing Disk Details](#Viewing\Disk\Details)
    - [Entering Interactive Mode](#Entering\Interactive\Mode)
  - [Interactive Mode Commands](#Interactive\Mode\Commands)
  - [Examples](#Examples)
    - [Creating a New Partition](#Creating\a\New\Partition)
    - [Deleting a Partition](#Deleting\a\Partition)
    - [Changing a Partition Type](#Changing\a\Partition\Type)
  - [Tips](#Tips)

# Fdisk Command Guide for Ubuntu

`fdisk` is a powerful command-line utility used for disk partitioning in Linux systems like Ubuntu. This guide provides a concise overview of its usage with practical examples.

## Basic Information

- **Command**: `fdisk`
- **Location**: Typically found in `/sbin/fdisk`
- **Permission**: Requires root access
- **Usage**: `sudo fdisk [options] <disk>`
- **Supported File Systems**: Ext2, Ext3, Ext4, Linux swap, and more.

## Common Operations
### Listing Partitions
`sudo fdisk -l`
Lists all partitions on all disks.
### Viewing Disk Details
`sudo fdisk -l /dev/sda`

Shows detailed information about the `/dev/sda` disk.

### Entering Interactive Mode
`sudo fdisk /dev/sda`

Opens interactive mode for `/dev/sda` disk.

## Interactive Mode Commands

- `m`: Display help menu.
- `p`: Print partition table.
- `n`: Add a new partition.
- `d`: Delete a partition.
- `t`: Change a partition's system id.
- `w`: Write table to disk and exit.
- `q`: Quit without saving changes.

## Examples

### Creating a New Partition

1. Start `fdisk` on a specific disk:
        
1. `sudo fdisk /dev/sda`
2. Press `n` to create a new partition.
3. Choose partition type: primary (`p`) or extended (`e`).
4. Specify partition number and size.
5. Press `w` to write changes to disk.

### Deleting a Partition
1. Enter `fdisk` on the target disk:    

1. `sudo fdisk /dev/sda`
2. Press `d` to delete a partition.
3. Select the partition number to delete.
4. Confirm and press `w` to save changes.

### Changing a Partition Type
1. Launch `fdisk`:

1. `sudo fdisk /dev/sda`
    
2. Press `t` to change a partition's system id.
3. Enter the partition number.
4. Enter the hexadecimal code for the new type.
5. Press `w` to write the changes.

## Tips

- Always back up data before modifying disk partitions.
- Use `lsblk` to identify the correct disk before using `fdisk`.
- Double-check partition numbers and sizes to avoid data loss.****

