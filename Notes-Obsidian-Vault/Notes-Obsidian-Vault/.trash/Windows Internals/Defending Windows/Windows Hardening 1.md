## Table of Contents




﻿**Trusted Application Store**  

Microsoft Store offers a complete range of applications (games, utilities) and allows downloading non-malicious files through a single click. Malicious actors bind legitimate software with trojans and viruses and upload it on the internet to infect and access the victim's computer. Therefore, downloading applications from the Microsoft Store ensures that the downloaded software is not malicious. 

We can access Microsoft Application Store by typing `ms-windows-store:` in the Run dialogue.

![Image for Application Store](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/55413d1781c530e53fa175b3e2fadb84.png)  

  

**Safe App Installation**

Only allow installation of applications from the Microsoft Store on your computer.  

Go to `Setting > Select Apps and Features` and then select `The Microsoft Store only`.   

 ![Safe Application](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/adf2295862b9d01aec5d967aaa9c6433.png)

  

**Malware Removal through Windows Defender Anti Virus**

Windows Defender Anti Virus is a complete anti-malware program capable of identifying malicious programs and taking remedial measures like quarantine. The program used to have an entire Graphical User Interface; however, Windows 10 and newer versions manage the same through Windows Security Centre. Windows Defender primarily offers four main functionalities:

- **Real-time protection** - Enables periodic scanning of the computer.
- **Browser integration** - Enables safe browsing by scanning all downloaded files, etc.
- **Application Guard** - Allows complete web session sandboxing to block malicious websites or sessions to make changes in the computer.
- **Controlled Folder Access** - Protect memory areas and folders from unwanted applications.

You have already learned about this in [Windows Fundamentals 3](https://tryhackme.com/room/windowsfundamentals3xzx)

**Microsoft Office Hardening**  

Microsoft Office Suite is one of the most widely used application suites in all sectors, including financial, telecom, education, etc. Malicious actors abuse its functionality through macros, Flash applets, object linking etc., to achieve Remote Code Execution. 

Hardening of Microsoft Office may vary from person to person as legitimate functionality of Microsoft Office is exploited to gain access. For example, disabling macros in a University may be helpful as no one uses it; however, banks cannot disable macros as they heavily rely on complex invoices and formulas through macros. 

The attached VM contains a batch file based on best practices and [Microsoft Attack Surface Reduction Rules](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference?view=o365-worldwide) for hardening Microsoft Office. To execute the script, right-click on the file office.bat on Desktop and Run as Administrator.

  

Command Prompt - Administrator

```shell-session
harden@tryhackme$ office.bat (Work in Progress)
Microsoft Office Hardened Successfully.
```

**AppLocker**

AppLocker is a recently introduced feature that allows users to block specific executables, scripts, and installers from execution through a set of rules. We can easily configure them on a single PC or network through a GUI by the following method:

![Image for App Locker](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/7809f59c32041093a48fb49e3dea1891.png)  

  

Now, we will see how to add a rule through AppLocker to block a file based on its publisher name.

  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/64f6885a19f1ea54250717a3af70efc0.gif)  

  

**Browser (MS Edge)**

Microsoft Edge is a built-in browser available on Windows machines based on Chromium, inline with Google Chrome and Brave. The browser often acts as an entry point to a system for further pivoting and lateral movement. It is therefore of utmost importance to block and mitigate critical attacks carried out through a browser that include ransomware, ads, unsigned application downloads and trojans. 

  

**Protecting the Browser through Microsoft Smart Screen**

Microsoft SmartScreen helps to protect you from phishing/malware sites and software when using Microsoft Edge. It helps to make informed decisions for downloads and lets you browse safely in Microsoft Edge by:  

- Displaying an alert if you are visiting any suspicious web pages.
- Vetting downloads by checking their hash, signature etc against a malicious software database.  
    
- Protecting against phishing and malicious sites by checking visited websites against a threat intelligence database.

  

To turn on the Smart Screen, go to `Settings > Windows Security > App and Browser Control > Reputation-based Protection`. Scroll down and turn on the `SmartScreen option`.

![Images for Smart Screen](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/79b7490c1617095a385f943342d13176.png)

  

Open Microsoft Edge, go to Settings and then click “**Privacy, Search and Services**” - Set "**Tracking prevention**" to **Strict** to avoid tracking through ads, cookies etc.

![Images for Browser Control](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/b7e3bb52cc6ea640aa487f6124dcbb72.png)




