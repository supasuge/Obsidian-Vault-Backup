# Shell Stability

```
script /dev/null -c /bin/bash
export TERM=xterm
stty raw -echo; fg
```


```
python3 -c 'import pty;pty.spawn("/bin/bash")'
stty raw -echo
fg
export TERM=xterm
stty rows 67 columns 318
```