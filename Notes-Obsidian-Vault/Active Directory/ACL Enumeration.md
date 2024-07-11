## Table of Contents

  - [Table of Contents](#Table\of\Contents)
        - [Import Power View](#Import\Power\View)
  - [Enumerating ACLs using PowerView](#Enumerating\ACLs\using\PowerView)
        - [Throws all information available](#Throws\all\information\available)
      - [Targeted Enumeration (more effective)](#Targeted\Enumeration\(more\effective))
    - [Get the SID of our target user to search effectively](#Get\the\SID\of\our\target\user\to\search\effectively)
        - [Then:](#Then:)
      - [Using Get-DomainObjectACL](#Using\Get-DomainObjectACL)
      - [Performing a Reverse Search & Mapping to a GUID Value](#Performing\a\Reverse\Search\&\Mapping\to\a\GUID\Value)
      - [Using the -ResolveGUIDs Flag](#Using\the\-ResolveGUIDs\Flag)
    - [Creating a list of domain users](#Creating\a\list\of\domain\users)
      - [A Useful foreach Loop](#A\Useful\foreach\Loop)
      - [Further Enumeration of Rights Using damundsen](#Further\Enumeration\of\Rights\Using\damundsen)
      - [Investigating the Help Desk Level 1 Group with Get-DomainGroup](#Investigating\the\Help\Desk\Level\1\Group\with\Get-DomainGroup)
      - [Investigating the Information Technology Group](#Investigating\the\Information\Technology\Group)
      - [Looking for Interesting Access](#Looking\for\Interesting\Access)
  - [Enumerating ACLs with BloodHound](#Enumerating\ACLs\with\BloodHound)
      - [Viewing Node Info through BloodHound](#Viewing\Node\Info\through\BloodHound)
      - [Investigating ForceChangePassword Further](#Investigating\ForceChangePassword\Further)
      - [Viewing Potential Attack Paths through BloodHound](#Viewing\Potential\Attack\Paths\through\BloodHound)
      - [Viewing Pre-Build queries through BloodHound](#Viewing\Pre-Build\queries\through\BloodHound)

## Table of Contents

- [Import Power View](#Import\Power\View)
  - [Enumerating ACLs using PowerView](#Enumerating\ACLs\using\PowerView)
        - [Throws all information available](#Throws\all\information\available)
      - [Targeted Enumeration (more effective)](#Targeted\Enumeration\(more\effective))
    - [Get the SID of our target user to search effectively](#Get\the\SID\of\our\target\user\to\search\effectively)
        - [Then:](#Then:)
      - [Using Get-DomainObjectACL](#Using\Get-DomainObjectACL)
      - [Performing a Reverse Search & Mapping to a GUID Value](#Performing\a\Reverse\Search\&\Mapping\to\a\GUID\Value)
      - [Using the -ResolveGUIDs Flag](#Using\the\-ResolveGUIDs\Flag)
    - [Creating a list of domain users](#Creating\a\list\of\domain\users)
      - [A Useful foreach Loop](#A\Useful\foreach\Loop)
      - [Further Enumeration of Rights Using damundsen](#Further\Enumeration\of\Rights\Using\damundsen)
      - [Investigating the Help Desk Level 1 Group with Get-DomainGroup](#Investigating\the\Help\Desk\Level\1\Group\with\Get-DomainGroup)
      - [Investigating the Information Technology Group](#Investigating\the\Information\Technology\Group)
      - [Looking for Interesting Access](#Looking\for\Interesting\Access)
  - [Enumerating ACLs with BloodHound](#Enumerating\ACLs\with\BloodHound)
      - [Viewing Node Info through BloodHound](#Viewing\Node\Info\through\BloodHound)
      - [Investigating ForceChangePassword Further](#Investigating\ForceChangePassword\Further)
      - [Viewing Potential Attack Paths through BloodHound](#Viewing\Potential\Attack\Paths\through\BloodHound)
      - [Viewing Pre-Build queries through BloodHound](#Viewing\Pre-Build\queries\through\BloodHound)

#powerview #ACLenumeration #windows #activedirectory 
Some of the Active Directory object permissions and types that we as attackers are interested in:

- **GenericAll** - full rights to the object (add users to a group or reset user's password)
    

- **GenericWrite** - update object's attributes (i.e logon script)
    

- **WriteOwner** - change object owner to attacker controlled user take over the object
    

- **WriteDACL** - modify object's ACEs and give attacker full control right over the object
    

- **AllExtendedRights** - ability to add user to a group or reset password
    

- **ForceChangePassword** - ability to change user's password
    

- **Self (Self-Membership)** - ability to add yourself to a group
##### Import Power View
```powershell
C:\Tools>Import-Module .\PowerView.ps1
C:\Tools>
```
## Enumerating ACLs using PowerView

##### Throws all information available
```powershell
Find-InterestingDomainAcl
```
- Takes a long time to go through


#### Targeted Enumeration (more effective)
- need a user that we have control over. `wley` for example sake
### Get the SID of our target user to search effectively
```powershell
PS C:\htb> Import-Module .\PowerView.ps1
PS C:\htb> $sid = Convert-NameToSid wley
```
##### Then:
Use `Get-DomainObjectACL` to perform targeted searches. 

in this case we are using this function to find all domain objects that out user has rights over by mapping the user's SID using the $sid variable to the `SecurityIdentifier` property which is what tells us who has the given right over an object. One important thing to note is if we search without the glag `ResolveGUIDs` it won't be sufficient


#### Using Get-DomainObjectACL

  Using Get-DomainObjectACL

```powershell-session
PS C:\htb> Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```

We could Google for the GUID value `00299570-246d-11d0-a768-00aa006e0529` and uncover [this](https://docs.microsoft.com/en-us/windows/win32/adschema/r-user-force-change-password) page showing that the user has the right to force change the other user's password. Alternatively, we could do a reverse search using PowerShell to map the right name back to the GUID value.

#### Performing a Reverse Search & Mapping to a GUID Value

  Performing a Reverse Search & Mapping to a GUID Value

```powershell
PS C:\htb> $guid= "00299570-246d-11d0-a768-00aa006e0529"
PS C:\htb> Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
```


#### Using the -ResolveGUIDs Flag

  Using the -ResolveGUIDs Flag

```powershell
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
It is essential that if one works to fall back to another tool. Easy first though.


###  Creating a list of domain users 
```powershell
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```

We then read each line of the file using a `foreach` loop, and use the `Get-Acl` cmdlet to retrieve ACL information for each domain user by feeding each line of the `ad_users.txt` file to the `Get-ADUser` cmdlet. We then select just the `Access property`, which will give us information about access rights. Finally, we set the `IdentityReference` property to the user we are in control of (or looking to see what rights they have), in our case, `wley`.


#### A Useful foreach Loop

  A Useful foreach Loop

```powershell-session
PS C:\htb> foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}
```



#### Further Enumeration of Rights Using damundsen

  Further Enumeration of Rights Using damundsen

```powershell-session
PS C:\htb> $sid2 = Convert-NameToSid damundsen
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid2} -Verbose
```


 our user `damundsen` has `GenericWrite` privileges over the `Help Desk Level 1` group. This means, among other things, that we can add any user (or ourselves) to this group and inherit any rights that this group has applied to it


Let's look and see if this group is nested into any other groups, remembering that nested group membership will mean that any users in group A will inherit all rights of any group that group A is nested into (a member of). A quick search shows us that the `Help Desk Level 1` group is nested into the `Information Technology` group, meaning that we can obtain any rights that the `Information Technology` group grants to its members if we just add ourselves to the `Help Desk Level 1` group where our user `damundsen` has `GenericWrite` privileges.

#### Investigating the Help Desk Level 1 Group with Get-DomainGroup

  Investigating the Help Desk Level 1 Group with Get-DomainGroup

```powershell-session
PS C:\htb> Get-DomainGroup -Identity "Help Desk Level 1" | select memberof
```


#### Investigating the Information Technology Group

  Investigating the Information Technology Group

```powershell-session
PS C:\htb> $itgroupsid = Convert-NameToSid "Information Technology"
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $itgroupsid} -Verbose
```


Finally, let's see if the `adunn` user has any type of interesting access that we may be able to leverage to get closer to our goal.

#### Looking for Interesting Access

  Looking for Interesting Access

```powershell-session
PS C:\htb> $adunnsid = Convert-NameToSid adunn 
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $adunnsid} -Verbose
```

## Enumerating ACLs with BloodHound

Now that we've enumerated the attack path using more manual methods like PowerView and built-in PowerShell cmdlets, let's look at how much easier this would have been to identify using the extremely powerful BloodHound tool. Let's take the data we gathered earlier with the SharpHound ingestor and upload it to BloodHound. Next, we can set the `wley` user as our starting node, select the `Node Info` tab and scroll down to `Outbound Control Rights`. This option will show us objects we have control over directly, via group membership, and the number of objects that our user could lead to us controlling via ACL attack paths under `Transitive Object Control`. If we click on the `1` next to `First Degree Object Control`, we see the first set of rights that we enumerated, `ForceChangePassword` over the `damundsen` user.

#### Viewing Node Info through BloodHound

![image](https://academy.hackthebox.com/storage/modules/143/wley_damundsen.png)

If we right-click on the line between the two objects, a menu will pop up. If we select `Help`, we will be presented with help around abusing this ACE, including:

- More info on the specific right, tools, and commands that can be used to pull off this attack
- Operational Security (Opsec) considerations
- External references.


- More info on the specific right, tools, and commands that can be used to pull off this attack
- Operational Security (Opsec) considerations
- External references.

We'll dig into this menu more later on.

#### Investigating ForceChangePassword Further

![image](https://academy.hackthebox.com/storage/modules/143/help_edge.png)

If we click on the `16` next to `Transitive Object Control`, we will see the entire path that we painstakingly enumerated above. From here, we could leverage the help menus for each edge to find ways to best pull off each attack.

#### Viewing Potential Attack Paths through BloodHound

![image](https://academy.hackthebox.com/storage/modules/143/wley_path.png)

Finally, we can use the pre-built queries in BloodHound to confirm that the `adunn` user has DCSync rights.

#### Viewing Pre-Build queries through BloodHound

![image](https://academy.hackthebox.com/storage/modules/143/adunn_dcsync.png)

We've now enumerated these attack paths in multiple ways. The next step will be performing this attack chain from start to finish. Let's dig in!


Get-ObjectAcl -SamAccountName delegate -ResolveGUIDs | ? {$_.ActiveDirectoryRights -eq "GenericAll"}  