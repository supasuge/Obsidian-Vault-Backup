## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Octal Permission Notation](#Octal\Permission\Notation)
    - [Examples](#Examples)
    - [Special Permissions](#Special\Permissions)
    - [Special Permission Examples](#Special\Permission\Examples)
    - [Table 1: Individual Permissions](#Table\1:\Individual\Permissions)
    - [Table 2: Combinations of Permissions](#Table\2:\Combinations\of\Permissions)

## Table of Contents

- [Octal Permission Notation](#Octal\Permission\Notation)
    - [Examples](#Examples)
    - [Special Permissions](#Special\Permissions)
    - [Special Permission Examples](#Special\Permission\Examples)
    - [Table 1: Individual Permissions](#Table\1:\Individual\Permissions)
    - [Table 2: Combinations of Permissions](#Table\2:\Combinations\of\Permissions)

| Permission | User Type | Numeric Value | Description |
|------------|-----------|---------------|-------------|
| Read (r)   | User (u)  | 4             | Allows reading the file or listing directory contents. |
| Write (w)  | Group (g) | 2             | Allows modifying the file or changing directory contents. |
| Execute (x)| Others (o)| 1             | Allows executing the file or accessing directory contents. |

### Octal Permission Notation
| User Category | Octal Notation | Permissions |
|---------------|----------------|-------------|
| User          | First digit    | Sum of permissions for the User. |
| Group         | Second digit   | Sum of permissions for the Group. |
| Others        | Third digit    | Sum of permissions for Others. |

### Examples
| chmod Command | Description |
|---------------|-------------|
| `chmod 700`     | Read, write, and execute for user; none for group and others. |
| `chmod 755`     | Full control for the owner; read and execute for group and others. |

### Special Permissions
| Special Permission | Numeric Value | Position in chmod | Description |
|--------------------|---------------|-------------------|-------------|
| Set User ID (SUID) | 4             | First digit of four-digit chmod | File executes with permissions of the file owner. |
| Set Group ID (SGID)| 2             | Second digit of four-digit chmod| File executes with permissions of the group; directories set group ID of new files. |
| Sticky Bit         | 1             | Third digit of four-digit chmod | In directories, restricts file deletion to the file's owner. |

### Special Permission Examples
| chmod Command | Description |
|---------------|-------------|
| `chmod 1777`    | Read, write, and execute for everyone; sticky bit set. |
| `chmod 2755`    | Read, write, and execute for owner; read and execute for group and others; SGID set. |

| Octal Value | Permission (User) | Permission (Group) | Permission (Others) | Symbolic Notation | Description |
|-------------|-------------------|--------------------|---------------------|-------------------|-------------|
| `0`           | No permission     | No permission      | No permission       | ---               | No permissions for user, group, or others |
| `1`           | No permission     | No permission      | Execute             | --x               | Execute permission for others only |
| `2`           | No permission     | No permission      | Write               | -w-               | Write permission for others only |
| `3`           | No permission     | No permission      | Write, Execute      | -wx               | Write and execute permissions for others |
| `4`           | No permission     | No permission      | Read                | r--               | Read permission for others only |
| `5`           | No permission     | No permission      | Read, Execute       | r-x               | Read and execute permissions for others |
| `6`           | No permission     | No permission      | Read, Write         | rw-               | Read and write permissions for others |
| `7`           | No permission     | No permission      | Read, Write, Execute| rwx               | Full permissions for others |
| `10`          | No permission     | Execute            | No permission       | --x               | Execute permission for group only |
| `20`          | No permission     | Write              | No permission       | -w-               | Write permission for group only |
| ...         | ...               | ...                | ...                 | ...               | ... |
| `70`          | Read, Write, Execute | No permission    | No permission       | rwx               | Full permissions for user only |
| `100`         | Read              | No permission      | No permission       | r--               | Read permission for user only |
| `200`         | Write             | No permission      | No permission       | -w-               | Write permission for user only |
| `300`         | Write, Read       | No permission      | No permission       | rw-               | Read and write permissions for user only |
| ...         | ...               | ...                | ...                 | ...               | ... |
| `777`         | Read, Write, Execute | Read, Write, Execute | Read, Write, Execute | rwxrwxrwx       | Full permissions for user, group, and others |
Sure! Below are two full-length markdown tables. The first table details the octal (numeric) values for individual permissions, and the second table shows the combinations of these permissions for user, group, and others using octal values:

### Table 1: Individual Permissions
| Permission     | Numeric Value |
|----------------|---------------|
| No permission  | `0`             |
| Execute only   | `1`             |
| Write only     | `2`             |
| Write & Execute| `3`             |
| Read only      | `4`             |
| Read & Execute | `5`             |
| Read & Write   | `6`             |
| All (Read, Write & Execute) | `7` |
### Table 2: Combinations of Permissions
| Octal Value | User Permissions | Group Permissions | Other Permissions | Symbolic Notation |
| ---- | ---- | ---- | ---- | ---- |
| `000` | No permission | No permission | No permission | --- --- --- |
| `001` | No permission | No permission | Execute | --- --- --x |
| `002` | No permission | No permission | Write | --- --- -w- |
| `003` | No permission | No permission | Write & Execute | --- --- -wx |
| `004` | No permission | No permission | Read | --- --- r-- |
| `005` | No permission | No permission | Read & Execute | --- --- r-x |
| `006` | No permission | No permission | Read & Write | --- --- rw- |
| `007` | No permission | No permission | All permissions | --- --- rwx |
| `010` | No permission | Execute | No permission | --- --x --- |
| `011` | No permission | Execute | Execute | --- --x --x |
| `770` | All permissions | All permissions | No permission | rwx rwx --- |
| `771` | All permissions | All permissions | Execute | rwx rwx --x |
| `772` | All permissions | All permissions | Write | rwx rwx -w- |
| `777` | All permissions | All permissions | All permissions | rwx rwx rwx |
These tables comprehensively cover all possible permission combinations. Remember, it's important to only grant necessary permissions to maintain system security.