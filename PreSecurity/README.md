## Introduction to Cyber Security
#### Gobuster

"[Gobuster](https://github.com/OJ/gobuster)" Gobuster will take a list of potential page or directory names and try accessing a website with each of them; if the page exists, it tells you.

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206160704.png)
#### SOC

**Security Operations Center (SOC)** is a team of IT security professionals tasked with monitoring, preventing , detecting , investigating, and responding to threats within a company’s network and systems.


> *If a patch is not available, organizations must implement alternative measures to safeguard the system and mitigate potential exploitation.*


***swag voucher: - <!-- KUWCTJ-79O2GR-GPYP8H-H5AL4G -->***

#### Threat Intelligence

In this context, _intelligence_ refers to information you gather about actual and potential enemies.


#### Digital Forensics

Forensics is the application of science to investigate crimes and establish facts.
It focuses on:
- File System (taking images)
- System memory (taking images)
- network logs
- system logs (very important)


#### Incident Response

Incident response specifies the methodology that should be followed to handle a cyber security incident case.
Phases include:
- Preparation
- Detection and Analysis
- Containment, Eradication and Recovery
- Post Incident activity


#### Malware Analysis:

Analysis of malicious software. Includes virus, trojan horse and ransomware. Analysis techniques mainly includes static and dynamic analysis.

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206161040.png)

#### Careers in Cyber Security

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206182127.png)

## Network Fundamentals

#### What is Networking:

- Network
	- Public Network
	- Private Network
- IP Address
	- IPv4
	- IPv6
- MAC Address
- MAC Spoofing
- Ping

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206155940.png)

#### Intro to LAN:

- LAN
- Topologies
	- Star topology
	- Bus topology
	- Ring topology
- Switch
- Router

- Subnetting (dividing a network into multiple parts)
	- Used for security and management.

- ARP
	- Map IP address to MAC Address on each port in a network.
	- Types of Signals by ARP:
		- ARP Request
		- ARP Reply.

When an **ARP request** is sent, a message is broadcasted on the network to other devices asking, "What is the mac address that owns this IP address?" When the other devices receive that message, they will only respond if they own that IP address and will send an **ARP reply** with its MAC address. The requesting device can now remember this mapping and store it in its **ARP cache** for future use.

- DHCP:
	- IP addresses can be assigned **manually** or **automatically using DHCP**. With DHCP, a device requests an IP when it joins a network, the DHCP server offers one, the device accepts it, and the server confirms—allowing the device to start using the IP address.
	- Requests:
		- DHCP discover
		- DHCP offer
		- DHCP request
		- DHCP ACK


Room completed:

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206210225.png)

