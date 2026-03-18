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

#### Maintaining your system: CRONTABS

We sometimes want to schedule the jobs or do automation. `Cron` comes in. We create `crontabs` for this. `Crontabs` is a process that starts during boot and take care of the `cron jobs`. 
A `crontab` is a special file that is recognized by `cron` process to execute each line step by step.

Crontab requires 6 specific values:

| Value | Description                               |
| ----- | ----------------------------------------- |
| MIN   | What minute to execute at                 |
| HOUR  | What hour to execute at                   |
| DOM   | What day of the month to execute at       |
| MON   | What month of the year to execute at      |
| DOW   | What day of the week to execute at        |
| CMD   | The actual command that will be executed. |

example for backing up files of a user at every midnight:

```bash
0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/
```

Crontabs also support wildcards `*` as don't cares.

For crontab generation:

- https://crontab-generator.org/
- https://crontab.guru/

In system the crontab can be edited using:

```bash
crontab -e
```

#### Maintaining your system: Package Management

**Introducing packages and software repos:**
If a developer wants to publish their tool or software, they submit it to a `apt` repository, once approved the tool is launched into the wild.

we can check the list of `apt` repositories in (these files are registry/gateways):

```bash
/etc/apt/
```

OS vendors like ubuntu maintain their own reporsitories but we can add community repos also.
It can be added using the command:

```bash
apt-add-repository
```

Apt is a package management tool used to install manage and remove packages.
we can also use the `dpkg` command for package management but using `apt` all software are checked for updates.

When adding the software repo, the integrity is checked using the `GPG(Gnu Privacy Gaurd) keys` the keys are downloaded and checked if the developer also used the same keys if the keys are different the software will not be downloaded.

For example:
1. adding the keys for sublime text editor:
```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```

2. Now add its repo to apt source list. Good to have a separate file for 3rd party or community repos.
3. create a file named **sublime-text.list** in **/etc/apt/sources.list.d/**
4. add this using nano:
```bash
deb https://download.sublimetext.com /apt/stable/
```
5. Now use `apt update` command to update `apt`.
6. Once updated we can download the sublime text editor using:
```bash
apt install sublime-text
```
7. Removing packages is as easy as reversing. This process is done by using the `add-apt-repository --remove ppa:PPA_Name/ppa` command or by manually deleting the file that we previously added to. Once removed, we can just use `apt remove [software-name-here]` i.e. `apt remove sublime-text`


#### Logs:

The `/var/log/` directory contains the logs in linux.
OS does log rotating:

Additional info:
Rotating in Linux, specifically **log rotation** via the `logrotate` utility, is ==the automatic process of archiving, compressing, and deleting old system or application logs to prevent them from consuming excessive disk space and slowing down the system==. It keeps logs manageable by creating new log files and cleaning up old ones.

The new log files are created by name.1 and so on.

**Room completed**

![[Pasted image 20260317013218.png]]



----



## Windows Fundamentals 1

**File Systems:** It decides that how files and folders are stored in a disk.

Common FIle Systems:
- NTFS (Journaling File System)
- FAT16/32
- HPFS

**NTFS volume permissions**
- Full control
- modify
- read and execute
- list folder contents
- read
- write

**ADS (Alternate Data Stream**
NTFS allows files to contain other data streams as well. Powershell allows us to view ADS for files.

Learn more about ADS: https://www.malwarebytes.com/blog/101/2015/07/introduction-to-alternate-data-streams

#### Environment variables:
_Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders_

The environment variable for the Windows directory is `%windir%`

#### User Accounts, Profiles, and Permissions

**Two types of accounts:**
- Administrator
- User

we can view the user management and group policy view using:

`win + r`
`lusrmgr.msc`

#### UAC

Windows use UAC to protect local user from admin privileges. user accounts operate in non privileged mode.

**Working:**
when a user with administrator account logs in, the session is not running in privileged mode.
When OS requires privileged access, UAC prompts to ask if allow or deny permission. This way programs are controlled.

|Feature|UAC (Windows)|sudo (Linux/macOS)|
|---|---|---|
|Interface|GUI prompt|Command-line|
|Control model|System-wide policy|User/group-based rules|
|Default behavior|Admins run as limited users|Users are non-root by default|
|Flexibility|Less granular|Highly configurable|
|Usage style|Automatic prompts|Explicit command (`sudo`)|

learn more about UAC at: https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/how-it-works

## Settings and the control panel

Windows allows users to change settings through control panel later from windows 8 the settings app was introduced.

## Task Manager

It provides insights into current processes and applications running along with resource usage on the system.


**Room completed:**

![[Pasted image 20260318202344.png]]


For now:

![[Pasted image 20260318202415.png]]



---



## System Configuration and Advanced System Settings

System configuration or `MSConfig` utility is used to advanced troubleshooting.

**It has 5 tabs:**
- General
- Boot
- Services
- Startups
- Tools

In windows server the `starup` are not shown in the `MSConfig` utility or `Task Manager`. It can be listed by going to the `Startup` folder or by pressing `Win + R` and entering `shell:startup` and press `Enter`.

In the `tools` tab, consists tools that we can further use to configure our OS.

#### Advanced system settings

It allows us to do additional configurations. can be used by typing `view advanced system settings` in the `Start Menu` and it contains many features but two are main:

- **File Paging:** Windows uses some of the memory as virtual memory. When the RAM runs out it uses that memory to prevent program crashes.
- **Startup and Recovery:** When system crashes or Screen of Death happens, the system automatically dumps file containing the root cause and actions took place during crash. That file is contained at environment variable; `%SystemRoot%/` where as the `SystemRoot` is `C:/Windows`.


#### Change UAC Settings:
you can change the UAC settings in the using the `MSConfig` utility or `User Account Control settings` 

**It has four level:**
- Always Notify
- Notify for apps
- Notify without dimming
- Never notify

