## Table of Contents

- [Linux System Enumeration:](#Linux\System\Enumeration:)
- [General OS Information](#general\os\information)
- [User Information](#user\information)
- [Network Information](#network\information)
- [Process Information](#process\information)
- [Service Configuration](#service\configuration)
- [Sensitive Files](#sensitive\files)
- [Scheduled Tasks](#scheduled\tasks)
    - [Windows System Enumeration:](#Windows\System\Enumeration:)
- [General OS Information](#general\os\information)
- [User Information](#user\information)
- [Network Information](#network\information)
- [Process Information](#process\information)
- [Service Configuration](#service\configuration)
- [Sensitive Files](#sensitive\files)
- [Scheduled Tasks](#scheduled\tasks)
    - [Web Server Enumeration:](#Web\Server\Enumeration:)
- [Website Root](#website\root)
- [Configuration Files](#configuration\files)
- [Logs](#logs)
- [Web Applications](#web\applications)
    - [Database Enumeration (Linux):](#Database\Enumeration\(Linux):)
- [MySQL/MariaDB](#mysql/mariadb)
- [PostgreSQL](#postgresql)
- [MongoDB](#mongodb)
    - [General Filesystem Enumeration (Linux):](#General\Filesystem\Enumeration\(Linux):)
- [Find SUID/SGID Files](#find\suid/sgid\files)
- [Find Configuration Files](#find\configuration\files)
- [Find Password Files](#find\password\files)
- [Find Writable Directories for All Users](#find\writable\directories\for\all\users)

- Linux System Enumeration
- Windows System Enumeration
- Web Server Enumeration
- Database Enumeration
- Filesystem Enumeration
___________
### Linux System Enumeration:
#Linuxpaths #linuxs #linuxenumeration
```bash
# General OS Information
/etc/os-release
/proc/version
uname -a
#system architechture
uname -m

# User Information
/etc/passwd
/etc/shadow
/home/
~/.bash_history

# Network Information
/etc/hosts
/etc/resolv.conf
/sbin/ifconfig
/sbin/route

# Process Information
/proc/self
ps -ef
top -n 1

# Service Configuration
/etc/init.d/
/etc/systemd/system/
/etc/nginx/sites-enabled/  # Default name - default|.conf
/etc/apache2/sites-enabled/

# Sensitive Files
/var/log/
/root/
/home/user/.ssh/

# Scheduled Tasks
/etc/cron.*
/var/spool/cron/crontabs/
```

### Windows System Enumeration:
#windows #windowsenumeration #ttps #LoL
```bash
# General OS Information
systeminfo
ver

# User Information
net users
net user [username]

# Network Information
ipconfig /all
route print
arp -a

# Process Information
tasklist
net start

# Service Configuration
sc qc [servicename]
sc queryex type= service state= all

# Sensitive Files
dir C:\ /s /b | findstr /i "passwords.xml"
dir C:\Users\ /s /b | findstr /i "credentials"

# Scheduled Tasks
schtasks /query /fo LIST /v
```

### Web Server Enumeration:
#webeneumeration #serverenumeration
```bash
# Website Root
/var/www/
/var/www/websitenamehere
/var/www/websitenamehere/app/orappname.py

# Configuration Files
/etc/nginx/nginx.conf
/etc/apache2/apache2.conf
/etc/httpd/conf/httpd.conf

# Logs
/var/log/nginx/access.log
/var/log/nginx/error.log
/var/log/apache2/access.log
/var/log/apache2/error.log

# Web Applications
/var/www/html/wp-config.php  # WordPress
/var/www/html/configuration.php  # Joomla
/var/www/html/sites/default/settings.php  # Drupal
```

### Database Enumeration (Linux):
#sql #mysql #mariadb #dbeneumeration
```bash
# MySQL/MariaDB
/etc/mysql/my.cnf
/var/log/mysql/error.log
~/.mysql_history

# PostgreSQL
/etc/postgresql/[version]/main/postgresql.conf
/var/log/postgresql/
~/.psql_history

# MongoDB
/etc/mongod.conf
/var/log/mongodb/mongod.log
```

### General Filesystem Enumeration (Linux):
#enumeration #find #privesc #linux
```bash
# Find SUID/SGID Files
find / -perm -4000 2>/dev/null
find / -perm -2000 2>/dev/null

# Find Configuration Files
find / -name *.conf 2>/dev/null

# Find Password Files
find / -name *.pwd 2>/dev/null

# Find Writable Directories for All Users
find / -writable -type d 2>/dev/null
```

![[Pasted image 20240129152424.png]]