## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Fdisk](#Fdisk)
    - [Basic Commands](#Basic\Commands)
    - [Within Fdisk Interactive Mode](#Within\Fdisk\Interactive\Mode)
    - [Example Workflow](#Example\Workflow)
  - [Parted](#Parted)
    - [Basic Usage](#Basic\Usage)
    - [Example Workflow](#Example\Workflow)
  - [Lsblk](#Lsblk)
    - [Basic Usage](#Basic\Usage)
    - [Example](#Example)
  - [Df](#Df)
    - [Basic Usage](#Basic\Usage)
    - [Example](#Example)
  - [Advanced Examples](#Advanced\Examples)
    - [Resizing a Partition](#Resizing\a\Partition)
    - [Formatting a Partition](#Formatting\a\Partition)
    - [Mounting a Partition](#Mounting\a\Partition)
    - [Unmounting a Partition](#Unmounting\a\Partition)
  - [Tips](#Tips)

## Table of Contents

  - [Fdisk](#Fdisk)
    - [Basic Commands](#Basic\Commands)
    - [Within Fdisk Interactive Mode](#Within\Fdisk\Interactive\Mode)
    - [Example Workflow](#Example\Workflow)
  - [Parted](#Parted)
    - [Basic Usage](#Basic\Usage)
    - [Example Workflow](#Example\Workflow)
  - [Lsblk](#Lsblk)
    - [Basic Usage](#Basic\Usage)
    - [Example](#Example)
  - [Df](#Df)
    - [Basic Usage](#Basic\Usage)
    - [Example](#Example)
  - [Advanced Examples](#Advanced\Examples)
    - [Resizing a Partition](#Resizing\a\Partition)
    - [Formatting a Partition](#Formatting\a\Partition)
    - [Mounting a Partition](#Mounting\a\Partition)
    - [Unmounting a Partition](#Unmounting\a\Partition)
  - [Tips](#Tips)

Linux offers a variety of command-line tools for disk management. This guide covers the usage of `fdisk`, `parted`, `lsblk`, and `df` commands.

## Fdisk
> Fdisk is a powerful disk partitioning tool.
### Basic Commands
- **List Disks**: `sudo fdisk -l`
- **Select Disk for Operation**: `sudo fdisk /dev/sdx`
    - Replace `/dev/sdx` with your disk, e.g., `/dev/sda`.

### Within Fdisk Interactive Mode
- **Create Partition**: `n`
- **Delete Partition**: `d`
- **Write Changes and Exit**: `w`
- **Quit without Saving**: `q`
### Example Workflow
1. List all disks: `sudo fdisk -l`
2. Select a disk: `sudo fdisk /dev/sda`
3. Create a new partition: `n`
4. Follow prompts to specify partition type, size, etc.
5. Write changes: `w`
## Parted
Parted is another tool for managing hard disk partitions.
### Basic Usage
- **List Partitions on All Disks**: `sudo parted -l`
- **Select Disk**: `sudo parted /dev/sdx`
- **Create Partition**: `(parted) mkpart`
- **Delete Partition**: `(parted) rm [number]`

### Example Workflow

1. Start Parted on a specific disk: `sudo parted /dev/sda`
2. Create a new partition: `mkpart primary ext4 1MB 500MB`
3. List partitions to verify: `print`

## Lsblk

Lsblk lists information about block devices.

### Basic Usage

- **List All Block Devices**: `lsblk`
- **Include Mount Points**: `lsblk -f`

### Example

- Check the block device's name and mount points: `lsblk -f`

## Df

Df command shows disk space usage.

### Basic Usage

- **Show Disk Usage**: `df`
- **In Human-Readable Format**: `df -h`

### Example

- Check disk space usage for all mount points: `df -h`

## Advanced Examples

### Resizing a Partition

1. Resize a partition with `parted`: `sudo parted /dev/sda`
2. Resize command: `resizepart [number] [end]`

### Formatting a Partition

1. Format a partition: `sudo mkfs.ext4 /dev/sda1`
    - Replace `ext4` with your preferred filesystem.

### Mounting a Partition

1. Create a mount point: `sudo mkdir /mnt/mydisk`
2. Mount a partition: `sudo mount /dev/sda1 /mnt/mydisk`

### Unmounting a Partition

- Unmount a partition: `sudo umount /mnt/mydisk`

## Tips

- Always back up data before modifying disk partitions.
- Use `lsblk` or `fdisk -l` to identify disks before modifying them.
- Be cautious with partition and filesystem commands to avoid data loss.****