## Table of Contents

      - [Script](#Script)
          - [From Contents found in the .git](#From\Contents\found\in\the\.git)
- [An example hook script to check the commit log message taken b](#an\example\hook\script\to\check\the\commit\log\message\taken\b)
- [applypatch from an e-mail message.](#applypatch\from\an\e-mail\message.)
- [The hook should exit with non-zero status after issuing an](#the\hook\should\exit\with\non-zero\status\after\issuing\an)
- [appropriate message if it wants to stop the commit.  The hook](#appropriate\message\if\it\wants\to\stop\the\commit.\\the\hook)
- [allowed to edit the commit message file.](#allowed\to\edit\the\commit\message\file.)
- [To enable this hook, rename this file to "applypatch-msg".](#to\enable\this\hook,\rename\this\file\to\"applypatch-msg".)

#gitdumper
#git
#vulnerabilities
#gitvuln
#HTB
#HackTheBox
```bash
pip install -r requirements.txt
https://github.com/arthaud/git-dumper
```

#### Script
###### From Contents found in the .git
	hooks/applypatch-msg.sample
```bash
#!/bin/sh
#
# An example hook script to check the commit log message taken b
# applypatch from an e-mail message.
#
# The hook should exit with non-zero status after issuing an
# appropriate message if it wants to stop the commit.  The hook 
# allowed to edit the commit message file.
#
# To enable this hook, rename this file to "applypatch-msg".

. git-sh-setup
commitmsg="$(git rev-parse --git-path hooks/commit-msg)"
test -x "$commitmsg" && exec "$commitmsg" ${1+"$@"}
:

```

using the “identify -verbose” command, we can obtain detailed information about the file in question. In this case, we will find a payload in hexadecimal format. To decode it, we can use the tool CyberChef, which allows us to convert it to ASCII text.



