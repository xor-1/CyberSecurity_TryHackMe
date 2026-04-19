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

| Feature          | UAC (Windows)               | sudo (Linux/macOS)            |
| ---------------- | --------------------------- | ----------------------------- |
| Interface        | GUI prompt                  | Command-line                  |
| Control model    | System-wide policy          | User/group-based rules        |
| Default behavior | Admins run as limited users | Users are non-root by default |
| Flexibility      | Less granular               | Highly configurable           |
| Usage style      | Automatic prompts           | Explicit command (`sudo`)     |

learn more about UAC at: https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/how-it-works

#### Settings and the control panel

Windows allows users to change settings through control panel later from windows 8 the settings app was introduced.

#### Task Manager

It provides insights into current processes and applications running along with resource usage on the system.


**Room completed:**

![[Pasted image 20260318202344.png]]


For now:

![[Pasted image 20260318202415.png]]



---



## Windows Fundamentals 2
#### System Configuration and Advanced System Settings

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

#### Computer management

The computer management or `compmgmt` has three primary sections:
- System tools:
	- Task Scheduler
	- Event viewer
	- Shared folders
	- Performance
	- Device manager

- Storage:
	- Windows server backup
	- disk management

- Services and applications:
	- Services (see properties for more info.)

> A service is a special type of application that runs in the background.


#### System information

We can access the tool `Msinfo32` from the `MSConfig` utility.
This tool gives detailed info about the system.

This includes the following:
- Hardware resources
- components
- Software environment

> We can get all the `Environment Variables` in `Software Enviroment` Section.


#### Resource Monitor

`remon`

_Resource Monitor displays per-process and aggregate CPU, memory, disk, and network usage information, in addition to providing details about which processes are using individual file handles and modules._

#### Command prompt:

we can use the command prompt to interact with the computer.
also known as `cmd`

some commands to try:

```cmd
hostname
whoami
ipconfig
cls
netstat
net [option]
ipconfig /all
```

A  command to retrieve the help manual for a command is `/?`. e.g `ipconfig /?`


#### Registry

It is a hierarchical database used to store information necessary to configure system.

it includes:
- Profiles for each user
- Applications installed on the computer and the types of documents that each can create
- Property sheet settings for folders and application icons
- What hardware exists on the system
- The ports that are being used.
- and other advanced security settings.

**WARNING: DO NOT MAKE CHANGES TO REGISTRY IF YOU DO NOT KNOW!**

We can edit the registry using `regedit`.

learn more about registry: https://docs.microsoft.com/en-us/troubleshoot/windows-server/performance/windows-registry-advanced-users

command to open: `regedt32.exe`

The main player for all these type of tools is `MSConfig`

**Room completed:**

![[Pasted image 20260319232553.png]]



---



## Windows Fundamentals 3

This module will cover the security features of windows operating system.

#### Windows Update

Windows updates are provided my Microsoft to provide updates, features and patches.

- **Patch Tuesday: THE TUESDAY ON WHICH THE MICROSOFT PUSHES UPDATES TO WINDOWS DEVICES, BUT IT IS NOT NECESSARY IF UPDATES ARE URGENT THEN IT IS PUSHED AT THAT TIME.**

The `Windows Updates` can be accessed from the `Settings` app or by typing the command in the `cmd`:

```cmd
control /name Microsoft.WindowsUpdate
```

From onwards Windows 10 the updates cannot be ignored and they can just be postponed and installed to keep the systems safe.


#### Windows Security

Per Microsoft, "_**Windows Security** is your home to manage the tools that protect your device and your data_".

Windows security contains the four major `Protection Areas`:
- Virus and Threat Protection
- Firewall and Network Protection
- App and browser control
- Device Security

so in further tasks we will go deep into each of this `Protection Area`

#### Virus and Threat Protection:
It is divided into two parts;
- **Current threats:**
	- We have **scan options** and **threat history**.

- **Virus and threat protection settings:**
	- **Real-time protection**
	- **Cloud-delivered protection** 
	- **Automatic sample submission** 
	- **Controlled folder access** 
	- **Exclusions**
	- **Notifications** 
- **Virus and threat protection updates**
	- Check for updates
- **Ransomware protection**
	- **Controlled folder access**


#### Firewall and network protection

A software that checks for the incoming and outgoing network traffic to a system.
We have 3 network profiles:
- Domain (works in AD and Domain controllers)
- Private (In our home and personal networks)
- Public (In public networks like Wifi and coffee shops)

In each profile we have 2 options:
- Turn it on/off
- Block all incoming connections

**Allow and APP through firewall:**
We can allow/disallow an app to pass through firewall. It basically have 2 options. i.e Public and Domain.

i also did an adventure using it in THM VM. In `Allow an app through firewall` i disallowed the `public` and `private` connections through firewall, the VM stucks and never responded again so I terminated it and re-deployed it again 😂😂😂.

we can also configure `Windows Defender Firewall` to create and import rules.
Command to open it: `WF.msc`


#### App and browser control

Windows Smart Screen protects against downloading of malware and phishing.
 It checks for files and applications from the browser.
 We can set the `App and Browser Control` to following states:
 - Block
 - Warn
 - Off

We also have `Exploit Protection`
In this section we have advanced configs like `DEP`.


#### Device Security

In device security we have, `Core Isolation` that keep `memory integrity` to prevent any malicious code to be injected into high-security processes.

It also contains `Security Processor` aka `TPM (Trusted Platform Module)`

Per Microsoft, "_Trusted Platform Module (TPM) technology is designed to provide hardware-based, security-related functions. A TPM chip is a secure crypto-processor that is designed to carry out cryptographic operations. The chip includes multiple physical security mechanisms to make it tamper-resistant, and malicious software is unable to tamper with the security functions of the TPM"._


#### Bitlocker

It is a disk encryption feature help to protect data in case of device stolen.
BitLocker provides best protection with TPM 1.2 or later.

When `TPM` is not installed on a device, saving up the `startup key` on a removable device is mandatory before enabling `BitLocker`.


#### Volume Shadow Copy Service

This service helps in actions creating and managing shadow copy of the data to be backup. The shadow copy is also known as:
- Snapshot
- Point-in-time copy

If VSS is enabled we can perform the following actions on the disk if protection is enabled on that disk:
- Create a restore point
- Perform system restore
- Configure restore settings
- Delete restore points

Malware writers also target these backups to disable you to revert to last backup unless you have offline or offsite backup.

**“Living off the land” (often shortened to LOTL)** has two main meanings depending on context:

#### 1. General meaning (everyday use)

It means **surviving using natural resources from the environment** instead of relying on modern systems.

Examples:

- Hunting, fishing, or farming your own food
- Using wood for shelter or fire
- Collecting water from natural sources

It’s commonly associated with **self-sufficiency, survival skills, or off-grid living**.

#### 2. Cybersecurity meaning (very important)

In cybersecurity, **“living off the land” refers to attackers using tools that already exist on a system instead of installing malware.**

These are called:

- **LOLBins (Living-Off-the-Land Binaries)**

Examples in **Microsoft Windows**:

- `PowerShell`
- `cmd.exe`
- `wmic`
- `schtasks`
- `net.exe`
#### Why attackers use LOTL

- Harder to detect (no new malware files)
- Blends in with normal system activity
- Bypasses traditional antivirus tools
#### Simple example

Instead of installing hacking software, an attacker might:

- Use `PowerShell` to download data
- Use `schtasks` to schedule persistence
- Use built-in tools to move laterally across systems

Everything looks like normal admin activity.


#### Credential Gaurd

**Credential Guard** is a security feature in **Microsoft Windows** designed to **protect user credentials from theft**, especially against advanced attacks like credential dumping.

**What Credential Guard does**

Credential Guard uses virtualization-based security to isolate sensitive data such as:

- NTLM password hashes
- Kerberos tickets
- Stored credentials

These secrets are stored in a protected environment that normal processes (even with admin rights) **cannot access directly**.


#### Windows hello

Windows Hello is a built-in authentication system in Microsoft Windows that lets you sign in without a traditional password, using biometrics or a PIN.

What Windows Hello is:
It’s a secure, passwordless sign-in method that uses:
- Facial recognition
- Fingerprint scanning
- PIN code


#### CSO Online
The new features of Windows OS blog post, give it a read: https://www.csoonline.com/article/564531/the-best-new-windows-10-security-features.html


**Room Completed:**

![[Pasted image 20260320170411.png]]



---



## Active Directory Basics

#### Domain
The **Windows Domain** is a group of users and computers under the administration of a given business.

#### Active Directory
In windows domain environment, all components of a windows computer network are stored in a single repository called Active Directory.

#### Domain Controller
The server that runs the Active Directory services is known as Domain Controller.

#### Active Directory Domain Service (AD DS)
This service acts as a catalogue that contains all information about the **objects**. The objects are the components in the AD DS.
The objects include:

- **Users (Security Principals)**: An object in AD, also known as the security principles which means that they can be authenticated by the domain and assigned privilege over resources. There are two types of **users** in AD DS.
	- People: Normal users in the network.
	- Services: Service as a user. They have privileges only to perform their tasks.

- **Machines:** Machines are also objects in the AD, and also considered as **Security Principles** because they are also assigned an account and authenticated when connected to the network but they have very limited access. Machine accounts are not accessed by any other accounts (Local Administrators) whereas other accounts use can use them if you have password. Machines account passwords are rotated over some period of time and of 120 characters long.  Machine account name is Machine name followed by a `$` dollar sign.

- **Security Groups:** Also known as security principles as groups can contain users, machines and other groups also to give access rights to several objects over resources in the network.

#### Active Directory Users and Computers
We can configure users, machines and groups from the **Active Directory Users and Computers** from the start menu.
The users and computers in here are organized as **Organizational Units (OU)**. These are sets with similar defined security requirements.

We can manage user and other things from here like resetting user password (I remember once I went to IT office for my friend's account password reset.)

always remember to differentiate in **OUs** and **Security groups**. As OUs are used to configure policies according to departmental roles and one user can be a part of one OU at a time. Security groups are used to give access to resources over the network like printer or a shared folder. A user can be a part of multiple groups.

---


## Managing users in the Active Directory

Learned how to add and delete OUs and users in the domain.
Manged the AD environment to the following structure:

![[Pasted image 20260411130508.png]]


![[Pasted image 20260411131006.png]]


#### Delegation:
Hehe: doing this module on morning of 11 April when delegations from Iran and America are reaching Islamabad for Ceasefire.

In AD we can use delegation to give control of a OU to a specific user to do specific configurations like resetting passwords. As it allows like `IT Department` to manage users of all departments rather than Domain admins.

In lab I also delegated control to `IT` of all departments.

Now resetting password of `sophie` using `phillip`. First RDP into `phillip`. 

Reset password of any user:

```powershell
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

Ask the user to reset password on next logon:

```powershell
Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
```

Now RDP into `sophie` and change the password as it will prompt you.
After login the `flag` is on the desktop.


---


## Managing Computers in AD:

When a new machine is connected in the AD it is transferred in to the `Computers` container and generally their type is written along with the username. Such as computers, laptops or servers.

At least you will see the following types of machines in the list:
- Workstation (must be a non privileged user)
- Server
- Domain Controllers: The most sensitive device in the network as it contains hashed passwords of all the users in the AD domain.

In the lab, made separate OU for servers and Workstations.


---


#### Group policies
we make OUs to apply group policies to different departments. For that we use GPO (Group Policy Objects) as they are the collection of configs that are applied to the OUs at once.

To configure GPOs we will use Group Policy Management Tool from the start menu.

First create the policy in the GPO container and then link in to the respective OU. As a GPO is applied to an OU the OU itself and OUs under it will be affected.

In the lab I examined the default domain policy and saw what inside it!

**Scope:** It tell where is the GPO applied in the Domain. We can also use the `Security Filtering` Option to apply the GPO to specific group of users and machines.

**Settings:** The actual security policy rules that are applied. `Default Domain Policy` only contains computer configurations.

> Note: Each GPO has Configurations related to `Users` and `Computers`

In the lab, I changed the GPO for `Computers` and changed the `minimum password length` from `7` to `10`.

#### GPO Distribution
GPOs are distributed by `SYSVOL` share on the network so all users must have access to this share for GPO fetch.
After changing the GPO it might take 2 hours to take effect but if want to make immediate affect, use the following command on that host:

```powershell
gpupdate /force
```


#### Creating GPOs for THM
As part of the lab, I have applied the following policies in to the domain.

- Restrict non IT users form accessing the `Control Panel`.
- After 5 minutes of inactivity, Lock the screen of the users.

Make a new GPO as `Restrict Control Panel Access`, click on `edit` to edit the `User configurations` and then go to `Policies -> Administrative Templates -> Control Panel -> Prohibit Access to Control Panel and PC Settings`

Now click on `edit` and `enable` the policy.

Now drag and drop the GPO to OUs I applied:

![[Pasted image 20260412105943.png]]


Now,
2. **Auto Lock screen GPO for computers**: We can apply them to the root as sub users will not inherit the GPO as it contains the configs for `Computers` only and not for `Users` . Make a new GPO as `Auto Lock Screen` and edit and go to the following path: `Computer configuration -> Policies -> Windows Settings -> Security settings -> Local Policies -> Security options -> Interactive Logon: Machine Inactivity Limit` and set it to `300 seconds` (5 minutes).  Now close the editor and drag the GPO to the `thm.local`. 

Now I have RDP in to the Marketing manager account `Mark`. Lets verify the GPOs applied.
As I am just checking for the `Control Panel Access` policy. Just opened the `Control Panel` to see what happens. 

AND!!! BOOOOOOOOOOOM!!! this error flies straight in to my eyes as one sometimes in the Uni Lab when i open the `Powershell` or `Control Panel`.

![[Pasted image 20260412111747.png]]


> Always remembers the `SYSVOL` and 

 ```powershell
 gpupdate /force
 ```


---


## Authentication Methods

We have discussed two authentication methods in the lab:
- Kerberos
- NetNTLM

For now I will only discuss NetNTLM later I will add the Kerberos.

#### NetNTLM
NetNTLM is a legacy authentication system used still to maintain  compatibility.

**How it works:**
1. The client sends an `authentication request` to the server.
2. The server responds with a random number called `challenge`.
3. The server calculates a `Response` using the `NTLM Hash` (User's hashed password) and `Challenge` from the server.
4. The client sends the Response to the server and server sends it to the DC along with challenge for verfication.
5. On the DC, as it also contains the NTLM Hash so it recalculates the response using the NTLM Hash and challenge and compares with the response it received from the server.
6. According to the calculation the Authentication Status (YAY/NAY) is sent back to the client.

![[Pasted image 20260412134945.png]]


#### Trees

we can have sub domains of a domain. For example: `uk.thm.local` and `us.thm.local` of `thm.local`. We can now enforce policies independently and manage both domains separately. Each domain has its domain admin but a new Security Groups is made named `Enterprise Admins` this includes the persons who can manage all the domains and users in them.

![[Pasted image 20260412140556.png]]


#### Forest

The union of several trees with different namespaces into the same network is called Forest.

![[Pasted image 20260412140817.png]]


#### Trust Relationships

The domains in trees and forests are joined together by `trust relationships`.
Means we can allow specific users of one domain to access resources in another domain.

**Types of trust relationships**
- **One way trust relationship:** If there are two domains `AAA` and `BBB` and `AAA` trusts `BBB` so the user authenticated on `BBB` can access the resources on `AAA`. The direction of `Trust` and `Resource` is always contrary in `One way trust relationship`
- **Two way trust relationship:** We can configure the both domains to trust each other and users can access resources on both ends but after mutual trust we can also customize all the configs to manage them according to our needs.

![[Pasted image 20260412141713.png]]


This was indeed the most time taking room in my whole Try Hack Me journey as studying AD for the first time hehe!

#### Room completed:

![[Pasted image 20260412141947.png]]


---



## Command Line


## Windows Command Line

#### 1. Introduction

CLI (Command Line Interface), we can use to manage and control the system by using commands instead of moving the mouse and graphics.

**Advantages of CLI:**
1. Efficient resource management
2. Automation
3. Remote systems management


The default command-line interpreter of the windows environment is `cmd.exe`


#### 2. Basic System Information

The `set` command list specifically the `PATH=` variable that contains all the directories that when a command is executed in the `cmd` the system checks for the command if present in the `PATH=` directories. Otherwise gives an error.

```powershell
set
```

Check for the Operating System (OS) version:

```powershell
ver
```

Check system information like OS, memory, name and manufacturer etc...

```powershell
systeminfo
```

> the command `systeminfo` is in the `Windows/system32/systeminfo.exe` directory which is also a part of the `PATH=`, which we can verify using the `set` command.

check all the installed device drivers on the device.

```powershell
driverquery
```

if you want more detailed information about device drivers use:

```powershell
#verbose

driverquery /v
```

and If you want to read the information page by page you can pipe it using `more` as:

```powershell
driverquery | more
```

Use help for any command:

```powershell
help
```

Clear the screen:

```powershell
cls
```



#### Network Troubleshooting

To check the `IP Address`, `Subnet Mask` and `Default Gateway`.

```powershell
ipconfig
```

to view all info including `DNS`,  and `DHCP` along with `MAC Address` we can use:

```powershell
ipconfig /all
```

To check if we can reach a specific target on the network (also internet).

```powershell
ping <IP_ADDRESS>
```

**How does `ping` work:** When a `ping` command is executed, the HOST sends the `ICMP_REQUEST` and upon reaching the SINK and the SINK replies with the `ICMP RESPONSE`. The information displayed on the output is:
- **BYTES:** size of the packet
- **time:** basically the RTT(Round Trip Time)
- **TTL(Time To Live):** The number of hops a packet travels before reaching the destination. Different systems use different TTL, e.g Windows use 128 and Linux use 64. We can estimate the number of hops in between source and destination.

To check the route of the network, that from which hops do our packet travels, we use the command:

```powershell
tracert <HOST_IP>      # WINDOWS
traceroute <HOST_IP>   # LINUX
```

To drive the `IP ADDRESS` from the terminal mostly in Recon, use:

```powershell
nslookup <IP/DOMAIN>
```

To check the active network connections and listening ports we use:

```powershell
netstat
```

for help use:

```powershell
netstat -h
```

- `-a` displays all established connections and listening ports
- `-b` shows the program associated with each listening port and established connection
- `-o` reveals the process ID (PID) associated with the connection
- `-n` uses a numerical form for addresses and port numbers


#### File and Disk Management

To view the current working directory like in linux we use `pwd`, in windows we use:

```powershell
cd
```

to list all the contents in the folder we use:

```powershell
dir
```

to view hidden and system files:

```powershell
dir /a
```

to view all contents of the sub-directories in the directory:

```powershell
dir /s
```

To change the directory to `FOLDER` use:

```powershell
cd FOLDER
```

to go back or one level up;

```powershell
cd ..
```

to make a directory name `FOLDER` use:

```powershell
mkdir FOLDER
```

to delete or remove the directory `FOLDER` use:

```powershell
rmdir FOLDER
```

we can use the contents of the file by using:

```powershell
type FILE
```

for reading a file page by page using piping with `more`

```powershell
type FILE | more
```

to copy files:

```powershell
copy FILE DESTINATION
```

we can move the files also

```powershell
move FILE DESTINATION
```

we can delete a file using `del` or `erase`

```powershell
erase FILE
```

we can also use the wildcard character `*`

#### Task and Process Management

To view all the tasks/processes on the system

```powershell
tasklist
```

we can terminate a process using:

```powershell
taskkill /PID <PID>
```


**Room Completed:**

![[Pasted image 20260413135209.png]]


---


## Windows Powershell

`PowerShell` is a task automation and configuration management tool from Microsoft, consisting of a shell and the associated language.

The version 6.0 and later are cross platform as, it is built using .NET core. Earlier were built using .NET framework which was natively only for windows and now not actively developed.

It is Object oriented as it can handle many complex data types.

An **object** is an item with **properties** and **methods**, properties of object `Car` like `Color`, `Model` and methods like `Horn()`, `Drive()` etc.

`cmd` commands are simple text based. But when we run commands in PowerShell they are known as `cmdlets` (verb-noun) and they return an object on the output.
For example:
	- Get-Content
	- Set-Location
> PowerShell is CaSe iNsEnSiTiVe

For SSH and RDP connections from the `AttackBox` we use `Remmina`.

#### Basic usage:

check the commands (cmdlets, functions and Aliases) we can use in the PowerShell session:

```powershell
Get-Command
```

we can also use filters like:

```powershell
Get-Command -CommandType "Function"
```

If you want help on a specific word or `cmdlet` use:

```powershell
Get-Help <NAME>
```

for example:

```powershell
Get-Help Get-Date
```

Alternative names for `cmdlets` just explore it:

```powershell
Get-Alias
```

> Collection of `cmdlets` is known as `Modules`

We can also get many `cmdlets` downloaded from the internet to extend the functionalities of powershell by finding it in the repo by its name or patial name if we don't know:

```powershell
Find-Module -Name "Powershell*"
```

It will list all the modules starting with the name `Powershell...`

#### Working with files:

List the contents of a directory or a specific directory:

```powershell
Get-ChildItem
```

```powershell
Get-ChildItem -Path "<PATH>"
```

to change the the current directory to a `PATH`

```powershell
Set-Location <PATH>
```

To make make a new file or directory in a single command, unlike cmd or unix we use:

```powershell
New-Item -Path "<PATH>" -ItemType "Directory/File"
```

Also for removing we use a unified command:

```powershell
Remove-Item -Path <PATH>
```

to copy items:

```powershell
Copy-Item -Path <PATH> -Destination <DESTINATION>
```

to move items:

```powershell
Move-Item -Path <PATH> -Destination <DESTINATION>
```

To view the content of file like `cat` in linux:

```powershell
Get-Content -Path <PATH>
```

#### Piping, Filtering and Sorting

We use piping `|` to pass the output of one command to the input of another command. In PowerShell piping shares the Objects rather than text.

For example:
List all the files in the current directory and sort them by length

```powershell
Get-ChildItem | Sort-Object Length
```

To filter the Contents with just .txt files

```powershell
Get-ChildItem | Where-object -Property "Extension" -eq ".txt"
```

Comparison Operators:
- eq
- gt
- lt
- ge
- le
- ne

- like :

```powershell
Get-ChildItem | Where-object -Property "Name" -like "ship*"
```

Select specific properties of objects:

```powershell
Get-ChildItem | Select-Object Name,Length
```

The `GREP` of Power Shell:

```powershell
Select-String -Path "<PATH>" -Pattrn "PATTERN"
```

#### System and Network Information

`cmdlet` same as `systeminfo` in `cmd`

```powershell
Get-ComputerInfo
```

to view local users on the system:

```powershell
Get-LocalUser
```

to get detailed network configs:

```powershell
Get-NetIPConfiguration
```

to get IP address info of all interfaces:

```powershell
Get-NetIPAddress
```

#### Real Time System Analysis

To view all the processes running on the system:

```powershell
Get-Process
```

to view all the services:

```powershell
Get-Service
```

to view all the current TCP connections established:

```powershell
Get-NetTCPConnection
```

to calculate the hash of a file, use the command along with the `-Path` parameter.

```powershell
Get-FileHash -Path "<PATH>"
```

to view the ADS of a file:

```powershell
Get-Item -Path "<PATH>" -Stream *
```

`:$DATA` is the default data stream of every NTFS file.

#### Scripting

The process of writing a series of commands to automate a task or for configurations is known as scripting.

Scripting is very helpful for automating tasks in cyber security like enumeration for penetration testers and also helpful for red teamers.

The command used to run commands on the remote server:
it is used to execute scripts and payloads on the remote machines.

```powershell
Invoke-Command
```

**Room Completed:**

![[Pasted image 20260415150650.png]]


---


## Linux Shells

**Shell** is the facilitator between the **user** and the **OS**. **Bash** is the default shell in most of the linux distros.

Done most of the basic commands in the previous rooms so skipping now like `cd`, `pwd`, `ls`.

#### Types of linux shells

The file `/etc/shells` contain the available shells. We can view it by using `cat /etc/shells`

To switch the shell type we can just type the name of the shell present on the system and it will open.

I want to change the default shell permanently:

```bash
chsh -s /PATH_TO_SHELL
```

#### Bourne Again Shell (Bash):

It is the enhanced version of the old shells like `sh`, `ksh` and `csh`. It includes unique capabilities like `history` and `Tab Completion`.

#### Friendly Interactive Shell (Fish):

It also provides the capabilities of Bash but as its name suggests, provides user friendliness and customization with different fish themes and colors.

#### Z Shell (Zsh):

Zsh is not installed by default on must linux distros. It offers advanced tab completion, customization options.

#### Shell Scripting and components:

Scripting is nothing just a series of commands to perform a task.

**Steps in writing a Bash Script:**

Make a file with the extension `.sh`.

```bash
nano file_name.sh
```

Start with the `Shebang` line. They are characters added at the start of the script with the name of the interpreter followed by `#!`.

```bash
#!/bin/bash
```

```shell
# Defining the Interpreter 
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"
```

now give the execute permissions to the script:

```shell
sudo chmod +x filename.sh 
```

Now execute it using `./`. We tell the shell to execute the script in the current directory. Otherwise It will give an error.

> The `.` represents the current directory.

```shell
./filename.sh
```

In the scripts, we can also use the `loops`, `conditions` and `comments`.

#### THE LOCKER SCRIPT:

```shell
# Defining the Interpreter 
#!/bin/bash 

# Defining the variables
username=""
companyname=""
pin=""

# Defining the loop
for i in {1..3}; do
# Defining the conditional statements
        if [ "$i" -eq 1 ]; then
                echo "Enter your Username:"
                read username
        elif [ "$i" -eq 2 ]; then
                echo "Enter your Company name:"
                read companyname
        else
                echo "Enter your PIN:"
                read pin
        fi
done

# Checking if the user entered the correct details
if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
        echo "Authentication Successful. You can now access your locker, John."
else
        echo "Authentication Denied!!"
fi
```


**Room Completed:**

![[Pasted image 20260415185638.png]]


---



#### Networking concepts:

Not making notes as already in the **Pre Security** path I made notes of OSI model. I can later reference there if needed.

The TCP/IP model was also covered in the presecurity path.

Also did IP addresses, subnets and routing, encapsulation, TCP, UDP and telnet in presecurity path.


![[Pasted image 20260418103027.png]]



---



#### Networking Essentials

#### Ping:
- ICMP Echo Request (type 8)
- ICMP Echo Response (type 0)
- ICMP time exceed message (type 11)

for sending specific number of packets in UNIX based systems through ping:

```Bash
ping <IPADDRESS> -c <NUMBER_OF_PACKETS>
```

#### Traceroute:
A tool in networking used to discover the routers (hops) between our system and the destination.
On each router the router forwards the ICMP packet to our machine.

Command:

```bash
traceroute <IP>
```

for Windows it is:

```cmd
tracert <IP>
```


#### Routing
When there are multiple networks are connected together through multiple links so there must be an algorithm to define the best path for the packets to travel.

**Routing Alogrithms:**
- OSPF (Open Shortest Path First)
- RIP (Routing Information Protocol)
- EIGRP (Enhanced Interior Gateway Routing Protocol)
- BGP (Border Gateway Protocol)

#### Network Address Translation (NAT):
To control the depletion of IPv4 addresses, NAT was introduced.
**Simple concept:** Use local IP addresses behind just one public IP address to connect the devices to the internet.

The router maintains a NAT Table to map internal ip address and port number of a device to the public IP address assigned to the router with a specific port number for that device.

![[Pasted image 20260418202916.png]]



**Room Completed:**

![[Pasted image 20260418203208.png]]



---



#### Networking Core Protocols


**1. DNS (Domain Name System):** - Already done in the **PreSecurity** Path so just reading it properly again and not making any notes.

#### WHOIS:
To check the information about the domain and its registerer. Can be used in terminal and on website.

#### FTP: 
A protocol for file transfer to remote.
FTP Commands that are sent to the server for enumeration on the backend:
- USER
- PASS
- RETR
- STOR


#### SMTP:
Used to send mails.
It defines rules how to send mail to server and how server sends to another.

Like FTP it also works on some commands defined by the protocol:
- HELO: It initiates an SMTP connection.
- MAIL FROM: The sender's email address.
- RCPT TO: The recipient email address.
- DATA: The message to be send.
- `.`: A dot in the end by self indicates end of message.

On command line, to use SMTP we use TELNET.

#### POP3:
The protocol used to retrieve mails from the server to the local client.

It also used some commands to perform operations:
- USER
- PASS
- STAT
- LIST
- RETR <message_number>
- DELE <message_number>
- QUIT

The POP3 server by default runs at port 110.

#### IMAP (Internet Message Access Protocol): 
A protocol used for email synchronization across multiple devices. It also uses some commands as directed by the IMAP protocol.

	LOGIN <username> <password>
	SELECT <mailbox>
	FETCH <mail_number> <data_item_name>
	MOVE <sequence_set> <mailbox>
	COPY <sequence_set> <data_item_name>
	LOGOUT

by default IMAP runs on port 143.

**Room Completed:**

![[Pasted image 20260419133419.png]]



---



#### Networking Secure Protocols:

#### TLS (Transport Layer Security):
Built on the top of SSL (Secure Socket Layer) to provide security to Application layer running on transport layer. Protocols like HTTP, SMTP, POP3, IMAP becomes HTTPS, SMTPS, POP3S, IMAPS. `S` means `Secure`. 

#### HTTPS:
For a standard HTTP protocol we communicate in the following order.
- Establish 3 way TCP handshake with the server.
- Communicate using the HTTP protocol.

but in using TLS over HTTP its a  bit like this:
- Establish 3 way TCP handshake
- Establish TLS session
- Communicate using the HTTP protocol

Firstly the 3 way tcp handshake is done then TLS session is established and then request and response from server over HTTP are transferred but in packets we see them as TLS packets in which application data is travelling over port 443. In general we don't know what is travelling inside it but due to port we can estimate that the HTTP is being used.

![[Pasted image 20260419230126.png]]


The contents of the packets are just like this and we can't see it without the encryption key.

![[Pasted image 20260419230244.png]]


To see the contents of the encrypted traffic we can give Encryption Key to wireshark and then we will be able to see the traffic.

![[Pasted image 20260419230939.png]]


#### SMTPS, POP3S, IMAPS:
All point from HTTPS also apply to these protocols. The port number changes as follows:

![[Pasted image 20260419231308.png]]


#### SSH:
This protocol is used to provide secure communication w.r.t TELNET. Today almost every SSH client uses Open-SSH libraries and source code for SSH.

SSH provides following functions:
- Secure Authentication
- Confidentiality
- Integrity
- Tunneling
- x11 Fowarding : -X (to use the Graphical User Interface of the remote machine on local machine.)

to use it:

```shell
ssh username@hostname
```

and if to use on the same system and same user is loggedin:

```shell
ssh hostname
```

for example to run wireshark on the remote system:

```shell
ssh username@hostname -X
```

after authentication:

```shell
wireshark
```

The local system must have any GUI installed.

#### SFTP and FTPS:

The **SFTP (SSH File Transfer Protocol)** is used to transfer files over the SSH connection and yes the same port 22.
Command:

```shell
sftp username@hostname
```

once logged in we can transfer files using `get filename` and `put filename` on the server. Its commands are most likely to SSH.

While, FTPS means File Transfer Protocol Secure and yeah the TLS is used in it. It requires special certificate setup. Its commands are of FTP.

