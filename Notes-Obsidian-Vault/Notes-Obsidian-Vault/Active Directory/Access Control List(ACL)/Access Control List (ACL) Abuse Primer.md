## Table of Contents

  - [Table of Contents](#Table\of\Contents)
  - [Access Control List (ACL) Overview](#Access\Control\List\(ACL)\Overview)
      - [Viewing forend's ACL](#Viewing\forend's\ACL)
      - [Viewing the SACLs through the Auditing Tab](#Viewing\the\SACLs\through\the\Auditing\Tab)
  - [Access Control Entities (ACEs)](#Access\Control\Entities\(ACEs))
      - [Viewing Permissions through Active Directory Users & Computers](#Viewing\Permissions\through\Active\Directory\Users\&\Computers)
    - [Why are ACEs important?](#Why\are\ACEs\important?)

## Table of Contents

  - [Access Control List (ACL) Overview](#Access\Control\List\(ACL)\Overview)
      - [Viewing forend's ACL](#Viewing\forend's\ACL)
      - [Viewing the SACLs through the Auditing Tab](#Viewing\the\SACLs\through\the\Auditing\Tab)
  - [Access Control Entities (ACEs)](#Access\Control\Entities\(ACEs))
      - [Viewing Permissions through Active Directory Users & Computers](#Viewing\Permissions\through\Active\Directory\Users\&\Computers)
    - [Why are ACEs important?](#Why\are\ACEs\important?)


## Access Control List (ACL) Overview

In their simplest form, ACLs are lists that define a) who has access to which asset/resource and b) the level of access they are provisioned. The settings themselves in an ACL are called `Access Control Entities` (`ACEs`). Each ACE maps back to a user, group, or process (also known as security principals) and defines the rights granted to that principal. Every object has an ACL, but can have multiple ACEs because multiple security principals can access objects in AD. ACLs can also be used for auditing access within AD.

There are two types of ACLs:

1. `Discretionary Access Control List` (`DACL`) - defines which security principals are granted or denied access to an object. DACLs are made up of ACEs that either allow or deny access. When someone attempts to access an object, the system will check the DACL for the level of access that is permitted. If a DACL does not exist for an object, all who attempt to access the object are granted full rights. If a DACL exists, but does not have any ACE entries specifying specific security settings, the system will deny access to all users, groups, or processes attempting to access it.
    
2. `System Access Control Lists` (`SACL`) - allow administrators to log access attempts made to secured objects

We see the ACL for the user account `forend` in the image below. Each item under `Permission entries` makes up the `DACL` for the user account, while the individual entries (such as `Full Control` or `Change Password`) are ACE entries showing rights granted over this user object to various users and groups.

#### Viewing forend's ACL

![image](https://academy.hackthebox.com/storage/modules/143/DACL_example.png)

The SACLs can be seen within the `Auditing` tab.

#### Viewing the SACLs through the Auditing Tab

![image](https://academy.hackthebox.com/storage/modules/143/SACL_example.png)

---

## Access Control Entities (ACEs)

As stated previously, Access Control Lists (ACLs) contain ACE entries that name a user or group and the level of access they have over a given securable object. There are `three` main types of ACEs that can be applied to all securable objects in AD:

|**ACE**|**Description**|
|---|---|
|`Access denied ACE`|Used within a DACL to show that a user or group is explicitly denied access to an object|
|`Access allowed ACE`|Used within a DACL to show that a user or group is explicitly granted access to an object|
|`System audit ACE`|Used within a SACL to generate audit logs when a user or group attempts to access an object. It records whether access was granted or not and what type of access occurred|

Each ACE is made up of the following `four` components:

1. The security identifier (SID) of the user/group that has access to the object (or principal name graphically)
2. A flag denoting the type of ACE (access denied, allowed, or system audit ACE)
3. A set of flags that specify whether or not child containers/objects can inherit the given ACE entry from the primary or parent object
4. An [access mask](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-dtyp/7a53f60e-e730-4dfe-bbe9-b21b62eb790b?redirectedfrom=MSDN) which is a 32-bit value that defines the rights granted to an object

We can view this graphically in `Active Directory Users and Computers` (`ADUC`). In the example image below, we can see the following for the ACE entry for the user `forend`:

#### Viewing Permissions through Active Directory Users & Computers

![image](https://academy.hackthebox.com/storage/modules/143/ACE_example.png)

1. The security principal is Angela Dunn (adunn@inlanefreight.local)
2. The ACE type is `Allow`
3. Inheritance applies to the "This object and all descendant objects,” meaning any child objects of the `forend` object would have the same permissions granted
4. The rights granted to the object, again shown graphically in this example

### Why are ACEs important?
Attacker utilize ACE entries to either further access or establish persistence. These can be great for us as penetration testers as many organizations are unaware of the ACEs applied to each object or the impact that these can have if applied incorrectly. They cannot be detected by vulnerability scanning tools, and often go unchecked if applied incorrectly. 

ACL abuse can be a great way for us to move laterally/vertically and even achieve full domain compromise. Some example Active Directory object security permissions are as follows. These can be enumerated (and visualized) using a tool such as BloodHound, and are all abusable with PowerView, among other tools:


- `ForceChangePassword` Abues with `Set- DomainUserPassword`
- `Add Members` abused with `Add-DomainGroupMember`
- `GenericAll` abused with `Set-DomainObject`
- `WriteOwner` abused with `Set-Domain ObjectOwner`
- `WriteDACL` abused with `Add-DomainObjectACL`
- `AllExtendedRights` abused with `Set-DomainUserPassword` or `Add-DomainGroupMember`
- `Addself` abused with `Add-DomainGroupMember`

- [ForceChangePassword](https://bloodhound.readthedocs.io/en/latest/data-analysis/edges.html#forcechangepassword) - gives us the right to reset a user's password without first knowing their password (should be used cautiously and typically best to consult our client before resetting passwords).

- [GenericWrite](https://bloodhound.readthedocs.io/en/latest/data-analysis/edges.html#genericwrite) - gives us the right to write to any non-protected attribute on an object. If we have this access over a user, we could assign them an SPN and perform a Kerberoasting attack (which relies on the target account having a weak password set). Over a group means we could add ourselves or another security principal to a given group. Finally, if we have this access over a computer object, we could perform a resource-based constrained delegation attack which is outside the scope of this module.

- `AddSelf` - shows security groups that a user can add themselves to.
- [GenericAll](https://bloodhound.readthedocs.io/en/latest/data-analysis/edges.html#genericall) - this grants us full control over a target object. Again, depending on if this is granted over a user or group, we could modify group membership, force change a password, or perform a targeted Kerberoasting attack. If we have this access over a computer object and the [Local Administrator Password Solution (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) is in use in the environment, we can read the LAPS password and gain local admin access to the machine which may aid us in lateral movement or privilege escalation in the domain if we can obtain privileged controls or gain some sort of privileged access.

This graphic(below), adapted from a graphic created by [Charlie Bromberg (Shutdown)](https://twitter.com/_nwodtuhs), shows an excellent breakdown of the varying possible ACE attacks and the tools to perform these attacks from both Windows and Linux (if applicable). In the following few sections, we will mainly cover enumerating and performing these attacks from a Windows attack host with mentions of how these attacks could be performed from Linux. A later module specifically on ACL Attacks will go much further in-depth on each of the attacks listed in this graphic and how to perform them from Windows and Linux.

![image](https://academy.hackthebox.com/storage/modules/143/ACL_attacks_graphic.png)

We will run into many other interesting ACEs (privileges) in Active Directory from time to time. The methodology for enumerating possible ACL attacks using tools such as BloodHound and PowerView and even built-in AD management tools should be adaptable enough to assist us whenever we encounter new privileges in the wild that we may not yet be familiar with. For example, we may import data into BloodHound and see that a user we have control over (or can potentially take over) has the rights to read the password for a Group Managed Service Account (gMSA) through the [ReadGMSAPassword](https://bloodhound.readthedocs.io/en/latest/data-analysis/edges.html#readgmsapassword) edge. In this case, there are tools such as [GMSAPasswordReader](https://github.com/rvazarkar/GMSAPasswordReader) that we could use, along with other methods, to obtain the password for the service account in question. Other times we may come across extended rights such as [Unexpire-Password](https://learn.microsoft.com/en-us/windows/win32/adschema/r-unexpire-password) or [Reanimate-Tombstones](https://learn.microsoft.com/en-us/windows/win32/adschema/r-reanimate-tombstones) using PowerView and have to do a bit of research to figure out how to exploit these for our benefit. It's worth familiarizing yourself with all of the [BloodHound edges](https://bloodhound.readthedocs.io/en/latest/data-analysis/edges.html) and as many Active Directory [Extended Rights](https://learn.microsoft.com/en-us/windows/win32/adschema/extended-rights) as possible as you never know when you may encounter a less common one during an assessment.


`What type of ACL defines which security principals are granted or denied access to an object?`
A: ==DACL==
`Which ACE Entry can be leveraged to perform a targeted Kerberoasting attack`
A: ==Generic All==


