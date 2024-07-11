## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [What is a restricted shell used for and why?](#What\is\a\restricted\shell\used\for\and\why?)
      - [RBASH](#RBASH)
        - [RKSH](#RKSH)
        - [RZSH](#RZSH)
      - [Escaping](#Escaping)
      - [Command Substitution](#Command\Substitution)
      - [Command Chaining](#Command\Chaining)
      - [Environment Variables](#Environment\Variables)
      - [Shell Functions](#Shell\Functions)

## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [What is a restricted shell used for and why?](#What\is\a\restricted\shell\used\for\and\why?)
      - [RBASH](#RBASH)
        - [RKSH](#RKSH)
        - [RZSH](#RZSH)
      - [Escaping](#Escaping)
      - [Command Substitution](#Command\Substitution)
      - [Command Chaining](#Command\Chaining)
      - [Environment Variables](#Environment\Variables)
      - [Shell Functions](#Shell\Functions)


A restricted shell is a shell that limits the user's ability to execute certain commands. In a restricted shell, the user is only allowed to execute a specific set of commands or only allowed to execute commands in specific directories. Restricted shells are often used to provide a safe envronment for users who may accidentaly or intentionally damage the system or provide a way for users to access only certain system features. 

### What is a restricted shell used for and why?
In large organization's in which segmentation is needed for assigning different users/groups different role/privileges, restricted shells are instead individually assigns users to various shell's with different configuration's. Ex: an employee who only needs access to email and file sharing would be assigned to `rbash` shells, which limits their ability to execute specific commands and access certain directories. Contractors who need to access to more advanced network features, such as database servers and web servers, are assigned to `rksh` shells. Finally, employees who need to access the network for specific purposes, such as to run specific applications or scripts, are assigned to `rzsh` shell. Which provide them the most flexibility but still limit their ability to execute specific comands and access certain directories.

#### RBASH
Restricted Version of the Bourne Shell
##### RKSH
Restricted Version of the Korn Shell
##### RZSH
Restricted Version of `zsh`

- Restricted Shells in enterprise networks are used to provide a safe and controlled environment for users who may accidentally or intentionally damage the system. By limiting the user's ability to execute specific commands or access certain directories, administrators can ensure that users cannot perform actions that could harm the system or compromise the network's security.

#### Escaping
- In some cases it may be possible to escape from a restricted shell by injecting commands into the command line or other inputs the shell accepts. Ex: An argument that is passed in to a built-in comand. In that case, it's possible to escape the shell by injecting additional commands

The command below attempts to inject a `pwd` command into the argument of the `ls` command:
```shell-session
ls -l `pwd` 
```
- This command would cause the `ls` command to be executed with the argument `-l`, followed by the output of the `pwd` command. Since the `pwd` command is not restricted by the shell, this would allow us to execute the `pwd` command and see the current working directory, even though the shell does not allow us to execute the `pwd` command directly.
#### Command Substitution
- Another method for escaping from a restricted shell is to use command substitution. This involves using the shell's command substitution syntax to execute a command. For example, imagine the shell allows users to execute commands by enclosing them in backticks (`). In that case, it may be possible to escape from the shell by executing a command in a backtick substitution that is not restricted by the shell.
#### Command Chaining
- In some cases, it may be possible to escape from a restricted shell by using command chaining. We would need to use multiple commands in a single command line, separated by a shell metacharacter, such as a semicolon (`;`) or a vertical bar (`|`), to execute a command. For example, if the shell allows users to execute commands separated by semicolons, it may be possible to escape from the shell by using a semicolon to separate two commands, one of which is not restricted by the shell.

#### Environment Variables
Escaping from a restricted shell to use environment variables involves modifying or creating an environment variables that the shell uses to execute commands that are not restricted by the shell. For example, if the shell uses an environment variable to specify the directory in which commands are executed, it may be possible to escape from the shell by modifying the value of the environment variable to specify a different directory.

#### Shell Functions

In some cases, it may be possible to escape from a restricted shell by using shell functions. For this we can define and call shell functions that execute commands not restricted by the shell. Let us say, the shell allows users to define and call shell functions, it may be possible to escape from the shell by defining a shell function that executes a command.

**Using the material learned in this section, SSH into the box and attempt to print the contents of flag.txt through a shell escaped command injection**
```bash
echo $(<flag.txt)
HTB{35c4p3_7h3_r3stricted_5h311}
```




