**Channel** - Bounded range of frequencies much like TV channles or radio stations. Each is allocated a range range of frequencies on which to transmit.

**TKIP** - Temporal Key Integrity Protocol
- WEP Concatenated the IV with the PSK (Pre-Shared Key), TKIP Mixed the keys. This is part of what allowed WEP-enabled devices to work with WPA./


**PMK** - Pairwise Master Key

**AES-CCMP** - AES Counter Mode CBC-MAC Protocol

**PTK** - Pairwise Transient Key

**GTK** - Group Temporal Key

**WPA2** - Wifi Protected Access 2
- Replaced RC4 Stream cipher with AES-CCMP (Counter Mode CBC-MAC Protocol)
- Introduces a Four-Way handshake to improve the protection of the key. There are two keys that are used through this process.
	- Firs is the pairwise master key (PMK), which is retained over time so it has to be protected and not transmitted. They key that gets derived is the pairwise transient key (PTK). There is also a group temporal key (GTK) that comes out of the handshake; it is used to broadcast multicast traffic. Meaning it's a key that is shared across multiple devices.
	1. The access point generates a random ephemeral nonce value.
	2. Access point sends it nonce to the station, along with a key replay counter, which is used to match messages that are sent so any replayed messages can get discarded.
	3. Station generates its own nonce and sends it to the access point, along with the key replay counter that matches the one sent by the access point. 
	4. The station also sends a message integrity check (MIC, message authentication code(MAC)) to ensure there is not tampering.
	5. Once the access point has the station's nonce, it can derive its own PTK. The AP send the **GTK** on to the station, along with a MIC.
	6. Station sends an acknowledgement and the four-way handshake is complete.
	7. Four-way handshake completes authentication since the right keys are in order to be able to complete the handshake
![[Pasted image 20240318130714.png]]

Just as with WPA personal/enterprise authentication modes are available. The handshake process is always the same regardless of which method of authentication is available. Personal requires a pre-shared key. Enterprise authentication uses 802.1X to handle user-level authentication, meaning someone connecting to the network has to provide a username and password. The authentication framework used is the EAP and the different variations of EAP. Common variations include: Lightweight Extensible Authentication Protocol(LEAP) and Protected Extensible Authentication Protocol(PEAP), EAP-TLS. Ensure authentication is done over a protected channel so any authentication parameters are not sent in the clear. And key exchange is done across a protected channel. Often TLS. This is how web traffic is encrypted. It makes use of certificates and the asymmetric key encryption using pub/priv keypairs.



**LEAP**: Lightweight Extensible Authentication Protocol

**SAE**: Simultaneous Authentication of Equals
- This is the process that is followed using SAE.
	1. The station (STA) sends a probe request to the access point (AP).
	2. The AP sends a probe response to the STA.
	3. STA sends an authentication commit to the AP. This is used to generate the pairwise master key (PMK) on the STA. This is an 802.11 authentication frame.
	4. AP sends an authentication commit to the STA. This is used to generate the pairwise master key on the AP. This is also an 802.11 authentication frame.
	5. STA sends an authentication commit to the AP. This is an 802.11 authentication frame. This contains a message with a key generated for the AP to validate.
	6. AP sends an authentication commit to the STA. This is also an 802.11 authentication frame. It contains a message with a key generated for the STA to validate.
	7. Regular association request.
	8. Regular association response.
	9. Four‐way handshake using the PMK that was generated in the previous steps.
**PEAP**: Protected Extensible Authentication Protocol

**802.11** - Set of wireless specifications for the Physical and Data Link Layers
- Physical layer specifies modulation schemes used to transmit the data and also specifies the frequency spectrum used.
- V1 of 802.11 specified transmission in the 2.4ghz range with data rates between 1 and 2 megabits per second.
- Devices like microwaves and some cordless phones operate within the same frequency spectrum as bluetooth devices and WiFi. Making it a little crowded and leading to the potential for conflict.
**802.11a** - supported transmission in the 5Ghz and 2.4GHz ranges, although wasn't widely implemented
- In 2009 **802.11n** was released and 5GHz became a viable frequency to work in the 5GHz range. 
- Since then, **802.11ac** and the proposed **802.11ax** have both been specified to work in the 5 GHz range. This causes issues, of course, since older radios in Wi‐Fi interfaces won't work on anything other than the frequency they have been tuned to. If a network decided to adopt **802.11ac** in the 5 GHz range but they still had a number of **802.11g** cards, those cards would not be able to connect to the new network.

Both Bluetooth and 802.11 can be easily misconfigured.



### Sniffing Traffic
To see the entire stack from wireless traffic, `airmon-ng` can be used. 

**Using airmon‐ng**  
```
root@quiche:~# airmon-ng start wlan0
```


- Use `tcpdump` to capture all network traffic that passes across the monitor interface. 

### Deauthentication Attack
Sends messages that force stations to re-authenticate against the access point. Essentially, it logs out any station, making the station reestablish the association. This is another place to use the `aircrack` suite of tools. In this case, use `aireplay-ng`. 
- The interface used needs to be on the right channel to know what frewuency to send messages on. Under linux, this is done using the `iwconfig` program. On windows, use the device properties of the wireless interface. 
**Deauthentication Attack**  
  
```
root@quiche:~# sudo iwconfig wlan0mon channel 6  
root@quiche:~# aireplay-ng -0 10 -a  C4:EA:1D:D3:78:39 -c 64:52:99:50:48:94 wlan0mon
```

To perform a deauth attack, you need to have the BSSID of the access point, or the MAC address for the device so it can be used in the frame headers. You also need the MAC address of the station on which you want to run the deauthentication attack. Use the `-c` parameter to run a death attack against all stations. 

**Evil twin**
- Rogue access point configured to look like a legit one. 
- Advertises a known SSID. 
- Gather information from stations, authentication information, PSK (Pre-Shared Key)
- 
##### Wireless Attack Tools
- `airgeddon` - DoS Attacks, Handshake tools, Offline WPA/WPA2 decrypt menu, Evil Twin attacks, WPS attacks, WEP attacks
	- Requires two wireless interfaces to perform an evil twin attack. One for DoS Pursuit mode, which present frequency hopping that can be a protection against wireless attacks. 
	- Second interface is also used to perform a DoS attack against the legit access point. This keeps it from responding to the station instead of the rogue access point


- `wifiphisher` - Impersonate a wireless network while jamming a legitimate access point and then redirect traffic to a site managed by you.
- `airodump-ng` - Used to perform deauthentication attacks.
- `aireplay-ng` - Used to perform deauthentication attacks.
- `airmon-ng` - Create monitor interfaces from a wireless interface
- `bettercap` - Perform spoofing attacks on top of just seeing wireless traffic.
	- This also allows the use of BeEF(Browser Exploitation Framework) to exploit vulnerabilities in browsers. 
- `mdk3/4` - Used to perform beacon flooding, authentication DoS attacks, and de-authentication attacks
	- First create a interface in monitor mode. The command below uses channel `1` for example
```
airmon-ng start wlan0 1
```
- Once enabled you can start launching sttacks
	- 

**Bluejacking**
Attacker sends data to a Bluetooth device without having to get through the pairing process, or perhaps the pairing happens without the receiver knowing about it. Bluejacking can be used to send an unsolicited message to a victim. This might be a picture or a text message, or a spoof attack where you send a message that appears to be from someone else in order tog et the recipient to do something

**Bluesnarf**
Bluesnarfing is getting data from a device. Bluetooth devices have to be exposed to a certain degree to allow other devices to begin a pairing process. This opens up the possibility of another device taking advantage of that little window

**Bluebugging**
Bluebuggign attack uses bluetooth to gain access to a phone in order to place a phone call. Once the phone call is placed, the attacker has a remote listening device. 

**Bluedump**
Attacker has to know the BDADDR (hardware address) of a trusted device. The attacker spoofs that address in a connection attempt. The target device will respond with an authentication request. 