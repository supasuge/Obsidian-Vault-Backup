## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Configuring Firewalls to block traffic](#configuring\firewalls\to\block\traffic)
    - [Configuring Firewalls to Block Traffic](#Configuring\Firewalls\to\Block\Traffic)
        - [UFW Status](#UFW\Status)
        - [UFW Default Policies](#UFW\Default\Policies)

## Table of Contents

- [Configuring Firewalls to block traffic](#configuring\firewalls\to\block\traffic)
    - [Configuring Firewalls to Block Traffic](#Configuring\Firewalls\to\Block\Traffic)
        - [UFW Status](#UFW\Status)
        - [UFW Default Policies](#UFW\Default\Policies)




[The DiamondModel](https://tryhackme.com/room/diamondmodelrmuwwg42)[](https://tryhackme.com/room/diamondmodelrmuwwg42) is a security analysis framework that seasoned professionals use to unravel the mysteries of adversary operations and identify the elements used in an intrusion. It comprises four core facets, interconnected to form a well-orchestrated blueprint of the attacker's plans:  

- Adversary
- Victim
- Infrastructure
- Capability
![[Pasted image 20231215011102.png]]




# Configuring Firewalls to block traffic
### Configuring Firewalls to Block Traffic

Van Twinkle knows that the uncomplicated firewall is the default firewall configuration tool available on Ubuntu hosts, and she decides to use it for this experiment. Initially, it's turned off by default, so we can check the status by running the command below:

##### UFW Status
```shell
vantwinkle@aocday13:~$ sudo ufw status
Status inactive
```


##### UFW Default Policies
```bash
sudo ufw default allow outgoing
```

```bash
sudo ufw default deny incoming
```


- Can add, mofiy, and delete rules by specifying an IP address, port number, service name, or protocol. In this example, we can add a rule to allow legitimate incoming connections to port 22  (SSH). Should get ==2== confirmations:
```bash
sudo ufw allow 22/tcp
```

UFW Deny Rules

```bash
vantwinkle@aocday13:~$ sudo ufw deny from 192.168.100.25
Rule added

vantwinkle@aocday13:~$ sudo ufw deny in on eth0 from 192.168.100.26
Rule added
```


Once we have added our rules, we can enable the service and check the rules set.

Enabling UFW

```shell-session
vantwinkle@aocday13:~$ sudo ufw enable
Firewall is active and enabled on system startup

vantwinkle@aocday13:~$ sudo ufw status verbose
```

What happens if the rules are incorrectly configured? We can reset the firewall and, revert to its default state and be able to configure the rules fresh.

Resetting UFW

```shell-session
vantwinkle@aocday13:~$ sudo ufw reset
```

1. Which Security model is being used to analyse the breach and defence strategies?
`diamond model`
2. Which defence capability is used to actively search for signs of malicious activity?
`threat hunting`
3. What are our main two infrastructure focuses? (Answer format: answer1, and answer2)
`firewall and honeypot`
4. Which firewall command is used to block traffic?
`deny`
5. There is a flag in one of the stores, you find it?
`THM{P0T$_W@11S_4_S@N7@}`































