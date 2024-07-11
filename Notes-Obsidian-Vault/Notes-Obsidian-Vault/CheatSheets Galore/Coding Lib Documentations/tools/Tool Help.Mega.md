## Table of Contents

  - [Username Anarchy](#Username\Anarchy)
- [CrackMapExec](#crackmapexec)

cd ADM```

git clone https://github.com/ropnop/go-windapsearch.git && cd go-windapsearch
$ go get github.com/magefile/mage
$ mage
Targets:
  build    Compile windapsearch for current OS and ARCH
  clean    Delete bin and dist dirs
  dist     Cross-compile for Windows, Linux, Mac x64 and put in ./dist
$ mage build
$ ./windapsearch --version



./windapsearch -h
windapsearch: a tool to perform Windows domain enumeration through LDAP queries
Version: dev (9f91330) | Built: 03/04/21 (go1.16) | Ronnie Flathers @ropnop

Usage: ./windapsearch [options] -m [module] [module options]

Options:
  -d, --domain string            The FQDN of the domain (e.g. 'lab.example.com'). Only needed if dc not provided
      --dc string                The Domain Controller to query against
  -u, --username string          The full username with domain to bind with (e.g. 'ropnop@lab.example.com' or 'LAB\ropnop')
                                  If not specified, will attempt anonymous bind
      --bindDN string            Full DN to use to bind (as opposed to -u for just username)
                                  e.g. cn=rflathers,ou=users,dc=example,dc=com
  -p, --password string          Password to use. If not specified, will be prompted for
      --hash string              NTLM Hash to use instead of password (i.e. pass-the-hash)
      --ntlm                     Use NTLM auth (automatic if hash is set)
      --port int                 Port to connect to (if non standard)
      --secure                   Use LDAPS. This will not verify TLS certs, however. (default: false)
      --proxy string             SOCKS5 Proxy to use (e.g. 127.0.0.1:9050)
      --full                     Output all attributes from LDAP
      --ignore-display-filters   Ignore any display filters set by the module and always output every entry
  -o, --output string            Save results to file
  -j, --json                     Convert LDAP output to JSON
      --page-size int            LDAP page size to use (default 1000)
      --version                  Show version info and exit
  -v, --verbose                  Show info logs
      --debug                    Show debug logs
  -h, --help                     Show this help
  -m, --module string            Module to use

Available modules:
    admin-objects       Enumerate all objects with protected ACLs (i.e admins)
    computers           Enumerate AD Computers
    custom              Run a custom LDAP syntax filter
    dns-names           List all DNS Names
    dns-zones           List all DNS Zones
    domain-admins       Recursively list all users objects in Domain Admins group
    gpos                Enumerate Group Policy Objects
    groups              List all AD groups
    members             Query for members of a group
    metadata            Print LDAP server metadata
    privileged-users    Recursively list members of all highly privileged groups
    search              Perform an ANR Search and return the results
    unconstrained       Find objects that allow unconstrained delegation
    user-spns           Enumerate all users objects with Service Principal Names (for kerberoasting)
    users               List all user objects
```

## Username Anarchy
https://github.com/urbanadventurer/username-anarchy
```bash
Usage: ./username-anarchy [OPTIONS]... [firstname|first last|first middle last]
	Author: Andrew Horton (urbanadventurer). Version: 0.5

	Names:
	 -i, --input-file FILE     Input list of names. Can be SPACE, CSV or TAB delimited.
	                           Defaults to firstname, lastname. Valid column headings are:
	                           firstinitial, firstname, lastinitial, lastname,
	                           middleinitial, middlename.
	 -a, --auto                Automatically generate names from a country/list
	 -c, --country COUNTRY     COUNTRY can be one of the following datasets:
	                           PublicProfiler:
	                           argentina, austria, belgium, canada, china,
	                           denmark, france, germany, hungary, india, ireland,
	                           italy, luxembourg, netherlands, newzealand, norway,
	                           poland, serbia, slovenia, spain, sweden,
	                           switzerland, uk, us
	                           Other:
	                           Facebook - uses the Facebook top 10,000 names
	     --given-names FILE    Dictionary of given names
	     --family-names FILE   Dictionary of family names
	 -s, --substitute STATE    Control name substitutions
	                           Valid values are 'on' and 'off'. Default: off
	                           Can substitute any part of a name not available
	 -m, --max-sub NUM         Limit quantity of substitutions per plugin.
	                           Default: -1 (Unlimited)

	Username format:
	 -l, --list-formats        List format plugins
	 -f, --select-format LIST  Select format plugins by name. Comma delimited list
	 -r, --recognise USERNAME  Recognise which format is in use for a username.
	                           This uses the Facebook dataset. Use verbose mode to
	                           show progress.
	 -F, --format FORMAT       Define the user format using either format string or
	                           ABK format. See README.md for format details.

	Output:
	 -@, --suffix BOOL         Suffix. e.g. @example.com
	                           Default: None
	 -C BOOL,                  Case insensitive usernames.
	     --case-insensitive    Default: True (All lower case)

	Miscellaneous:
	 -v, --verbose             Display plugin format comments in output and displays
	                           last name searches in plugin format recogniser
	 -h, --help

/username-anarchy --input-file fullnames.txt --select-format first,flast,first.last,firstl > unames.txt

```

# CrackMapExec

- -u Username `The user whose credentials we will use to authenticate`
- -p Password `User's password`
- Target (IP or FQDN) `Target host to enumerate` (in our case, the Domain Controller)
- --users `Specifies to enumerate Domain Users`
- --groups `Specifies to enumerate domain groups`
- --loggedon-users `Attempts to enumerate what users are logged on to a target, if any`