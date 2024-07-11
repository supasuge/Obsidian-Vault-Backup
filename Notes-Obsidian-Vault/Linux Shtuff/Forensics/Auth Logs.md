# Table of Contents
- [Location and Structure](#Location\and\Structure)
- [Analyzing Auth Logs](#Analyzing\Auth\Logs)
- [Failure or Success of Login Attempts](#Failure\or\Success\of\Login\Attempts)
- [Sudo command executions](#Sudo\Command\Executions)
- [Time-Based Filters](#Time-Based\Filters)
Auth logs or authentication logs are crucial for monitoring and analysing user authentication events on a Linux system. These logs provide detailed records of login attempts, both successful and unsuccessful, and can be instrumental in identifying potential security incidents, unauthorized access attempts, and suspicious activities.

## Location and Structure

Auth logs are normally stored in the `/var/log/auth.log` file on most Linux distributions. This log file records all authentication-related events, such as SSH logins, sudo attempts, etc. Each record consists of a timestamp, the service generating the log, the relevant user, and a short description of the event.
### Analyzing Auth Logs

Auth logs can be analysed using standard command-line tools like grep, awk, and tail. Understanding and interpreting these logs can help system administrators and security professionals detect and respond to unauthorized access attempts and other security incidents.

As already discussed, auth logs are present in the `/var/log` directory in a file named `auth.log`

The `auth.log` file is a standard text log file and can be viewed using any of the text file parsers present in linux like `cat`, `less`, and `tail`. For instance, to see the most recent entries in the auth log, we might use the tail utility:
`sudo tail /var/log/auth.log`

This command displays the last ten lines of the auth log, giving us a quick overview of recent authentication events. We will see an output similar to the following as a result of running this command.

```
ubuntu@tryhackme:~$ tail /var/log/auth.log Jun 19 19:07:12 tryhackme sudo: pam_unix(sudo:session): session opened for user root by (uid=0) Jun 19 19:07:12 tryhackme su: (to root) ubuntu on pts/0 Jun 19 19:07:12 tryhackme su: pam_unix(su:session): session opened for user root by (uid=0) Jun 19 19:07:23 tryhackme su: pam_unix(su:session): session closed for user root Jun 19 19:07:23 tryhackme sudo: pam_unix(sudo:session): session closed for user root Jun 19 19:07:33 tryhackme sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/vim /etc/hosts Jun 19 19:07:33 tryhackme sudo: pam_unix(sudo:session): session opened for user root by (uid=0) Jun 19 19:07:39 tryhackme sudo: pam_unix(sudo:session): session closed for user root Jun 19 19:07:48 tryhackme sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/tail /var/log/auth.log Jun 19 19:07:48 tryhackme sudo: pam_unix(sudo:session): session opened for user root by (uid=0)
```
- Logs are rolled over after a set size is reached, only the latest logs are present in the auth.log file. Historical logs are stored in archived formats in files such as auth.log.1, auth.log.2.gz and so on. To search historical logs, use `auth.log.*`

### Filtering Auth Logs

Since auth logs can be parsed using any of the text parsing utilities in Linux, we can use simple text filtering techniques to filter the auth logs as well. Filtering auth logs can help narrow down specific events of interest, such as failed login attempts or sudo command executions. Let's go through some common scenarios and practice how to filter them:


### Failure or Success of Login Attempts

To find all failed login attempts, we can use `grep` to search for "failure":

`sudo grep -i "failure" /var/log/auth.log`

As is evident, this command returns all entries where a login attempt was unsuccessful, providing details about the user and the source of the attempt.

Similarly, to filter for sessions opened for a user, search for "session opened":

`sudo grep -i "session opened" /var/log/auth.log`

As an example, this is how the result for searching for failed logins will look like:

```shell-session
/var/log$ sudo grep -i "failure" /var/log/auth.log*
```

- Change the logging levels to provide us with more or less information

#### Sudo Command Executions

To track the use of the sudo command, we can again use `grep` to search for "sudo":

`sudo grep "sudo" /var/log/auth.log`

This filter shows when users invoked sudo, which commands were executed, and whether the attempts were successful or failed.

An example execution of the above search will look like the following:

```shell-session
grep "sudo" /var/log/auth.log*
```

#### Time-Based Filters
