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

#### OSI Model
- All devices follow same unique framework.
- Seven layers arranged from 7 to 1 (bottom to top).
- Encapsulation in the OSI model refers to the process of wrapping data with protocol information at each layer of the model. As data is passed down from the application layer to the physical layer, each layer adds its own header (and sometimes trailer) to the data. This process ensures that the data is properly handled and understood by the receiving device, as each layer only interacts with its adjacent layers. This is crucial for effective communication over a network.


##### Physical Layer:
- Lowest layer in OSI
- Devices use binary to send and receive data on hardware.

##### Data Link Layer:
- focuses on physical addressing like MAC address that each NIC in hardware have.
- adds MAC of receiving device after receiving packet from network layer with IP header added.
- Converts the data that is suitable for transmission.

##### Network Layer:
- Third layer of OSI.
- Main Goals: Routing and Reassembly.
- Routing Protocols used:
	- OSPF
	- RIP
- Shortest path is decided by following factors:
	- Shortest path.
	- Reliable path.
	- Fastest Path.
- Layer 3 devices e.g Routers

##### Transport Layer:
- 4th Layer
- Protocols used:
	- TCP:
		- Connection oriented, provides reliable data transfer, flow control and congestion control.
	- UDP:
		- Connectionless protocol, suitable for protocols require fast queries like DNS.
		- No gaurantee no synchronization, just hope for the best.


##### Session Layer:
- 5th Layer
- Creates, maintains and terminates sessions with other devices.
- Uses checkpoints to retransmit lost data.
- Technical term for established connection = "session"


##### Presentation Layer:
- Layer 6
- Used for standardisation.
- converts data in one format to and from Application layer.
- Example: two developers developing two different email clients.
- Performs functions such as HTTPS for secure website preview.
- Acts as a "translator" to application layer.


##### Application Layer:
- Last layer in OSI
- Protocols used determines how user will interact with data sent or received.
- Exmples:
	- HTTP/HTTPS
	- DNS
	- FTP - etc


#### OSI Dungeon Escaped:

![[Pasted image 20260207164000.png]]


#### Completed:

![[Pasted image 20260207164057.png]]
