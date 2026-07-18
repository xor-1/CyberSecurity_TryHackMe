**Cybersecurity**


**├── Pre-Security**

**├── Cyber Security 101**

**├── Jr. Penetration Tester**



My Pentesting Methodology:

Passive Reconn:
1. `nslookup` and `dig`
2. `whois` and `rdap` (curl -s "https://rdap.verisign.com/com/v1/domain/tryhackme.com" | jq) 
3. `crt.sh`
4. `dnsdumpster`
5. `shodan`

Active Reconn:
1. Developer tools
2. `ping`
3. `traceroute`
4. `telnet` (can also use `nmap` for banner grabbing)
5. `curl` for response headers, codes and view in terminal
6. `netcat` to listen on the ports of our choice (client and server both)
7. `ncat` by `nmap` for ssl based connections
8. `nmap`


