## Table of Contents


**Ping Sweep Example**
```shell
for i in {1..254}; do (ping -c 1 172.19.0.${i} | grep "bytes from" | grep -v "Unreachable" &); done;
```
#pingsweep

```shell
root@3a453ab39d3d:~ for port in {1..65535}; do echo > /dev/tcp/172.19.0.1/$port && echo "$port open"; done 
```



