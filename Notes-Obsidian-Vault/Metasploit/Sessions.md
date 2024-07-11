## Table of Contents

  - [Using Sessions](#Using\Sessions)
      - [Listing Active Sessions](#Listing\Active\Sessions)
      - [Interacting with a Session](#Interacting\with\a\Session)
  - [Jobs](#Jobs)
      - [Viewing the Jobs Command Help Menu](#Viewing\the\Jobs\Command\Help\Menu)
      - [Viewing the Exploit Command Help Menu](#Viewing\the\Exploit\Command\Help\Menu)
      - [Running an Exploit as a Background Job](#Running\an\Exploit\as\a\Background\Job)
      - [Listing Running Jobs](#Listing\Running\Jobs)

`msfconsole` can manage multiple modules at the same time. This is one of the many reasons it provides the user with so much flexibility. This is done with the use of `Sessions`, which creates dedicated control interfaces for all of your deployed modules.

## Using Sessions
While running any available exploits or auxiliary modules in msfconsole, we can background the session as long as they form a channel of communication with the target host.

This can be done either by pressing the `[CTRL] + [Z]` key combination or by typing the `background` command in the case of Meterpreter stages. This will prompt us with a confirmation message. After accepting the prompt, we will be taken back to the msfconsole prompt (`msf6 >`) and will immediately be able to launch a different module.


#### Listing Active Sessions

We can use the `sessions` command to view our currently active sessions.

```shell
msf6 exploit(windows/smb/psexec_psh) > sessions
```

#### Interacting with a Session

You can use the `sessions -i [no.]` command to open up a specific session.

```shell
msf6 exploit(windows/smb/psexec_psh) > sessions -i 1
```


## Jobs

If, for example, we are running an active exploit under a specific port and need this port for a different module, we cannot simply terminate the session using `[CTRL] + [C]`. If we did that, we would see that the port would still be in use, affecting our use of the new module. So instead, we would need to use the `jobs` command to look at the currently active tasks running in the background and terminate the old ones to free up the port.

Other types of tasks inside sessions can also be converted into jobs to run in the background seamlessly, even if the session dies or disappears.

#### Viewing the Jobs Command Help Menu

We can view the help menu for this command, like others, by typing `jobs -h`.

```shell
msf6 exploit(multi/handler) > jobs -h
Usage: jobs [options]

Active job manipulation and interaction.

OPTIONS:

    -K        Terminate all running jobs.
    -P        Persist all running jobs on restart.
    -S <opt>  Row search filter.
    -h        Help banner.
    -i <opt>  Lists detailed information about a running job.
    -k <opt>  Terminate jobs by job ID and/or range.
    -l        List all running jobs.
    -p <opt>  Add persistence to job by job ID
    -v        Print more detailed info.  Use with -i and -l
```

#### Viewing the Exploit Command Help Menu

When we run an exploit, we can run it as a job by typing `exploit -j`. Per the help menu for the `exploit` command, adding `-j` to our command. Instead of just `exploit` or `run`, will "run it in the context of a job."

#### Running an Exploit as a Background Job

Sessions

```shell
msf6 exploit(multi/handler) > exploit -j
```

#### Listing Running Jobs
To list all running jobs, we can use the `jobs -l` command. To kill a speccific job, look at the index no. of the jobs and use the `kill [Index no.]` command. Use the `jobs -K` command to kill all running jobs.






