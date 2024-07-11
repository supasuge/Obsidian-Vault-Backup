## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Enumerating ACLs with PowerView](#Enumerating\ACLs\with\PowerView)
      - [Using Find-InterestingDomainAcl](#Using\Find-InterestingDomainAcl)
      - [Using Get-DomainObjectACL](#Using\Get-DomainObjectACL)
      - [Using the -ResolveGUIDs Flag](#Using\the\-ResolveGUIDs\Flag)
      - [Creating a List of Domain Users](#Creating\a\List\of\Domain\Users)

## Table of Contents

  - [Enumerating ACLs with PowerView](#Enumerating\ACLs\with\PowerView)
      - [Using Find-InterestingDomainAcl](#Using\Find-InterestingDomainAcl)
      - [Using Get-DomainObjectACL](#Using\Get-DomainObjectACL)
      - [Using the -ResolveGUIDs Flag](#Using\the\-ResolveGUIDs\Flag)
      - [Creating a List of Domain Users](#Creating\a\List\of\Domain\Users)


## Enumerating ACLs with PowerView

We can use PowerView to enumerate ACLs, but the task of digging through _all_ of the results will be extremely time-consuming and likely inaccurate. For example, if we run the function `Find-InterestingDomainAcl` we will receive a massive amount of information back that we would need to dig through to make any sense of:

#### Using Find-InterestingDomainAcl

  Using Find-InterestingDomainAcl

```powershell
PS C:\htb> Find-InterestingDomainAcl
```

 Let's focus on the user `wley`, which we obtained after solving the last question in the `LLMNR/NBT-NS Poisoning - from Linux` section. Let's dig in and see if this user has any interesting ACL rights that we could take advantage of. We first need to get the SID of our target user to search effectively.

  #### Get the SID of target user.

```powershell-session
PS C:\htb> Import-Module .\PowerView.ps1
PS C:\htb> $sid = Convert-NameToSid wley
```

We can then use the `Get-DomainObjectACL` function to perform our targeted search.

In the below example, we are using this function to find all domain objects that our user has rights over by mapping the user's SID using the `$sid` variable to the `SecurityIdentifier` property which is what tells us _who_ has the given right over an object. 

One important thing to note is that if we search without the flag `ResolveGUIDs`, we will see results like the below, where the right `ExtendedRight` does not give us a clear picture of what ACE entry the user `wley` has over `damundsen`. This is because the `ObjectAceType` property is returning a GUID value that is not human readable.

Note that this command will take a while to run, especially in a large environment. It may take 1-2 minutes to get a result in our lab.

#### Using Get-DomainObjectACL

  Using Get-DomainObjectACL

```powershell-session
PS C:\htb> Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}
```
We could Google for the GUID value `00299570-246d-11d0-a768-00aa006e0529` and uncover [this](https://docs.microsoft.com/en-us/windows/win32/adschema/r-user-force-change-password) page showing that the user has the right to force change the other user's password. Alternatively, we could do a reverse search using PowerShell to map the right name back to the GUID value.


  
Performing a Reverse Search & Mapping to a GUID Value

```powershell
PS C:\htb> $guid= "00299570-246d-11d0-a768-00aa006e0529"



PS C:\htb> Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
```

This gave us our answer, but would be highly inefficient during an assessment. PowerView has the `ResolveGUIDs` flag, which does this very thing for us. Notice how the output changes when we include this flag to show the human-readable format of the `ObjectAceType` property as `User-Force-Change-Password`.

#### Using the -ResolveGUIDs Flag

  Using the -ResolveGUIDs Flag

```powershell
PS C:\htb> Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid} 
```
 Before moving on, let's take a quick look at how we could do this using the [Get-Acl](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-acl?view=powershell-7.2) and [Get-ADUser](https://docs.microsoft.com/en-us/powershell/module/activedirectory/get-aduser?view=windowsserver2022-ps) cmdlets which we may find available to us on a client system. Knowing how to perform this type of search without using a tool such as PowerView is greatly beneficial and could set us apart from our peers. We may be able to use this knowledge to achieve results when a client has us work from one of their systems, and we are restricted down to what tools are readily available on the system without the ability to pull in any of our own.

This example is not very efficient, and the command can take a long time to run, especially in a large environment. It will take much longer than the equivalent command using PowerView. In this command, we've first made a list of all domain users with the following command:

#### Creating a List of Domain Users


```powershell
PS C:\htb> Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```












