## Table of Contents

- [smbclient Cheatsheet](#smbclient\cheatsheet)
  - [Basic Usage](#Basic\Usage)
    - [Common Flags](#Common\Flags)
    - [Authentication Options](#Authentication\Options)
    - [Connection Options](#Connection\Options)
  - [Example Commands](#Example\Commands)
    - [Connect with Username and Password](#Connect\with\Username\and\Password)
    - [Execute Command on Remote Share](#Execute\Command\on\Remote\Share)
    - [Using a Credentials File](#Using\a\Credentials\File)
    - [Tar Files from a Share](#Tar\Files\from\a\Share)
    - [Sending a Message](#Sending\a\Message)

# smbclient Cheatsheet

## Basic Usage
```bash
smbclient //SERVER/SHARE - Connect to a SMB share.
```
- **SERVER:** Replace with the server name or IP.
- **SHARE:** Replace with the share name.

### Common Flags
```bash
- -M, --message=HOST           - Send a message to a host.
- -I, --ip-address=IP          - Specify the IP address to connect to.
- -E, --stderr                 - Output messages to stderr instead of stdout.
- -T, --tar=<c|x>IXFvgbNan     - Perform tar operations.
- -c, --command=STRING         - Execute semicolon-separated commands.
- -p, --port=PORT              - Specify the port to connect to.
- -d, --debuglevel=DEBUGLEVEL  - Set the debug level.
- -s, --configfile=CONFIGFILE  - Use an alternative configuration file.
```

### Authentication Options
```bash
- -U, --user=[DOMAIN/]USERNAME[%PASSWORD]  - Network username, optional domain and password.
- -N, --no-pass                            - Connect without a password.
- -A, --authentication-file=FILE           - Use credentials stored in a file.
- --use-kerberos=desired|required|off      - Use Kerberos for authentication.
```

### Connection Options
```bash
- -R, --name-resolve=ORDER  - Name resolution order.
- -O, --socket-options=OPT  - Define socket options.
- -m, --max-protocol=PROT   - Maximum protocol level to use.
- -W, --workgroup=GROUP     - Workgroup name.
```

## Example Commands

### Connect with Username and Password
```bash
smbclient //server/share -U username%password
```
- **server:** Server name or IP.
- **share:** Share name.
- **username:** Your username.
- **password:** Your password.

### Execute Command on Remote Share
```bash
smbclient //server/share -c 'ls' -U username%password
```
- Lists files in the specified share.

### Using a Credentials File
```bash
smbclient //server/share -A /path/to/credfile
```
- **credfile:** File with credentials in format `username = <username>`, `password = <password>`.

### Tar Files from a Share
```bash
smbclient //server/share -TcXvgbNan backup.tar /path/to/directory
```
- Create a tar archive of a directory from the share.

### Sending a Message
```bash
smbclient -M host
```
- Sends a message to the specified host.
---
