## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Permissions](#Permissions)
    - [Abbreviations](#Abbreviations)
  - [to who does it belong?](#to\who\does\it\belong?)
  - [Chmod Examples](#Chmod\Examples)
    - [Operators](#Operators)
    - [chmod 600](#chmod\600)
    - [chmod 664](#chmod\664)
    - [chmod 777](#chmod\777)
  - [Chmod Practices](#Chmod\Practices)
    - [SSH Permissions](#SSH\Permissions)
    - [Web Permissions](#Web\Permissions)
    - [Batch Change](#Batch\Change)

## Table of Contents

  - [Permissions](#Permissions)
    - [Abbreviations](#Abbreviations)
  - [to who does it belong?](#to\who\does\it\belong?)
  - [Chmod Examples](#Chmod\Examples)
    - [Operators](#Operators)
    - [chmod 600](#chmod\600)
    - [chmod 664](#chmod\664)
    - [chmod 777](#chmod\777)
  - [Chmod Practices](#Chmod\Practices)
    - [SSH Permissions](#SSH\Permissions)
    - [Web Permissions](#Web\Permissions)
    - [Batch Change](#Batch\Change)

[Source](https://quickref.me/chmod)

| #  | Perms  | Description  |
|---|---|---|
|`400`|r--------|Readable by owner only|
|`500`|r-x------|Avoid Changing|
|`600`|rw-------|Changeable by user|
|`644`|rw-r--r--|Read and change by user|
|`660`|rw-rw----|Changeable by user and group|
|`700`|rwx------|Only user has full access|
|`755`|rwxr-xr-x|Only changeable by user|
|`775`|rwxrwxr-x|Sharing mode for a group|
|`777`|rwxrwxrwx|Everybody can do everything|


## Permissions
| Permission  | Description  | Octal  | Decimal  |
|---|---|---|---|
|`---`|No Permission|000|0 (0+0+0)|
|`--x`|Execute|001|1 (0+0+1)|
|`-w-`|Write|010|2 (0+2+0)|
|`-wx`|Execute and Write|011|3 (0+2+1)|
|`r--`|Read|100|4 (4+0+0)|
|`r-x`|Read and Execute|101|5 (4+0+1)|
|`rw-`|Read and Write|110|6 (4+2+0)|
|`rwx`|Read, Write and Execute|111|7 (4+2+1|
### Abbreviations
| Abbreviation  | description  | Value  |
|---|---|---|
|`r`|`R`ead|4|
|`w`|`W`rite|2|
|`x`|E`x`ecute|1|
|`-`|No permission|0|

## to who does it belong?
| Who (abbr.)  | Meaning  |
|---|---|
|`u`|`U`ser|
|`g`|`G`roup|
|`o`|`O`thers|
|`a`|`A`ll, same as ugo|
## Chmod Examples
### Operators
|   |   |
|---|---|
|`+`|Add|
|`-`|Remove|
|`=`|Set|
### chmod 600

```shell
$ chmod 600 example.txt
$ chmod u=rw,g=,o= example.txt
$ chmod a+rwx,u-x,g-rwx,o-rwx example.txt
```

### chmod 664

```shell
$ chmod 664 example.txt
$ chmod u=rw,g=rw,o=r example.txt
$ chmod a+rwx,u-x,g-x,o-wx example.txt
```

### chmod 777

```shell
$ chmod 777 example.txt
$ chmod u=rwx,g=rwx,o=rwx example.txt
$ chmod a=rwx example.txt
```



Deny execute permission to everyone.

```shell
$ chmod a-x chmodExampleFile.txt
```

Allow read permission to everyone.

```shell
$ chmod a+r chmodExampleFile.txt
```

Make a file readable and writable by the group and others.

```shell
$ chmod go+rw chmodExampleFile.txt
```

Make a shell script executable by the user/owner.

```shell
$ chmod u+x chmodExampleScript.sh
```
---
Allow everyone to read, write, and execute the file and turn on the set group-ID.

```shell
$ chmod =rwx,g+s chmodExampleScript.sh
```


## Chmod Practices

### SSH Permissions

```shell
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
$ chmod 600 ~/.ssh/id_rsa
$ chmod 600 ~/.ssh/id_rsa.pub
$ chmod 400 /path/to/access_key.pem
```

### Web Permissions

```shell
$ chmod -R 644 /var/www/html/
$ chmod 644 .htaccess
$ chmod 644 robots.txt
$ chmod 755 /var/www/uploads/
$ find /var/www/html -type d -exec chmod 755 {} \;
```

### Batch Change

```shell
$ chmod -R 644 /your_path
$ find /path -type d -exec chmod 755 {} \;
$ find /path -type f -exec chmod 644 {} \;
```