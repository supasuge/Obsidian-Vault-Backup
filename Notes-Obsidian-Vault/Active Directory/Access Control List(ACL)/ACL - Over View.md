## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Access Control List (ACL) Overview](#access\control\list\(acl)\overview)
  - [Discretionary Access Control List (DACL)](#Discretionary\Access\Control\List\(DACL))
  - [System Access Control List (SACL)](#System\Access\Control\List\(SACL))
    - [Visualizing ACLs](#Visualizing\ACLs)
- [Access Control Entities (ACEs)](#access\control\entities\(aces))
    - [Visualizing ACEs](#Visualizing\ACEs)
  - [The Significance of ACEs](#The\Significance\of\ACEs)
  - [Embracing Continuous Learning](#Embracing\Continuous\Learning)

## Table of Contents

- [Access Control List (ACL) Overview](#access\control\list\(acl)\overview)
  - [Discretionary Access Control List (DACL)](#Discretionary\Access\Control\List\(DACL))
  - [System Access Control List (SACL)](#System\Access\Control\List\(SACL))
    - [Visualizing ACLs](#Visualizing\ACLs)
- [Access Control Entities (ACEs)](#access\control\entities\(aces))
    - [Visualizing ACEs](#Visualizing\ACEs)
  - [The Significance of ACEs](#The\Significance\of\ACEs)
  - [Embracing Continuous Learning](#Embracing\Continuous\Learning)

Certainly, let's transform this content into a detailed Markdown format for your Infosec blog:

---

# Access Control List (ACL) Overview

Access Control Lists (ACLs) are fundamental components of security in Active Directory (AD). At their core, ACLs serve two crucial purposes:

1. **Access Management:** ACLs define who can access specific assets or resources within AD and the level of access they are granted. These access settings are represented as `Access Control Entries` (ACEs).

2. **Auditing:** ACLs can also be used to audit access to objects within AD, providing a valuable tool for monitoring and maintaining security.

There are two primary types of ACLs in AD:

## Discretionary Access Control List (DACL)

The DACL is the heart of access management. It determines which security principals (users, groups, or processes) are granted or denied access to an object. The DACL consists of ACEs, each of which can either allow or deny access. If an object lacks a DACL, any attempt to access it grants full rights. However, if a DACL exists without specific ACE entries, all access attempts are denied.

## System Access Control List (SACL)

The SACL serves the purpose of auditing access attempts made to secured objects. It enables administrators to monitor and log access activities, providing valuable insights into the security of AD.

### Visualizing ACLs

In the example below, we can see the ACL for the user account `forend`:

![Viewing forend's ACL](https://academy.hackthebox.com/storage/modules/143/DACL_example.png)

For this user account, the `Permission entries` make up the `DACL`. Each entry, such as `Full Control` or `Change Password`, represents an ACE that specifies the rights granted to various users and groups.

The SACLs, on the other hand, are accessible through the `Auditing` tab:

![Viewing the SACLs through the Auditing Tab](https://academy.hackthebox.com/storage/modules/143/SACL_example.png)

# Access Control Entities (ACEs)

As mentioned earlier, ACLs contain ACE entries that determine who has access to an object and the level of access they possess. There are three main types of ACEs that can be applied to all securable objects in AD:

| **ACE Type**        | **Description**                                                      |
|---------------------|----------------------------------------------------------------------|
| Access denied ACE   | Explicitly denies access to a user or group.                         |
| Access allowed ACE  | Explicitly grants access to a user or group.                         |
| System audit ACE    | Generates audit logs when a user or group attempts to access an object. Records access details and outcomes. |

Each ACE comprises four components:

1. **Security Identifier (SID):** Identifies the user or group with access to the object.
2. **ACE Type:** Specifies whether the ACE denies, allows, or audits access.
3. **Inheritance Flags:** Determine whether child containers/objects can inherit the ACE from the parent object.
4. **Access Mask:** A 32-bit value defining the rights granted to the object.

### Visualizing ACEs

In Active Directory Users and Computers (ADUC), you can view ACEs graphically. In the example image below, we examine the ACE entry for the user `forend`:

![Viewing Permissions through Active Directory Users & Computers](https://academy.hackthebox.com/storage/modules/143/ACE_example.png)

1. The security principal is Angela Dunn (adunn@inlanefreight.local).
2. The ACE type is `Allow`.
3. Inheritance applies to "This object and all descendant objects," meaning child objects inherit these permissions.
4. The rights granted to the object are shown graphically.

## The Significance of ACEs

ACE entries are essential for ethical hackers and penetration testers. Organizations often overlook or misconfigure ACEs, making them attractive attack vectors. ACE abuse can facilitate lateral movement, privilege escalation, and even domain compromise. For example:

- `ForceChangePassword`: Allows resetting a user's password without knowing the current password, often used cautiously.
- `GenericWrite`: Grants the right to write to non-protected attributes, enabling various attacks, such as Kerberoasting.
- `GenericAll`: Provides full control over an object, allowing for extensive manipulation.
- `AddSelf`: Shows security groups a user can add themselves to, potentially leading to privilege escalation.

Understanding and exploiting ACEs is a valuable skill for ethical hackers, as these permissions are often undetected by vulnerability scanning tools.

## Embracing Continuous Learning

In the ever-evolving landscape of AD security, there's always more to discover. Familiarize yourself with all [BloodHound edges](https://bloodhound.readthedocs.io/en/latest/data-analysis/edges.html) and as many Active Directory [Extended Rights](https://learn.microsoft.com/en-us/windows/win32/adschema/extended-rights) as possible. These knowledge assets will prove invaluable whenever you encounter new privileges or security challenges during assessments.

