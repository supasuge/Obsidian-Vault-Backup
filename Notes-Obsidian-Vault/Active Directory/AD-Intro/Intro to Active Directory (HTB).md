## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [**History of Active Directory**](#**history\of\active\directory**)
- [Active Directory Research Over the years~](#active\directory\research\over\the\years~)
        - [AD Attacks & Tools Timeline](#AD\Attacks\&\Tools\Timeline)
- [Active Directory Structure](#active\directory\structure)
    - [What Active directory structure can contain one or more domains?](#What\Active\directory\structure\can\contain\one\or\more\domains?)
    - [True or Flase; it can be common to see multiple domains linked together by trust relationships.](#True\or\Flase;\it\can\be\common\to\see\multiple\domains\linked\together\by\trust\relationships.)
    - [ Active Directory provides authentication and <____> within a Windows domain environment.](# Active\Directory\provides\authentication\and\<____>\within\a\Windows\domain\environment.)
- [Active Directory Terminology](#active\directory\terminology)
      - [Domain](#Domain)
      - [Forest](#Forest)
      - [Tree](#Tree)
      - [Container](#Container)
      - [Leaf](#Leaf)
      - [Read-only Domain Controller](#Read-only\Domain\Controller)
      - [Replication](#Replication)
      - [Service Principal Name (SPN)](#Service\Principal\Name\(SPN))
      - [Group Policy Object (GPO)](#Group\Policy\Object\(GPO))
      - [Access Control List (ACL)](#Access\Control\List\(ACL))
      - [Access Control Entities (ACEs)](#Access\Control\Entities\(ACEs))
      - [Discretionary Access Control List (DACL)](#Discretionary\Access\Control\List\(DACL))
      - [System Access Control Lists (SACL)](#System\Access\Control\Lists\(SACL))
      - [Fully Qualified Domain Name (FQDN)](#Fully\Qualified\Domain\Name\(FQDN))
      - [Tombstone](#Tombstone)
      - [AD Recycle Bin](#AD\Recycle\Bin)
      - [SYSVOL](#SYSVOL)
      - [AdminSDHolder](#AdminSDHolder)
      - [dsHeuristics](#dsHeuristics)
      - [adminCount](#adminCount)
      - [Active Directory Users and Computers (ADUC)](#Active\Directory\Users\and\Computers\(ADUC))
      - [ADSI Edit](#ADSI\Edit)
      - [sIDHistory](#sIDHistory)
      - [NTDS.DIT](#NTDS.DIT)
      - [MSBROWSE](#MSBROWSE)

## Table of Contents

- [**History of Active Directory**](#**history\of\active\directory**)
- [Active Directory Research Over the years~](#active\directory\research\over\the\years~)
        - [AD Attacks & Tools Timeline](#AD\Attacks\&\Tools\Timeline)
- [Active Directory Structure](#active\directory\structure)
    - [What Active directory structure can contain one or more domains?](#What\Active\directory\structure\can\contain\one\or\more\domains?)
    - [True or Flase; it can be common to see multiple domains linked together by trust relationships.](#True\or\Flase;\it\can\be\common\to\see\multiple\domains\linked\together\by\trust\relationships.)
    - [ Active Directory provides authentication and <____> within a Windows domain environment.](# Active\Directory\provides\authentication\and\<____>\within\a\Windows\domain\environment.)
- [Active Directory Terminology](#active\directory\terminology)
      - [Domain](#Domain)
      - [Forest](#Forest)
      - [Tree](#Tree)
      - [Container](#Container)
      - [Leaf](#Leaf)
      - [Read-only Domain Controller](#Read-only\Domain\Controller)
      - [Replication](#Replication)
      - [Service Principal Name (SPN)](#Service\Principal\Name\(SPN))
      - [Group Policy Object (GPO)](#Group\Policy\Object\(GPO))
      - [Access Control List (ACL)](#Access\Control\List\(ACL))
      - [Access Control Entities (ACEs)](#Access\Control\Entities\(ACEs))
      - [Discretionary Access Control List (DACL)](#Discretionary\Access\Control\List\(DACL))
      - [System Access Control Lists (SACL)](#System\Access\Control\Lists\(SACL))
      - [Fully Qualified Domain Name (FQDN)](#Fully\Qualified\Domain\Name\(FQDN))
      - [Tombstone](#Tombstone)
      - [AD Recycle Bin](#AD\Recycle\Bin)
      - [SYSVOL](#SYSVOL)
      - [AdminSDHolder](#AdminSDHolder)
      - [dsHeuristics](#dsHeuristics)
      - [adminCount](#adminCount)
      - [Active Directory Users and Computers (ADUC)](#Active\Directory\Users\and\Computers\(ADUC))
      - [ADSI Edit](#ADSI\Edit)
      - [sIDHistory](#sIDHistory)
      - [NTDS.DIT](#NTDS.DIT)
      - [MSBROWSE](#MSBROWSE)

**AD is a directory service for windows network environments. It is a distributed, hierarchical structure that allows for centralized management of an organizations resources, including users, computers, groups, network devices, file shares, group policies, devices, and trusts. AD provides authentication and authorization functions within a windows domain environemt. It has come under increasing attack in recent years. it is designed to be backward-compatible, and many features are arguably not "secure by default". and it can be easily misconfigured. This weakness can be leveraged to move laterally and vertically within a network and gain unauthorized accesss. AD is essentially a sizeable read-only database accessible to all users within the domain, regardless of their privilege level. A basic AD user account with no added priviileges can enumerate most objects within AD. This fact makes it ectremely important to properly secure an AD implementation because any use3r account, regardless of their priv level, can be used to enumerate the domain and hunt for misconfigurations and flaws thoroughly. Also multiple attacks can be performed with only a standard domain user account, showing the importance of a defense in depth strategy and careful planning focusing on security and hardeing AD, nework segmentation, and least privilege. One example is the noPac attack that was first released in Decemeber of 2021.


Active Directory makes information easy to find and use for administrators and users. AD is highly scalable, supports millions of objects per domain, and allows the creation of additional domains as an organization grows.



AD Organization:
AD Server----------Application Library | File Share
_______________________________________
Domain policy(Enterprise-wide policy and licensing), Group Policy Object(Manages application portfolio and settings), marketing and sales.



# **History of Active Directory**

LDAP, the foundation of Active Directory, was first introduced in [RFCs](https://en.wikipedia.org/wiki/Request_for_Comments) as early as 1971. Active Directory was predated by the [X.500](https://en.wikipedia.org/wiki/X.500) organizational unit concept, which was the earliest version of all directory systems created by Novell and Lotus and released in 1993 as [Novell Directory Services](https://en.wikipedia.org/wiki/NetIQ_eDirectory).


Active Directory was first introduced in the mid-'90s but did not become part of the Windows operating system until the release of Windows Server 2000. Microsoft first attempted to provide directory services in 1990 with the release of Windows NT 3.0. This operating system combined features of the [LAN Manager](https://en.wikipedia.org/wiki/LAN_Manager) protocol and the [OS/2](https://en.wikipedia.org/wiki/OS/2) operating systems, which Microsoft created initially along with IBM lead by [Ed Iacobucci](https://en.wikipedia.org/wiki/Ed_Iacobucci) who also led the design of [IBM DOS](https://en.wikipedia.org/wiki/IBM_PC_DOS) and later co-founded Citrix Systems. The NT operating system evolved throughout the 90s, adapting protocols such as LDAP and Kerberos with Microsoft's proprietary elements. The first beta release of Active Directory was in 1997.


The release of Windows Server 2003 saw extended functionality and improved administration and added the Forest feature, which allows sysadmins to create "containers" of separate domains, users, computers, and other objects all under the same umbrella.  [Active Directory Federation Services (ADFS)](https://en.wikipedia.org/wiki/Active_Directory_Federation_Services) was introduced in Server 2008 to provide Single Sign-On (SSO) to systems and applications for users on Windows Server operating systems. ADFS made it simpler and more streamlined for users to sign into applications and systems, not on their same LAN.

**Active Directory (AD) Fundamentals:**

- **ADFS (Active Directory Federation Services):** Single credential access across organizational boundaries; uses claims-based Access Control Authorization model.
- **Server 2016 Updates:**
    - Cloud migration capability.
    - Security enhancements: user access monitoring, Group Managed Service Accounts (gMSA).
    - gMSA: Mitigation against Kerberoasting attack.
- **Azure AD Connect (2016):** Single sign-on for Office 365 migration.
- **Vulnerabilities & Misconfigurations:**
    - Regular discoveries affecting AD and interfacing technologies (e.g., Microsoft Exchange).
    - Importance of patching and implementing fixes.
- **Structure & Function:**
    - Protocols, user rights, privileges management.
    - Sysadmin tasks; one change can introduce issues elsewhere.
- **Prevalence:**
    - Used by 95% of Fortune 500 companies.
    - On-prem AD still relevant despite cloud transition.
- **Pentesting & Remediation:**
    - Foundational knowledge for better attacking and remediation advice.
    - Confidence in approaching environments of various sizes.


# Active Directory Research Over the years~

##### AD Attacks & Tools Timeline
- 2021~ The [PrintNightmare](https://en.wikipedia.org/wiki/PrintNightmare) vulnerability was released. This was a remote code execution flaw in the Windows Print Spooler that could be used to take over hosts in an AD environment. The [Shadow Credentials](https://posts.specterops.io/shadow-credentials-abusing-key-trust-account-mapping-for-takeover-8ee1a53566ab) attack was released which allows for low privileged users to impersonate other user and computer accounts if conditions are right, and can be used to escalate privileges in a domain. The [noPac](https://www.secureworks.com/blog/nopac-a-tale-of-two-vulnerabilities-that-could-end-in-ransomware) attack was released in mid-December of 2021 when much of the security world was focused on the Log4j vulnerabilities. This attack allows an attacker to gain full control over a domain from a standard domain user account if the right conditions exist.
- 2020: The [ZeroLogon](https://blog.malwarebytes.com/exploits-and-vulnerabilities/2021/01/the-story-of-zerologon/) attack debuted late in 2020. This was a critical flaw that allowed an attacker to impersonate any unpatched domain controller in a network.
- 2019: harmj0y delivered the talk "Kerberoasting revisited" https://www.slideshare.net/harmj0y/derbycon-2019-kerberoasting-revisitedat Derbycon which laid out new approaches to kerberoasting. Elad Shamir released a blog post() outlining techniques for abusing resource-based constrained delegation (RBCD) in Active Directory. The company BC Security released [Empire 3.0](https://github.com/BC-SECURITY/Empire) (now version 4) which was a re-release of the PowerShell Empire framework written in Python3 with many additions and changes.
- 2018~ The "Printer Bug" bug was discovered by Lee Christensen and the [SpoolSample](https://github.com/leechristensen/SpoolSample) PoC tool was released which leverages this bug to coerce Windows hosts to authenticate to other machines via the MS-RPRN RPC interface. harmj0y released the [Rubeus toolkit](http://www.harmj0y.net/blog/redteaming/from-kekeo-to-rubeus/) for attacking Kerberos. Late in 2018 harmj0y also released the blog ["Not A Security Boundary: Breaking Forest Trusts"](http://www.harmj0y.net/blog/redteaming/not-a-security-boundary-breaking-forest-trusts/) which presented key research on performing attacks across forest trusts. The [DCShadow](https://www.dcshadow.com/) attack technique was also released by Vincent LE TOUX and Benjamin Delpy at the Bluehat IL 2018 conference. The [Ping Castle](https://github.com/vletoux/pingcastle/commits/master?after=f128d84e86e675f1ad65c4b9b05bd529e1f9dc7c+34&branch=master) tool was released by Vincent LE TOUX for performing security audits of Active Directory by looking for misconfigurations and other flaws that can raise the risk level of a domain and producing a report that can be used to identify ways to further harden the environment.
- 2017~ The [ASREPRoast](http://www.harmj0y.net/blog/activedirectory/roasting-as-reps/) technique was introduced for attacking user accounts that don't require Kerberos preauthentication. _wald0 and harmj0y delivered the pivotal talk on Active Directory ACL attacks ["ACE Up the Sleeve"](https://www.slideshare.net/harmj0y/ace-up-the-sleeve) at Black Hat and DEF CON. harmj0y released his ["A Guide to Attacking Domain Trusts"](https://www.harmj0y.net/blog/redteaming/a-guide-to-attacking-domain-trusts/) blog post on enumerating and attacking domain trusts.
- 2016~ BloodHound (https://wald0.com/?p=68) was released as a game changing tool for visualizing attack path in AD at https://www.youtube.com/watch?v=wP8ZCczC1OU
- 2015~2015 saw the release of some of the most impactful Active Directory tools of all time. The [PowerShell Empire framework](https://github.com/EmpireProject/Empire) was released. [PowerView 2.0](http://www.harmj0y.net/blog/redteaming/powerview-2-0/) released as part of the (now deprecated) [PowerTools](https://github.com/PowerShellEmpire/PowerTools/) repository, which was a part of the PowerShellEmpire GitHub account. The DCSync attack was first released by Benjamin Delpy and Vincent Le Toux as part of the [mimikatz](https://github.com/gentilkiwi/mimikatz/) tool. It has since been included in other tools. The first stable release of CrackMapExec ([(v1.0.0)](https://github.com/byt3bl33d3r/CrackMapExec/releases?page=3) was introduced. Sean Metcalf gave a talk at Black Hat USA about the dangers of Kerberos Unconstrained Delegation and released an excellent [blog post](https://adsecurity.org/?p=1667) on the topic. The [Impacket](https://github.com/SecureAuthCorp/impacket/releases?page=2) toolkit was also released in 2015. This is a collection of Python tools, many of which can be used to perform Active Directory attacks. It is still actively maintained as of January 2022 and is a key part of most every penetration tester's toolkit.
- 2014- Veil-PowerView first [released](https://github.com/darkoperator/Veil-PowerView/commit/fdfd47c0a1e06e529bf31c93da7caed3479d08e1#diff-1695122ff2b5844b625f6d05c9274ce0a8b75b9b7cde84386df07e24ae98181b). This project later became part of the [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) framework as the (no longer supported) [PowerView.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1) AD recon tool. The Kerberoasting attack was first presented at a conference by [Tim Medin](https://twitter.com/timmedin) at SANS Hackfest 2014.
- 2013- The Responder(https://github.com/SpiderLabs/Responder/commits/master?after=c02c74853298ea52a2bfaa4d250c3898886a44ac+174&branch=master) tool was released. Responder is a toold used for poisoning LLMNR, NBT-NS, and MDNS on an active directory netowrk. It can be used to obtain password hashes and also perform SMB relay attacks(when combined with other tools) to move laterally and vertically in an AD environment. It has evolved considerably over the years and is still actively supported (with new features added) as of January 2022.


# Active Directory Structure

[Active Directory (AD)](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) is a directory service for Windows network environments. It is a distributed, hierarchical structure that allows for centralized management of an organization's resources, including users, computers, groups, network devices and file shares, group policies, servers and workstations, and trusts. AD provides authentication and authorization functions within a Windows domain environment. A directory service, such as [Active Directory Domain Services (AD DS)](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) gives an organization ways to store directory data and make it available to both standard users and administrators on the same network. AD DS stores information such as usernames and passwords and manages the rights needed for authorized users to access this information. It was first shipped with Windows Server 2000; it has come under increasing attack in recent years. It is designed to be backward-compatible, and many features are arguably not "secure by default." It is difficult to manage properly, especially in large environments where it can be easily misconfigured.

Active Directory flaws and misconfigurations can often be used to obtain a foothold (internal access), move laterally and vertically within a network, and gain unauthorized access to protected resources such as databases, file shares, source code, and more. AD is essentially a large database accessible to all users within the domain, regardless of their privilege level. A basic AD user account with no added privileges can be used to enumerate the majority of objects contained within AD, including but not limited to:

|Domain Computers | Domain Users|
|Domain Group Information | Organizational Units (OUs)|
|Default Domain Policy | Functional Domain Levels|
|Password Policy | Group Policy Objects (GPOs)|
|Domain Trusts | Access Control Lists (ACLs)|

Active Directory is arranged in a hierarchical tree structure, with a forest at the top containing one or more domains, which can themselves have nested subdomains. A forest is the security boundary within which all objects are under administrative control. A forest may contain multiple domains, and a domain may include further child or sub-domains. A domain is a structure within which contained objects (users, computers, and groups) are accessible. It has many built-in Organizational Units (OUs), such as `Domain Controllers`, `Users`, `Computers`, and new OUs can be created as required. OUs may contain objects and sub-OUs, allowing for the assignment of different group policies.

At a very (simplistic) high level, an AD structure may look as follows:
```shell-session
INLANEFREIGHT.LOCAL/
├── ADMIN.INLANEFREIGHT.LOCAL
│   ├── GPOs
│   └── OU
│       └── EMPLOYEES
│           ├── COMPUTERS
│           │   └── FILE01
│           ├── GROUPS
│           │   └── HQ Staff
│           └── USERS
│               └── barbara.jones
├── CORP.INLANEFREIGHT.LOCAL
└── DEV.INLANEFREIGHT.LOCAL
```

Here we could say that INLANEFREIGHT.LOCAL is the root domain and contains the subdomains (either child or tree root domains) ADMIN.INLANEFREIGHT.LOCAL, CORP.INLANEFREIGHT.LOCAL and DEV.INLANEFREIGHT.LOCAL as well as other objects that make up a domain such as users, groups, computers, and more as we will see in detail below. It is common to see multiple domains (or forests) linked together via trust relationships in organizations that perform a lot of acquisisions. It is often quicker and easier to create a trust relationship with another domain/forest than recreate all new users in the current domain. 

### What Active directory structure can contain one or more domains?
Forest

### True or Flase; it can be common to see multiple domains linked together by trust relationships.
True

###  Active Directory provides authentication and <____> within a Windows domain environment.
Authorization


# Active Directory Terminology

Object - an object can be defined as any resource present within an Active Directory environment such as OUs, printers, users, domain controllers, etc.

Attributes - Every object in Active Directory has an associated set of [attributes](https://docs.microsoft.com/en-us/windows/win32/adschema/attributes-all) used to define characteristics of the given object. A computer object contains attributes such as the hostname and DNS name. All attributes in AD have an associated LDAP name that can be used when performing LDAP queries, such as `displayName` for `Full Name` and `given name` for `First Name`.

Schema - The blueprint of any enterprise environment. Defines what types of objects can exist in the AD database and their associated attributes. It lists definitions corresponding to AD objects and holds information about each object. For example, users in AD belong to the class "user," and computer objects to "computer," and so on. Each object has its own information (some required to be set and others optional) that are stored in Attributes.


#### Domain

A domain is a logical group of objects such as computers, users, OUs, groups, etc. We can think of each domain as a different city within a state or country. Domains can operate entirely independently of one another or be connected via trust relationships.

#### Forest

A forest is a collection of Active Directory domains. It is the topmost container and contains all of the AD objects introduced below, including but not limited to domains, users, groups, computers, and Group Policy objects. A forest can contain one or multiple domains and be thought of as a state in the US or a country within the EU. Each forest operates independently but may have various trust relationships with other forests.

#### Tree
A tree is a collection of Active Directory domains that begins at a single root domain. A forest is a collection of AD trees. Each domain in a tree shares a boundary with the other domains. A parent-child trust relationship is formed when a domain is added under another domain in a tree. Two trees in the same forest cannot share a name (namespace). Let's say we have two trees in an AD forest: `inlanefreight.local` and `ilfreight.local`. A child domain of the first would be `corp.inlanefreight.local` while a child domain of the second could be `corp.ilfreight.local`. All domains in a tree share a standard Global Catalog which contains all information about objects that belong to the tree.

#### Container
Container Objects hold other objects and have a defined place in the directory subtree hierarchy

#### Leaf
Leaf objects hold other objects and have a defined place in the directory subtree hierarchy




**GUID (Globally Unique Identifier):**

- **Definition:** Unique 128-bit value assigned to every AD object (users, groups, computers, etc.).
- **Storage:** Stored in the ObjectGUID attribute.
- **Usage:**
    - Querying AD objects using PowerShell or by specifying distinguished name, GUID, SID, or SAM account name.
    - Identifying objects internally in AD.
    - Most accurate way to find exact objects, especially in global catalogs.
    - Never changes; associated with the object as long as it exists in the domain.

**Security Principals:**

- **Definition:** Anything OS can authenticate (users, computer accounts, threads/processes like applications running in context of service account).
- **Types:**
    - **Domain Objects:** Manage access to resources within the domain.
    - **Local User Accounts & Security Groups:** Control access to specific computer resources; managed by Security Accounts Manager (SAM), not AD.

**Security Identifier (SID):**

- **Definition:** Unique identifier for security principal or group.
- **Characteristics:**
    - Issued by domain controller; stored in secure database.
    - Unique; never reused even if the principal is deleted.
    - Used in access tokens to check rights (includes user's SID, rights, group SIDs).
- **Well-Known SIDs:** Identify generic users/groups; same across all OS (e.g., Everyone group).

**Distinguished Name (DN):**

- **Definition:** Full path to an object in AD (e.g., `cn=bjones, ou=IT, ou=Employees, dc=inlanefreight, dc=local`).
- **Components:**
    - **CN (Common Name):** One way to search for/access user object.
    - **OU (Organizational Unit):** Holds accounts for specific categories (e.g., IT department, company employees).

**sAMAccountName:**

- **Definition:** User's logon name (e.g., `bjones`).
- **Characteristics:** Must be unique; 20 or fewer characters.

**userPrincipalName:**

- **Definition:** Another way to identify users in AD.
- **Format:** `bjones@inlanefreight.local`; consists of a prefix (user account name) and a suffix (domain name).
- **Note:** Not mandatory.

**FSMO (Flexible Single Master Operation) Roles:**

- **Background:** Solution to conflicts and single point of failure in multi-DC environments.
- **Roles:**
    - **Forest Level:** Schema Master, Domain Naming Master.
    - **Domain Level:** Relative ID (RID) Master, Primary Domain Controller (PDC) Emulator, Infrastructure Master.
    - **Assignment:** All five to the first DC in forest root domain; RID Master, PDC Emulator, Infrastructure Master to new domains.
- **Function:** Enable DCs to authenticate users, grant permissions, ensure smooth replication, and critical service operation.
- **Management:** Typically set during DC creation; transferable by sysadmins.

**Global Catalog (GC):**

- **Definition:** Domain controller storing copies of all objects in an AD forest.
- **Content:**
    - Full copy of all objects in the current domain.
    - Partial copy of objects in other domains in the forest.
- **Difference from Standard DC:** Standard DC holds complete replica of objects in its domain only.
- **Functions:**
    - **Authentication:** Authorization for all groups a user account belongs to; included in access token generation.
    - **Object Search:** Makes directory structure transparent across all domains in a forest; allows search with just one attribute about an object.


#### Read-only Domain Controller
A [Read-Only Domain Controller (RODC)](https://docs.microsoft.com/en-us/windows/win32/ad/rodc-and-active-directory-schema) has a read-only Active Directory database. No AD account passwords are cached on an RODC (other than the RODC computer account & RODC KRBTGT passwords.) No changes are pushed out via an RODC's AD database, SYSVOL, or DNS. RODCs also include a read-only DNS server, allow for administrator role separation, reduce replication traffic in the environment, and prevent SYSVOL modifications from being replicated to other DCs.

#### Replication
Happens in AD when AD objects are updated and transderred from one Domain Controller to another. Whenever a DC is added, connection objects are created to manage replication between them. These connections are made by the knowledge Consistency Checker(KCC) service, whic is presnet on all DCs. Replication ensures that changes are synchronized with all other DCs in a forest, helping to create a backup in case on domain controller fails

#### Service Principal Name (SPN)

A [Service Principal Name (SPN)](https://docs.microsoft.com/en-us/windows/win32/ad/service-principal-names) uniquely identifies a service instance. They are used by Kerberos authentication to associate an instance of a service with a logon account, allowing a client application to request the service to authenticate an account without needing to know the account name.


#### Group Policy Object (GPO)

[Group Policy Objects (GPOs)](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/policy/group-policy-objects) are virtual collections of policy settings. Each GPO has a unique GUID. A GPO can contain local file system settings or Active Directory settings. GPO settings can be applied to both user and computer objects. They can be applied to all users and computers within the domain or defined more granularly at the OU level.

#### Access Control List (ACL)

An [Access Control List (ACL)](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-lists) is the ordered collection of Access Control Entities (ACEs) that apply to an object.

#### Access Control Entities (ACEs)

Each [Access Control Entity (ACE)](https://docs.microsoft.com/en-us/windows/win32/secauthz/access-control-entries) in an ACL identifies a trustee (user account, group account, or logon session) and lists the access rights that are allowed, denied, or audited for the given trustee.


#### Discretionary Access Control List (DACL)

DACLs define which security principles are granted or denied access to an object; it contains a list of ACEs. When a process tries to access a securable object, the system checks the ACEs in the object's DACL to determine whether or not to grant access. If an object does NOT have a DACL, then the system will grant full access to everyone, but if the DACL has no ACE entries, the system will deny all access attempts. ACEs in the DACL are checked in sequence until a match is found that allows the requested rights or until access is denied.

#### System Access Control Lists (SACL)

Allows for administrators to log access attempts that are made to secured objects. ACEs specify the types of access attempts that cause the system to generate a record in the security event log.

#### Fully Qualified Domain Name (FQDN)

An FQDN is the complete name for a specific computer or host. It is written with the hostname and domain name in the format [host name].[domain name].[tld]. This is used to specify an object's location in the tree hierarchy of DNS. The FQDN can be used to locate hosts in an Active Directory without knowing the IP address, much like when browsing to a website such as google.com instead of typing in the associated IP address. An example would be the host `DC01` in the domain `INLANEFREIGHT.LOCAL`. The FQDN here would be `DC01.INLANEFREIGHT.LOCAL`.


#### Tombstone

A [tombstone](https://ldapwiki.com/wiki/Tombstone) is a container object in AD that holds deleted AD objects. When an object is deleted from AD, the object remains for a set period of time known as the `Tombstone Lifetime,` and the `isDeleted` attribute is set to `TRUE`. Once an object exceeds the `Tombstone Lifetime`, it will be entirely removed. Microsoft recommends a tombstone lifetime of 180 days to increase the usefulness of backups, but this value may differ across environments. Depending on the DC operating system version, this value will default to 60 or 180 days. If an object is deleted in a domain that does not have an AD Recycle Bin, it will become a tombstone object. When this happens, the object is stripped of most of its attributes and placed in the `Deleted Objects` container for the duration of the `tombstoneLifetime`. It can be recovered, but any attributes that were lost can no longer be recovered.

#### AD Recycle Bin

The [AD Recycle Bin](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/the-ad-recycle-bin-understanding-implementing-best-practices-and/ba-p/396944) was first introduced in Windows Server 2008 R2 to facilitate the recovery of deleted AD objects. This made it easier for sysadmins to restore objects, avoiding the need to restore from backups, restarting Active Directory Domain Services (AD DS), or rebooting a Domain Controller. When the AD Recycle Bin is enabled, any deleted objects are preserved for a period of time, facilitating restoration if needed. Sysadmins can set how long an object remains in a deleted, recoverable state. If this is not specified, the object will be restorable for a default value of 60 days. The biggest advantage of using the AD Recycle Bin is that most of a deleted object's attributes are preserved, which makes it far easier to fully restore a deleted object to its previous state.

#### SYSVOL

The [SYSVOL](https://social.technet.microsoft.com/wiki/contents/articles/8548.active-directory-sysvol-and-netlogon.aspx) folder, or share, stores copies of public files in the domain such as system policies, Group Policy settings, logon/logoff scripts, and often contains other types of scripts that are executed to perform various tasks in the AD environment. The contents of the SYSVOL folder are replicated to all DCs within the environment using File Replication Services (FRS). You can read more about the SYSVOL structure [here](http://www.techiebird.com/Sysvol_structure.html).

#### AdminSDHolder

The [AdminSDHolder](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory) object is used to manage ACLs for members of built-in groups in AD marked as privileged. It acts as a container that holds the Security Descriptor applied to members of protected groups. The SDProp (SD Propagator) process runs on a schedule on the PDC Emulator Domain Controller. When this process runs, it checks members of protected groups to ensure that the correct ACL is applied to them. It runs every hour by default. For example, suppose an attacker is able to create a malicious ACL entry to grant a user certain rights over a member of the Domain Admins group. In that case, unless they modify other settings in AD, these rights will be removed (and they will lose any persistence they were hoping to achieve) when the SDProp process runs on the set interval.

#### dsHeuristics

The [dsHeuristics](https://docs.microsoft.com/en-us/windows/win32/adschema/a-dsheuristics) attribute is a string value set on the Directory Service object used to define multiple forest-wide configuration settings. One of these settings is to exclude built-in groups from the [Protected Groups](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory) list. Groups in this list are protected from modification via the `AdminSDHolder` object. If a group is excluded via the `dsHeuristics` attribute, then any changes that affect it will not be reverted when the SDProp process runs.

#### adminCount

The [adminCount](https://docs.microsoft.com/en-us/windows/win32/adschema/a-admincount) attribute determines whether or not the SDProp process protects a user. If the value is set to `0` or not specified, the user is not protected. If the attribute value is set to `value`, the user is protected. Attackers will often look for accounts with the `adminCount` attribute set to `1` to target in an internal environment. These are often privileged accounts and may lead to further access or full domain compromise.

#### Active Directory Users and Computers (ADUC)

ADUC is a GUI console commonly used for managing users, groups, computers, and contacts in AD. Changes made in ADUC can be done via PowerShell as well.

#### ADSI Edit

ADSI Edit is a GUI tool used to manage objects in AD. It provides access to far more than is available in ADUC and can be used to set or delete any attribute available on an object, add, remove, and move objects as well. It is a powerful tool that allows a user to access AD at a much deeper level. Great care should be taken when using this tool, as changes here could cause major problems in AD.

#### sIDHistory

[This](https://docs.microsoft.com/en-us/defender-for-identity/cas-isp-unsecure-sid-history-attribute) attribute holds any SIDs that an object was assigned previously. It is usually used in migrations so a user can maintain the same level of access when migrated from one domain to another. This attribute can potentially be abused if set insecurely, allowing an attacker to gain prior elevated access that an account had before a migration if SID Filtering (or removing SIDs from another domain from a user's access token that could be used for elevated access) is not enabled.

#### NTDS.DIT

The NTDS.DIT file can be considered the heart of Active Directory. It is stored on a Domain Controller at `C:\Windows\NTDS\` and is a database that stores AD data such as information about user and group objects, group membership, and, most important to attackers and penetration testers, the password hashes for all users in the domain. Once full domain compromise is reached, an attacker can retrieve this file, extract the hashes, and either use them to perform a pass-the-hash attack or crack them offline using a tool such as Hashcat to access additional resources in the domain. If the setting [Store password with reversible encryption](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/store-passwords-using-reversible-encryption) is enabled, then the NTDS.DIT will also store the cleartext passwords for all users created or who changed their password after this policy was set. While rare, some organizations may enable this setting if they use applications or protocols that need to use a user's existing password (and not Kerberos) for authentication.

#### MSBROWSE

MSBROWSE is a Microsoft networking protocol that was used in early versions of Windows-based local area networks (LANs) to provide browsing services. It was used to maintain a list of resources, such as shared printers and files, that were available on the network, and to allow users to easily browse and access these resources.

In older version of Windows we could use `nbtstat -A ip-address` to search for the Master Browser. If we see MSBROWSE it means that's the Master Browser. Aditionally we could use `nltest` utility to query a Windows Master Browser for the names of the Domain Controllers.

Today, MSBROWSE is largely obsolete and is no longer in widespread use. Modern Windows-based LANs use the Server Message Block (SMB) protocol for file and printer sharing, and the Common Internet File System (CIFS) protocol for browsing services.


What is known as the "Blueprint" of an Active Directory environment?
schema

What uniquely identifies a service instance? (Full name, space-seperated, not abbreviated)
service principal name


True or False; Group Policy Objects can be applied to user and computer objects.
true

What container in AD holds deleted objects?
tombstone

What file contains the hashes of passwords for all users in a domain? 

[Part 2: Intro to active directory](obsidian://open?vault=Obsidian%20Vault&file=Intro%20to%20Active%20Directory%20PT.2)**















