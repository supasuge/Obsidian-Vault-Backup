## Table of Contents

  - [Tagging Unknown Traffic](#Tagging\Unknown\Traffic)
  - [Routing Between VLANs](#Routing\Between\VLANs)
    - [Historical Context:](#Historical\Context:)
    - [Modern Solution:](#Modern\Solution:)
  - [Isolation in VLANs](#Isolation\in\VLANs)
    - [Traffic Filtering](#Traffic\Filtering)
    - [Zone-Pairs](#Zone-Pairs)
    - [SSL/TLS Inspection](#SSL/TLS\Inspection)

The 802.1 tag provides a standard between vendors that will always define the VLAN of a frame. For example, if our switches are Cisco and our routers are Juniper, they can send tagged frames and digest them equally because it is standardized. As a demonstration, we will show how to configure tags on the interfaces of a switch.

In this example, we will use the open-source switch: **[Open vSwitch](https://www.openvswitch.org/)**

Let's first look at the default configuration of the switch.

```shell
$ ovs-vsctl show
```

To add a VLAN and tags to a sub-interface, we need to modify the configuration through `ovs-vsctl`

```shell
$ ovs-vsctl set port <interface> tag=<0-99>
```
Tag should now be listed under the sub-interface in the configuration:
```shell

Port eth1
		tag: 10
		Interface eth1

```


## Tagging Unknown Traffic

**Native VLAN** is utilized for untagged traffic that moves through a switch. It's crucial to assign an interface and tag to the Native VLAN for configuration purposes.

**Example using Open vSwitch:**

bashCopy code

`$ ovs-vsctl set port eth0 tag=10 vlan_mode=native-untagged`

The above command ensures that all traffic, irrespective of its origin, gets tagged.

---

## Routing Between VLANs

VLANs, due to their segmented nature, cannot communicate with devices outside their tagged boundary. Traditionally, routers enabled communication between different networks. Similarly, they facilitate routing between VLANs.

### Historical Context:

In the past, each VLAN required a distinct physical connection between a switch and a router.

### Modern Solution:

The **ROAS (Router on a Stick)** design simplifies the process. A designated interface on a switch, termed as the **switch port**, is configured for VLANs to communicate with a router. The link between the router and the switch is termed as a **trunk**. With this configuration, just one trunk/connection is needed between the router and the switch.

**Lab Demonstration:** In a typical lab setup, trunks are inherently configured as bridges. Configuration methods might differ across vendors. Some might even employ proprietary protocols.

**Example Configuration:**

bashCopy code

`$ ovs-vsctl add-br br0 $ ovs-vsctl add-port br0 eth0 tag=10`

The next step involves configuring the router to route the tagged VLAN traffic. Given the standardized nature of the 802.1q tag, it's essential to set the switch port and specify tags for each interface on the router.

**Virtual Sub-Interfaces:** These are vital since all tagged traffic originates from a singular connection. They act akin to physical interfaces and typically follow the format:

bashCopy code

`<name>.<vlan/sub-interface id>`

**Example using VyOS Router:**

bashCopy code

`vyos@vyos-rtr# set interfaces ethernet eth0 vif 10 description 'VLAN 10' vyos@vyos-rtr# set interfaces ethernet eth0 vif 10 address '192.168.100.1/24'`

If executed and configured correctly, traffic can be routed between VLANs while maintaining tag segregation.

---

## Isolation in VLANs

Although VLANs are physically isolated, the presence of routes between them eliminates a strict security boundary. Any device can communicate across VLANs if a route exists between them. This leads to the significance of designing secure VLANs and the introduction of **zoning**.

|   |   |   |
|---|---|---|
|**Zone  <br>**|**Explanation  <br>**|**Examples**|
|External|All devices and entities outside of our network or asset control.|Devices connecting to a web server|
|DMZ (demilitarized zone)|Separates untrusted networks or devices from internal resources.|BYOD, remote users/guests, public servers|
|Trusted|Internal networks or devices. A device may be placed in the trusted zone if there is no confidential or sensitive information.|Workstations, B2B|
|Restricted|Any high-risk servers or databases.|Domain controllers, client information|
|Management|Any devices or services dedicated to network or other device management. This zone is less commonly seen and can be grouped with the audit zone.|Virtualization management, backup servers|
|Audit|Any devices or services dedicated to security or monitoring. This zone is less commonly seen and can be grouped with management.|SIEM, telemetry|

  
From the above table, what zone would a user connecting to a public web server be in?

**External**

From the above table, what zone would a public web server be in?

**DMZ**

From the above table, what zone would a core domain controller be placed in?
**restricted**


Policies aid in defining how network traffic is controlled. A network traffic policy may determine how and if a router will route traffic before other routing protocols are employed. **IEEE** (**I**nstitute of **E**lectrical and **E**lectronics **E**ngineers) has standardized a handful of access control and traffic policies, such as **QoS** (**Q**uality **o**f **S**ervice) (**802.11e**). There are still many other routing and traffic policies that are not standardized by IEEE but are commonly used by all vendors following the same objectives.

This task will focus on traffic filtering and introduce network policy concepts.

### Traffic Filtering

Before formally defining what traffic filtering is, let’s discuss one of the most common standards of defining traffic filtering: **ACL(s)** (**A**ccess **C**ontrol **L**ist**(s)**).

An ACL is used as a loose standard to create a ruleset for different implementations and access control protocols. In this task, we will use ACLs within a router to decide whether to route or drop a packet based on the defined list.

An ACL contains **ACE(s)** (**A**ccess **C**ontrol **E**ntry) or rules that define a list’s profile based on pre-defined criteria (source address, destination address, etc.)

Once defined, we can use an ACL for several vendor-specific implementations. For example, [Cisco](https://www.cisco.com/c/en/us/td/docs/routers/asr9000/software/asr9k_r4-0/addr_serv/command/reference/ir40asrbook_chapter1.html#:~:text=An%20access%20control%20list%20(ACL,queueing%2C%20and%20dynamic%20access%20control.)) uses ACLs for traffic filtering, priority or custom queuing, and dynamic access control.

Formally, traffic filtering provides network security, validation, and segmentation by filtering network traffic based on pre-defined criteria.

We’ve now defined what traffic filtering is and how the criteria are defined. Let’s look at how we can implement ACLs in traffic filtering or access control policies.

To get hands-on, we will look at the policies that VyOS offers. The VyOS access-list policy is the platform’s most basic implementation of filtering. The policy will use ACLs or prefix lists to define the criteria for filtering.

Let’s break down how VyOS creates an ACL and defines the policy.

Create the new access-list policy, defining the ACL number (1 - 2699)

`set policy access-list <acl_number>`

Set the description of the access list.

`set policy access-list <acl_number> description <text>`

Create a new rule (or ACE) under the ACL and define the action of the rule.

`set policy access-list <acl_number> rule <1-65535> action <permit|deny>`

Define criteria or parameters for the rule to enforce/match.

`set policy access-list <acl_number> rule <1-65535> <destination|source> <any|host|inverse-mask|network>`

At this point, we have an ACL and an ACE that is actively being enforced by the router.

Let’s recap, the ACL is defined as an ACL number and is made of separate rules or ACEs that describe the behavior of the ACL. Each ACE can determine the action, destination/source, and the specific address/range to trigger the ACE.

Below is a diagram showing a valid SSH request permitted by the ACL.

```c
Internet Protocol Version 4, Src: 10.10.212.209, Dst: 10.10.212.209
    Protocol: TCP (6)
    Header checksum: 0xbfdd [validation disabled]
    [Header checksum status: Unverified]
    Source: 10.10.212.209
    Destination: 10.10.212.209
Transmission Control Protocol, Src Port: 35560, Dst Port: 22, Seq: 1578, Ack: 1670, Len: 148
    Source Port: 35560
    Destination Port: 22
```

![]()  

`set policy access-list 1 rule 1 action permit`

`set policy access-list 1 rule 1 source 10.10.212.209`

Below is a diagram showing an invalid SSH request denied by the ACL.

```c
Internet Protocol Version 4, Src: 10.10.212.209, Dst: 10.10.212.209
    Protocol: TCP (6)
    Header checksum: 0xbfdd [validation disabled]
    [Header checksum status: Unverified]
    Source: 10.10.212.209
    Destination: 10.10.212.209
Transmission Control Protocol, Src Port: 35560, Dst Port: 22, Seq: 1578, Ack: 1670, Len: 148
    Source Port: 35560
    Destination Port: 2
```

![]() 

`set policy access-list 1 rule 1 action deny`

`set policy access-list 1 rule 1 source 10.10.212.209`

If correctly implemented, the router should now drop or accept packets based on their source or destination address.

But is this the best solution? Recapping what was covered in task 3, what if we want a public web server in the DMZ and ensure that only HTTP traffic originates from it? What if specific hosts need specific protocols to be open on a server? A router can only provide a limited amount of extensibility for security.

### Zone-Pairs

**Zone-pairs** are a direction-based and stateful policy that will enforce the traffic in single directions per each VLAN, hence, zone-pair. For example, **DMZ → LAN** or **LAN → DMZ.**

Each zone in a given topology must have a different zone-pair for each other in the topology and every possible direction. This approach provides the most visibility from a firewall and drastically improves the filtering capabilities.

In this task, we will use VyOS as an example of configuring a firewall for zone-pairing.

Before defining each pair's zoning policy, we must add a common name and default action for each VLAN.

Recall from task two that VLANs are routed through a trunk and divided through sub-interfaces. The syntax is `<interface>.<VLAN #>` e.g. `eth0.30`

Below is an example of setting a default action for the DMZ and assigning the VLAN to the common zone name.

1. `set zone-policy zone dmz default-action drop`
2. `set zone-policy zone dmz interface eth0.30`

Repeat this process for each VLAN interface or zone defined in a network.


![Network diagram showing a VyOS firewall connected to a DMZ, LAN, and WAN](https://tryhackme-images.s3.amazonaws.com/user-uploads/5e73cca6ec4fcf1309f2df86/room-content/e7af1be010220560bbb182fcbd57ebe9.png)


Below is an example table of each zone-pair in the above topology and what possible protocols/actions may look like.

|   |   |   |   |
|---|---|---|---|
|**Zone A**|**Zone B**|**Protocol**|**Action**|
|LAN|WAN|ICMP|Drop|
|LAN|LOCAL|-|-|
|LAN|DMZ|-|-|
|WAN|LAN|ICMP|Accept|
|WAN|LOCAL|-|-|
|WAN|DMZ|-|-|
|LOCAL|LAN|-|-|
|LOCAL|WAN|-|-|
|LOCAL|DMZ|-|-|
|DMZ|LAN|HTTP|Accept|
|DMZ|WAN|-|-|
|DMZ|LOCAL|-|-|


This should be the same at the top of each ruleset you create.

```c
name lan-wan {
  default-action drop
  enable-default-log
  rule 1 {
    action accept
    state {
      established enable
      related enable
    }
  }
  rule 2 {
    action drop
    log enable
    state {
      invalid enable
    }
  }
}
```

Now let's add the rule for ICMP.

```c
rule 100 {
    action drop # Define the action for the rule
    log enable # Enable logging to track connection attempts in VyOS
    protocol ipv4-icmp # Protocol to monitor and enforce the action on
 }
```

Now that we have our first zone-pair ruleset, let's create the rules for the opposite zone-pair direction: WAN → LAN. Below is the ruleset required to allow ICMP traffic.

```c
name wan-lan {
  default-action drop
  enable-default-log
  rule 1 {
    action accept
    state {
      established enable
      related enable
    }
  }
  rule 2 {
    action drop
    log enable
    state {
      invalid enable
    }
  }
	rule 100 {
    action accept
    log enable
    protocol ipv4-icmp
 }
}
```

The zone-pair is now defined with appropriate actions and states. We can now combine the firewall ruleset with a previously configured zone.

**Recall:** At the beginning of this task, we configured zone policies with a corresponding interface and common name.

Below is the generic syntax for adding a zone-pair.

`set zone-policy zone <zone A> from <zone B> firewall <name> <ruleset name>`

Below we will set the zone-pair for both the LAN → WAN and WAN → LAN pairs.

1. `set zone-policy zone LAN from WAN firewall name lan-wan`
2. `set zone-policy zone WAN from LAN firewall name wan-lan`

We should have our first zone-pairs defined and enforced now. To test our new configuration, we can attempt to send a `ping` command in both directions to ensure the firewall is dropping or accepting our ICMP packets.





### SSL/TLS Inspection
SSL/TLS inspection uses an **SSL proxy** to intercept protocols, including HTTP, POP3, SMTP, or other SSL/TLS encrypted traffic. Once intercepted, the proxy will decrypt the traffic and send it to be processed by a **UTM** (**U**nified **T**hreat **M**anagement) platform. UTM solutions will employ deep SSL inspection, feeding the decrypted traffic from the proxy into other UTM services, including but not limited to web filters or **IPS** (**I**ntrusion **P**revention **S**ystem), to process the information.
























