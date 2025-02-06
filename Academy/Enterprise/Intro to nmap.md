-  Use Cases:
	- Security Auditing
	- Simulate Pentests
	- Check Firewall and IDS settings and config
	- Types of possible connections
	- network mapping
	- response analysis
	- identify open ports
	- Vulnerability Assessments
### Syntax:
```
nmap <scan types> <options> <target>
```
### Scan Techniques
```
$ nmap --help  

<SNIP>

SCAN TECHNIQUES: 
  -sS/sT/sA/sW/sM:                 TCP SYN/Connect()/ACK/Window/Maimon scans   
  -sU:                             UDP Scan   
  -sN/sF/sX:                       TCP Null, FIN, and Xmas scans   
  --scanflags <flags>:             Customize TCP scan flags   
  -sI <zombie host[:probeport]>:   Idle scan   
  -sY/sZ:                          SCTP INIT/COOKIE-ECHO scans   
  -sO:                             IP protocol scan   
  -b <FTP relay host>:             FTP bounce scan

<SNIP>
```
- TCP-SYN Scan
	- used by default
	- most popular
	- able to scan several thousand ports/sec
	- send one packet with the SYN flag and never completes the handshake
		- If our target sends a `SYN-ACK` flagged packet back to us, Nmap detects that the port is `open`.
		- If the target responds with an `RST` flagged packet, it is an indicator that the port is `closed`.
		- If Nmap does not receive a packet back, it will display it as `filtered`. Depending on the firewall configuration, certain packets may be dropped or ignored by the firewall.

### Host Discovery
- Methods
	- most effective method is to use ICMP echo requests
	- important that we store every scan!
		- can be used for compairison, documentation, and reporting
- example:
```bash

$ sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28
```

| Scanning Option | Description                                                     |
| --------------- | --------------------------------------------------------------- |
| `10.129.2.0/24`   | target network range                                            |
| `-sn`             | disables port scanning                                          |
| `-oA tnet`        | stores the results in all formats starting with the name 'tnet' |
- Scan IP List
	- more often than not, we will be given a list of hosts that need to be tested.
- Example:
```bash
$ cat hosts.lst

10.129.2.4
10.129.2.10
10.129.2.11
10.129.2.18
10.129.2.19
10.129.2.20
10.129.2.28

& sudo nmap -sn -oA tnet -iL hosts.lst

10.129.2.18
10.129.2.19
10.129.2.20
```
- Scanning multiple IPs
	- can be used if you only need to scan a small pert of a network. also, if the ip addresses are next to each other, you can define the range by the respective octet
```bash
$ sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5

10.129.2.18
10.129.2.19
10.129.2.20

$ sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5

10.129.2.18
10.129.2.19
10.129.2.20
```

# Host and Port Scanning
- After we have found out that our target is alive, we want to get a more accurate picture of the system. The information we need includes:
	- Open ports and its services
	- Service versions
	- Information that the services provided
	- Operating system
- there are six diferent states that could be obtained:

| state            | description                                                                                                                                                                                             |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| open             | This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**.                          |
| closed           | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not. |
| filtered         | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.                  |
| unfiltered       | This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed.                                           |
| open\|filtered   | If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port.                                                |
| closed\|filtered | This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.                                           |
### Discovering Open TCP Ports
- By default, `Nmap` scans the top 1000 TCP ports with the SYN scan (`-sS`). This SYN scan is set only to default when we run it as root because of the socket permissions required to create raw TCP packets. Otherwise, the TCP scan (`-sT`) is performed by default. This means that if we do not define ports and scanning methods, these parameters are set automatically. We can define the ports one by one (`-p 22,25,80,139,445`), by range (`-p 22-445`), by top ports (`--top-ports=10`) from the `Nmap` database that have been signed as most frequent, by scanning all ports (`-p-`) but also by defining a fast port scan, which contains top 100 ports (`-F`)

![[NMAP.jpg]]