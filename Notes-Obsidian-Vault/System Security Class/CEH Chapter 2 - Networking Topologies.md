## Table of Contents

  - [Bus Network](#Bus\Network)
- [Physical Networking](#physical\networking)

IP - Internet Protocol
TCP - Transmission Control Protocol
UDP - User Datagram Protocol

Service Providers are a popular way to provide oursourced resources through the use of Cloud Computing. 

Prior to the late 1970's, communications systems typically used proprietary protocols.

![[Pasted image 20240117103629.png]]


![[Pasted image 20240117231206.png]]


**Application (Layer 7):**
- Closest to end user. 
- Application layer protocols manage the communication needs of the application. They may identify resources and manage interacting with those resources.
- HTTP(Appliocation Layer protocol) takes care of negotiating for resources between the client and server

**Presentation (Layer 6)**:
- Prepares data for the Application layer. Makes sure the data that is handed up to the application is in the right format so it can be consumed. When systems are communicating, there may be disconnects in formatting between the two endpoints, and the presentation layers makes sure that data is formatted correctly.
- ASCII(American Standard Code for Information Interchange), Unicode, and the Extended Binary Coded Decimal Interchange Code (EBCDIC) all belong at the Presentation layer. Additionally, the Joint photographic experts group (JPEG) format is at the presentation layer

**Session (Layer 5)**: 
- Handles RPC (Remote Procedure Calls) There are also components of file sharing that also live at the Session layer, since negotiation of communication between the endpoints needs to take place. The application layer takes care of managing the resources while the Session layer takes care of making sure that files, as an example are successfully transmitted and complete

**Transport (Layer 4)**:
- Takes care of segmenting messages for transmission.
- Also takes care of multiplexing of the communication. Both the TCP and TDP are transport protocols. These protocols use ports for addressing, so receiving systems know which application to pass the traffic to.

**Network (Layer 3)**:
- Network layer get messages from one endpoint to another, it does thgis by taking care of addressing and routing. IP is one protocol that exists or operates at this layer.

**Data Link (Layer 2)**:
- One other address to contend with is the Media Access Control (MAC) address, this is a layer 2 address, identifying the network interface on the network so communications can get from one system to another on the local network. 
- ARP(Address Resolution Protocol), VLAN(Virtual Local Area Network), Ethernet, and Frame relay are Data link protocols

**Physical (Layer 1)**:
- All the protocols that manage the physical communications. (10BaseT, 10Base2, 100BaseTX, and 1000BaseT) are all examples of physical layer protocols. They dictate how the pulses on the wire are handled.

![[Pasted image 20240117231046.png]]

## Bus Network
![[Pasted image 20240117232049.png]]
- Consists of a single network cable to which every device on the network connects. A bus is a communication channel. You may find a bus inside your computer to communicate between channels. In our case, it's a communication channel (a single network cable) that allows communication between multiple computers. 
- Connections are made through the use of a T-connector that provides a w3ay to extract the signal from the bus (Coaxial cable) in order to provide connectivity/communication to the systems on the bus network.


# Physical Networking
Components needed for a network connection:
- Medium that is going to carry the communication (Wired, Wireless)
- You need to have something on the other end of the communication
	_- This section ignores protocols typically used for telecom providers and other service providers. This will almost entirely be focused on Ethernet.

Each layer of the network stack has a different term to refer to the chunk of data encapsulated by that layer. These chunks are called PDUs (Protocol Data Units). The PDU at layer 2, which is a part of what we are talking about here, is a frame. When looking at a chunk of data with the physical address in it, you are looking at a frame. 

MAC Address format - 6 octets (8-bit bytes) separated by colons ':'
Ex: `BA:00:4C:78:57:00` 
- Address is broken down into two parts:
	- Organizationally unique identifier (OUI). This is also called the vendor ID because it ifentifies the name of the company that manufactured the interface. The second half of the MAC address is the unique address within the Vendor ID.