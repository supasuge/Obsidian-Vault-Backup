## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Wordpress Database Config file](#Wordpress\Database\Config\file)
          - [Config Files](#Config\Files)
          - [SSH Keys](#SSH\Keys)

## Table of Contents

          - [Wordpress Database Config file](#Wordpress\Database\Config\file)
          - [Config Files](#Config\Files)
          - [SSH Keys](#SSH\Keys)

Always be sure to note any credentials or potential credentials during enumeration. These may be found in configuration files (`.conf`, `.config`, `.xml`) etc. Shell scripts, a User's bash history, backup (`.bak`) files, within database files or even text files. Credentials may be useful for escalating to other users or even root, accessing datases and other systems within the environment. 


The `/var` directory usually contains the web root for web server's. The web root usually contains database credentials or other types of credentials to be leveraged to further access. A common example is MySQL Database credentials within WordPress configuration files:
###### Wordpress Database Config file
```bash
cat wp-config.php | grep  -i 'DB_USER\|DB_PASSWORD'
```

The `spool` or `mail` directories may also contain valuable information or even credentials. It's common to find credentials stored in files in the web root (MySQL connection strings, WordPress config files.)
###### Config Files
```bash
find / ! -path "*/proc/*" -iname "*config*" -type f -exec cat {} 2>/dev/null
```

###### SSH Keys
```bash
ls ~/.ssh
id_rsa id_rsa.pub known_hosts
```


