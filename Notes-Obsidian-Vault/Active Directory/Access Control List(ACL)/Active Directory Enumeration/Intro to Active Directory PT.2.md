## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [AD Objects](#ad\objects)
      - [Contacts](#Contacts)
      - [Printers](#Printers)
- [Active Directory Functionality](#active\directory\functionality)
      - [Trusts](#Trusts)
- [Kerberos, DNS, LDAP, MSRPC](#kerberos,\dns,\ldap,\msrpc)
      - [Kerberos authentication Process~](#Kerberos\authentication\Process~)
      - [DNS](#DNS)
      - [AD DS](#AD\DS)
      - [AD LDAP Authentication](#AD\LDAP\Authentication)
  - [MSRPC](#MSRPC)

## Table of Contents

- [AD Objects](#ad\objects)
      - [Contacts](#Contacts)
      - [Printers](#Printers)
- [Active Directory Functionality](#active\directory\functionality)
      - [Trusts](#Trusts)
- [Kerberos, DNS, LDAP, MSRPC](#kerberos,\dns,\ldap,\msrpc)
      - [Kerberos authentication Process~](#Kerberos\authentication\Process~)
      - [DNS](#DNS)
      - [AD DS](#AD\DS)
      - [AD LDAP Authentication](#AD\LDAP\Authentication)
  - [MSRPC](#MSRPC)

# AD Objects
We will often see the term "objects" when referring to AD. What is an object? An object can be defined as ANY resource present within an Active Directory environment such as OUs, printers, users, domain controllers.

**Users in AD Environment:**

- **Type:** Leaf objects (cannot contain other objects); similar to a mailbox in Microsoft Exchange.
- **Security Principal:** Has a security identifier (SID) and a global unique identifier (GUID).
- **Attributes:**
    - Common: Display name, last login time, date of last password change, email address, account description, manager, address, etc.
    - Over 800 possible attributes, depending on AD setup.
    - Example link detailing all possible attributes (not provided in the text).
- **Importance in Security:**
    - Crucial target for attackers.
    - Even low privileged user access can grant access to many objects/resources.
    - Allows detailed enumeration of the entire domain or forest.

#### Contacts

A contact object is usually used to represent an external user and contains informational attributes such as first name, last name, email address, telephone number, etc. They are `leaf objects` and are NOT security principals (securable objects), so they don't have a SID, only a GUID. An example would be a contact card for a third-party vendor or a customer.

#### Printers
A printer object points to a printer accessible within the AD network. Like a contact, a printer is a lead object and not a security principal, so it only has a GUID. Printers have attributes such as the printers name, driver info, port number etc.

**Computers:**

- **Definition:** Any computer (workstation or server) joined to AD.
- **Type:** Leaf objects; security principals with SID and GUID.
- **Security Importance:** Full administrative access grants similar rights to a standard domain user; prime targets for attackers.

**Shared Folders:**

- **Definition:** Points to shared folders on specific computers.
- **Access Control:** Can be open to everyone, authenticated users, or specific users/groups.
- **Attributes:** Name, location, security access rights; NOT security principals, only have GUID.

**Groups:**

- **Definition:** Container objects; can contain users, computers, other groups.
- **Type:** Security principals with SID and GUID.
- **Usage:** Manage user permissions; nested groups can lead to unintended rights.
- **Tools:** BloodHound for auditing group membership.
- **Attributes:** Name, description, membership, etc.

**Organizational Units (OUs):**

- **Definition:** Containers for similar objects; used for administrative delegation.
- **Usage:** Managing Group Policy, administrative tasks like password resets, user/group modifications.
- **Attributes:** Name, members, security settings, etc.

**Domain:**

- **Definition:** Structure of AD network; contains users, computers, groups, OUs.
- **Policies:** Default (e.g., password policy) and custom (e.g., blocking cmd.exe for non-admin users).

**Domain Controllers:**

- **Definition:** Brains of AD; handle authentication, access control, security policies.
- **Functions:** Validate access requests, enforce security policies, store information about domain objects.

**Sites:**

- **Definition:** Set of computers across subnets; connected via high-speed links.
- **Usage:** Efficient replication across domain controllers.

**Built-in:**

- **Definition:** Container holding default groups in AD; predefined upon domain creation.

**Foreign Security Principals (FSPs):**

- **Definition:** Represent security principals from trusted external forests.
- **Creation:** Automatically created when an external object is added to a group in the current domain.
- **Usage:** Placeholder for SID of foreign object; used to resolve object's name via trust relationship.
- **Location:** Specific container named ForeignSecurityPrincipals (e.g., `cn=ForeignSecurityPrincipals,dc=inlanefreight,dc=local`).


# Active Directory Functionality
As mentioned before, there are five Flexible Single Master Operation (FSMO) roles. These roles can be defined as follows:

|**Roles**|**Description**|
|---|---|
|`Schema Master`|This role manages the read/write copy of the AD schema, which defines all attributes that can apply to an object in AD.|
|`Domain Naming Master`|Manages domain names and ensures that two domains of the same name are not created in the same forest.|
|`Relative ID (RID) Master`|The RID Master assigns blocks of RIDs to other DCs within the domain that can be used for new objects. The RID Master helps ensure that multiple objects are not assigned the same SID. Domain object SIDs are the domain SID combined with the RID number assigned to the object to make the unique SID.|
|`PDC Emulator`|The host with this role would be the authoritative DC in the domain and respond to authentication requests, password changes, and manage Group Policy Objects (GPOs). The PDC Emulator also maintains time within the domain.|
|`Infrastructure Master`|This role translates GUIDs, SIDs, and DNs between domains. This role is used in organizations with multiple domains in a single forest. The Infrastructure Master helps them to communicate. If this role is not functioning properly, Access Control Lists (ACLs) will show SIDs instead of fully resolved names.|


Microsoft introduced functional levels to determine the various features and capabilities available in Active Directory Domain Services (AD DS) at the domain and forest level. They are also used to specify which Windows Server operating systems can run a Domain Controller in a domain or forest. [This](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754918(v=ws.10)?redirectedfrom=MSDN) and [this](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-functional-levels) article describe both the domain and forest functional levels from Windows 2000 native to Windows Server 2012 R2. Below is a quick overview of the differences in `domain functional levels` from Windows 2000 native up to Windows Server 2016, aside from all default Active Directory Directory Services features from the level just below it (or just the default AD DS features in the case of Windows 2000 native.)

|Domain Functional Level|Features Available|Supported Domain Controller Operating Systems|
|---|---|---|
|Windows 2000 native|Universal groups for distribution and security groups, group nesting, group conversion (between security and distribution and security groups), SID history.|Windows Server 2008 R2, Windows Server 2008, Windows Server 2003, Windows 2000|
|Windows Server 2003|Netdom.exe domain management tool, lastLogonTimestamp attribute introduced, well-known users and computers containers, constrained delegation, selective authentication.|Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003|
|Windows Server 2008|Distributed File System (DFS) replication support, Advanced Encryption Standard (AES 128 and AES 256) support for the Kerberos protocol, Fine-grained password policies|Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008|
|Windows Server 2008 R2|Authentication mechanism assurance, Managed Service Accounts|Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2|
|Windows Server 2012|KDC support for claims, compound authentication, and Kerberos armoring|Windows Server 2012 R2, Windows Server 2012|
|Windows Server 2012 R2|Extra protections for members of the Protected Users group, Authentication Policies, Authentication Policy Silos|Windows Server 2012 R2|
|Windows Server 2016|[Smart card required for interactive logon](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/interactive-logon-require-smart-card) new [Kerberos](https://docs.microsoft.com/en-us/windows-server/security/kerberos/whats-new-in-kerberos-authentication) features and new [credential protection](https://docs.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection) features|Windows Server 2019 and Windows Server 2016|

A new functional level was not added with the release of Windows Server 2019. However, Windows Server 2008 functional level is the minimum requirement for adding Server 2019 Domain Controllers to an environment. Also, the target domain has to use [DFS-R](https://docs.microsoft.com/en-us/windows-server/storage/dfs-replication/dfsr-overview) for SYSVOL replication.

Forest functional levels have introduced a few key capabilities over the years:

|**Version**|**Capabilities**|
|---|---|
|`Windows Server 2003`|saw the introduction of the forest trust, domain renaming, read-only domain controllers (RODC), and more.|
|`Windows Server 2008`|All new domains added to the forest default to the Server 2008 domain functional level. No additional new features.|
|`Windows Server 2008 R2`|Active Directory Recycle Bin provides the ability to restore deleted objects when AD DS is running.|
|`Windows Server 2012`|All new domains added to the forest default to the Server 2012 domain functional level. No additional new features.|
|`Windows Server 2012 R2`|All new domains added to the forest default to the Server 2012 R2 domain functional level. No additional new features.|
|`Windows Server 2016`|[Privileged access management (PAM) using Microsoft Identity Manager (MIM).](https://docs.microsoft.com/en-us/windows-server/identity/whats-new-active-directory-domain-services#privileged-access-management)|


#### Trusts


A trust is used to establish forest-torest or domain-domain authentication, allowing users to access resources in (or administer) another doain outside of the domain their account resides in. A trust creates a link between the authentication systems of two domains.

|**Trust Type**|**Description**|
|---|---|
|`Parent-child`|Domains within the same forest. The child domain has a two-way transitive trust with the parent domain.|
|`Cross-link`|a trust between child domains to speed up authentication.|
|`External`|A non-transitive trust between two separate domains in separate forests which are not already joined by a forest trust. This type of trust utilizes SID filtering.|
|`Tree-root`|a two-way transitive trust between a forest root domain and a new tree root domain. They are created by design when you set up a new tree root domain within a forest.|
|`Forest`|a transitive trust between two forest root domains.|

Trusts can be transitive or non-transitive.

- A transitive trust means that trust is extended to objects that the child domain trusts.
    
- In a non-transitive trust, only the child domain itself is trusted.
    

Trusts can be set up to be one-way or two-way (bidirectional).

- In bidirectional trusts, users from both trusting domains can access resources.
- In a one-way trust, only users in a trusted domain can access resources in a trusting domain, not vice-versa. The direction of trust is opposite to the direction of access.

What role maintains time for a domain?
PDC Emulator

What domain functional level introduced managed service accounts?
Windows Server 2008 R2

What type of trust is a link between two child domains in a forest?
Cross-link


What role ensures that objects in a domain are not assigned the same SID? (full name)
Relative ID Master


# Kerberos, DNS, LDAP, MSRPC

While Windows operating systems use a variety of protocols to communicate, Active Directory specifically requires [Lightweight Directory Access Protocol (LDAP)](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol), Microsoft's version of [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)), [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) for authentication and communication, and [MSRPC](https://ldapwiki.com/wiki/MSRPC) which is the Microsoft implementation of [Remote Procedure Call (RPC)](https://en.wikipedia.org/wiki/Remote_procedure_call), an interprocess communication technique used for client-server model-based applications.

#### Kerberos authentication Process~
1. The user logs on, and their password is converted to an NTLM hash, which is used to encrypt the TGT ticket. This decouples the users credentials from requests to resources.
2. The KDC service on the DC checks the authentication service request (AS-REQ), verifies the user information, and creates a Ticket Granting Ticket (TGT), which is delivered to the users
3. The user presents the TGT to the DC, requesting a Ticket Granting Service (TGS) ticket for a specific service. This is the TGS-REQ. If the TGT is successfully validated, its data is copied to create a TGS ticket.|
4. The TGS is encrypted with the NTLM password hash of the service or computer account in whose context the service instance is running and is delivered to the user in the TGS_REP.|
5. The user presents the TGS to the service, and if it is valid, the user is permitted to connect to the resource (AP_REQ).|

The Kerberos protocol uses port 88 (both TCP and UDP). When enumerating an Active Directory environment, we can often locate Domain Controllers by performing port scans looking for open port 88 using a tool such as Nmap.

#### DNS
Active Directory Domain Services (AD DS) uses DNS to allow clients (workstations, servers, and other systems that communicate with the domain) to locate Domain Controllers and for Domain Controllers that host the directory service to communicate amongst themselves. DNS is used to resolve hostnames to IP addresses and is broadly used across internal networks and the internet. Private internal networks use Active Directory DNS namespaces to facilitate communications between servers, clients, and peers. AD maintains a database of services running on the network in the form of service records (SRV). These service records allow clients in an AD environment to locate services that they need, such as a file server, printer, or Domain Controller. Dynamic DNS is used to make changes in the DNS database automatically should a system's IP address change. Making these entries manually would be very time-consuming and leave room for error. If the DNS database does not have the correct IP address for a host, clients will not be able to locate and communicate with it on the network. When a client joins the network, it locates the Domain Controller by sending a query to the DNS service, retrieving an SRV record from the DNS database, and transmitting the Domain Controller's hostname to the client. The client then uses this hostname to obtain the IP address of the Domain Controller. DNS uses TCP and UDP port 53. UDP port 53 is the default, but it falls back to TCP when no longer able to communicate and DNS messages are larger than 512 bytes.

#### AD DS
  
Active Directory Domain Services (AD DS) relies on DNS to enable clients (such as workstations, servers) to find Domain Controllers and allow these controllers to communicate with each other. DNS resolves hostnames to IP addresses and is widely used in both internal networks and the internet. Within private internal networks, Active Directory DNS namespaces aid in communication between various devices. AD maintains a database of service records (SRV), helping clients locate needed services like file servers or printers. Dynamic DNS automatically updates the DNS database if a system's IP address changes, preventing time-consuming manual entries and potential errors. If the DNS database has an incorrect IP address, clients cannot find or communicate with the host. When a client joins the network, it locates the Domain Controller by querying the DNS service, retrieving an SRV record, and obtaining the Domain Controller's hostname and IP address. DNS uses TCP and UDP port 53, with UDP port 53 as the default. It switches to TCP if communication is hindered or if DNS messages exceed 512 bytes.



Forward DNS lookup
```
nslookup INLANEFREIGHT.LOCAL
```

Reverse DNS lookup-obtain the DNS name of a single host using the IP address:
```
nslookup 172.16.16.5
```

Find ip Address of a host If we would like to find the IP address of a single host, we can do this in reverse. We can do this with or without specifying the FQDN.:
```
nslookup ACADEMY-EA-DC01
```


Active Directory supports [Lightweight Directory Access Protocol (LDAP)](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) for directory lookups. LDAP is an open-source and cross-platform protocol used for authentication against various directory services (such as AD). The latest LDAP specification is [Version 3](https://tools.ietf.org/html/rfc4511), published as RFC 4511. A firm understanding of how LDAP works in an AD environment is crucial for attackers and defenders. LDAP uses port 389, and LDAP over SSL (LDAPS) communicates over port 636.

AD stores user account info and security information such as passwords and facilitates sharing this information with other devices on the network. LDAP is the language that applications use to communicate with other servers that provide directory services. In other words, LDAP is how systems in the network environment can "Speak" to AD.
An LDAP session begins by first connecting to an LDAP server, also known as a Directory System Agent. The Domain Controller in AD actively listens for LDAP requests, such as security authentication requests.


#### AD LDAP Authentication
LDAP is set up to authenticate credentials against AD using a "BIND" operation to set the authentication state for an LDAP session. Two types of LDAP authentication:
1. simple Authentication: This includes anonymous authentication, unauthenticated authentication, and username/password authentication. Simple authentication means that a `username` and `password` create a BIND request to authenticate to the LDAP server.
2. SASL Authentication: The Simple Authentication and Security Layer(SASL) framework uses other authentication services, such as kerberos, to bind to the LDAP server and then uses this authentication service (kerboeros in this example) to authenticate to LDAP. The LDAP server uses the LDAP protocol to send an LDAP message to the authorization service, which initiates a series of challenge/response messages resulting in either successful or unsuccessful authentication. SASL can provide additional security due to the separation of authentication methods from application protocols

## MSRPC

As mentioned above, MSRPC is Microsoft's implementation of Remote Procedure Call (RPC), an interprocess communication technique used for client-server model-based applications. Windows systems use MSRPC to access systems in Active Directory using four key RPC interfaces.

|Interface Name|Description|
|---|---|
|`lsarpc`|A set of RPC calls to the [Local Security Authority (LSA)](https://networkencyclopedia.com/local-security-authority-lsa/) system which manages the local security policy on a computer, controls the audit policy, and provides interactive authentication services. LSARPC is used to perform management on domain security policies.|
|`netlogon`|Netlogon is a Windows process used to authenticate users and other services in the domain environment. It is a service that continuously runs in the background.|
|`samr`|Remote SAM (samr) provides management functionality for the domain account database, storing information about users and groups. IT administrators use the protocol to manage users, groups, and computers by enabling admins to create, read, update, and delete information about security principles. Attackers (and pentesters) can use the samr protocol to perform reconnaissance about the internal domain using tools such as [BloodHound](https://github.com/BloodHoundAD/) to visually map out the AD network and create "attack paths" to illustrate visually how administrative access or full domain compromise could be achieved. Organizations can [protect](https://stealthbits.com/blog/making-internal-reconnaissance-harder-using-netcease-and-samri1o/) against this type of reconnaissance by changing a Windows registry key to only allow administrators to perform remote SAM queries since, by default, all authenticated domain users can make these queries to gather a considerable amount of information about the AD domain.|
|`drsuapi`|drsuapi is the Microsoft API that implements the Directory Replication Service (DRS) Remote Protocol which is used to perform replication-related tasks across Domain Controllers in a multi-DC environment. Attackers can utilize drsuapi to [create a copy of the Active Directory domain database](https://attack.mitre.org/techniques/T1003/003/) (NTDS.dit) file to retrieve password hashes for all accounts in the domain, which can then be used to perform Pass-the-Hash attacks to access more systems or cracked offline using a tool such as Hashcat to obtain the cleartext password to log in to systems using remote management protocols such as Remote Desktop (RDP) and WinRM.|



What networking port does Kerberos Use?
88

What protocol is utilized to translate names into IP addresses?
DNS

What protocol does RFC 4511 specify? (acronym)
LDAP

[Intro to Active Directory PT.3](obsidian://open?vault=Obsidian%20Vault&file=Active%20Directory%2FIntro%20to%20Active%20Directory%20PT.3)


