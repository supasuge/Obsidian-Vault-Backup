## Table of Contents

- [PowerShell Filesystem Manipulation Cheatsheet](#powershell\filesystem\manipulation\cheatsheet)
  - [Navigating Directories](#Navigating\Directories)
  - [File Operations](#File\Operations)
  - [Directory Operations](#Directory\Operations)
  - [File Content](#File\Content)
  - [File and Directory Permissions](#File\and\Directory\Permissions)
  - [Searching](#Searching)
  - [Miscellaneous](#Miscellaneous)

Certainly! Below is a concise cheatsheet for filesystem manipulation and management using PowerShell, formatted in Markdown. This covers common tasks like file and directory operations, file content manipulation, and permissions handling. 

---

# PowerShell Filesystem Manipulation Cheatsheet

## Navigating Directories
- **List contents**: `Get-ChildItem`
- **Change directory**: `Set-Location <path>`
- **Get current directory**: `Get-Location`

## File Operations
- **Create a new file**: `New-Item <file_name> -ItemType File`
- **Copy a file**: `Copy-Item <source> <destination>`
- **Move a file**: `Move-Item <source> <destination>`
- **Delete a file**: `Remove-Item <file_name>`
- **Rename a file**: `Rename-Item <old_name> <new_name>`

## Directory Operations
- **Create a new directory**: `New-Item <directory_name> -ItemType Directory`
- **Copy a directory**: `Copy-Item <source_directory> <destination_directory> -Recurse`
- **Move a directory**: `Move-Item <source_directory> <destination_directory>`
- **Delete a directory**: `Remove-Item <directory_name> -Recurse`
- **Rename a directory**: `Rename-Item <old_name> <new_name>`

## File Content
- **Read file content**: `Get-Content <file_name>`
- **Write to a file**: `Set-Content <file_name> -Value <content>`
- **Append to a file**: `Add-Content <file_name> -Value <content>`

## File and Directory Permissions
- **Get ACL (Access Control List)**: `Get-Acl <file_or_directory_name>`
- **Set ACL**: `Set-Acl <file_or_directory_name> -AclObject <acl_object>`
## Searching
- **Find files/directories by name**: `Get-ChildItem -Path <path> -Recurse | Where-Object { $_.Name -like "<pattern>" }`
- **Find files containing specific text**: `Select-String -Path <file_pattern> -Pattern <search_pattern>`
## Miscellaneous
- **Show file properties**: `Get-ItemProperty <file_name>`
- **Compare two files**: `Compare-Object (Get-Content <file1>) (Get-Content <file2>)`
---
