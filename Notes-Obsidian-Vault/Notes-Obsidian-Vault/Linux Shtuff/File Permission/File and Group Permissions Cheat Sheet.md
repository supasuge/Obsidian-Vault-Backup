## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Understanding Permissions](#Understanding\Permissions)
  - [Viewing Permissions](#Viewing\Permissions)
  - [Changing Permissions](#Changing\Permissions)
  - [Numeric Mode](#Numeric\Mode)
  - [Changing Ownership](#Changing\Ownership)
  - [Changing Group Ownership](#Changing\Group\Ownership)
  - [Special Permissions](#Special\Permissions)
  - [Working with Groups](#Working\with\Groups)
  - [File Access Control Lists (ACLs)](#File\Access\Control\Lists\(ACLs))

## Table of Contents

  - [Understanding Permissions](#Understanding\Permissions)
  - [Viewing Permissions](#Viewing\Permissions)
  - [Changing Permissions](#Changing\Permissions)
  - [Numeric Mode](#Numeric\Mode)
  - [Changing Ownership](#Changing\Ownership)
  - [Changing Group Ownership](#Changing\Group\Ownership)
  - [Special Permissions](#Special\Permissions)
  - [Working with Groups](#Working\with\Groups)
  - [File Access Control Lists (ACLs)](#File\Access\Control\Lists\(ACLs))

## Understanding Permissions
- **File Types**
  - `-`: Regular file
  - `d`: Directory
  - `l`: Symbolic link
___
- **Permission Types**
  - `r`: Read
  - `w`: Write
  - `x`: Execute
____ 
- **Permission Structure**
  - `user-group-others` (e.g., `rw-r--r--`)
___
## Viewing Permissions
- **List files with permissions**
  ```bash
  ls -l
  ```
___
## Changing Permissions
- **chmod**: Modify file access rights
  - Syntax: `chmod [options] mode file`
  - Examples:
    - *Grant execute permission to the user*:
      ```bash
chmod u+x filename
      ```
    - *Set read and write permissions for user and group*:
      ```bash
chmod ug+rw filename
      ```
    - *Remove execute permissions from others*:
      ```bash
chmod o-x filename
      ```
___
## Numeric Mode 
- **chmod with Numeric Mode**
  - Syntax: `chmod [options] numeric_mode file`
  - Examples:
    - Set permissions to `rw-r--r--` (644):
      ```bash
chmod 644 filename
      ```
    - Set permissions to `rwxr-xr-x` (755):
      ```bash
chmod 755 filename
      ```
___
## Changing Ownership
- **chown**: Change file owner and group
  - Syntax: `chown [options] user[:group] file`
  - Examples:
    - Change owner to `john`:
      ```bash
chown john filename
      ```
    - Change owner to `john` and group to `developers`:
      ```bash
chown john:developers filename
      ```
____
## Changing Group Ownership
- **chgrp**: Change group ownership
  - Syntax: `chgrp [options] group file`
  - Examples:
    - Change group to `admin`:
      ```bash
chgrp admin filename
      ```
___
## Special Permissions
- **Setuid, Setgid, and Sticky Bit**
  - **setuid** `(4)`: Executable runs as the user who owns the file
  - **setgid** `(2)`: Executable runs as the group that owns the file
  - **Sticky bit** `(1)`: Restricts deletion of files
  - Examples:
    - Set `setuid` on a file:
      ```bash
      chmod 4755 filename
      ```
    - Set `setgid` on a directory:
      ```bash
      chmod 2775 dirname
      ```
    - Set Sticky bit on a directory:
      ```bash
      chmod 1777 dirname
      ```
___
## Working with Groups
- **groupadd**: Create a new group
  - Syntax: `groupadd [options] groupname`
  - Example:
    ```bash
    groupadd developers
    ```
- **usermod**: Modify user's group membership
  - Syntax: `usermod -aG groupname username`
  - Example:
    ```bash
    usermod -aG developers john
    ```
- **groups**: Display groups for a user
  - Syntax: `groups username`
  - Example:
    ```bash
    groups john
    ```
___
## File Access Control Lists (ACLs)
- **setfacl**: Set file ACLs
  - Syntax: `setfacl -m u:user:permission file`
  - Example:
    - Grant `rw` access to user `mary` on `filename`:
      ```bash
      setfacl -m u:mary:rw filename
      ```
- **getfacl**: Get file ACLs
  - Syntax: `getfacl file`
  - Example:
    ```bash
    getfacl filename
    ```
___

