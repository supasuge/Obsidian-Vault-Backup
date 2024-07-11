## Table of Contents



**Search File System**
```powershell
Get-ChildItem -Path C:\ -Recurse -Directory -ErrorAction SilentlyContinue | Where-Object { $_.Name -ieq "tools" }
```
- `Get-ChildItem -Path C:\` starts the search from the `C:\` directory.
- `-Recurse` ensures that the search is recursive, meaning it will search inside all subdirectories.
- `-Directory` ensures that only directories (and not files) are returned.
- `-ErrorAction SilentlyContinue` ensures that any errors (like permission denied) are ignored and the search continues.
- `Where-Object { $_.Name -ieq "tools" }` filters the results to only show directories named "tools". The `-ieq` operator is a case-insensitive equality check.

































**Install and Use Bloodhound-python**
```bash
#Install One Liner [Linux]
sudo apt update && sudo apt upgrade -y && sudo sudo apt install bloodhound bloodhound-python && sudo pip install bloodhound-python
```

