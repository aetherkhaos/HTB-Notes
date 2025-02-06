# Shieldwall2
- background
	- **We have managed to confirm the location of the malicious actor who compromised our Government WiFi network.**

	- Recently a dawn raid as part of OP ERADICATE was conducted on an address within the Velorian capital and a sizeable amount of evidence was seized. Of particular note is an Android device belonging to the Agent, believed to have been with him at the location of the attack.

	- We require your expertise to analyse this device and answer the questions detailed below. You don't have much time, a Velorian COBR meeting has been arranged to discuss your findings...
- Analysis
	- download and extract the evidence
		- password: hacktheblue
	- Locate the agents email that was used in various apps/services
		- `grep -Eiorh '([[:alnum:]_.-]+@[[:alnum:]_.-]+?\.[[:alpha:].]{2,6})' "$@" * | sort`
		- olegpachinksy@gmail.com
	- locate the contact number for the handler assigned to the agent who was arrested
		- 


# Shieldwall3

- Background
	- The Velorian government, with the support of GASC, has heavily invested in its offensive cybersecurity capabilities, often considering offensive operations as a way to supplement its wider cybersecurity defensive strategy. You have tasked the department with providing offensive operational support and compromising the C2 servers identified in our department's previous analysis. Please compromise the Ravenskian government's command and control infrastructure and retrieve as much information as possible about the threats affecting our great nation.
	- **You may have to gain access to the host OS with the highest possible privileges to retrieve the required intelligence. Good luck.**
### Enumeration
- Nmap
	- sudo nmap 10.129.231.43
```bash
└─$ sudo nmap 10.129.231.43
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-24 12:36 EST
Nmap scan report for 10.129.231.43
Host is up (0.045s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 0.99 seconds

```
###  Analysis of results
- SSH and HTTP are open
		1. check the website source code and 
		2. conduct a directory scan to locate potential entry points

### Checking the website
1. main page analysis
	1. nothing of interest in the source code for the page
2. conduct dir scan on the site
	1. `gobuster dir -u http://10.129.231.43/ -w /usr/share/dirb/wordlists/common.txt`