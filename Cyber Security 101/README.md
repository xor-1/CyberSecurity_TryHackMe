As I have already done the **Offensive Security Intro** and **Defensive security Intro** in the Pre Security Learning Path so skipping them for now.

Starting out with **Search Skills**.

## Search Skills

**Few things to consider when evaluating information online:**
- Source
- Evidence and reasoning
- Objectivity and Bias
- Corroboration and Consistency

>For information: What do you call a cryptographic method or product considered bogus or fraudulent? --> **snake oil**

#### Search Engines:

Common search engines:
- Google
- Bing
- DuckDuckGo

Google search operations - sometimes we say it Google Dorking.
- `"exact phrase"`: search the web pages with exact phrases.
- `site: domain.com`: we can limit our search to a specific domain
- `-` allows us to omit some results for example: searching for pyramids but also want to omit the tourism websites, I will use: `pyramids -tourism`.
- `filetype:` operator allows us to search for specific file types.

See the [Advanced Search Operations List](https://github.com/cipher387/Advanced-search-operators-list) for reference.


#### Specialized Search Engines

1. **SHODAN:** A search engine for devices connected to the internet (such as servers, routers, webcams, and IoT devices).

![[Pasted image 20260315140227.png]]


2. **Censys:** The search engines for hosts, websites, certificates and other assets.

![[Pasted image 20260315140855.png]]


3. **Virus Total:** A website providing online service for virus scanning for files using multiple antivirus engines. It also has the ability to scan for URLs and file hashes to provide more detailed insights.

![[Pasted image 20260315141457.png]]


4. **Have I been Pwned:** It just does one thing, tells if an email address has appeared in a data breach.

![[Pasted image 20260315141723.png]]


#### Vulnerabilities and Exploits

1. **CVE:** The Common Vulnerability Exposure database of known registered vulnerabilities.
Each CVE has a specific ID.
2. **National Vulnerability Database:** NVD, an alternative to CVE.

3. **ExploitDB:** A database for exploits to the vulnerabilities.
4. **Github:** We can get sophisticated tools and scripts to work to these vulnerabilities like scanning with PoC.


#### Technical Documentation

When we have some issues always go up and look for official documentations of the vendors.
Such as:
- Linux man command
- Windows Documentation
- Other products documentation:
	- Php
	- Node.js
	- Apache


#### Social Media

Social media platforms are great source to grow socially as it will help you to connect with people and companies of your interest. These are great applications but be sure not to over share the information.


**Room Completed**

![[Pasted image 20260315151127.png]]



---



## Linux Fundamentals Part 3

As I have recently did the Linux Fundamentals part 1 and 2 in the PreSecurity Learning path so skipping it for now. The notes are in the PreSecurity Learning Path.

The Linux Fundamentals Part 3 is also documented up to the section **General/Useful Utilities**.


#### Processes 101

**Processes:** Processes are the programs that are running on our machine, they are managed by the kernel assigned a PID which increments in the order they are started.
For example:
29th process has PID of 29.

check processes of user's session:

```bash
ps
```

processes from other users and those that donot run from user's session:

```bash
ps aux
```

the dynamic view of processes in the terminal as it refreshes the terminal every time you press an arrow key to navigate or every 10 seconds:

```bash
top
```

#### Managing processes:

We can use signals to terminate processes.
To stop a process or kill a command, we use the `kill` command.

```bash
kill PID
```

Some other signals:

```bash
SIGTERM

SIGKILL

SIGSTOP
```


#### How processes are started

OS uses namespaces, processes have a small amount of resources in these namespaces and processes in the same namespaces can see each other.

The processes start from PID 0 and so on. The first process on Ubuntu is `systemd` which is a process manager sits between OS and user.

For example:
When a system starts, the first process is the `systemd` with a `PID 0` and all other processes start as child of `systemd`. It will be controlled by `systemd` but will run as its own process.

#### Getting services to start on boot:

Some applications can be started on boot that we own.
We can use the `systemctl` command for that. Using this command we can interact with the `systemd` (which is the process manager sitting between user and OS).

The command formatting is as:

```bash
systemctl [option] [service]
```

For example:

```bash
systemctl start apache2
```

We have 5 `options` for `systemctl`

```bash
start
stop
enable
disable
status
```

#### Backgrounding and foregrounding

Processes can run in two states
- **Background:** After adding the `&` the command runs in the background. We are just given the PID of the process. Its great for copying large files, while we can do other tasks simultaneously.
- **Foreground:** That return the output to your terminal. For example the `echo` command directly returns the output to our terminal is a good example of foreground processing.

If a process is running in the foreground and we want to background it (suspend or stop it). We can use the `ctrl + z` command. To again retrieve it in the foreground we can use the `fg` command and it will start executing again.

