## Introduction to Cyber Security
#### Gobuster

"[Gobuster](https://github.com/OJ/gobuster)" Gobuster will take a list of potential page or directory names and try accessing a website with each of them; if the page exists, it tells you.

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206160704.png)


---

#### SOC

**Security Operations Center (SOC)** is a team of IT security professionals tasked with monitoring, preventing , detecting , investigating, and responding to threats within a company’s network and systems.


> *If a patch is not available, organizations must implement alternative measures to safeguard the system and mitigate potential exploitation.*


***swag voucher: - <!-- KUWCTJ-79O2GR-GPYP8H-H5AL4G -->***


---

#### Threat Intelligence

In this context, _intelligence_ refers to information you gather about actual and potential enemies.


---

#### Digital Forensics

Forensics is the application of science to investigate crimes and establish facts.
It focuses on:
- File System (taking images)
- System memory (taking images)
- network logs
- system logs (very important)


---

#### Incident Response

Incident response specifies the methodology that should be followed to handle a cyber security incident case.
Phases include:
- Preparation
- Detection and Analysis
- Containment, Eradication and Recovery
- Post Incident activity


---

#### Malware Analysis:

Analysis of malicious software. Includes virus, trojan horse and ransomware. Analysis techniques mainly includes static and dynamic analysis.

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206161040.png)


---

#### Careers in Cyber Security

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260206182127.png)


---

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


---

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


---

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

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260207164000.png)


---

#### Completed:

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260207164057.png)


---

## Packets and Frames:

#### What are packets and frames:

Packets are formed on network layer and frames are formed on data link layer after encapsulation. 
Some notable headers in an IP packets are as follows:
- **TTL (Time To Live):** A packet that can live in a network if it is not reached to the intended destination. TTL 64 ms is for Linux and 128 ms is for Windows. Routers typically use 255 ms TTL. To find the hops manually subtract the TTL from the received TTL. For example I did ping to Google's DNS and received 119 ms as TTL. It means It was running a windows server and 128 - 119 = 9. There are 9 hops in between.

- **Checksum:** It is an error detection mechanism in TCP packets. It adds all the headers, payload and Pseudo header and takes one's complement and then send it along the packet to the other device and then it is recalculated there and compared with the sent checksum. If all the bits of both checksum are 1s. Otherwise there will be errors.

- **Source IP address:** The IP address of the device from which the data is being sent from.

- **Destination IP address:** The IP address of the device to which next device the data is being sent.


---

#### TCP/IP - Three way handshake:

- Transmission Control Protocol
- Connection oriented protocol
- Also can be called summarized version of OSI (arguably).

- **These layers include:**
	- Application layer
	- Network layer
	- Transport layer
	- Physical layer
- It also performs encapsulation, antonym is decapsulation.
- **Following headers are found in a TCP packet:**
	- **Src Port:** Port from which TCP packet is send chosen randomly that is currently not in use.
	- **Dst Port:** The port value at which the data is sent. On which a service is running like HTTP on port 80.
	- **Source IP:** The IP address of the device from which the data is sent.
	- **Dest. IP:** The IP address of the device to which the data is sent.
	- **Sequence Number:** In each session when first packet is sent it is assigned a random number.
	- **Acknowledgement Number:** After the first packet the later packets are assigned numbers +1.
	- **[[#What are packets and frames | Checksum]]:** Discussed above.
	- **Data:** The actual data for example the file.
	- **Flag:** This header determines how the packet should be handled by either device during the handshake process.


---

##### Three way handshake messages for TCP:

- **SYN:** This packet is sent by client to initiate the connection and requests for synchronization. Sends ISN to SYNchronise.
- **SYN/ACK:** This packet is sent by the second device to first one to confirm the initiation of synchronization. Also sends the ISN to first device and ACK their ISN. Both devices must be agreed upon a ISN(Initail Sequence Number).
- **ACK:** This packet can be sent by either device to confirm that the series of messages has been successfully received.
- **DATA:** Once a connection is established the data is as BYTES of a file are sent through the DATA message.
- **FIN:** This packet is used to properly close the connection after the data transfer.
- **RST:** This packet abruptly ends all communications and can be due to any error.

##### **TCP Closing a Connection:**
To close the connection the device 1 will send the FIN packet and also the device 2 will also have to ACK this FIN and again send FIN to device 1 and then device 1 will ACK that FIN.


---

#### UDP/IP:

- Stateless protocol
- No SYN
- No three way handshake
- No process takes place to setup a connection
- no data integrity
- simple than TCP and have less headers than TCP
- UDP headers are as follows:
	- **TTL (Time To Live):** This is as the expiry timer of the packet if it doesn't manages to reach the intended host.
	- **Source IP address:** The IP address of the device sending the packet.
	- **Destination IP Address:** The IP Address of the device to which data is being sent.
	- **Source port:** The port number from which the data is being sent. It can be in in range 0 - 65535.
	- **Destination port:** The port number on which the intended application or service is running on the remote host. For example: HTTP on port 80 and SSH on port 22.
	- **Data:** The actual Bytes of the data being sent. e.g the file.
- Repeat: Connectionless and no acknowledgement.


---

#### Ports:
Studied about common ports (0 - 1024) For example:
- 80 for HTTP
- 443 for HTTPS
- 21 for FTP
- 22 for SSH
- 3389 for RDP
- 445 for SMB (Server Message Block - Same as FTP but also allows sharing devices such as printers.)
- $nc


---


## **Completed:**

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260208004555.png)

---


