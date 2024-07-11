## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [Domain](#Domain)
      - [Domain Controller](#Domain\Controller)
          - [Trees](#Trees)
          - [Forests](#Forests)
    - [Trust in Active Directory](#Trust\in\Active\Directory)

## Table of Contents

- [Domain](#Domain)
      - [Domain Controller](#Domain\Controller)
          - [Trees](#Trees)
          - [Forests](#Forests)
    - [Trust in Active Directory](#Trust\in\Active\Directory)



### Domain
The domain acts as a core unit regarding the logical structure of the active directory. It stores all the critical infotmation about the objects that belong to the domain only.

#### Domain Controller
An active directory server that acts as the brain for a windows server domain. Supervises the entire network within the domain and acts as a gatekeeper for user authentication and IT resources authorisation.

	Tree -> Set of domains
	Forests -> A Forest of Trees

###### Trees
Responsible for sharing resources between the domains. The communication betweeen the domains inside a tree is possible by either one-way or two way trust/ when a domain is adderd to the tree, it becomes the offspring domain of that particualr omain to which it is added- now  a parent domain.

###### Forests
When the sharing of the standard global catalogue, directory shcema, lgocial structure, and directory configuration between collections of trees is made successfully, it is called a Forest. Communication between two forests becomes possible once a forest=;eve; trust is created.

### Trust in Active Directory
	Trust in Active Directory   

AD trust is the established communication bridge between the domains in Active Directory. When we say one domain trusts another in the AD network, it means its resources can be shared with another domain. However, one domain's resources are not directly available to every other domain, as it is not safe. Thus, the resource sharing availability is governed by Trusts in AD. The AD trusts are of two categories, which are classified based on their characteristics or the current direction.

![[Pasted image 20231108005414.png]]

**AD Trusts categorised based on characteristics are known as Transitive and Non-Transitive relationships.**
