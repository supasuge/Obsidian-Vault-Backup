## Table of Contents

  - [User.txt](#User.txt)
- [Privilege Escalation](#privilege\escalation)
  - [root.txt](#root.txt)


## User.txt
032d2fc8952a8c24e39c8f0ee9918ef7

# Privilege Escalation
Check current privileges>
```
daniel@MARKUP C:\Users\daniel\Desktop>whoami /priv
```


Looking at the permissions of job.bat using icacls reveals that the group BUILTIN\Users has full control (F) over the file. The BUILTIN\Users group represents all local users, which includes Daniel as well. We might be able to get a shell by transferring netcat to the system and modifying the script to execute a reverse shell. Before then, we need to check if the wevtutil process mentioned in the job.bat file is running. We can see the currently scheduled tasks by typing the schtasks command. If our permission level doesn't allow us to view this list through Windows' command line, we can quickly use powershell's ps command instead, which represents another security misconfiguration that works against the server.

## root.txt
f574a3e7650cebd8c39784299cb570f8

