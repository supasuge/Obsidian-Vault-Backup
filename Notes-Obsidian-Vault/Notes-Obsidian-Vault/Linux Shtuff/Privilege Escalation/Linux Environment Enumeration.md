## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Situational Awareness](#Situational\Awareness)
          - [Getting Started](#Getting\Started)
          - [Check What Operating System and Version we are dealing with](#Check\What\Operating\System\and\Version\we\are\dealing\with)

## Table of Contents

  - [Situational Awareness](#Situational\Awareness)
          - [Getting Started](#Getting\Started)
          - [Check What Operating System and Version we are dealing with](#Check\What\Operating\System\and\Version\we\are\dealing\with)


- [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)
- [LineEnum](https://github.com/rebootuser/LinEnum)


**OS Version**: Knowing the distribution (Ubuntu, debian, FreeBSD, SUSE, Red Hate, CentOS, etc.) will give us an idea of tools that may available that are native to the kernel. It's also worth noting the version to search for public Exploits

**Kernel Version**: Public Exploits may be available in a specific kernel version, these can cause system instability or even a complete crash. Be careful Running these against any production system, make suer you fully unserstand the exploit and possible ramifications before running one

**Running Services**: Knowing what services are running on the host is important, especially those running as root. A misconfigured or vulnerable service running as root can be easy to use as a privilege escalation vector (Potentially). Ex: Nagios, Exim, Samba, ProFTPd, etc. Public exploits such as CVE-2016-6566 (local privilege scalation flaw in Nagios Core < 4.2.4)


## Situational Awareness
After gainaing initial access on a host, always start by gathering basic info about the system we are working with.
- OS Version
- Kernel Version
- Running Services

Commands may  look different depending on the OS/Kernel version. If we land on a host such as FreeBSD, Solaris, or something more obscure such as the HP propietary OS HP-UX, or the IBM OS AIX. The commands we would work with will likely be different. Though the commands may be different.

###### Getting Started
```bash
whoami    #what user are we
id        # what groups does our user belong to?
hostname  # What is the server named
ifconfig  # what subnet did we land it
#OR   
ip -a     # What subnet did we land in, any other NIC's?
sudo -l   # Can our user run anything with sudo(or as another user)
sudo su   # Drop into a root shell.
```
###### Check What Operating System and Version we are dealing with
```bash
cat /etc/os-release
```
Ex: `Ubuntu 20.04.4 LTS ("Focal Fossa")`
[Ubuntu Release Cycle](https://ubuntu.com/about/release-cycle)
- The release cycle states each specific version and the end














