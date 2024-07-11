## Table of Contents

    - [Installing ClamAV on apt/deb Based Systems (Ubuntu, Debian, Mint, Kali, etc..)](#Installing\ClamAV\on\apt/deb\Based\Systems\(Ubuntu,\Debian,\Mint,\Kali,\etc..))



### Installing ClamAV on apt/deb Based Systems (Ubuntu, Debian, Mint, Kali, etc..)

Installing on Debian based systems is just as easy. Just use apt like so:

```
savona@ubuntudev:~$ sudo apt install clamav clamtk
```


The ClamAV team says they update the virus signature database approx twice daily. If you do not update the signatures often you can be using an old database. The easiest way to keep the signatures updates is to use the clamav-freshclam service. Let's start the service and enable it at boot.

Start the clamav-freshclam service:

```
[savona@putor ~]$ sudo systemctl start clamav-freshclam
```

Set the clamav-freshclam [service to start on boot](https://www.putorius.net/start-services-on-boot-in-red-hat-7-or.html):

```
[savona@putor ~]$ sudo systemctl enable clamav-freshclam
```
