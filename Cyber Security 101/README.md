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


