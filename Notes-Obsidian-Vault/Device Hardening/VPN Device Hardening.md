## Table of Contents

          - [Command to restart the OpenVPN service](#Command\to\restart\the\OpenVPN\service)
          - [Encryption](#Encryption)
          - [Keep Software up to date](#Keep\Software\up\to\date)
          - [Implement Strong Authentication](#Implement\Strong\Authentication)

- The openVPN server config file is located at `/etc/openvpn/server/server.conf`
###### Command to restart the OpenVPN service
- `sudo systemctl restart openvpn-server@server.service`

###### Encryption
- Configure the VPN to use strong encryption via **AES-256-CBC**. The *cipher* directive in the config file can be used to select the encryption scheme. The possible options Include:
	- AES
	- Blowfish
	- Camelia

###### Keep Software up to date
Ensure the VPN gateway software is always updated with the latest security patches and updates. Every VPN software has a different method  for it.
- To update OpenVPN: `sudo apt upgrade openvpn`


###### Implement Strong Authentication
Use strong authentication mechanisms such as a combination of Transport Layer Security (TLS) and a secure Hashing Algorithm. this is found in the `auth` directive to specify the exact algorithm in the OpenVPN configuration file that will be used for packet authentication.
[OpenVPN](https://community.openvpn.net/openvpn/wiki/Openvpn24ManPage#--auth%20alg)

- Change default settings: Change the default usernames and passwords to something unique to reduce the risk of unauthorized access to the VPN gateway.


- **Enable Perfect Forward Secrecy(PFS):** This simply means that the OpenVPN server generates unique sessions keys for each session so that no one key is used twice. This way, ever if someone successfully obtained a session key, they could not use it to decode more sessions. This is also known as *ephemeral* Session keys
		- We can use the `tls-crypt` directive in the OpenVPN configuration file to enable PFS. the `tls-crypt` directive requires a key that can be generated using the command `sudo openvpn --genkey --secret my.key` and should be placed in the same directory on the server. Choosing the appropriate cipher (`AES-256-CBC`) and `auth SHA256` The exact configuration is mentioned below:




















