## Table of Contents

      - [Hashcat-Utils - Installation](#Hashcat-Utils\-\Installation)
      - [Cap2hccapx - Syntax](#Cap2hccapx\-\Syntax)
      - [Cap2hccapx - Convert To Crackable File](#Cap2hccapx\-\Convert\To\Crackable\File)
      - [Hashcat - Cracking WPA Handshakes](#Hashcat\-\Cracking\WPA\Handshakes)
  - [Cracking PMKID](#Cracking\PMKID)
      - [Hcxpcaptool - Help](#Hcxpcaptool\-\Help)
      - [Extract PMKID - Using Hcxpcaptool](#Extract\PMKID\-\Using\Hcxpcaptool)
      - [Hcxtools - Installation](#Hcxtools\-\Installation)

#aircrack-ng #wpa #wpa-2 #hashcracking

These keys are used to generate a common key called the Message Integrity Check (MIC) used by an AP to verify that each packet has not been compromised and received in its original state.

The 4-way handshake is illustrated in the following diagram:

![image](https://academy.hackthebox.com/storage/modules/20/NEW_4-way-handshake.png)

Once we have successfully captured a 4-way handshake with a tool such as [airodump-ng](https://www.aircrack-ng.org/doku.php?id=airodump-ng), we need to convert it to a format that can be supplied to `Hashcat` for cracking. The format required is `hccapx`, and `Hashcat` hosts an online service to convert to this format (not recommended for actual client data but fine for lab/practice exercises): [cap2hashcat online](https://hashcat.net/cap2hashcat). To perform the conversion offline, we need the `hashcat-utils` repo from GitHub.

We can clone the repo and compile the tool as follows:

#### Hashcat-Utils - Installation

  Hashcat-Utils - Installation

```shell-session
gdxqpardo@htb[/htb]$ git clone https://github.com/hashcat/hashcat-utils.git
gdxqpardo@htb[/htb]$ cd hashcat-utils/src
gdxqpardo@htb[/htb]$ make
```


Once the tool is compiled, we can run it and see the usage options:

#### Cap2hccapx - Syntax

  Cap2hccapx - Syntax

```shell-session
gdxqpardo@htb[/htb]$ ./cap2hccapx.bin 

usage: ./cap2hccapx.bin input.cap output.hccapx [filter by essid] [additional network essid:bssid]
```

Next, we need to supply a packet capture (.cap) file to the tool to convert to the .hccapx format to supply to `Hashcat`.

#### Cap2hccapx - Convert To Crackable File

  Cap2hccapx - Convert To Crackable File

```shell-session
gdxqpardo@htb[/htb]$ ./cap2hccapx.bin corp_capture1-01.cap mic_to_crack.hccapx
```

With this file, we can then move on to cracking using one or more of the techniques discussed earlier in this module. For this example, we will perform a straight dictionary attack to crack the WPA handshake. To attempt to crack this hash, we will use mode `22000`, as the previous mode `2500` has been deprecated. Our command for cracking this hash will look like `hashcat -a 0 -m 22000 mic_to_crack.hccapx /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt`.

Let's go ahead and try to recover the key!

#### Hashcat - Cracking WPA Handshakes

  Hashcat - Cracking WPA Handshakes

```shell-session
gdxqpardo@htb[/htb]$ hashcat -a 0 -m 22000 mic_to_crack.hccapx /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt

```

## Cracking PMKID

This attack can be performed against wireless networks that use WPA/WPA2-PSK (pre-shared key) and allows us to obtain the PSK being used by the targeted wireless network by attacking the AP directly. The attack does not require deauthentication (deauth) of any users from the target AP. The PMK is the same as in the MIC (4-way handshake) attack but can generally be obtained faster and without interrupting any users.

The Pairwise Master Key Identifier (PMKID) is the AP's unique identifier to keep track of the Pairwise Master Key (PMK) used by the client. The PMKID is located in the 1st packet of the 4-way handshake and can be easier to obtain since it does not require capturing the entire 4-way handshake. PMKID is calculated with HMAC-SHA1 with the PMK (Wireless network password) used as a key, the string "PMK Name," MAC address of the access point, and the MAC address of the station. Below is a visual representation of the PMKID calculation:

![image](https://academy.hackthebox.com/storage/modules/20/NEW_PMKID_calc.png)

To perform PMKID cracking, we need to obtain the pmkid hash. The first step is extracting it from the capture (.cap) file using a tool such as `hcxpcaptool` from `hcxtools`. We can install `hcxtools` on Parrot using apt: `sudo apt install hcxtools`.

**Tool replaced by: https://github.com/ZerBea/hcxtools**
Once we've successfully installed `hcxtools` using apt (if working from own VM), we can issue the command `hcxpcaptool -h` to check out the options available with the tool.

#### Hcxpcaptool - Help

  Hcxpcaptool - Help

```shell-session
gdxqpardo@htb[/htb]$ hcxpcaptool -h
```

#### Extract PMKID - Using Hcxpcaptool

  Extract PMKID - Using Hcxpcaptool

```shell-session

gdxqpardo@htb[/htb]$ hcxpcaptool -z pmkidhash_corp cracking_pmkid.cap 
```


Once again, we will perform a straight dictionary attack in an attempt to crack the WPA handshake. To attempt to crack this hash, we will use mode `22000` once again as the previous mode `16800` has also been depcreated. Here our command will be in the format `hashcat -a 0 -m 22000 pmkidhash_corp /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt`.
#### Hcxtools - Installation

  Hcxtools - Installation

```shell-session
gdxqpardo@htb[/htb]$ git clone https://github.com/ZerBea/hcxtools.git
gdxqpardo@htb[/htb]$ cd hcxtools
gdxqpardo@htb[/htb]$ make && make install
```

Once installed we can then use `hcxpcapngtool` to extract the hash in a similar manner.

  Hcxtools - Installation

```shell-session
gdxqpardo@htb[/htb]$ hcxpcapngtool cracking_pmkid.cap -o pmkidhash_corp2
```