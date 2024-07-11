## Table of Contents

- [Diskpart Cheatsheet](#diskpart\cheatsheet)
  - [Understanding Partitions](#Understanding\Partitions)
  - [Opening Diskpart](#Opening\Diskpart)
  - [Basic Commands](#Basic\Commands)
    - [List Disks](#List\Disks)
    - [Select Disk](#Select\Disk)
    - [List Partitions](#List\Partitions)
    - [Select Partition](#Select\Partition)
    - [Creating a Partition](#Creating\a\Partition)
    - [Formatting a Partition](#Formatting\a\Partition)
    - [Deleting a Partition](#Deleting\a\Partition)
    - [Extending a Partition](#Extending\a\Partition)
    - [Shrinking a Partition](#Shrinking\a\Partition)
  - [Example Workflow](#Example\Workflow)
    - [Formatting a New Drive](#Formatting\a\New\Drive)
    - [Resizing a Partition](#Resizing\a\Partition)
  - [Advanced Diskpart Commands](#Advanced\Diskpart\Commands)
    - [Convert a Disk to GPT](#Convert\a\Disk\to\GPT)
    - [Convert a Disk to MBR](#Convert\a\Disk\to\MBR)
    - [Clean a Disk](#Clean\a\Disk)
    - [Create a Logical Partition](#Create\a\Logical\Partition)
    - [Create an Extended Partition](#Create\an\Extended\Partition)
    - [Assigning a Drive Letter](#Assigning\a\Drive\Letter)
    - [Removing a Drive Letter](#Removing\a\Drive\Letter)
    - [Setting Active Partition](#Setting\Active\Partition)
    - [Details of a Disk](#Details\of\a\Disk)
    - [Details of a Partition](#Details\of\a\Partition)
  - [Advanced Usage Scenarios](#Advanced\Usage\Scenarios)
    - [Preparing a New Hard Drive](#Preparing\a\New\Hard\Drive)
    - [Setting Up a Bootable Partition](#Setting\Up\a\Bootable\Partition)
    - [Cleaning a Disk for Disposal](#Cleaning\a\Disk\for\Disposal)
    - [Additional Tips](#Additional\Tips)
- [Drive letters](#drive\letters)
    - [Partition Types](#Partition\Types)
      - [MBR vs. GPT](#MBR\vs.\GPT)

# Diskpart Cheatsheet

## Understanding Partitions
A *partition* is a distinct region of a hard drive or a solid-state drive. It's a subdivision or a "room" within a drive. Each partition can be formatted with a different file system. (i.e., NTFS, FAT32, exFAT) and can have different operating systems installed on them.
**Benefits**:
- Organization/Optimization: Separating different types of data so they are more organized and more efficient
- Running different OS's


## Opening Diskpart
- Open Command Prompt as an administrator.
- Type `diskpart` and press Enter.

## Basic Commands

### List Disks
- `list disk`
  - Lists all the storage devices connected to your computer.

### Select Disk
- `select disk [number]`
  - Replace `[number]` with the disk number you want to work on.

### List Partitions
- `list partition`
  - Lists the partitions on the selected disk.

### Select Partition
- `select partition [number]`
  - Replace `[number]` with the partition number.

### Creating a Partition
- `create partition primary size=[size in MB]`
  - Replace `[size in MB]` with the size of the new partition.

### Formatting a Partition
- `format fs=ntfs label="[label]" quick`
  - Replace `[label]` with your desired label for the partition.

### Deleting a Partition
- `delete partition`
  - Deletes the selected partition.

### Extending a Partition
- `extend size=[size in MB]`
  - Replace `[size in MB]` with the additional size in megabytes.

### Shrinking a Partition
- `shrink desired=[size in MB]`
  - Replace `[size in MB]` with the size to reduce.

## Example Workflow

### Formatting a New Drive
1. `list disk`
2. `select disk 1`
3. `clean`
4. `create partition primary`
5. `format fs=ntfs quick`
6. `assign letter=D`
   - Assigns a drive letter.

### Resizing a Partition
1. `select disk 0`
2. `list partition`
3. `select partition 2`
4. `shrink desired=10000`
   - Shrinks by 10 GB.
5. `extend size=5000`
   - Extends by 5 GB.

## Advanced Diskpart Commands

### Convert a Disk to GPT
- `convert gpt`
  - Converts the selected disk to the GPT partition style. Ensure the disk does not contain important data as this process can erase the disk.

### Convert a Disk to MBR
- `convert mbr`
  - Converts the selected disk to the MBR partition style. Similar to GPT conversion, this will erase the disk.

### Clean a Disk
- `clean`
  - Removes all partitions and data from the selected disk.

### Create a Logical Partition
- `create partition logical size=[size in MB]`
  - Replace `[size in MB]` with the size for the new logical partition. Logical partitions are typically created within an extended partition.

### Create an Extended Partition
- `create partition extended size=[size in MB]`
  - Replace `[size in MB]` with the size for the new extended partition.

### Assigning a Drive Letter
- `assign letter=[letter]`
  - Assigns a drive letter to the selected partition. Replace `[letter]` with your preferred drive letter.

### Removing a Drive Letter
- `remove letter=[letter]`
  - Removes the assigned drive letter from the selected partition.

### Setting Active Partition
- `active`
  - Marks the selected partition as active. This is crucial for boot partitions on MBR disks.

### Details of a Disk
- `detail disk`
  - Provides detailed information about the selected disk, including partitions and volumes.

### Details of a Partition
- `detail partition`
  - Provides detailed information about the selected partition.

## Advanced Usage Scenarios

### Preparing a New Hard Drive
1. `list disk`
2. `select disk [number]`
3. `clean`
4. `convert gpt` (or `convert mbr` depending on your requirement)
5. `create partition primary size=50000` (50 GB for OS)
6. `format fs=ntfs quick`
7. `assign letter=C`
8. `create partition extended size=100000` (100 GB for data)
9. `create partition logical size=100000`
10. `format fs=ntfs quick`
11. `assign letter=D`

### Setting Up a Bootable Partition
1. `select disk 0`
2. `list partition`
3. `select partition [number]` (select the partition intended for OS installation)
4. `active`
5. `format fs=ntfs quick`

### Cleaning a Disk for Disposal
1. `select disk [number]`
2. `clean`
   - This will remove all partitions and data, making it safer for disposal or resale.

> **Warning:** The `clean` command will erase all data on the disk without any confirmation prompts. Use it with utmost caution.

### Additional Tips
- Always double-check the disk and partition numbers before executing commands.
- It's recommended to use the `list disk` and `list partition` commands frequently to confirm selections.
- Be aware of the differences between `clean` (which removes partition and volume formatting) and `format` (which prepares a partition for use by writing a new file system).

# Drive letters
I windows OS, drive letters are a method of labeling different storage devices and their partitions.
- Makes it easier to access and manage storage areas in Windows
- Assignment: By default, the system drive (where Windows is installed) is usually C:. Other drives, like external hard drives, USB drives, and additional partitions are all assigned letters.

### Partition Types
Two main types:
- Logical partitions
- Primary partitions

**Extended Partitions**:
- Container that can hold multiple logical partitions
- Used when more than four partitions are need on an MBR disk
- Cannot be used directly, they act as a placeholder for logical partitions


**Logical Partitions**:
- Contained Within: Logical partitions exist within an extended partition
- Functionality: They function like primary partitions but are used primarily for data storage, not for booting an OS.
- Flexibility: You can create several logical partitions within a single partition


#### MBR vs. GPT

- **MBR (Master Boot Record)**: An older partition style. MBR has limitations like a maximum disk size of 2 TB and supports only four primary partitions.
- **GPT (GUID Partition Table)**: A newer partition style with advantages like support for disks larger than 2 TB, and it can have up to 128 primary partitions in Windows.



