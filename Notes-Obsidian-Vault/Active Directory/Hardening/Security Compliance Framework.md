Microsoft Security Compliance Toolkit (MSCT) is an official toolkit provided by Microsoft to implement and manage local and domain-level policies. You don't have to worry about complex policy syntaxes and scripts, as Microsoft will provide pre-developed security baselines per the end user environment. You can download MSCT from the official Microsoft website [link.](https://www.microsoft.com/en-us/download/details.aspx?id=55319)

## Installing Security Baselines   

Microsoft offers its customers security baselines readily available in consumable formats, like, Group Policy Objects Backups. You can easily download it as zip files and extract the content. Here is how you can download and install the security baselines for Windows Server in a simple way:

`Open Microsoft Security Compliance Website > click Download > click Windows Servers Security Baseline.zip > Download`


![how to download baseline for windows server](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/ad2a73b2287a9bf29b0f07a4762480e5.png)

`Open extracted folder > Scripts > & select desired baseline & execute with PowerShell`

![how to install the baselines](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/f19023b4d1b5df9f2a6f371811a8bca3.png)

## Policy Analyser   

One of the Security Compliance ToolKit's features is a policy analyser which allows comparison of group policies to quickly check inconsistencies, redundant settings, and the alterations that need to be made between them. Consider a scenario where plenty of GPOs are applied at diverse levels. There will be conflicting, redundant settings and many more avenues that can be quickly resolved with a policy analyser. Same as security baselines, it is downloaded as a zip file from the [same link](https://www.microsoft.com/en-us/download/details.aspx?id=55319), and one can easily extract the content and then follow the procedure, as shown in the following steps:  

![how to download policy analyser](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/1b2e80e317439e72d5397f840177ebc8.png)

Once downloaded, you can run the `PolicyAnalyzer.exe` to add and manage local or domain-level policies.

  

![how to add policies in a policy analyser](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/5a7f7844170f99d5085543d765cf5c5c.png)

Be careful while downloading security baselines, as they should only be downloaded from the official Microsoft website.


