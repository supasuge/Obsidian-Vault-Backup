## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Objectives](#Objectives)
  - [What is memory Forensics?](#What\is\memory\Forensics?)
    - [Volatile Data](#Volatile\Data)
    - [Memory Dump](#Memory\Dump)
    - [Volatility](#Volatility)
  - [Identifying the Process using Various Linux tools](#Identifying\the\Process\using\Various\Linux\tools)
      - [Process Extraction](#Process\Extraction)
      - [Checking Cronjobs](#Checking\Cronjobs)
- [Edit this file to introduce tasks to be run by cron](#edit\this\file\to\introduce\tasks\to\be\run\by\cron)
      - [Checking for Running Services](#Checking\for\Running\Services)
          - [To stop the Service](#To\stop\the\Service)
- [usually must be run as root](#usually\must\be\run\as\root)
- [check the service status to confirm state](#check\the\service\status\to\confirm\state)
- [1. What is the exposed password that we find from the bash history output?](#1.\what\is\the\exposed\password\that\we\find\from\the\bash\history\output?)
- [2. What is the PID of the miner process that we find?](#2.\what\is\the\pid\of\the\miner\process\that\we\find?)
- [3. What is the MD5 hash of the miner process?](#3.\what\is\the\md5\hash\of\the\miner\process?)
- [4. What is the MD5 hash of the MySQL server process?](#4.\what\is\the\md5\hash\of\the\mysql\server\process?)
- [5. Use the command `strings extracted/miner.<PID from question 2>.0x400000 | grep http://`. What is the suspicious URL? (Fully defang the URL using CyberChef)](#5.\use\the\command `strings\extracted/miner.<pid\from\question\2>.0x400000\|\grep\http://`.\what\is\the\suspicious\url?\(fully\defang\the\url\using\cyberchef))
- [6. After reading the elfie file, what location is the mysqlserver process dropped in on the file system?](#6.\after\reading\the\elfie\file,\what\location\is\the\mysqlserver\process\dropped\in\on\the\file\system?)

## Table of Contents

          - [Objectives](#Objectives)
  - [What is memory Forensics?](#What\is\memory\Forensics?)
    - [Volatile Data](#Volatile\Data)
    - [Memory Dump](#Memory\Dump)
    - [Volatility](#Volatility)
  - [Identifying the Process using Various Linux tools](#Identifying\the\Process\using\Various\Linux\tools)
      - [Process Extraction](#Process\Extraction)
      - [Checking Cronjobs](#Checking\Cronjobs)
- [Edit this file to introduce tasks to be run by cron](#edit\this\file\to\introduce\tasks\to\be\run\by\cron)
      - [Checking for Running Services](#Checking\for\Running\Services)
          - [To stop the Service](#To\stop\the\Service)
- [usually must be run as root](#usually\must\be\run\as\root)
- [check the service status to confirm state](#check\the\service\status\to\confirm\state)
- [1. What is the exposed password that we find from the bash history output?](#1.\what\is\the\exposed\password\that\we\find\from\the\bash\history\output?)
- [2. What is the PID of the miner process that we find?](#2.\what\is\the\pid\of\the\miner\process\that\we\find?)
- [3. What is the MD5 hash of the miner process?](#3.\what\is\the\md5\hash\of\the\miner\process?)
- [4. What is the MD5 hash of the MySQL server process?](#4.\what\is\the\md5\hash\of\the\mysql\server\process?)
- [5. Use the command `strings extracted/miner.<PID from question 2>.0x400000 | grep http://`. What is the suspicious URL? (Fully defang the URL using CyberChef)](#5.\use\the\command `strings\extracted/miner.<pid\from\question\2>.0x400000\|\grep\http://`.\what\is\the\suspicious\url?\(fully\defang\the\url\using\cyberchef))
- [6. After reading the elfie file, what location is the mysqlserver process dropped in on the file system?](#6.\after\reading\the\elfie\file,\what\location\is\the\mysqlserver\process\dropped\in\on\the\file\system?)




###### Objectives
- Understand what memory forensics is and how to use it in a digital forensics investigation
- Understand volatile data and memory dumps
- Learn about `Volatility`

## What is memory Forensics?
Memory forensics also known as volatile memory analysis or `RAM` (Random-access Memory) is a branch of digital forensics. It involves the examination and analysis of a computers RAM to uncover evidence. This differs from hard disk forensics, where all files on the disk can be recovered and then studied. 

### Volatile Data
Volatile data refers to information stored in a computer's memory (RAM) and can be easily lost or altered when the computer is powered off or restarted. Volatile data is crucial for digital investigator's because it provides a snapshoit of the computer's state at the time of an incident. Any  incident responder should be aware of what volatile data is. The reason is that when looking into a device that has been compromised, an initial reaction would be to turn off the device to contain the threat. 

Examples: 
- Running Processes
- Network connections
- RAM Contents
Volatile data is not written to disk and is constantly changing in memory.

The issue here lies in that any malware will be running in memory, meaning that any network connections and running processes that spawned from the malware wiull be lost. Powering down teh device means valuable evidence will be destroyed.

### Memory Dump
Memory Forensics offers valuable benefits in digital investigations by capturing real-time data from a computer's volatile memory. It provides rapid-insight into ongoing activities, detects stealthy threats, captures volatile data like passwords, and allows investigators to understand user actions and system states during incidents - all without altering the target system. In other words, memory forensics helps confirm malicious actors' activities by analysing a computer systems volatilke memory to uncover evidence of unauthorised or malicious actions. 


|**Category**|**Description**| **Example** |
|:-:|:-:|:-:|
|User Process|These are processes a user has started. They typically involve applications and software users interact with directly.|Firefox: This is a web browser that we can use to surf the web.|
|Background Process|These are processes that operate without direct user interaction. They often perform tasks that are essential for the system's operation or for providing services to user processes.|Automated backups: Backup software often runs in the background, periodically backing up data to ensure its safety and recoverability.|

### Volatility

**Volatility** is a command-line tool that lets digital forensics and incident response teams analyse a memory dump in order to perform memory analysis. Volatility is written in Python, and it can analyse snapshots taken from Linux, Mac OS, and Windows. Volatility has a wide range of use cases, including the following:  

- Listing any active and closed network connections
- Listing a device's running processes at the time of capture
- Listing possible command line history values
- Extracting possible malicious processes for further analysis
- And the list keeps on going

Memory Analysis

The file **linux.mem** contains the memory dump of the Linux server we're going to analyse. This file is located in our home directory. For Volatility to begin the analysis, we have to specify the file with the `-f` flag and the profile with the `--profile` flag. We can use the `-h` flag to look at all the different **plugins** we can use to help with our analysis.

Volatility's Command Menu Example

```shell-session
ubuntu@volatility:~/Desktop/Evidence$ vol.py -f linux.mem --profile="LinuxUbuntu_5_4_0-163-generic_profilex64" -h
Volatility Foundation Volatility Framework 2.6.1 Usage: Volatility - A memory forensics analysis platform.

Options:
  -h, --help            List all available options and their default values. 
  --conf-file=/home/thm/.volatilityrc 
User based configuration file
  --d, --debug          Debug volatility

--cropped for brevity--

Supported Plugin Commands:
linux_banner           Prints the Linux banner information
linux_bash             Recover bash history from bash process memory
linux_bash_env         Recover a process' dynamic environment variables
linux_enumerate_files  Lists files referenced by the filesystem cache
linux_find_file        Lists and recovers files from memory
linux_lsmod            Gather loaded kernel modules
linux_malfind          Looks for suspicious process mappings 
linux_procdump         Dumps a process's executable image to disk
linux_pslist           Gather active tasks by walking the task_struct->task list
```

## Identifying the Process using Various Linux tools
`top` - This command shows us a list of processes in real time with their usage values from the CPU. It's dynamic list, meaning it changes with the resource usage of each process




#### Process Extraction
Extracting the binary of the process. This will help us analyse its behavior using `malware analysis`. We also want to extract the binary of the process as a form of evidence preservation. To extract the binary of the process ofor examination, we can utilise the `linux_procdump`. We firsts need to create a directory to indicate where we would like the extracted process to go with `mkdir extracted`. then the
`-D` flag: tells volatility where to place the extracted binary and indicate the process PID's with the `-p` flag. Creating a separate direcotry doesn't help us to stay organized; it's required by Volatility in order to avoid errors. 



#### Checking Cronjobs
```bash
crontab -l
# Edit this file to introduce tasks to be run by cron
```

#### Checking for Running Services
List all services and grep's for enabled to respawn process's
```bash
systemctl list-unit-files | grep enabled
```

- If something is found or has a string name for a normal service. Get more information about this service by checking its status"
```bash
systemctl status [redacted]
```

###### To stop the Service
```bash
# usually must be run as root
sudo su
systemctl stop [PID]
# check the service status to confirm state
systemctl status



```


present in the system. To completely eradicate the service, we will have to remove the files from the file system as well. Let's do that. Here, we see the location of the service is `/etc/systemd/system/[redacted]` and the location of the process is `/etc/systemd/system/a`. To permanently kill the service, let's delete these two files.


```
rm -rf /etc/systemd/system/a 
root@tryhackme:/home/ubuntu# rm -rf /etc/systemd/system/[redacted] root@tryhackme:/home/ubuntu# systemctl status [redacted] Unit [redacted]  
root@tryhackme:/home/ubuntu#
```




# 1. What is the exposed password that we find from the bash history output?

Use the Command to list the bash history in volatility`   vol.py -f linux.mem — profile=”LinuxUbuntu_5_4_0–163-generic_profilex64" linux_bash`

Ans: NEhX4VSrN7sV

# 2. What is the PID of the miner process that we find?

Use the below command to find the PID of the miner process`   vol.py -f linux.mem — profile=”LinuxUbuntu_5_4_0–163-generic_profilex64" linux_pslist`

Ans: 10280

# 3. What is the MD5 hash of the miner process?

Create a dir with any name — `mkdir extracted`

Paste the below command to extract the binary, make sure to add the directory name in the command`   vol.py -f linux.mem — profile=”LinuxUbuntu_5_4_0–163-generic_profilex64" linux_procdump -D extracted -p 10280`

Ans: 153a5c8efe4aa3be240e5dc645480dee

# 4. What is the MD5 hash of the MySQL server process?

Use the below command`   vol.py -f linux.mem — profile=”LinuxUbuntu_5_4_0–163-generic_profilex64" linux_procdump -D extracted -p 10291`

Ans: c586e774bb2aa17819d7faae18dad7d1

# 5. Use the command `strings extracted/miner.<PID from question 2>.0x400000 | grep http://`. What is the suspicious URL? (Fully defang the URL using CyberChef)

Use the command to extract the strings from the binary file — miner  
`strings extracted/miner.10280.0x400000 | grep http://`

Defang it with [Cyberchef](https://gchq.github.io/CyberChef/)

Ans: hxxp[://]mcgreedysecretc2[.]thm

# 6. After reading the elfie file, what location is the mysqlserver process dropped in on the file system?

1. Use the below command`   vol.py -f linux.mem — profile=”LinuxUbuntu_5_4_0–163-generic_profilex64" linux_find_file -i 0xffff9ce9b78280e8 -O elfie`
2. Now Use `cat elfie` to find the location

Ans: /var/tmp/.system-python3.8-updates/mysqlserver





























































