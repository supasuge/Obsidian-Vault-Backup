## Table of Contents

  - [Linux Persistence](#Linux\Persistence)
    - [rc.local](#rc.local)
    - [Linux service](#Linux\service)
      - [Create/Open New service file using nano](#Create/Open\New\service\file\using\nano)
      - [Add service information to file. path is full path to .sh to execute on startup](#Add\service\information\to\file.\path\is\full\path\to\.sh\to\execute\on\startup)
      - [When done press CTRL + X Then press Y then ENTER to save and close](#When\done\press\CTRL\+\X\Then\press\Y\then\ENTER\to\save\and\close)
    - [Reload Service manager](#Reload\Service\manager)


https://media.licdn.com/dms/document/media/D4D1FAQEsZUTdEnm0HA/feedshare-document-pdf-analyzed/0/1700710524121?e=1701907200&v=beta&t=FXrQNBydPJWC87haXREKDQG4QETr7s8b2N6HxwU8sSU
![[Pasted image 20231122212816.png]]

![[Pasted image 20231122212837.png]]
![[Pasted image 20231122212846.png]]

![[Pasted image 20231122212857.png]]


![[Pasted image 20231122212909.png]]
`chaattr -i <FILE_PATH> bs=3145728 count=100`
	Generate random file.
![[Pasted image 20231122213121.png]]



## Linux Persistence

### rc.local
	Add full path to rc.local file. This full path will be executed on system startup
```bash
nano /etc/rc.local  
#OR
echo "<FILE_PATH>" >> /etc/rc.local
```

### Linux service
#### Create/Open New service file using nano
`nano /etc/systemd/system/<SERVICE_NAME>.service`

#### Add service information to file. path is full path to .sh to execute on startup
```shell
[Unit]
after=network.targetDescription=My service description

[Service]
Type=simple
Restart=always
ExecStart=<FILE_PATH>

[Install]
WantedBy=multi-user.target


```
#### When done press CTRL + X Then press Y then ENTER to save and close


### Reload Service manager
```shell
systemctl daemon-reload

#eneable the service
systemctl enable <SERVICE_NAME>.service

systemctl start <SERVICE_NAME>.service
```

![[Pasted image 20231122213857.png]]

![[Pasted image 20231122213958.png]]

![[Pasted image 20231122214010.png]]


![[Pasted image 20231122214041.png]]

![[Pasted image 20231122214116.png]]

![[Pasted image 20231122214125.png]]

![[Pasted image 20231122214312.png]]
![[Pasted image 20231122214326.png]]

![[Pasted image 20231122214445.png]]
![[Pasted image 20231122214525.png]]


![[Pasted image 20231122214646.png]]