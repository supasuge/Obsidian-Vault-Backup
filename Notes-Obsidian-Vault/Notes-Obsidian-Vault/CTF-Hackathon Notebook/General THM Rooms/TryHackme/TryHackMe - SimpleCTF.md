## Table of Contents


**How many services are running under port 1000?**
`nmap -p 1-1000 MACHINE_IP`
> `2`


**What is running on the higher port?**
`nmap -T4 -sC -sV MACHINE_IP`
> `ssh`


**What's the CVE you're using against the application?**
> `CVE-2019-9053`

**To what kind of vulnerability is the application vulnerable?**
> `SQLi`

**What's the password?**
> `secret`

**Where can you login with the details obtained?**
> `ssh`

**What's the user flag?**
> `G00d j0b, keep up!`

**Is there any other user in the home directory? What's its name?**
> `sunbath`

**What can you leverage to spawn a privileged shell?**
`vim`

**What's the root flag?**
> `W3ll d0n3. You made it!`

![[Pasted image 20240124033320.png]]