## Table of Contents

    - [`lsblk` Cheat Sheet](#`lsblk`\Cheat\Sheet)
      - [Basic Information](#Basic\Information)
      - [Common Usage and Options](#Common\Usage\and\Options)
      - [Examples](#Examples)
      - [Tips](#Tips)

### `lsblk` Cheat Sheet

#### Basic Information

- **Command**: `lsblk`
- **Purpose**: Lists information about all available block devices.
- **Commonly Used For**: Identifying device names, sizes, and partition layouts.
#### Common Usage and Options
1. **List All Block Devices**
- `lsblk`
    - Displays all block devices in a tree-like format.
- **Display More Information**
- `lsblk -f`
    - Shows filesystem type, mount point, and UUID.
- **Exclude Certain Types**
- `lsblk -e 7`
    - Excludes devices of a specified major number (e.g., 7 for loop devices).
- **Output as List Instead of Tree**
- `lsblk -l`
    - Displays devices in a list format, not as a tree.
- **Include Empty Devices*
- `lsblk -a`
    - Shows all devices, including those not holding any media.
- **Show Specific Columns**
1. `lsblk -o NAME,SIZE,FSTYPE,UUID,MOUNTPOINT`
    - Customizes the output to show only specified columns.

#### Examples
- **Identify USB Drive Name**
    - Plug in a USB drive.
    - Run `lsblk`.
    - Look for a new device, usually named `sdX`, where X is a letter.
- **Check Filesystem Type**
    - `lsblk -f`
    - Find the device of interest and check its `FSTYPE` column.
#### Tips
- `lsblk` is especially useful for identifying the correct device file before using commands like `fdisk` or `mount`.
- Regularly use `lsblk` to verify the status of disks, especially when adding new hardware or making changes.