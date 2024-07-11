## Bluetooth Hacking
Bluetooth is designed for short range wireless communication between devices. It's convenient but opens up a lot of attack surface for hackers etc.

- `bluesnarfing`: An attack involving unauthorised access to information from wireless devices through bluetooth
- `bluejacking`: An attack involving unauthorised access to information from wireless devices 
- `bluesmacking`: a DOS attack that overwhelm's a devices bluetooth connection
- `bluebugging`: A technique use to gain control over a device iva Bluetooth
- `blueborne`: a set of vulnerabilities that allows attackers to take control of devices, spread malware, or perform other malicious activities via Bluetooth
- `KNOB`: Key Negotiation of Bluetooth, an attack that manipulates the data encryption process during Bluetooth connection establishment, weakening security.
- `BIAS`: Bluetooth impersonation Attacks exploits a vulnerability in the pairing process, allowing an attacker to impersonate a trusted device.

## Cryptanalysis Side-Channel Attacks

Cryptanalysis side-channel attacks are an intriguing topic in cybersecurity. These attacks utilise information gained from implementing and running a computer system rather than brute force or theoretical weaknesses in algorithms. We'll discuss:

- A short history of side-channel attacks
- `Timing Attacks`: These exploit the correlation between the computation time of cryptographic algorithms and the secrets they process.
- `Power-Monitoring Attacks`: These monitor the power consumption of a device to determine what data it is processing.

## Microprocessor Vulnerabilities

Microprocessors form the backbone of any computational device. However, their complex design and optimisation strategies often introduce vulnerabilities. We'll explore what microprocessors are, and two notorious microprocessor vulnerabilities:

- `Spectre` and `Meltdown`

As well as delve into mitigation strategies such as:

- `Retpoline`: A binary modification technique used to thwart branch target injection.
- Compiler modifications
- `Kernel Page Table Isolation (KPTI)`: A technique used to isolate the kernel's memory space from user space processes.
- Microcode updates

# Introduction to Bluetooth

Bluetooth is a wireless tech standard designed for transferring data over short distances from fixed and movile devices. The  technology operates by establishing `personal area networks` (PANs) using `radio frequencies` in the `ISM band from 2.402 GHz to 2.480 GHz`. Conceived as a `wireless alternative to RS-232 data cables`. Bluetooth has been widely adopted due to its convenience and flexibility. 


Bluetooth functionality is based on several key concepts, including `device pairing`, `piconets`, and data `transfer protocols`. The first step in establishing a Bluetooth connection is the `pairing process`. This involves two devices `discovering` each other and establishing a connection:

1. `Discovery`: One device makes itself `discoverable`, broadcasting its presence to other Bluetooth devices within range.
2. `Pairing Request`: A second device finds the discoverable device and sends a `pairing request`.
3. `Authentication`: The devices authenticate each other through a process involving a shared secret, known as a `link key` or `long-term key`. This may involve entering a PIN on one or both devices


Once the devices are paired, they remember each other's details and can automatically connect in the future without needing to go through the pairing process again.

After pairing, Bluetooth devices form a network known as a `piconet`. This collection of devices connected via Bluetooth technology consists of one `main` device and up to seven `active client` devices. The `main` device coordinates communication within the piconet.

Multiple piconets can interact to form a larger network known as a `scatternet`. In this configuration, some devices serve as `bridges`, participating in multiple `piconets` and thus enabling inter-`piconet `communication.


Bluetooth connections facilitate both `data` and `audio communication`, with data transfer occurring via packets. The primary device in the piconet dictates the schedule for packet transmission. The Bluetooth specification identifies two types of links for data transfer:

1. `Synchronous Connection-Oriented (SCO) links`: Primarily used for audio communication, these links reserve slots at regular intervals for data transmission, guaranteeing steady, uninterrupted communication ideal for audio data.
2. `Asynchronous Connection-Less (ACL) links`: These links cater to transmitting all other types of data. Unlike SCO links, ACL links do not reserve slots but transmit data whenever bandwidth allows.

### Risks of Bluetooth
Bluetooth risks may potentially allow for `remote, unauthorised access` to a device. There are several categories of bluetooth risks, including:

- `Unauthorised Access`: This risk involves unauthorised entities gaining unsolicited access to Bluetooth-enabled devices. Attackers can exploit vulnerabilities to take control of the device or eavesdrop on data exchanges, potentially compromising sensitive information and user privacy.
- `Data Theft`: Bluetooth-enabled devices store and transmit vast amounts of personal and sensitive data. The risk of data theft arises when attackers exploit vulnerabilities to extract this data without authorisation. Stolen information may include contact lists, messages, passwords, financial details, or other confidential data.
- `Interference`: Bluetooth operates on the 2.4 GHz band, which is shared by numerous other devices and technologies. This creates a risk of interference, where malicious actors may disrupt or corrupt Bluetooth communication. Intentional interference can lead to data loss, connection instability, or other disruptions in device functionality.
- `Denial of Service (DoS)`: Attackers can launch Denial of Service attacks on Bluetooth-enabled devices by overwhelming them with an excessive volume of requests or by exploiting vulnerabilities in Bluetooth protocols. This can result in the targeted device becoming unresponsive, rendering it unable to perform its intended functions.
- `Device Tracking`: Bluetooth technology relies on radio signals to establish connections between devices. Attackers can exploit this characteristic to track the physical location of Bluetooth-enabled devices. Such tracking compromises the privacy and security of device owners, potentially leading to stalking or other malicious activities.


## Bluetooth Attacks

Over the years, several classifications of Bluetooth attacks have been identified, each with its unique characteristics and potential risks. Understanding these attack types is essential for promoting awareness and implementing effective security measures. The following table provides a comprehensive overview of various Bluetooth attacks and their descriptions:

| **Bluetooth Attack**                 | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Bluejacking**                      | Bluejacking is a relatively harmless type of Bluetooth hacking where an attacker `sends unsolicited messages` or business cards to a device. Although it doesn't pose a direct threat, it can infringe on privacy and be a nuisance to the device owner.                                                                                                                                                                                                               |
| **Bluesnarfing**                     | Bluesnarfing entails `unauthorised access to a Bluetooth-enabled device's data`, such as contacts, messages, or calendar entries. Attackers exploit vulnerabilities to retrieve sensitive information without the device owner's knowledge or consent. This could lead to privacy violations and potential misuse of personal data.                                                                                                                                    |
| **Bluebugging**                      | Bluebugging enables an attacker to `control a Bluetooth device`, including making calls, sending messages, and accessing data. By exploiting security weaknesses, attackers can gain unauthorised access and manipulate the device's functionalities, posing a significant risk to user privacy and device security.                                                                                                                                                   |
| **Car Whisperer**                    | Car Whisperer is a Bluetooth hack that `specifically targets vehicles`. Attackers exploit Bluetooth vulnerabilities to remotely unlock car doors or even start the engine without physical access. This poses a serious security threat as it can lead to car theft and compromise the safety of vehicle owners.                                                                                                                                                       |
| **Bluesmacking & Denial of Service** | Denial of Service (DoS) attacks leverage vulnerabilities in Bluetooth protocols to `disrupt or disable the connection between devices`. Bluesmacking, a specific type of DoS attack, involves sending excessive Bluetooth connection requests, overwhelming the target device and rendering it unusable. These attacks can disrupt normal device operations and cause inconvenience to users.                                                                          |
| **Man-in-the-Middle**                | Man-in-the-Middle (MitM) attacks intercept and manipulate data exchanged between Bluetooth devices. By `positioning themselves between the communicating devices`, attackers can eavesdrop on sensitive information or alter the transmitted data. MitM attacks compromise the confidentiality and integrity of Bluetooth communications, posing a significant risk to data security.                                                                                  |
| **BlueBorne**                        | Discovered in 2017, BlueBorne is a critical Bluetooth vulnerability that allows an attacker to take `control of a device without requiring any user interaction or device pairing`. Exploiting multiple zero-day vulnerabilities, BlueBorne presents a severe threat to device security and user privacy. Its pervasive nature makes it a significant concern across numerous Bluetooth-enabled devices.                                                               |
| **Key Extraction**                   | Key extraction attacks aim to `retrieve encryption keys` used in Bluetooth connections. By obtaining these keys, attackers can decrypt and access sensitive data transmitted between devices. Key extraction attacks undermine the confidentiality of Bluetooth communications and can result in exposure of sensitive information.                                                                                                                                    |
| **Eavesdropping**                    | Eavesdropping attacks involve `intercepting and listening to Bluetooth communications`. Attackers capture and analyse data transmitted between devices, potentially gaining sensitive information such as passwords, financial details, or personal conversations. Eavesdropping attacks compromise the confidentiality of Bluetooth communications and can lead to severe privacy violations.                                                                         |
| **Bluetooth Impersonation Attack**   | In this attack type, an attacker `impersonates a trusted Bluetooth device` to gain unauthorised access or deceive the user. By exploiting security vulnerabilities, attackers can trick users into connecting to a malicious device, resulting in data theft, unauthorised access, or other malicious activities. Bluetooth impersonation attacks undermine trust and integrity in Bluetooth connections, posing a significant risk to device security and user trust. |

## Legacy Attacks

Considering the age of Bluetooth, many of the better-known and frequently cited Bluetooth attacks and vulnerabilities were discovered in the early to mid-2000s.

### Bluejacking

`Bluejacking` refers to a legacy attack involving `sending unsolicited messages` to Bluetooth-enabled devices. Unlike Bluesnarfing, Bluejacking does `not involve accessing` or stealing data from the targeted device. Instead, it uses the Bluetooth "business card" feature to `send anonymous messages`, typically in text or images. As such, it is often seen as a prank or nuisance rather than a serious security threat.

![](https://academy.hackthebox.com/storage/modules/230/K600i_Bluejacked.jpg)

In a typical bluejacking scenario, the attacker starts by scanning for other nearby Bluetooth devices. The attacker then chooses a target from the discovered devices and proceeds to craft a text message or selects an image. Using the `Object Push Profile` (OPP), they send unsolicited messages or images to the targeted device.

The `Object Push Profile` (OPP) is a standard Bluetooth profile that facilitates the basic exchange of objects or files between devices. These objects can range from `Virtual Business Cards` (`vCards`), `calendar entries` (`vCalendars`), notes, or other forms of data encapsulated in a file.

Notably, this entire process can be accomplished without establishing a paired connection with the target device, making bluejacking a stealthy operation that leaves little trace other than an unexpected message on the recipient's device.

The impact of Bluejacking is generally limited to `annoyance` or `inconvenience`; however, in some instances, Bluejacking can be used maliciously. For example, it could be used as part of a `social engineering attack`, where the unsolicited message is designed to trick the recipient into revealing sensitive information or performing an action that compromises their security, such as installing malware onto their device.

Over time, the awareness about bluejacking spread, and manufacturers and developers began implementing security measures to address the vulnerability, such as changing default settings. These improvements made it significantly more difficult for unauthorised individuals to perform bluejacking.

Apple devices were susceptible to an attack very similar to Bluejacking via `AirDrop`. AirDrop is a proprietary service Apple provides that allows users to transfer files among supported Macintosh computers and iOS devices over Wi-Fi and Bluetooth—without using mail or a mass storage device.

![[ios-16-iphone-13-pro-receive-airdrop.webp]]

AirDrop utilises Bluetooth to create a peer-to-peer network between devices. To share a file via AirDrop, the user selects the `Share` button on the document, photo, or webpage they wish to send, chooses AirDrop, and selects the device within the range they want to share with. When a file is received via AirDrop, it `appears as a notification`, allowing the user to either accept or decline. If the incoming AirDrop item is accepted, it is automatically saved into the user's Photos, Downloads, or Files folder on their device.

However, this feature has been exploited for spamming purposes.

If your AirDrop was set up to receive files from `Everyone`, this means that anyone within AirDrop range—roughly 30 feet (9 meters)—could share files with you.

In crowded areas such as concerts, subway trains, or aeroplanes, malicious users could send unsolicited photos or other files to any device set to receive from `Everyone`. The sending of unsolicited files is often termed `AirDrop spamming` or `cyber flashing`. However, Apple modified these settings in `iOS 16.2`, where the `Everyone` option had to be explicitly enabled before it automatically disabled itself after 10 minutes, thus only allowing your contacts to send you unsolicited files.
