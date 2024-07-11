## Table of Contents

      - [Trust Application Store](#Trust\Application\Store)
    - [Safe App Installation](#Safe\App\Installation)
      - [Malware Removal through Windows Defender Anti Virus](#Malware\Removal\through\Windows\Defender\Anti\Virus)
    - [Microsoft Office Hardening](#Microsoft\Office\Hardening)
    - [AppLocker](#AppLocker)
    - [Protecting the Browser through Microsoft Smart Screen](#Protecting\the\Browser\through\Microsoft\Smart\Screen)
    - [Data Encryption Through BitLocker](#Data\Encryption\Through\BitLocker)
    - [Windows Sandbox](#Windows\Sandbox)
    - [**Windows Secure Boot**](#**Windows\Secure\Boot**)
    - [**Enable File Backups**](#**Enable\File\Backups**)

#### Trust Application Store
To access the Microsoft Application Store:
`CTRL + r`
Type: `ms-windows-store`

### Safe App Installation
- Only allow installation of applications from the Microsoft Store on your computer.  
- Go to `Setting > Select Apps and Features` and then select `The Microsoft Store only`.


#### Malware Removal through Windows Defender Anti Virus
Windows Defender Anti Virus is a complete anti-malware program capable of identifying malicious programs and taking remedial measures like quarantine. The program used to have an entire GUI; however Windows 10 and newer versions manage the same through Windows Security Centre. Windows Defender primarily offers four main functionalities

- **Real-time protection** - Enables periodic scanning of the computer.
- **Browser integration** - Enables safe browsing by scanning all downloaded files, etc.
- **Application Guard** - Allows complete web session sandboxing to block malicious websites or sessions to make changes in the computer.
- **Controlled Folder Access** - Protect memory areas and folders from unwanted applications.

### Microsoft Office Hardening

Microsoft Office Suite is one of the most widely used application suites in all sectors, including financial, telecom, education, etc. Malicious actors abuse its functionality through macros, Flash applets, object linking etc., to achieve Remote Code Execution. 

Hardening of Microsoft Office may vary from person to person as legitimate functionality of Microsoft Office is exploited to gain access. For example, disabling macros in a University may be helpful as no one uses it; however, banks cannot disable macros as they heavily rely on complex invoices and formulas through macros. 

The attached VM contains a batch file based on best practices and [Microsoft Attack Surface Reduction Rules](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference?view=o365-worldwide) for hardening Microsoft Office. To execute the script, right-click on the file office.bat on Desktop and Run as Administrator.


```powershell
harden@tryhackme$ office.bat (Work in Progress)
```


### AppLocker

AppLocker is a recently introduced feature that allows users to block specific executables, scripts, and installers from execution through a set of rules. We can easily configure them on a single PC or network through a GUI by the following method:

![Image for App Locker](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/7809f59c32041093a48fb49e3dea1891.png)  

  

Now, we will see how to add a rule through AppLocker to block a file based on its publisher name.

  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/64f6885a19f1ea54250717a3af70efc0.gif)  

  

**Browser (MS Edge)**

Microsoft Edge is a built-in browser available on Windows machines based on Chromium, inline with Google Chrome and Brave. The browser often acts as an entry point to a system for further pivoting and lateral movement. It is therefore of utmost importance to block and mitigate critical attacks carried out through a browser that include ransomware, ads, unsigned application downloads and trojans. 

  
### Protecting the Browser through Microsoft Smart Screen

Microsoft SmartScreen helps to protect you from phishing/malware sites and software when using Microsoft Edge. It helps to make informed decisions for downloads and lets you browse safely in Microsoft Edge by:  

- Displaying an alert if you are visiting any suspicious web pages.
- Vetting downloads by checking their hash, signature etc against a malicious software database.  
    
- Protecting against phishing and malicious sites by checking visited websites against a threat intelligence database.

  

To turn on the Smart Screen, go to `Settings > Windows Security > App and Browser Control > Reputation-based Protection`. Scroll down and turn on the `SmartScreen option`.

![Images for Smart Screen](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/79b7490c1617095a385f943342d13176.png)

  

Open Microsoft Edge, go to Settings and then click “**Privacy, Search and Services**” - Set "**Tracking prevention**" to **Strict** to avoid tracking through ads, cookies etc.

![Images for Browser Control](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/b7e3bb52cc6ea640aa487f6124dcbb72.png)


### Data Encryption Through BitLocker

Encryption of the computer is one of the most vital things to which we usually pay little attention. The worst nightmare is that someone gets unfettered access to your devices' data. Encryption ensures that you or someone you share the recovery key with can access the stored content.

Microsoft, for its business edition of Windows, utilises the encryption tools by BitLocker. Let us have a quick look at how one can ensure to protect the data through BitLocker encryption features available on the Home Editions of Windows 10. You have already read about it [here (Task 8)](https://tryhackme.com/room/windowsfundamentals3xzx).

Go to `Start > Control Panel > System and Security > BitLocker Drive Encryption`. You can easily see if the option to BitLocker Drive Encryption is enabled or not. 

![Image for Bitlocker Drive Encryption](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/e36ecbbe7b00820e10c38199baa357c5.png)  

  

A trusted Platform Module chip TPM is one of the basic requirements to support BitLocker device encryption. Keeping the BitLocker recovery key in a secure place (preferably not on the same computer) is imperative. You can read more about BitLocker Recovery [here](https://support.microsoft.com/en-us/windows/finding-your-bitlocker-recovery-key-in-windows-6b71ad27-0b89-ea08-f143-056f5ab347d6).

**Note**: The BitLocker feature is not available in the attached VM.

  

### Windows Sandbox

To run applications safely, we can use a temporary, isolated, lightweight desktop environment called Windows Sandbox. We can install software inside this safe environment, and this software will not be a part of our host machine, it will remain sandboxed. Once the Windows Sandbox is closed, everything, including files, software, and states will be deleted. We would require Virtualisation enabled on our OS to run this feature. We cannot try this in the attached VM but the steps for enabling the Sandbox feature are as below:

`Click Start > Search for 'Windows Features' and turn it on > Select Sandbox > Click OK to restart` 

![Image for Sandbox](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/42ad322953f66fdd2d911f331b54a49a.png)  

If you want to close the Sandbox, click the `close button,` and it will disappear. Opening suspicious files in a Windows Sandbox before blindly executing them in your base OS is recommended.

### **Windows Secure Boot**  

﻿Secure boot – an advanced security standard - checks that your system is running on trusted hardware and firmware before booting, which ensures that your system boots up safely while preventing unauthorised software access from taking control of your PC, like malware.

You are already in a secure boot environment if you run a modern PC with Unified Extensible Firmware Interface UEFI (the best replacement for BIOS) or Windows 10. You can check the status of the secure boot by following:

![Image for Secure Boot](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/7e86a928fcad06fc3a30d1e69620fa45.png)  

The incredible thing is that you do not need to enable or install it as it works silently in the background. Windows allows you to disable these features, which is not recommended.  You can enable [Secure boot from BIOS settings](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/disabling-secure-boot?view=windows-11) (if disabled).


### **Enable File Backups**  

The last option, but certainly not the least important one to prevent losing irreplaceable and critical files is to enable file backups. Despite all the above techniques, if you somehow lose essential data/files, you can recover the loss by restoring it, if you have a file backup option. Creating file backups is the best option to avoid disasters like malware attacks or hardware failure. You can enable the file backup option through  `Settings > Update and Security > Backup`:- 

![Image for Backup](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/c2e8a9441b14db952c6771518efd047f.png)  

Therefore, the most convenient option is enabling it from the '`File History`' option - a built-in functionality of Windows 10 and 11.


Hackers are continuously bypassing and exploiting Windows' legitimate features. You can see a list of Windows vulnerabilities by following [this](https://www.cvedetails.com/vulnerability-list/vendor_id-26/product_id-32238/Microsoft-Windows-10.html) link. The most critical part of hardening computers is enabling the Windows auto-updates.  

`Click Start > Settings > Update & Security > Window Updates.` 

This ensures that all the urgent security updates, if any, are installed immediately without causing any delay. It is most important because the quicker you apply the new Windows protection patch, the faster you can fix the potential vulnerabilities – to ensure the security from the latest known threats.

![Image for Windows Update](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/ca26fa3afcbabf99ce1d98b0396659c6.png)  
  

Remember, users who run the older Windows versions are always at greater risk and vulnerable to new security threats. So, be very careful about this.

Q: **What is the CVE score for the vulnerability CVE ID CVE-2022-32230**
> `7.8`