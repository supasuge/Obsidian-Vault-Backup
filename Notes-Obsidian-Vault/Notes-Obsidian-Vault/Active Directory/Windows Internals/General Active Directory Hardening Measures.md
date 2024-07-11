## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [LAPS](#LAPS)
      - [Audit Policy Settings (Logging and Monitoring)](#Audit\Policy\Settings\(Logging\and\Monitoring))
      - [Group Policy Security Settings](#Group\Policy\Security\Settings)
    - [Group Policy Objects (GPOs)](#Group\Policy\Objects\(GPOs))
      - [1. **Settings and Configurations:**](#1.\**Settings\and\Configurations:**)
      - [2. **Linking to Containers:**](#2.\**Linking\to\Containers:**)
      - [3. **Order of Precedence:**](#3.\**Order\of\Precedence:**)
      - [4. **Security Filtering:**](#4.\**Security\Filtering:**)
      - [5. **Use in Security:**](#5.\**Use\in\Security:**)
      - [6. **Potential for Abuse:**](#6.\**Potential\for\Abuse:**)
    - [Implementing Security through GPOs:](#Implementing\Security\through\GPOs:)
      - [Order of Precedence](#Order\of\Precedence)
  - [GPO Order of Precedence](#GPO\Order\of\Precedence)

## Table of Contents

      - [LAPS](#LAPS)
      - [Audit Policy Settings (Logging and Monitoring)](#Audit\Policy\Settings\(Logging\and\Monitoring))
      - [Group Policy Security Settings](#Group\Policy\Security\Settings)
    - [Group Policy Objects (GPOs)](#Group\Policy\Objects\(GPOs))
      - [1. **Settings and Configurations:**](#1.\**Settings\and\Configurations:**)
      - [2. **Linking to Containers:**](#2.\**Linking\to\Containers:**)
      - [3. **Order of Precedence:**](#3.\**Order\of\Precedence:**)
      - [4. **Security Filtering:**](#4.\**Security\Filtering:**)
      - [5. **Use in Security:**](#5.\**Use\in\Security:**)
      - [6. **Potential for Abuse:**](#6.\**Potential\for\Abuse:**)
    - [Implementing Security through GPOs:](#Implementing\Security\through\GPOs:)
      - [Order of Precedence](#Order\of\Precedence)
  - [GPO Order of Precedence](#GPO\Order\of\Precedence)

The microsoft local administrator password solution (LAPS) is used to randomize and rotate local admin passwords on Windows hosts and prevent laterla movment

#### LAPS

Accounts can be set up to have their password rotated on a fixed interval (i.e., 12 hours, 24 hours, etc.). This free tool can be beneficial in reducing the impact of an individual compromised host in an AD environment.




#### Audit Policy Settings (Logging and Monitoring)

Every organization needs to have logging and monitoring setup to detect and react to unexpected changes or activities that may indicate an attack. Effective logging and monitoring can be used to detect an attacker or unauthorized employee adding a user or computer, modifying an object in AD, changing an account password, accessing a system in an unauthorized or non-standard manner, performing an attack such as password spraying, or more advanced attacks such as modern Kerberos attacks.



#### Group Policy Security Settings

As mentioned earlier in the module, Group Policy Objects (GPOs) are virtual collections of policy settings that can be applied to specific users, groups, and computers at the OU level. These can be used to apply a wide variety of [security policies](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/security-policy-settings) to help harden Active Directory. The following is a non-exhaustive list of the types of security policies that can be applied:

- [Account Policies](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/account-policies) - Manage how user accounts interact with the domain. These include the password policy, account lockout policy, and Kerberos-related settings such as the lifetime of Kerberos tickets
    
- [Local Policies](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/security-options) - These apply to a specific computer and include the security event audit policy, user rights assignments (user privileges on a host), and specific security settings such as the ability to install drivers, whether the administrator and guest accounts are enabled, renaming the guest and administrator accounts, preventing users from installing printers or using removable media, and a variety of network access and network security controls.
    
- [Software Restriction Policies](https://docs.microsoft.com/en-us/windows-server/identity/software-restriction-policies/software-restriction-policies) - Settings to control what software can be run on a host.
    
- [Application Control Policies](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) - Settings to control which applications can be run by certain users/groups. This may include blocking certain users from running all executables, Windows Installer files, scripts, etc. Administrators use [AppLocker](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview) to restrict access to certain types of applications and files. It is not uncommon to see organizations block access to CMD and PowerShell (among other executables) for users that do not require them for their day-to-day job. These policies are imperfect and can often be bypassed but necessary for a defense-in-depth strategy.
    
- [Advanced Audit Policy Configuration](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/secpol-advanced-security-audit-policy-settings) - A variety of settings that can be adjusted to audit activities such as file access or modification, account logon/logoff, policy changes, privilege usage, and more.


Update Management (SCCM/WSUS), Group Managed Service Accounts (gMSA), and Security Groups:

**Update Management (SCCM/WSUS):**

- **Critical Importance:** Ensures proper patching for Windows/Active Directory systems.
- **WSUS (Windows Server Update Service):** Minimizes manual patching, can be installed as a Windows Server role.
- **SCCM (System Center Configuration Manager):** Paid solution offering more features, relies on WSUS.
- **Benefits:** Timely deployment of patches, maximizes coverage, prevents missed critical security patches.
- **Manual Patching Risks:** Time-consuming, potential to miss systems, leaving them vulnerable.

**Group Managed Service Accounts (gMSA):**

- **Domain-Managed Accounts:** Higher security for non-interactive applications, services, processes, and automated tasks.
- **Automatic Password Management:** 120-character password, generated by domain controller, changed regularly.
- **User Independence:** Password unknown to users, allows credentials across multiple hosts.

**Security Groups:**

- **Purpose:** Assign access to network resources, manage rights at the group level.
- **Default Groups:** Created during AD installation (e.g., Account Operators, Administrators, Backup Operators, Domain Admins, Domain Users).
- **Application:** Assign permission to resources like file shares, folders, printers, documents.
- **Efficiency:** Enables granular permissions en masse, avoiding individual user management.


**Account Separation:**

- **Two Separate Accounts for Administrators:** Regular and administrative tasks must be separated to reduce the risk of a compromised system granting access to sensitive areas.
    - _Example:_ `sjones` for emails and `sjones_adm` for administrative tasks.
- **Different Passwords:** Prevents password reuse attacks.

**Password Complexity Policies + Passphrases + 2FA:**

- **Passphrases & Large Passwords:** Preferable to standard short passwords; resistant to brute-force attacks.
- **Password Complexity Insufficiency:** Simple complex passwords like `Welcome1` are still vulnerable.
- **Password Filters & Length Requirements:** Help enforce strong password practices.
- **Multi-Factor Authentication (MFA):** Adds an extra layer of security.

**Limiting Domain Admin Account Usage:**

- **Restrict Usage:** Only for logging into Domain Controllers to reduce potential attack paths.
- **Prevents Passwords in Memory:** Reduces risks across the environment.

**Periodically Auditing and Removing Stale Users and Objects:**

- **Clean Up Unused Accounts:** Removes easy footholds or methods for attacks within the domain.
    - _Example:_ A privileged service account with an old, weak password.

**Auditing Permissions and Access:**

- **Regular Access Control Audits:** Ensures that access levels are appropriate and limited.
- **Review Privileged Security Groups:** Minimizes attack surfaces.

**Audit Policies & Logging:**

- **Robust Logging & Anomaly Detection:** Detects suspicious activities, such as failed login attempts or Kerberoasting attacks.
- **Compliance with Microsoft's Recommendations:** Helps in early detection of compromise.

**Using Restricted Groups:**

- **Control Group Membership via Group Policy:** Enhances control over privileged groups.
- **Restricted Access to Key Groups:** Reduces risk.

**Limiting Server Roles:**

- **Avoid Additional Roles on Sensitive Hosts:** Reduces the attack surface.
    - _Example:_ Do not install the IIS role on a Domain Controller.

**Limiting Local Admin and RDP Rights:**

- **Tight Control over Local Admin Rights:** Ensures that privilege levels match user needs.
- **Restrict RDP Rights:** Prevents sensitive data exposure or potential privilege escalation attacks.


### Group Policy Objects (GPOs)

**Group Policy Objects (GPOs)** are the essential building blocks in Group Policy infrastructure. They contain specific configuration settings that can be applied to users or computers within a domain. Below, we will break down different aspects of GPOs:

#### 1. **Settings and Configurations:**

GPOs can enforce various settings and configurations, from simple user preferences to complex security policies.

- **Example:** Setting up a password policy that defines the minimum length and complexity requirements.

#### 2. **Linking to Containers:**

GPOs can be linked to Organizational Units (OUs), domains, or sites within Active Directory, allowing administrators to target specific parts of the organization.

- **Example:** Linking a GPO that disables USB ports to an OU containing computers in a secure lab environment.

#### 3. **Order of Precedence:**

When multiple GPOs are applied, they are processed in the following order: Local, Site, Domain, and OU. This is often remembered as LSDO.

- **Example:** A GPO at the OU level may override a conflicting setting at the Domain level.

#### 4. **Security Filtering:**

Administrators can use security filtering to apply GPOs to specific users, groups, or computers.

- **Example:** Applying a specific wallpaper setting only to the marketing department.

#### 5. **Use in Security:**

Group Policy plays a vital role in implementing security measures across a domain.

- **Example:** Enforcing encryption protocols for network communication.

#### 6. **Potential for Abuse:**

Improper configuration or insecure handling of GPOs can lead to security vulnerabilities.

- **Example:** An attacker gaining control over a GPO could modify login scripts to include malicious code, leading to lateral movement within the network.

### Implementing Security through GPOs:

Here's how GPOs can be used to strengthen an organization's security posture:

1. **Centralized Control:** GPOs enable centralized management of security policies, allowing for consistent enforcement and simplified updates.
    
2. **Fine-Grained Policy Management:** Administrators can tailor policies for different parts of the organization, enhancing control and adaptability.
    
3. **Enforcing Compliance Standards:** GPOs can be used to ensure that systems meet regulatory requirements, such as HIPAA or GDPR.
    
4. **Mitigating Common Vulnerabilities:** By enforcing security policies like regular updates and restricting unnecessary services, GPOs can reduce potential attack vectors.
    
5. **Monitoring and Auditing:** Using GPOs, administrators can enable logging and monitoring to detect and respond to suspicious activities.


Some examples of things we can do with GPOs may include:

- Establishing different password policies for service accounts, admin accounts, and standard user accounts using separate GPOs
- Preventing the use of removable media devices (such as USB devices)
- Enforcing a screensaver with a password
- Restricting access to applications that a standard user may not need, such as cmd.exe and PowerShell
- Enforcing audit and logging policies
- Blocking users from running certain types of programs and scripts
- Deploying software across a domain
- Blocking users from installing unapproved software
- Displaying a logon banner whenever a user logs into a system
- Disallowing LM hash usage in the domain
- Running scripts when computers start/shutdown or when a user logs in/out of their machine

Let's use as example a default Windows Server 2008 Active Directory implementation, password complexity is enforced by default. The password complexity requirements are as follows:

- Passwords must be at least 7 characters long.
- Passwords must contain characters from at least three of the following four categories:
    - Uppercase characters (A-Z)
    - Lowercase characters (a-z)
    - Numbers (0-9)
    - Special characters (e.g. !@#$%^&*()_+|~-=`{}[]:";'<>?,./)


#### Order of Precedence

|**Level**|**Description**|
|---|---|
|`Local Group Policy`|The policies are defined directly to the host locally outside the domain. Any setting here will be overwritten if a similar setting is defined at a higher level.|
|`Site Policy`|Any policies specific to the Enterprise Site that the host resides in. Remember that enterprise environments can span large campuses and even across countries. So it stands to reason that a site might have its own policies to follow that could differentiate it from the rest of the organization. Access Control policies are a great example of this. Say a specific building or `site` performs secret or restricted research and requires a higher level of authentication for access to resources. You could specify those settings at the site level and ensure they are linked so as not to be overwritten by domain policy. This is also a great way to perform actions like printer and share mapping for users in specific sites.|
|`Domain-wide Policy`|Any settings you wish to have applied across the domain as a whole. For example, setting the password policy complexity level, configuring a Desktop background for all users, and setting a Notice of Use and Consent to Monitor banner at the login screen.|
|`Organizational Unit` (OU)|These settings would affect users and computers who belong to specific OUs. You would want to place any unique settings here that are role-specific. For example, the mapping of a particular share drive that can only be accessed by HR, access to specific resources like printers, or the ability for IT admins to utilize PowerShell and command-prompt.|
|`Any OU Policies nested within other OU's`|Settings at this level would reflect special permissions for objects within nested OUs. For example, providing Security Analysts a specific set of Applocker policy settings that differ from the standard IT Applocker settings.|

We can manage Group Policy from the Group Policy Management Console (found under Administrative Tools in the Start Menu on a domain controller), custom applications, or using the PowerShell [GroupPolicy](https://docs.microsoft.com/en-us/powershell/module/grouppolicy/?view=windowsserver2022-ps) module via command line. The Default Domain Policy is the default GPO that is automatically created and linked to the domain. It has the highest precedence of all GPOs and is applied by default to all users and computers. Generally, it is best practice to use this default GPO to manage default settings that will apply domain-wide. The Default Domain Controllers policy is also created automatically with a domain and sets baseline security and auditing settings for all domain controllers in a given domain. It can be customized as needed, like any GPO.

## GPO Order of Precedence

GPOs are processed from the top down when viewing them from a domain organizational standpoint. A GPO linked to an OU at the highest level in an Active Directory network (at the domain level, for example) would be processed first, followed by those linked to a child OU, etc. This means that a GPO linked directly to an OU containing user or computer objects is processed last. In other words, a GPO attached to a specific OU would have precedence over a GPO attached at the domain level because it will be processed last and could run the risk of overriding settings in a GPO higher up in the domain hierarchy. One more thing to keep track of with precedence is that a setting configured in Computer policy will always have a higher priority of the same setting applied to a user. The following graphic illustrates precedence and how it is applied.

^
| 5. Child OU policy 
| 4. Parent OU policy
| 3. Domain policy
| 2. Site policy
| 1. Local security policy
^
Computer Policy settings are applied at startup and then at 90-minute intervals.
User policy settings are applied at the time of login to a domain host, and at regular intervals after that.
Group policy settings are processed from local policy to child OU policies



This image also shows an example of several GPOs being linked to the `Corp` OU. When more than one GPO is linked to an OU, they are processed based on the `Link Order`. The GPO with the lowest Link Order is processed last, or the GPO with link order 1 has the highest precedence, then 2, and 3, and so on. So in our example above, the `Disallow LM Hash` GPO will have precedence over the `Block Removable Media` and `Disable Guest Account` GPOs, meaning it will be processed first.

It is possible to specify the `Enforced` option to enforce settings in a specific GPO. If this option is set, policy settings in GPOs linked to lower OUs `CANNOT` override the settings. If a GPO is set at the domain level with the `Enforced` option selected, the settings contained in that GPO will be applied to all OUs in the domain and cannot be overridden by lower-level OU policies. In the past, this setting was called `No Override` and was set on the container in question under Active Directory Users and Computers. Below we can see an example of an `Enforced` GPO, where the `Logon Banner` GPO is taking precedence over GPOs linked to lower OUs and therefore will not be overridden.



