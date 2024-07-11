## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Account creation](#Account\creation)
- [Create a user account with a home dir](#create\a\user\account\with\a\home\dir)
- [to be able to login into the new account, a password must be set](#to\be\able\to\login\into\the\new\account,\a\password\must\be\set)
    - [Root/Superuser Creation](#Root/Superuser\Creation)
- [Add the user to the sudoer's group.](#add\the\user\to\the\sudoer's\group.)
    - [Persistence using SSH Authorized Keys](#Persistence\using\SSH\Authorized\Keys)
    - [Persistence Using Scheduled tasks](#Persistence\Using\Scheduled\tasks)
      - [CronJobs](#CronJobs)
    - [What is Dynamic linker 101](#What\is\Dynamic\linker\101)

## Table of Contents

  - [Account creation](#Account\creation)
- [Create a user account with a home dir](#create\a\user\account\with\a\home\dir)
- [to be able to login into the new account, a password must be set](#to\be\able\to\login\into\the\new\account,\a\password\must\be\set)
    - [Root/Superuser Creation](#Root/Superuser\Creation)
- [Add the user to the sudoer's group.](#add\the\user\to\the\sudoer's\group.)
    - [Persistence using SSH Authorized Keys](#Persistence\using\SSH\Authorized\Keys)
    - [Persistence Using Scheduled tasks](#Persistence\Using\Scheduled\tasks)
      - [CronJobs](#CronJobs)
    - [What is Dynamic linker 101](#What\is\Dynamic\linker\101)

[Art of Linux persistence](https://hadess.io/the-art-of-linux-persistence/)
## Account creation
```bash
# Create a user account with a home dir
sudo useradd -m <username>
# to be able to login into the new account, a password must be set
sudo passwd <username>

```

### Root/Superuser Creation
```bash
sudo useradd <username>
# Add the user to the sudoer's group.
sudo usermod -aG sudo <username>
```

### Persistence using SSH Authorized Keys
`ssh-keygen` - command to generate the public key `id_rsa.pub`
Ex:
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCwIqohDVyEsHt5lHcI86scq5EWVm+DYpvhuolEV8EnkOonUFABgC2/9KdbMlG/di19N3oWRo60WG1F/LbRg5TNBzfuaKSU5UDoGCOI6m/DzwBkSfJUcnRoYg/2OSSPnqQP+V8aCISyiHcs5LuS996t9oGKWiwyyg4ScXeIGtlKZzgHPUl2+L6K2Rtga+GsI+X4sXUSAYbNR9xPDxwPqw5+ShwT7F+1HzR3ITI+uzySXKQVq4cXMkaJvuiwW1r/R8oeyd05DWlj67OCyH9ZS4dnamDoXdGYZ1B/DFp4eZQX5TB9Ggwu2FZ/aeWzv+tRPBDw5LKGdNtSfS7l+wNZNFUSeuNJdWYBNA0Dww4SMkgZdY8K95s1QiG/EcajFjGulbsl8Cpnmx3nTJsMdBtsRLgKIPylA0DWysgrL6cyEIXkCoIs/tnv+YCvvnTAEvbINEB0VMSaJUtqID5tG7+MbdOt/Lew9jmeh/uYfQ7i60zHfZNKJ3/lCPeKEN/aExui7k0= root@kali
```

- Can be helpful to replace the name at the end to something legit

### Persistence Using Scheduled tasks
#### CronJobs
`/etc/crontab`
`/etc/cron.d/*`
`/etc/cron.{hourly,daily,weekly,monthly}/*`
`/var/spool/cron/crontab`
- users who can modify their own crontab, using `crontab -e` will create a file in `/var/spool/cron/crontab/<user>` in specific to user who is doing the modification. Example of a malicious cron job file:
- `*/5 * * * *  /opt/backdoor.sh`



| Files | Working |
| ---- | ---- |
| /etc/bash.bashrc | systemwide files executed at the start of interactive shell  <br>(tmux) |
|  |  |
| **Files** | **Working** |
| /etc/bash.bashrc | systemwide files executed at the start of interactive shell  <br>(tmux) |
| /etc/bash_logout | Systemwide files executed when we terminate the shell |
| ~/.bashrc | Widly exploited user specific startup script executed at  <br>the start of shell |
| ~/.bash_profile, ~/.bash_login,  <br>~/.profile | User specific files , but which found first are executed  <br>first |
| ~.bash_logout | User specific files, executed when shell session closes |
| ~/.bash_logout | User-specific clean up script at the end of the session |
| /etc/profile | Systemwide files executed at the start of login shells |
| /etc/profile.d | all the .sh files are exeucted at the start of login shells |

### What is Dynamic linker 101

In mordern operating system the program can be linked statically or dynamically during the runtime. Dynamically linked binaries use shared libraries located on the operating system. These libraries will be resolved, loaded, and linked at runtime. The Linux component that is in charge of this operation is the ****dynamic linker****, also known as `ld.so` or `ld-linux.so.*`. A number of environment variables are used during the execution of the dynamic linker, the most important of which is ****LD_PRELOAD****.
