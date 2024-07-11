## Table of Contents

  - [Table of Contents](#Table\of\Contents)
        - [LSAISO](#LSAISO)

## Table of Contents

        - [LSAISO](#LSAISO)


The **Windows Initialization Process**, **wininit.exe**, is responsible for launching services.exe (Service Control Manager), lsass.exe (Local Security Authority), and lsaiso.exe within Session 0. It is another critical Windows process that runs in the background, along with its child processes.

Note: lsaiso.exe is a process associated with **Credential Guard and KeyGuard**. You will only see this process if Credential Guard is enabled. 
What is normal?

![Wininit.exe properties.](https://assets.tryhackme.com/additional/windows-processes/wininit.png)  
**Image Path**:  %SystemRoot%\\System32\\wininit.exe
**Parent Process**:  Created by an instance of smss.exe
**Number of Instances**:  One
**User Account**:  Local System
**Start Time**:  Within seconds of boot time

##### LSAISO
*LSA Isolated* process (lsass.exe, lsaiso.exe) - Credential Guard
	Virtualization-based isolation technology for LSASS which prevents attackers from stealing credentials that could be used for pass the hash attacks.


Similar to the first filter we can see that Wireshark is combing through the packets and filtering based on the source and destination we set.

Filtering by TCP Protocols: The last filter that we will be covering is the protocol filter, this allows you to set a port or protocol to filter by and can be handy when trying to keep track of an unusual protocol or port being used.

It is worthwhile to mention that Wireshark can filter by both port numbers as well as protocol names.

Syntax: `tcp.port eq <Port #> or <Protocol Name>`

  

![](https://assets.tryhackme.com/additional/wireshark101/11.png)  

  

Filtering by UDP Protocols: You can also filter by UDP ports by changing the prefix from tcp to udp

Syntax: `udp.port eq <Port #> or <Protocol Name>`  

That is the end of filtering for this task however I recommend you play around with other filters and operators on your own. Once you're ready move on to Task 5.






