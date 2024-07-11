## Table of Contents

    - [Maintaining Access](#Maintaining\Access)


### Maintaining Access
- Keylogging
	- Low and Slow
	- Smash and grab
Ex:
1. Get initial access
2. `ps` - list processes 
3. `migrate <num>` then dump the output
4. 

Metasploit has a Meterpreter script, **persistence.rb**, that will create a Meterpreter service that will be available to you even if the remote system is rebooted.

One word of warning here before we go any further. The persistent Meterpreter as shown here requires no authentication. This means that anyone that gains access to the port could access your back door!