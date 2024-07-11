## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Environment Commands for Host & Network Recon](#Environment\Commands\for\Host\&\Network\Recon)

## Table of Contents

    - [Environment Commands for Host & Network Recon](#Environment\Commands\for\Host\&\Network\Recon)


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