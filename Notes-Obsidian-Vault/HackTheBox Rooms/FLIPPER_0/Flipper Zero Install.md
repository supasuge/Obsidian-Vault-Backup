## Table of Contents

  - [KeyWords](#KeyWords)
    - [RFID](#RFID)
        - [High Frequency Tags (**13.56MHz range**)](#High\Frequency\Tags\(**13.56MHz\range**))
        - [Low Frequency Tags(**125khz range**)](#Low\Frequency\Tags(**125khz\range**))
          - [HOW IT WORKS:](#HOW\IT\WORKS:)
        - [High Frequency tags](#High\Frequency\tags)
  - [RFID](#RFID)
    - [HID](#HID)
    - [Ndala](#Ndala)
  - [Bank Cards (EMV)](#Bank\Cards\(EMV))
- [Marauder Firmware](#marauder\firmware)
      - [Requesting all TGS Tickets](#Requesting\all\TGS\Tickets)



Store all shit on the `SD Card` 

Install the firmware 

## KeyWords
ISO-1443: ISO-DEP 



### RFID
- Near Field Communications
	- reader/writer devices
	- mobile terminals
	- RF Tags
##### High Frequency Tags (**13.56MHz range**)
- Work at high levels 
- Lower effective range
- more complex protocols
- Supports encryption, authentication, cryptography etc.
	- Antenna wire is wider and has less turns
##### Low Frequency Tags(**125khz range**)
- Work at higher ranges with low security
- Antenna wire is thinner and has more turns
	- spaces between coils are invisible



###### HOW IT WORKS:
As soon as something come's in range:
- The EMV field activates the RF tag. Tag can only activate when it's inside a suitable field


WHEN FINDING ANTENNA
- Use a light


TAG TYPES
- Low frequency tags used for low security features and such, such as paid parking, building access, etc.
	- Low Frequencies are primitive and hard to implment two-way data transfers and such. only transfer short-id's with no means of authentication.

##### High Frequency tags
- Reader/Writer:
	- RTD
	- NDEF
	- Tag Operation
	- Tag Platform
	- ISO-DEP(T4T)
	- NFC Technology
	- RF Analog
- Card Emulation
	- ISO-DEP
	- NFC Technology
	- RF Analog
	- Payment
	- Ticketing
	- Legacy App.
- Peer-To-Peer
	- RTD
	- NDEF
	- SNEP
	- LLCP
	- NFC-DEP
	- NFC Technology
	- RF Analog










FLIPPER RFID
- Supports both *High and Low frequencies*
- Antenna situated on the bottom of the device
![[Pasted image 20231118161714.png]]

- ID is incsribed into the `FUCKING card itself`

## RFID


### HID
more complex and close reading shit

### Ndala
- Compplex low-level protocol. 


Can be Read/Saved/Writed to. 


## Bank Cards (EMV)
	Virtual Card
- Impossible to use intercepted data for web payment
- Cardholdername unavailable

	Physical Card
- Possible to make a web payment if PAN and exp. date are known
- Cardholder name sometimes available





# Marauder Firmware
[(15) Flipper Zero: The Ultimate WI-FI Guide | Marauder ESP32 - YouTube](https://www.youtube.com/watch?v=nEBZ4VeTj7I)

JustCallMeKoko Github repo: [https://github.com/justcallmekoko/ESP..](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbnBKM0Rqd0VfQlc5V2FaWjl3VDllbGtQNGJBd3xBQ3Jtc0trN1RGWlBuNjZiQ2xCYk9PNEJ2cWZURFFQMEhlcENTSHM0ZXA4RFpGUGR5c1JMS3Q2b1hFai1EQzN0Y2c5WERKQ2dzaExtODBzQUVUWUxHS0RoNHN4NDhYdXE5UElJTGRld1BIVWJfcGNUZE9oVWlDSQ&q=https%3A%2F%2Fgithub.com%2Fjustcallmekoko%2FESP..&v=nEBZ4VeTj7I). Arduino IDE Setup - [https://www.arduino.cc/en/software](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGtYclBPaTNiRE5YZFlOZ3FPd2tmbnZaYXZhZ3xBQ3Jtc0tuLURRak05Mi1JenFZRzdMMmEtRjl3ZjRoZFdrOXFBa0drMmE3bEFwajVCZy1FdmVEZmY5RWJUTHUwMVNDeURTcU5sMzlBQ0VuUG5MN0U0X1ZWUFhBTl9UMEJoV09LeTNUSkRvSUpyRWZVdmtwOElGWQ&q=https%3A%2F%2Fwww.arduino.cc%2Fen%2Fsoftware&v=nEBZ4VeTj7I) Arduino code: [https://raw.githubusercontent.com/jus...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbVVqTVBlWm55dzBlTTZ6b2ZaNzd1OGFJYlljd3xBQ3Jtc0tuRGtmdS1IZTlLY0tablM2LXZ6ZmU0TzVqMHRXMzdkaU1udzJrclNTNnVMQTY0dlM3bm5Wd2w2ZkRqQ3RIN1JvWEJzeXYyWUtwdlQ3dUVndkl1dzMwc2NkOVlXLVNxa1hEc3FTNkhRQjhtUDE4amRQQQ&q=https%3A%2F%2Fraw.githubusercontent.com%2Fjustcallmekoko%2FESP32Marauder%2Fmaster%2FMarauderOTA%2FMarauderOTA.ino&v=nEBZ4VeTj7I) Arduino IDE Setup: [https://github.com/justcallmekoko/ESP...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbk5QU2RhYmxRNFQtVndlTUxUdUVnMTgySVJOQXxBQ3Jtc0trbEUxSUJWb25VYUEzUlFOWHhfNTRJc2ZHN0pKVGVQN29NMFg4aFZ4clhlRF9QdEw1Vy1iS1lKeW81OFZBNGRtWmpQV20wRDhwWlJkY1g0eE5kR0RNalBSOHhicF9YWjYtS0tEMkx1WWo5UE9lSEE5WQ&q=https%3A%2F%2Fgithub.com%2Fjustcallmekoko%2FESP32Marauder%2Fwiki%2Farduino-ide-setup&v=nEBZ4VeTj7I) FZ_Marauder_Flasher Github repo: [https://github.com/UberGuidoZ/Flipper...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGh0cTNKMEVHcUdIQ1J5aDdfejlyM2E2QVBXUXxBQ3Jtc0tuTGFCSEo3a0IyUi1vaDMxTmwyektjOHRHN3EtYThwZXFpTnhkd1hPNFhMbUxtejRqOXZwOGt0OVFVUFRZYVVxQl9LY1p1YzRjRWp4MHJRTVRzSlgxUnB5OTcyZFB0c0hzQVZCMERiVEloSXp3QkY1UQ&q=https%3A%2F%2Fgithub.com%2FUberGuidoZ%2FFlipper%2Ftree%2Fmain%2FWifi_DevBoard%2FFZ_Marauder_Flasher&v=nEBZ4VeTj7I)


![[Pasted image 20231118162824.png]]


![[Pasted image 20231118163029.png]]
![[Pasted image 20231118163112.png]]


> Boards Manager

`type: esp32`
`2.0.1 RC1`


![[Pasted image 20231118163238.png]]

- Hold for 3 Seconds then Relsease
![[Pasted image 20231118163313.png]]


![[Pasted image 20231118163320.png]]

>> Copy and paste code from dudes page into IDE

> Upload


![[Pasted image 20231118163842.png]]

![[Pasted image 20231118164148.png]]

We can now pull all TGS tickets for offline processing using the `-request` flag. The TGS tickets will be output in a format that can be readily provided to Hashcat or John the Ripper for offline password cracking attempts.

#### Requesting all TGS Tickets

  Requesting all TGS Tickets

```shell-session
gdxqpardo@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request 
```


