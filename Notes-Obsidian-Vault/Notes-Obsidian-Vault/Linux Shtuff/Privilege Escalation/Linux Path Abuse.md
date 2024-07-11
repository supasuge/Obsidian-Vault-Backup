## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Path Abuse](#path\abuse)

## Table of Contents

- [Path Abuse](#path\abuse)

# Path Abuse
`PATH` is an environment variables that specifies the set of directories where an executable can be located. An account's PATH variable is a set of absolute paths, allowing a user to type a command without specifying the absolute path to the binary. For example, `cat /tmp/test.txt` - instead of specifying the aboslute path `/bin/cat /tmp/test.txt`. we can check the contents of the PATH variable `env | grep PATH` or `echo $PATH`
```bash
echo $PATH
[OUTPUT]

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```

Creating a script or program in a directory specified in the PATH will make it executable from any directory on the system.
```bash
pwd && conncheck
```
the `conncheck` script created in `/usr/local/sbin` will still run when in the `/tmp` directory because it was created in a directory specified in the PATH.

- Adding a `.` to a user's PATH adds their current working directory to the list. For example, if we can modify the user's path, we could replace a common binary such as `ls` with a malicous script such as a reverse shell. If we add `.` to the path by issuing the command `PATH=.:$PATH` then `export PATH` we will be able to run binaries located in our current working directory by just typing the name of the file. (I.e., `ls` will call the script named `ls` in the current working directory instead of the binary located at `/bin/ls`)
```bash
PATH=.:${PATH}
export PATH
echo $PATH
```
In this example, we modify the path ro run a simple `echo` command when the command `ls` is typed. 

**What is the unusual directory located in the current users PATH?**
`/tmp`
























