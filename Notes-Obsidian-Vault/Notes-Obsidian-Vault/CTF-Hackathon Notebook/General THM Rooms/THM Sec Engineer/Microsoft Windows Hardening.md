## Table of Contents

  - [Overview](#Overview)
  - [Significance for Hackers](#Significance\for\Hackers)
  - [Categories of Events](#Categories\of\Events)

- Machine IP: `10.10.4.102`
- Username: `Harden`
- Password: `harden`

Windows Registry 

The Windows registry is a unified container database that stores configurational settings, essential keys and shared preferences for Windows and third-party applications. Usually, on the installation of most applications, it uses a registry editor for storing various states of the application. For example, suppose an application (malicious or normal) wants to execute itself during the computer boot-up process; In that case, it will store its entry in the Run & Run Once key.

Usually, a malicious program makes undesired changes in the registry editor and tries to abuse its program or service as part of system routine activities. It is always recommended to protect the registry editor by limiting its access to unauthorised users.

Type `regedit` in the Run dialogue or taskbar search to access the registry editor.

![Image for Accessing Registry](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/f10675954519d7470106a15273aeaa7f.png)

Event Viewer

Event Viewer is an app that shows log details about all events occurring on your computer, including driver updates, hardware failures, changes in the operating system, invalid authentication attempts and application crash logs. Event Viewer receives notifications from different services and applications running on the computer and stores them in a centralised database.   

Hackers and malicious actors access Event Viewer to increase their attack surface and enhance the target system's profiling. Event categories are as below:

- Application: Records events of already installed programs.
- System: Records events of system components.
- Security: Logs events related to security and authentication etc.
- # Event Viewer

Event Viewer is an integral component of the Windows operating system that provides a centralized platform for recording and displaying logs. These logs offer crucial insights into the activities and events that have transpired on the system.

## Overview

- **Purpose**: Event Viewer captures and showcases logs detailing numerous events on a computer. These events encompass a wide range, from driver updates and hardware malfunctions to changes in the OS and unsuccessful authentication attempts.
    
- **Centralization**: All notifications from various services and applications running on the computer converge in the Event Viewer, which then consolidates them into a centralized database. This centralization makes it easier to monitor and troubleshoot issues.
    

## Significance for Hackers

Hackers often target Event Viewer for several reasons:

1. **Enhanced Profiling**: By accessing the Event Viewer, hackers can gain insights into the target system's operations, understanding its vulnerabilities and weak points.
2. **Increasing Attack Surface**: Information in the Event Viewer can expose potential areas of exploitation, thus widening the attack surface.

## Categories of Events

Event Viewer classifies logs into several categories, each capturing specific types of information:

1. **Application**: This category logs events associated with installed applications. For instance, if an application crashes or updates, the event will be recorded here.
    _Example_: Suppose you have Microsoft Word installed on your computer. If it encounters an error and crashes, an event log detailing this incident will appear under the "Application" category.
2. **System**: Logs under this category pertain to the system's components. This can range from driver failures to OS-level changes.
    _Example_: If a specific driver, say for a graphics card, fails to initialize or update, the "System" category will capture this event.
3. **Security**: As the name suggests, this category is dedicated to recording events related to security and authentication. It provides insights into any security breaches, failed login attempts, and other security-related matters.
    _Example_: If an unauthorized user attempts to access the system and fails, the "Security" category will log this attempt.


Telemetry

Telemetry is a data collection system used by Microsoft to enhance the user experience by preemptively identifying security and functional issues in software. An application seamlessly shares data (crash logs, application-specific) with Microsoft to improve the user experience for future releases.  

Telemetry functionality is achieved by Universal Telemetry Client (UTC) services available in Windows and runs through `diagtrack.dll`. Contents acquired through telemetry service are stored encrypted in a local folder `%ProgramData%\Microsoft\Diagnosis` and sent to Microsoft after 15 minutes or so.

We can access `The DiagTrack` through the Services console in Windows 10.

**Standard vs Admin Account** 

Identity and access management involves employing best practices to ensure that only authenticated and authorised users can access the system. There are two types of accounts in Windows, i.e. Admin and Standard Account. Per best practice, the Admin account should only be used to carry out tasks like software installation and accessing the registry editor, service panel, etc. Routine functions like access to regular applications, including Microsoft Office, browser, etc., can be allowed to standard accounts. Go to `Control Panel > User Accounts` to create standard or administrator accounts.


In either case, a user can authenticate themselves on the system through a **password**; however, Windows 10 has introduced a new feature called **Windows Hello**, which allows authenticating someone based on “something you have, something you know or something you are”. 

To access accounts and select the sign-in option, go to `Settings > Accounts > Sign-in Options.`

**User Account Control (UAC)**

User Account Control (UAC) is a feature that enforces enhanced access control and ensures that all services and applications execute in non-administrator accounts. It helps mitigate malware's impact and minimises privilege escalation by [bypassing UAC](https://tryhackme.com/room/bypassinguac). Actions requiring elevated privileges will automatically prompt for administrative user account credentials if the logged-in user does not already possess these.  

For example, installing device drivers or allowing inbound connections through Windows Firewall requires more permissions than already available privileges for a standard user. We have covered the topic in detail in [Windows Fundamental 1](https://tryhackme.com/room/windowsfundamentals1xbx).

  

As a principle, always follow the **Principle of Least Privilege**, which states that (Per [CISA](https://www.cisa.gov/uscert/bsi/articles/knowledge/principles/least-privilege#:~:text=The%20Principle%20of%20Least%20Privilege%20states%20cthat%20a%20subject%20should,should%20not%20have%20that%20right)) “_a subject should be given only those privileges needed for it to complete its task. If a subject does not need an access right, the subject should not have that right_”.  

To access UAC, go to `Control Panel -> User Accounts` and click on `Change User Account Control Setting`.

**Local Policy and Group Policies Editor**

Group Policy Editor is a built-in interactive tool by Microsoft that allows to configure and implement local and group policies. We mainly use this feature when part of a network; however, we can also use it for a workstation to limit the execution of vulnerable extensions, set password policies, and other administrative settings.

**Password Policies**

One primary use of a local policy editor is to ensure complex and strong passwords for user accounts. For example, we can design password policies to maximise our security:

- Passwords must contain both uppercase and lowercase characters.
- Check passwords against leaked or already hacked databases or a dictionary of compromised passwords.
- In case of 6 failed login attempts within 15 minutes, the account will remain locked for at least 1 hour.

We can access Password policies through the Local group policy editor.

Go to `Security settings > Account Policies > Password policy` 

![Image for Password Policies](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/e159d0daed7b8b8f217cbcd85d10c0b2.png)

**Setting A Lockout Policy**

To protect your system password from being guessed by an attacker, we can set out a lockout policy so the account will automatically lock after certain invalid attempts. To set a lockout policy, go to `Local Security Policy > Windows Settings > Account Policies > Account Lockout Policy` and configure values to lock out hackers after three invalid attempts.

**Windows Defender Firewall**

Windows Defender Firewall is a built-in application that protects computers from malicious attacks and blocks unauthorised traffic through inbound and outbound rules or filters. As an analogy, this is equivalent to “who is coming in and going out of your home”.

Malicious actors abuse Windows Firewall by bypassing existing rules. For example, if we have configured the firewall to allow incoming connections, hackers will try to manipulate the functionality by creating a remote connection to the victim's computer.

You can see more details about Windows Firewall Configuration [here](https://tryhackme.com/room/redteamfirewalls).

We can access Windows Defender Firewall by accessing `WF.msc` in the Run dialogue.

As mentioned in the [Windows Fundamentals room](https://tryhackme.com/room/windowsfundamentals3xzx), it has three main profiles `Domain, Public and Private`.  The Private profile must be activated with "Blocked Incoming Connections" while using the computer at home. 

View detailed settings for each profile by clicking on Windows Defender Firewall Properties.

Whenever possible, enable the Windows Defender Firewall default settings. For blocking all the incoming traffic, always configure the firewall with a 'default deny' rule before making an exception rule that allows more specific traffic.



