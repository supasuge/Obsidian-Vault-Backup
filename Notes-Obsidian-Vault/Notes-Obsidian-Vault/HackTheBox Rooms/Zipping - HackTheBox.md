IP=`10.10.11.229`


## Enumeration
**nmap**
```bash
nmap -p- --min-rate 10000 -Pn $IP
22/tcp    open     ssh
80/tcp    open     http
58986/tcp filtered unknown
62971/tcp filtered unknown
64414/tcp filtered unknown
```
Moving on...
```bash
sudo nmap -p22,80,58986,62971,64414 -sCV -A -Pn $IP
```

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-24 14:28 EDT
Nmap scan report for 10.10.11.229
Host is up (0.039s latency).

```bash
PORT      STATE  SERVICE VERSION
22/tcp    open   ssh     OpenSSH 9.0p1 Ubuntu 1ubuntu7.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 9d:6e:ec:02:2d:0f:6a:38:60:c6:aa:ac:1e:e0:c2:84 (ECDSA)
|_  256 eb:95:11:c7:a6:fa:ad:74:ab:a2:c5:f6:a4:02:18:41 (ED25519)
80/tcp    open   http    Apache httpd 2.4.54 ((Ubuntu))
|_http-server-header: Apache/2.4.54 (Ubuntu)
|_http-title: Zipping | Watch store
58986/tcp closed unknown
62971/tcp closed unknown
64414/tcp closed unknown
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=4/24%OT=22%CT=58986%CU=43426%PV=Y%DS=2%DC=T%G=Y%TM=
OS:66294F5F%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%T
OS:S=A)SEQ(SP=103%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)SEQ(SP=103%GCD=1%ISR=10
OS:C%TI=Z%CI=Z%II=I%TS=A)OPS(O1=M53CST11NW7%O2=M53CST11NW7%O3=M53CNNT11NW7%
OS:O4=M53CST11NW7%O5=M53CST11NW7%O6=M53CST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4
OS:=FE88%W5=FE88%W6=FE88)ECN(R=Y%DF=Y%T=40%W=FAF0%O=M53CNNSNW7%CC=Y%Q=)T1(R
OS:=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=
OS:A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=
OS:Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=A
OS:R%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%R
OS:UD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 62971/tcp)
HOP RTT      ADDRESS
1   42.05 ms 10.10.14.1
2   45.40 ms 10.10.11.229
```

Next, I began further enumeration using `gobuster` starting with directories... 
`/uploads`
`/shop`
`/assets`
