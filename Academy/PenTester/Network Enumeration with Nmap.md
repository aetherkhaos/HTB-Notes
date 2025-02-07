Nmap is used to identify and scan systems on the network. It is an important part of network diagnostics and evaluation of network-connected systems. In this module, we will learn the basics of this tool and how it can be used efficiently to map out the internal network by identifying live hosts and performing port scanning, service enumeration, and operating system detection.

In this module, we will cover:

- An overview of Nmap
- Host discovery and port scanning
- Saving scan results
- Service enumeration
- Using the powerful Nmap scripting language
- Firewall and IDS/IPS evasion

`CREST CPSA/CRT`-related Sections:

- All sections

`CREST CCT APP`-related Sections:

- All sections

`CREST CCT INF`-related Sections:

- All sections

This module is broken down into sections with accompanying hands-on exercises to practice each of the tactics and techniques we cover. The module ends with three hands-on labs of increasing difficulty to gauge your understanding of the various topic areas.

As you work through the module, you will see example commands and command output for the various topics introduced. It is worth reproducing as many of these examples as possible to reinforce further the concepts introduced in each section. You can do this in the Pwnbox provided in the interactive sections or your own virtual machine.

You can start and stop the module at any time and pick up where you left off. There is no time limit or "grading," but you must complete all of the exercises and the labs to receive the maximum number of cubes and have this module marked as complete in any paths you have chosen.

The module is classified as "Easy" but assumes a working knowledge of the Linux command line and an understanding of information security fundamentals.

A firm grasp of the following modules can be considered prerequisites for successful completion of this module:

- Introduction to Networking
- Linux Fundamentals

![[Pasted image 20250202175730.png]]

# Enumeration

`Enumeration` is the most critical part of all. The art, the difficulty, and the goal are not to gain access to our target computer. Instead, it is identifying all of the ways we could attack a target we must find.

It is not just based on the tools we use. They will only do much good if we know what to do with the information we get from them. The tools are just tools, and tools alone should never replace our knowledge and our attention to detail. Here it is much more about actively interacting with the individual services to see what information they provide us and what possibilities they offer us.

It is essential to understand how these services work and what syntax they use for effective communication and interaction with the different services.

This phase aims to improve our knowledge and understanding of the technologies, protocols, and how they work and learn to deal with new information and adapt to our already acquired knowledge. Enumeration is collecting as much information as possible. The more information we have, the easier it will be for us to find vectors of attack.

Imagine the following situation:

Our partner is not at home and has misplaced our car keys. We call our partner and ask where the keys are. If we get an answer like "in the living room," it is entirely unclear and can take much time to find them there. However, what if our partner tells us something like "in the living room on the white shelf, next to the TV, in the third drawer"? As a result, it will be much easier to find them.

It's not hard to get access to the target system once we know how to do it. Most of the ways we can get access we can narrow down to the following two points:

- `Functions and/or resources that allow us to interact with the target and/or provide additional information.`
    
- `Information that provides us with even more important information to access our target.`
    

When scanning and inspecting, we look exactly for these two possibilities. Most of the information we get comes from misconfigurations or neglect of security for the respective services. Misconfigurations are either the result of ignorance or a wrong security mindset. For example, if the administrator only relies on the firewall, Group Policy Objects (GPOs), and continuous updates, it is often not enough to secure the network.

`Enumeration is the key`.

That's what most people say, and they are right. However, it is too often misunderstood. Most people understand that they haven't tried all the tools to get the information they need. Most of the time, however, it's not the tools we haven't tried, but rather the fact that we don't know how to interact with the service and what's relevant.

That's precisely the reason why so many people stay stuck in one spot and don't get ahead. Had these people invested a couple of hours learning more about the service, how it works, and what it is meant for, they would save a few hours or even days from reaching their goal and get access to the system.

`Manual enumeration` is a `critical` component. Many scanning tools simplify and accelerate the process. However, these cannot always bypass the security measures of the services. The easiest way to illustrate this is to use the following example:

Most scanning tools have a timeout set until they receive a response from the service. If this tool does not respond within a specific time, this service/port will be marked as closed, filtered, or unknown. In the last two cases, we will still be able to work with it. However, if a port is marked as closed and Nmap doesn't show it to us, we will be in a bad situation. This service/port may provide us with the opportunity to find a way to access the system. Therefore, this result can take much unnecessary time until we find it.

# Introduction to Nmap

Network Mapper (`Nmap`) is an open-source network analysis and security auditing tool written in C, C++, Python, and Lua. It is designed to scan networks and identify which hosts are available on the network using raw packets, and services and applications, including the name and version, where possible. It can also identify the operating systems and versions of these hosts. Besides other features, Nmap also offers scanning capabilities that can determine if packet filters, firewalls, or intrusion detection systems (IDS) are configured as needed.

## Use Cases

The tool is one of the most used tools by network administrators and IT security specialists. It is used to:

- Audit the security aspects of networks
- Simulate penetration tests
- Check firewall and IDS settings and configurations
- Types of possible connections
- Network mapping
- Response analysis
- Identify open ports
- Vulnerability assessment as well.

## Nmap Architecture

Nmap offers many different types of scans that can be used to obtain various results about our targets. Basically, Nmap can be divided into the following scanning techniques:

- Host discovery
- Port scanning
- Service enumeration and detection
- OS detection
- Scriptable interaction with the target service (Nmap Scripting Engine)

## Syntax

The syntax for Nmap is fairly simple and looks like this:

```
crimsonguard@htb[/htb]`$ nmap <scan types> <options> <target>
```

## Scan Techniques

Nmap offers many different scanning techniques, making different types of connections and using differently structured packets to send. Here we can see all the scanning techniques Nmap offers:


```
crimsonguard@htb[/htb]`$ nmap --help  

<SNIP> SCAN TECHNIQUES:   
-sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans   
-sU: UDP Scan   
-sN/sF/sX: TCP Null, FIN, and Xmas scans   
--scanflags <flags>: Customize TCP scan flags
-sI <zombie host[:probeport]>: Idle scan   
-sY/sZ: SCTP INIT/COOKIE-ECHO scans
-sO: IP protocol scan
-b <FTP relay host>: FTP bounce scan 
<SNIP>`
```

For example, the TCP-SYN scan (`-sS`) is one of the default settings unless we have defined otherwise and is also one of the most popular scan methods. This scan method makes it possible to scan several thousand ports per second. The TCP-SYN scan sends one packet with the SYN flag and, therefore, never completes the three-way handshake, which results in not establishing a full TCP connection to the scanned port.

- If our target sends a `SYN-ACK` flagged packet back to us, Nmap detects that the port is `open`.
- If the target responds with an `RST` flagged packet, it is an indicator that the port is `closed`.
- If Nmap does not receive a packet back, it will display it as `filtered`. Depending on the firewall configuration, certain packets may be dropped or ignored by the firewall.

Let us take an example of such a scan.

```
crimsonguard@htb[/htb]`$ sudo nmap -sS localhost

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-11 22:50 UTC 
Nmap scan report for localhost (127.0.0.1) 
Host is up (0.000010s latency). 
Not shown: 996 closed ports 
PORT     STATE SERVICE 
22/tcp   open  ssh 
80/tcp   open  http 
5432/tcp open  postgresql 
5901/tcp open  vnc-1  

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```

In this example, we can see that we have four different TCP ports open. In the first column, we see the number of the port. Then, in the second column, we see the service's status and then what kind of service it is.

# Host Discovery

When we need to conduct an internal penetration test for the entire network of a company, for example, then we should, first of all, get an overview of which systems are online that we can work with. To actively discover such systems on the network, we can use various `Nmap` host discovery options. There are many options `Nmap` provides to determine whether our target is alive or not. The most effective host discovery method is to use **ICMP echo requests**, which we will look into.

It is always recommended to store every single scan. This can later be used for comparison, documentation, and reporting. After all, different tools may produce different results. Therefore it can be beneficial to distinguish which tool produces which results.

#### Scan Network Range

crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5  10.129.2.4 10.129.2.10 10.129.2.11 10.129.2.18 10.129.2.19 10.129.2.20 10.129.2.28`

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.0/24`|Target network range.|
|`-sn`|Disables port scanning.|
|`-oA tnet`|Stores the results in all formats starting with the name 'tnet'.|

This scanning method works only if the firewalls of the hosts allow it. Otherwise, we can use other scanning techniques to find out if the hosts are active or not. We will take a closer look at these techniques in "`Firewall and IDS Evasion`".

## Scan IP List

During an internal penetration test, it is not uncommon for us to be provided with an IP list with the hosts we need to test. `Nmap` also gives us the option of working with lists and reading the hosts from this list instead of manually defining or typing them in.

Such a list could look something like this:

crimsonguard@htb[/htb]`$ cat hosts.lst  10.129.2.4 10.129.2.10 10.129.2.11 10.129.2.18 10.129.2.19 10.129.2.20 10.129.2.28`

If we use the same scanning technique on the predefined list, the command will look like this:

crimsonguard@htb[/htb]`$ sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5  10.129.2.18 10.129.2.19 10.129.2.20`

|**Scanning Options**|**Description**|
|---|---|
|`-sn`|Disables port scanning.|
|`-oA tnet`|Stores the results in all formats starting with the name 'tnet'.|
|`-iL`|Performs defined scans against targets in provided 'hosts.lst' list.|

In this example, we see that only 3 of 7 hosts are active. Remember, this may mean that the other hosts ignore the default **ICMP echo requests** because of their firewall configurations. Since `Nmap` does not receive a response, it marks those hosts as inactive.

## Scan Multiple IPs

It can also happen that we only need to scan a small part of a network. An alternative to the method we used last time is to specify multiple IP addresses.

crimsonguard@htb[/htb]`$ sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5  10.129.2.18 10.129.2.19 10.129.2.20`

If these IP addresses are next to each other, we can also define the range in the respective octet.

crimsonguard@htb[/htb]`$ sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5  10.129.2.18 10.129.2.19 10.129.2.20`

## Scan Single IP

Before we scan a single host for open ports and its services, we first have to determine if it is alive or not. For this, we can use the same method as before.

crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.18 -sn -oA host   Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-14 23:59 CEST Nmap scan report for 10.129.2.18 Host is up (0.087s latency). MAC Address: DE:AD:00:00:BE:EF Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds`

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.18`|Performs defined scans against the target.|
|`-sn`|Disables port scanning.|
|`-oA host`|Stores the results in all formats starting with the name 'host'.|

If we disable port scan (`-sn`), Nmap automatically ping scan with `ICMP Echo Requests` (`-PE`). Once such a request is sent, we usually expect an `ICMP reply` if the pinging host is alive. The more interesting fact is that our previous scans did not do that because before Nmap could send an ICMP echo request, it would send an `ARP ping` resulting in an `ARP reply`. We can confirm this with the "`--packet-trace`" option. To ensure that ICMP echo requests are sent, we also define the option (`-PE`) for this.

crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace   Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 00:08 CEST SENT (0.0074s) ARP who-has 10.129.2.18 tell 10.10.14.2 RCVD (0.0309s) ARP reply 10.129.2.18 is-at DE:AD:00:00:BE:EF Nmap scan report for 10.129.2.18 Host is up (0.023s latency). MAC Address: DE:AD:00:00:BE:EF Nmap done: 1 IP address (1 host up) scanned in 0.05 seconds`

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.18`|Performs defined scans against the target.|
|`-sn`|Disables port scanning.|
|`-oA host`|Stores the results in all formats starting with the name 'host'.|
|`-PE`|Performs the ping scan by using 'ICMP Echo requests' against the target.|
|`--packet-trace`|Shows all packets sent and received|

Another way to determine why Nmap has our target marked as "alive" is with the "`--reason`" option.

crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.18 -sn -oA host -PE --reason   Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 00:10 CEST SENT (0.0074s) ARP who-has 10.129.2.18 tell 10.10.14.2 RCVD (0.0309s) ARP reply 10.129.2.18 is-at DE:AD:00:00:BE:EF Nmap scan report for 10.129.2.18 Host is up, received arp-response (0.028s latency). MAC Address: DE:AD:00:00:BE:EF Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds`

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.18`|Performs defined scans against the target.|
|`-sn`|Disables port scanning.|
|`-oA host`|Stores the results in all formats starting with the name 'host'.|
|`-PE`|Performs the ping scan by using 'ICMP Echo requests' against the target.|
|`--reason`|Displays the reason for specific result.|

We see here that `Nmap` does indeed detect whether the host is alive or not through the `ARP request` and `ARP reply` alone. To disable ARP requests and scan our target with the desired `ICMP echo requests`, we can disable ARP pings by setting the "`--disable-arp-ping`" option. Then we can scan our target again and look at the packets sent and received.

crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping   Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 00:12 CEST SENT (0.0107s) ICMP [10.10.14.2 > 10.129.2.18 Echo request (type=8/code=0) id=13607 seq=0] IP [ttl=255 id=23541 iplen=28 ] RCVD (0.0152s) ICMP [10.129.2.18 > 10.10.14.2 Echo reply (type=0/code=0) id=13607 seq=0] IP [ttl=128 id=40622 iplen=28 ] Nmap scan report for 10.129.2.18 Host is up (0.086s latency). MAC Address: DE:AD:00:00:BE:EF Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds`

We have already mentioned in the "`Learning Process`," and at the beginning of this module, it is essential to pay attention to details. An `ICMP echo request` can help us determine if our target is alive and identify its system. More strategies about host discovery can be found at:

[https://nmap.org/book/host-discovery-strategies.html](https://nmap.org/book/host-discovery-strategies.html)

---
Question:

Based on the last result, find out which operating system it belongs to. Submit the name of the operating system as result.
windows

---

# Host and Port Scanning

It is essential to understand how the tool we use works and how it performs and processes the different functions. We will only understand the results if we know what they mean and how they are obtained. Therefore we will take a closer look at and analyze some of the scanning methods. After we have found out that our target is alive, we want to get a more accurate picture of the system. The information we need includes:

- Open ports and its services
- Service versions
- Information that the services provided
- Operating system

There are a total of 6 different states for a scanned port we can obtain:

|**State**|**Description**|
|---|---|
|`open`|This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**.|
|`closed`|When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not.|
|`filtered`|Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.|
|`unfiltered`|This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed.|
|`open\|filtered`|If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port.|
|`closed\|filtered`|This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.|

## Discovering Open TCP Ports

By default, `Nmap` scans the top 1000 TCP ports with the SYN scan (`-sS`). This SYN scan is set only to default when we run it as root because of the socket permissions required to create raw TCP packets. Otherwise, the TCP scan (`-sT`) is performed by default. This means that if we do not define ports and scanning methods, these parameters are set automatically. We can define the ports one by one (`-p 22,25,80,139,445`), by range (`-p 22-445`), by top ports (`--top-ports=10`) from the `Nmap` database that have been signed as most frequent, by scanning all ports (`-p-`) but also by defining a fast port scan, which contains top 100 ports (`-F`).

#### Scanning Top 10 TCP Ports

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 --top-ports=10   

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:36 
CEST Nmap scan report for 10.129.2.28 
Host is up (0.021s latency).  
PORT     STATE    SERVICE 
21/tcp   closed   ftp 
22/tcp   open     ssh 
23/tcp   closed   telnet 
25/tcp   open     smtp 
80/tcp   open     http 
110/tcp  open     pop3 
139/tcp  filtered netbios-ssn 
443/tcp  closed   https 
445/tcp  filtered microsoft-ds 
3389/tcp closed   ms-wbt-server 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 1.44 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`--top-ports=10`|Scans the specified top ports that have been defined as most frequent.|

We see that we only scanned the top 10 TCP ports of our target, and `Nmap` displays their state accordingly. If we trace the packets `Nmap` sends, we will see the `RST` flag on `TCP port 21` that our target sends back to us. To have a clear view of the SYN scan, we disable the ICMP echo requests (`-Pn`), DNS resolution (`-n`), and ARP ping scan (`--disable-arp-ping`).

#### Nmap - Trace the Packets

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:39 CEST 
SENT (0.0429s) TCP 10.10.14.2:63090 > 10.129.2.28:21 S ttl=56 id=57322 iplen=44  seq=1699105818 win=1024 <mss 1460> 
RCVD (0.0573s) TCP 10.129.2.28:21 > 10.10.14.2:63090 RA ttl=64 id=0 iplen=40 seq=0 win=0 

Nmap scan report for 10.11.1.28 
Host is up (0.014s latency).  
PORT   STATE  SERVICE 
21/tcp closed ftp 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 21`|Scans only the specified port.|
|`--packet-trace`|Shows all packets sent and received.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|

We can see from the SENT line that we (`10.10.14.2`) sent a TCP packet with the `SYN` flag (`S`) to our target (`10.129.2.28`). In the next RCVD line, we can see that the target responds with a TCP packet containing the `RST` and `ACK` flags (`RA`). `RST` and `ACK` flags are used to acknowledge receipt of the TCP packet (`ACK`) and to end the TCP session (`RST`).

#### Request

|**Message**|**Description**|
|---|---|
|`SENT (0.0429s)`|Indicates the SENT operation of Nmap, which sends a packet to the target.|
|`TCP`|Shows the protocol that is being used to interact with the target port.|
|`10.10.14.2:63090 >`|Represents our IPv4 address and the source port, which will be used by Nmap to send the packets.|
|`10.129.2.28:21`|Shows the target IPv4 address and the target port.|
|`S`|SYN flag of the sent TCP packet.|
|`ttl=56 id=57322 iplen=44 seq=1699105818 win=1024 mss 1460`|Additional TCP Header parameters.|

#### Response

|**Message**|**Description**|
|---|---|
|`RCVD (0.0573s)`|Indicates a received packet from the target.|
|`TCP`|Shows the protocol that is being used.|
|`10.129.2.28:21 >`|Represents targets IPv4 address and the source port, which will be used to reply.|
|`10.10.14.2:63090`|Shows our IPv4 address and the port that will be replied to.|
|`RA`|RST and ACK flags of the sent TCP packet.|
|`ttl=64 id=0 iplen=40 seq=0 win=0`|Additional TCP Header parameters.|

#### Connect Scan

The Nmap [TCP Connect Scan](https://nmap.org/book/scan-methods-connect-scan.html) (`-sT`) uses the TCP three-way handshake to determine if a specific port on a target host is open or closed. The scan sends an `SYN` packet to the target port and waits for a response. It is considered open if the target port responds with an `SYN-ACK` packet and closed if it responds with an `RST` packet.

The `Connect` scan (also known as a full TCP connect scan) is highly accurate because it completes the three-way TCP handshake, allowing us to determine the exact state of a port (open, closed, or filtered). However, it is not the most stealthy. In fact, the Connect scan is one of the least stealthy techniques, as it fully establishes a connection, which creates logs on most systems and is easily detected by modern IDS/IPS solutions. That said, the Connect scan can still be useful in certain situations, particularly when accuracy is a priority, and the goal is to map the network without causing significant disruption to services. Since the scan fully establishes a TCP connection, it interacts cleanly with services, making it less likely to cause service errors or instability compared to more intrusive scans. While it is not the most stealthy method, it is sometimes considered a more "polite" scan because it behaves like a normal client connection, thus having minimal impact on the target services.

It is also useful when the target host has a personal firewall that drops incoming packets but allows outgoing packets. In this case, a Connect scan can bypass the firewall and accurately determine the state of the target ports. However, it is important to note that the Connect scan is slower than other types of scans because it requires the scanner to wait for a response from the target after each packet it sends, which could take some time if the target is busy or unresponsive.

Scans like the SYN scan (also known as a half-open scan) are generally considered more stealthy because they do not complete the full handshake, leaving the connection incomplete after sending the initial SYN packet. This minimizes the chance of triggering connection logs while still gathering port state information. Advanced IDS/IPS systems, however, have adapted to detect even these subtler techniques.

#### Connect Scan on TCP Port 443

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT   

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:26 CET 
CONN (0.0385s) TCP localhost > 10.129.2.28:443 => Operation now in progress 
CONN (0.0396s) TCP localhost > 10.129.2.28:443 => Connected 

Nmap scan report for 10.129.2.28 Host is up, received user-set (0.013s latency).  
PORT    STATE SERVICE REASON 
443/tcp open  https   syn-ack  

Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds`
```
## Filtered Ports

When a port is shown as filtered, it can have several reasons. In most cases, firewalls have certain rules set to handle specific connections. The packets can either be `dropped`, or `rejected`. When a packet gets dropped, `Nmap` receives no response from our target, and by default, the retry rate (`--max-retries`) is set to `10`. This means `Nmap` will resend the request to the target port to determine if the previous packet was accidentally mishandled or not.

Let us look at an example where the firewall `drops` the TCP packets we send for the port scan. Therefore we scan the TCP port **139**, which was already shown as filtered. To be able to track how our sent packets are handled, we deactivate the ICMP echo requests (`-Pn`), DNS resolution (`-n`), and ARP ping scan (`--disable-arp-ping`) again.

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:45 CEST 
SENT (0.0381s) TCP 10.10.14.2:60277 > 10.129.2.28:139 S ttl=47 id=14523 iplen=44  seq=4175236769 win=1024 <mss 1460> 
SENT (1.0411s) TCP 10.10.14.2:60278 > 10.129.2.28:139 S ttl=45 id=7372 iplen=44  seq=4175171232 win=1024 <mss 1460> 

Nmap scan report for 10.129.2.28 Host is up.  
PORT    STATE    SERVICE 
139/tcp filtered netbios-ssn 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 2.06 seconds`
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 139`|Scans only the specified port.|
|`--packet-trace`|Shows all packets sent and received.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`-Pn`|Disables ICMP Echo requests.|

We see in the last scan that `Nmap` sent two TCP packets with the SYN flag. By the duration (`2.06s`) of the scan, we can recognize that it took much longer than the previous ones (`~0.05s`). The case is different if the firewall rejects the packets. For this, we look at TCP port `445`, which is handled accordingly by such a rule of the firewall.

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p 445 --packet-trace -n --disable-arp-ping -Pn  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:55 CEST 
SENT (0.0388s) TCP 10.129.2.28:52472 > 10.129.2.28:445 S ttl=49 id=21763 iplen=44  seq=1418633433 win=1024 <mss 1460> 
RCVD (0.0487s) ICMP [10.129.2.28 > 10.129.2.28 Port 445 unreachable (type=3/code=3) ] IP [ttl=64 id=20998 iplen=72 ] 

Nmap scan report for 10.129.2.28 
Host is up (0.0099s latency).  
PORT    STATE    SERVICE 
445/tcp filtered microsoft-ds 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 0.05 seconds`
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 445`|Scans only the specified port.|
|`--packet-trace`|Shows all packets sent and received.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`-Pn`|Disables ICMP Echo requests.|

As a response, we receive an `ICMP` reply with `type 3` and `error code 3`, which indicates that the desired port is unreachable. Nevertheless, if we know that the host is alive, we can strongly assume that the firewall on this port is rejecting the packets, and we will have to take a closer look at this port later.

## Discovering Open UDP Ports

Some system administrators sometimes forget to filter the UDP ports in addition to the TCP ones. Since `UDP` is a `stateless protocol` and does not require a three-way handshake like TCP. We do not receive any acknowledgment. Consequently, the timeout is much longer, making the whole `UDP scan` (`-sU`) much slower than the `TCP scan` (`-sS`).

Let's look at an example of what a UDP scan (`-sU`) can look like and what results it gives us.

#### UDP Port Scan

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -F -sU  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:01 CEST 
Nmap scan report for 10.129.2.28 
Host is up (0.059s latency). 
Not shown: 95 closed ports 

PORT     STATE         SERVICE 
68/udp   open|filtered dhcpc 
137/udp  open          netbios-ns 
138/udp  open|filtered netbios-dgm 
631/udp  open|filtered ipp 
5353/udp open          zeroconf 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 98.07 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-F`|Scans top 100 ports.|
|`-sU`|Performs a UDP scan.|

Another disadvantage of this is that we often do not get a response back because `Nmap` sends empty datagrams to the scanned UDP ports, and we do not receive any response. So we cannot determine if the UDP packet has arrived at all or not. If the UDP port is `open`, we only get a response if the application is configured to do so.

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason   

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:15 CEST 
SENT (0.0367s) UDP 10.10.14.2:55478 > 10.129.2.28:137 ttl=57 id=9122 iplen=78 RCVD (0.0398s) UDP 10.129.2.28:137 > 10.10.14.2:55478 ttl=64 id=13222 iplen=257 Nmap scan report for 10.129.2.28 
Host is up, received user-set (0.0031s latency).  
PORT    STATE SERVICE    REASON 
137/udp open  netbios-ns udp-response ttl 64 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-sU`|Performs a UDP scan.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`-p 137`|Scans only the specified port.|
|`--reason`|Displays the reason a port is in a particular state.|

If we get an ICMP response with `error code 3` (port unreachable), we know that the port is indeed `closed`.

```
crimsonguard@htb/htb$ sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 100 --reason   

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:25 CEST 
SENT (0.0445s) UDP 10.10.14.2:63825 > 10.129.2.28:100 ttl=57 id=29925 iplen=28 
RCVD (0.1498s) ICMP [10.129.2.28 > 10.10.14.2 Port unreachable (type=3/code=3) ] IP [ttl=64 id=11903 iplen=56 ] 

Nmap scan report for 10.129.2.28 
Host is up, received user-set (0.11s latency).  
PORT    STATE  SERVICE REASON 
100/udp closed unknown port-unreach ttl 64 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in  0.15 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-sU`|Performs a UDP scan.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`-p 100`|Scans only the specified port.|
|`--reason`|Displays the reason a port is in a particular state.|

For all other ICMP responses, the scanned ports are marked as (`open|filtered`).

```
crimsonguard@htb/htb$ sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 138 --reason   

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 16:32 CEST 
SENT (0.0380s) UDP 10.10.14.2:52341 > 10.129.2.28:138 ttl=50 id=65159 iplen=28 
SENT (1.0392s) UDP 10.10.14.2:52342 > 10.129.2.28:138 ttl=40 id=24444 iplen=28 Nmap scan report for 10.129.2.28 
Host is up, received user-set.  

PORT    STATE         SERVICE     REASON 
138/udp open|filtered netbios-dgm no-response 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 2.06 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-sU`|Performs a UDP scan.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`-p 138`|Scans only the specified port.|
|`--reason`|Displays the reason a port is in a particular state.|

Another handy method for scanning ports is the `-sV` option which is used to get additional available information from the open ports. This method can identify versions, service names, and details about our target.

#### Version Scan

```
crimsonguard@htb/htb$ sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason  -sV  

Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-04 11:10 GMT SENT (0.3426s) TCP 10.10.14.2:44641 > 10.129.2.28:445 S ttl=55 id=43401 iplen=44  seq=3589068008 win=1024 <mss 1460> 
RCVD (0.3556s) TCP 10.129.2.28:445 > 10.10.14.2:44641 SA ttl=63 id=0 iplen=44  seq=2881527699 win=29200 <mss 1337> 
NSOCK INFO [0.4980s] nsock_iod_new2(): nsock_iod_new (IOD #1) 
NSOCK INFO [0.4980s] nsock_connect_tcp(): TCP connection requested to 10.129.2.28:445 (IOD #1) EID 8 
NSOCK INFO [0.5130s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 8 [10.129.2.28:445] Service scan sending probe NULL to 10.129.2.28:445 (tcp) 
NSOCK INFO [0.5130s] nsock_read(): Read request from IOD #1 [10.129.2.28:445] (timeout: 6000ms) EID 18 
NSOCK INFO [6.5190s] nsock_trace_handler_callback(): Callback: READ TIMEOUT for EID 18 [10.129.2.28:445] Service scan sending probe SMBProgNeg to 10.129.2.28:445 (tcp) 
NSOCK INFO [6.5190s] nsock_write(): Write request for 168 bytes to IOD #1 EID 27 [10.129.2.28:445] 
NSOCK INFO [6.5190s] nsock_read(): Read request from IOD #1 [10.129.2.28:445] (timeout: 5000ms) EID 34 
NSOCK INFO [6.5190s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 27 [10.129.2.28:445] 
NSOCK INFO [6.5320s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 34 [10.129.2.28:445] (135 bytes) Service scan match (Probe SMBProgNeg matched with SMBProgNeg line 13836): 10.129.2.28:445 is netbios-ssn.  Version: |Samba smbd|3.X - 4.X|workgroup: WORKGROUP| 
NSOCK INFO [6.5320s] nsock_iod_delete(): nsock_iod_delete (IOD #1) 
Nmap scan report for 10.129.2.28 
Host is up, received user-set (0.013s latency).  

PORT    STATE SERVICE     REASON         VERSION 
445/tcp open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP) Service Info: Host: Ubuntu  

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . Nmap done: 1 IP address (1 host up) scanned in 6.55 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`-p 445`|Scans only the specified port.|
|`--reason`|Displays the reason a port is in a particular state.|
|`-sV`|Performs a service scan.|

More information about port scanning techniques we can find at: [https://nmap.org/book/man-port-scanning-techniques.html](https://nmap.org/book/man-port-scanning-techniques.html)

---
# Question:
###### Target: 10.129.2.49

-  Find all TCP ports on your target. Submit the total number of found TCP ports as the answer.
	- 7
```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sT -Pn 10.129.2.49 -p- -sV           
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 18:49 EST
Nmap scan report for 10.129.2.49
Host is up (0.034s latency).
Not shown: 65528 closed tcp ports (conn-refused)
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http        Apache httpd 2.4.29 ((Ubuntu))
110/tcp   open  pop3        Dovecot pop3d
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp   open  imap        Dovecot imapd (Ubuntu)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
31337/tcp open  Elite?
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31337-TCP:V=7.95%I=7%D=2/2%Time=67A004A8%P=x86_64-pc-linux-gnu%r(NU
SF:LL,1F,"220\x20HTB{pr0F7pDv3r510nb4nn3r}\r\n")%r(GetRequest,1F,"220\x20H
SF:TB{pr0F7pDv3r510nb4nn3r}\r\n");
Service Info: Host: NIX-NMAP-DEFAULT; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 200.26 seconds
```

- Enumerate the hostname of your target and submit it as the answer. (case-sensitive)
	- NIX-NMAP-DEFAULT
---

# Saving the Results

## Different Formats

While we run various scans, we should always save the results. We can use these later to examine the differences between the different scanning methods we have used. `Nmap` can save the results in 3 different formats.

- Normal output (`-oN`) with the `.nmap` file extension
- Grepable output (`-oG`) with the `.gnmap` file extension
- XML output (`-oX`) with the `.xml` file extension

We can also specify the option (`-oA`) to save the results in all formats. The command could look like this:

```
crimsonguard@htb/htb$ sudo nmap 10.129.2.28 -p- -oA target  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-16 12:14 CEST 
Nmap scan report for 10.129.2.28 
Host is up (0.0091s latency). 
Not shown: 65525 closed ports 
PORT      STATE SERVICE 
22/tcp    open  ssh 
25/tcp    open  smtp 
80/tcp    open  http 

MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 10.22 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p-`|Scans all ports.|
|`-oA target`|Saves the results in all formats, starting the name of each file with 'target'.|

If no full path is given, the results will be stored in the directory we are currently in. Next, we look at the different formats `Nmap` has created for us.

crimsonguard@htb[/htb]`$ ls  target.gnmap target.xml  target.nmap`

#### Normal Output

```
crimsonguard@htb[/htb]$ cat target.nmap  
# Nmap 7.80 scan initiated Tue Jun 16 12:14:53 2020 as: nmap -p- -oA target 10.129.2.28 
Nmap scan report for 10.129.2.28 
Host is up (0.053s latency). 
Not shown: 4 closed ports 
PORT   STATE SERVICE 
22/tcp open  ssh 
25/tcp open  smtp 
80/tcp open  http 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

# Nmap done at Tue Jun 16 12:15:03 2020 -- 1 IP address (1 host up) scanned in 10.22 seconds
```

#### Grepable Output

```
crimsonguard@htb[/htb]`$ cat target.gnmap  

# Nmap 7.80 scan initiated Tue Jun 16 12:14:53 2020 as: nmap -p- -oA target 10.129.2.28 Host: 10.129.2.28 ()	Status: Up 
Host: 10.129.2.28 ()	
Ports: 22/open/tcp//ssh///, 25/open/tcp//smtp///, 80/open/tcp//http///	Ignored State: closed (4) 
# Nmap done at Tue Jun 16 12:14:53 2020 -- 1 IP address (1 host up) scanned in 10.22 seconds`
```

#### XML Output

```
crimsonguard@htb[/htb]`$ cat target.xml  

<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE nmaprun> 
<?xml-stylesheet href="file:///usr/local/bin/../share/nmap/nmap.xsl" type="text/xsl"?> 
<!-- Nmap 7.80 scan initiated Tue Jun 16 12:14:53 2020 as: nmap -p- -oA target 10.129.2.28 --> 
<nmaprun scanner="nmap" args="nmap -p- -oA target 10.129.2.28" start="12145301719" startstr="Tue Jun 16 12:15:03 2020" version="7.80" xmloutputversion="1.04"> 
<scaninfo type="syn" protocol="tcp" numservices="65535" services="1-65535"/> 
<verbose level="0"/> 
<debugging level="0"/> 
<host starttime="12145301719" endtime="12150323493"><status state="up" reason="arp-response" reason_ttl="0"/> 
<address addr="10.129.2.28" addrtype="ipv4"/> 
<address addr="DE:AD:00:00:BE:EF" addrtype="mac" vendor="Intel Corporate"/> 
<hostnames> 
</hostnames> 
<ports><extraports state="closed" count="4"> 
<extrareasons reason="resets" count="4"/> 
</extraports> 
<port protocol="tcp" portid="22"><state state="open" reason="syn-ack" reason_ttl="64"/><service name="ssh" method="table" conf="3"/></port> 
<port protocol="tcp" portid="25"><state state="open" reason="syn-ack" reason_ttl="64"/><service name="smtp" method="table" conf="3"/></port> 
<port protocol="tcp" portid="80"><state state="open" reason="syn-ack" reason_ttl="64"/><service name="http" method="table" conf="3"/></port> </ports> <times srtt="52614" rttvar="75640" to="355174"/> 
</host>
<runstats><finished time="12150323493" timestr="Tue Jun 16 12:14:53 2020" elapsed="10.22" summary="Nmap done at Tue Jun 16 12:15:03 2020; 1 IP address (1 host up) scanned in 10.22 seconds" exit="success"/><hosts up="1" down="0" total="1"/> 
</runstats>
</nmaprun>
```

## Style sheets

With the XML output, we can easily create HTML reports that are easy to read, even for non-technical people. This is later very useful for documentation, as it presents our results in a detailed and clear way. To convert the stored results from XML format to HTML, we can use the tool `xsltproc`.

crimsonguard@htb[/htb]`$ xsltproc target.xml -o target.html`

If we now open the HTML file in our browser, we see a clear and structured presentation of our results.

#### Nmap Report
![[Pasted image 20250202190639 1.png]]

More information about the output formats can be found at: [https://nmap.org/book/output.html](https://nmap.org/book/output.html)

---
Question:
target: 10.129.2.49

- Perform a full TCP port scan on your target and create an HTML report. Submit the number of the highest port as the answer.
	- 31337
```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sT -Pn 10.129.2.49 -oX nmapscan.xml                                                                 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 19:21 EST
Nmap scan report for 10.129.2.49
Host is up (0.035s latency).
Not shown: 993 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
110/tcp   open  pop3
139/tcp   open  netbios-ssn
143/tcp   open  imap
445/tcp   open  microsoft-ds
31337/tcp open  Elite

Nmap done: 1 IP address (1 host up) scanned in 1.66 seconds

┌──(kali㉿kali)-[~]
└─$ xsltproc nmapscan.xml -o nmap.html

```
![[Pasted image 20250202192610 1.png]]

# Service Enumeration

For us, it is essential to determine the application and its version as accurately as possible. We can use this information to scan for known vulnerabilities and analyze the source code for that version if we find it. An exact version number allows us to search for a more precise exploit that fits the service and the operating system of our target.

## Service Version Detection

It is recommended to perform a quick port scan first, which gives us a small overview of the available ports. This causes significantly less traffic, which is advantageous for us because otherwise we can be discovered and blocked by the security mechanisms. We can deal with these first and run a port scan in the background, which shows all open ports (`-p-`). We can use the version scan to scan the specific ports for services and their versions (`-sV`).

A full port scan takes quite a long time. To view the scan status, we can press the `[Space Bar]` during the scan, which will cause `Nmap` to show us the scan status.

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p- -sV  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 19:44 CEST 
[Space Bar] 
Stats: 0:00:03 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan SYN Stealth Scan Timing: About 3.64% done; ETC: 19:45 (0:00:53 remaining)`
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p-`|Scans all ports.|
|`-sV`|Performs service version detection on specified ports.|

Another option (`--stats-every=5s`) that we can use is defining how periods of time the status should be shown. Here we can specify the number of seconds (`s`) or minutes (`m`), after which we want to get the status.

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p- -sV --stats-every=5s  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 19:46 CEST 
Stats: 0:00:05 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan SYN Stealth Scan Timing: About 13.91% done; ETC: 19:49 (0:00:31 remaining) 
Stats: 0:00:10 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan 
SYN Stealth Scan Timing: About 39.57% done; ETC: 19:48 (0:00:15 remaining)
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p-`|Scans all ports.|
|`-sV`|Performs service version detection on specified ports.|
|`--stats-every=5s`|Shows the progress of the scan every 5 seconds.|

We can also increase the `verbosity level` (`-v` / `-vv`), which will show us the open ports directly when `Nmap` detects them.

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p- -sV -v   

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 20:03 CEST 
NSE: Loaded 45 scripts for scanning. Initiating ARP Ping Scan at 20:03 
Scanning 10.129.2.28 [1 port] 
Completed ARP Ping Scan at 20:03, 0.03s elapsed (1 total hosts) 
Initiating Parallel DNS resolution of 1 host. at 20:03 
Completed Parallel DNS resolution of 1 host. at 20:03, 0.02s elapsed 
Initiating SYN Stealth Scan at 20:03 Scanning 10.129.2.28 [65535 ports] 
Discovered open port 995/tcp on 10.129.2.28 
Discovered open port 80/tcp on 10.129.2.28 
Discovered open port 993/tcp on 10.129.2.28 
Discovered open port 143/tcp on 10.129.2.28 
Discovered open port 25/tcp on 10.129.2.28 
Discovered open port 110/tcp on 10.129.2.28 
Discovered open port 22/tcp on 10.129.2.28 
<SNIP>
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p-`|Scans all ports.|
|`-sV`|Performs service version detection on specified ports.|
|`-v`|Increases the verbosity of the scan, which displays more detailed information.|

## Banner Grabbing

Once the scan is complete, we will see all TCP ports with the corresponding service and their versions that are active on the system.

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p- -sV  
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 20:00 CEST 
Nmap scan report for 10.129.2.28 
Host is up (0.013s latency). 
Not shown: 65525 closed ports
PORT      STATE    SERVICE      VERSION 
22/tcp    open     ssh          OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0) 
25/tcp    open     smtp         Postfix smtpd 
80/tcp    open     http         Apache httpd 2.4.29 ((Ubuntu)) 
110/tcp   open     pop3         Dovecot pop3d 
139/tcp   filtered netbios-ssn 
143/tcp   open     imap         Dovecot imapd (Ubuntu) 
445/tcp   filtered microsoft-ds 
993/tcp   open     ssl/imap     Dovecot imapd (Ubuntu) 
995/tcp   open     ssl/pop3     Dovecot pop3d 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate) 
Service Info: Host:  inlane; OS: Linux; CPE: cpe:/o:linux:linux_kernel  

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . 
Nmap done: 1 IP address (1 host up) scanned in 91.73 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p-`|Scans all ports.|
|`-sV`|Performs service version detection on specified ports.|

Primarily, `Nmap` looks at the banners of the scanned ports and prints them out. If it cannot identify versions through the banners, `Nmap` attempts to identify them through a signature-based matching system, but this significantly increases the scan's duration. One disadvantage to `Nmap`'s presented results is that the automatic scan can miss some information because sometimes `Nmap` does not know how to handle it. Let us look at an example of this.

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p- -sV -Pn -n --disable-arp-ping --packet-trace  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-16 20:10 CEST 
<SNIP> 
NSOCK INFO [0.4200s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 18 [10.129.2.28:25] (35 bytes): 220 inlane ESMTP Postfix (Ubuntu).. 
Service scan match (Probe NULL matched with NULL line 3104): 10.129.2.28:25 is smtp.  Version: |Postfix smtpd||| 
NSOCK INFO [0.4200s] nsock_iod_delete(): nsock_iod_delete (IOD #1) 
Nmap scan report for 10.129.2.28 
Host is up (0.076s latency).  

PORT   STATE SERVICE VERSION 
25/tcp open  smtp    Postfix smtpd 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate) 
Service Info: Host:  inlane  

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . 
Nmap done: 1 IP address (1 host up) scanned in 0.47 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p-`|Scans all ports.|
|`-sV`|Performs service version detection on specified ports.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|

If we look at the results from `Nmap`, we can see the port's status, service name, and hostname. Nevertheless, let us look at this line here:

- `NSOCK INFO [0.4200s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 18 [10.129.2.28:25] (35 bytes): 220 inlane ESMTP Postfix (Ubuntu)..`

Then we see that the SMTP server on our target gave us more information than `Nmap` showed us. Because here, we see that it is the Linux distribution `Ubuntu`. It happens because, after a successful three-way handshake, the server often sends a banner for identification. This serves to let the client know which service it is working with. At the network level, this happens with a `PSH` flag in the TCP header. However, it can happen that some services do not immediately provide such information. It is also possible to remove or manipulate the banners from the respective services. If we `manually` connect to the SMTP server using `nc`, grab the banner, and intercept the network traffic using `tcpdump`, we can see what `Nmap` did not show us.

#### Tcpdump

```
crimsonguard@htb[/htb]`$ sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28  

tcpdump: verbose output suppressed, use -v or -vv for full protocol decode listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
```

#### Nc

```
crimsonguard@htb[/htb]$  nc -nv 10.129.2.28 25  

Connection to 10.129.2.28 port 25 [tcp/*] succeeded! 220 inlane ESMTP Postfix (Ubuntu)
```

#### Tcpdump - Intercepted Traffic

```shell-session
18:28:07.128564 IP 10.10.14.2.59618 > 10.129.2.28.smtp: Flags [S], seq 1798872233, win 65535, options [mss 1460,nop,wscale 6,nop,nop,TS val 331260178 ecr 0,sackOK,eol], length 0
18:28:07.255151 IP 10.129.2.28.smtp > 10.10.14.2.59618: Flags [S.], seq 1130574379, ack 1798872234, win 65160, options [mss 1460,sackOK,TS val 1800383922 ecr 331260178,nop,wscale 7], length 0
18:28:07.255281 IP 10.10.14.2.59618 > 10.129.2.28.smtp: Flags [.], ack 1, win 2058, options [nop,nop,TS val 331260304 ecr 1800383922], length 0
18:28:07.319306 IP 10.129.2.28.smtp > 10.10.14.2.59618: Flags [P.], seq 1:36, ack 1, win 510, options [nop,nop,TS val 1800383985 ecr 331260304], length 35: SMTP: 220 inlane ESMTP Postfix (Ubuntu)
18:28:07.319426 IP 10.10.14.2.59618 > 10.129.2.28.smtp: Flags [.], ack 36, win 2058, options [nop,nop,TS val 331260368 ecr 1800383985], length 0
```

The first three lines show us the three-way handshake.

| 1.  | `[SYN]`     | `18:28:07.128564 IP 10.10.14.2.59618 > 10.129.2.28.smtp: Flags [S], <SNIP>`  |
| --- | ----------- | ---------------------------------------------------------------------------- |
| 2.  | `[SYN-ACK]` | `18:28:07.255151 IP 10.129.2.28.smtp > 10.10.14.2.59618: Flags [S.], <SNIP>` |
| 3.  | `[ACK]`     | `18:28:07.255281 IP 10.10.14.2.59618 > 10.129.2.28.smtp: Flags [.], <SNIP>`  |

After that, the target SMTP server sends us a TCP packet with the `PSH` and `ACK` flags, where `PSH` states that the target server is sending data to us and with `ACK` simultaneously informs us that all required data has been sent.

| 4.  | `[PSH-ACK]` | `18:28:07.319306 IP 10.129.2.28.smtp > 10.10.14.2.59618: Flags [P.], <SNIP>` |
| --- | ----------- | ---------------------------------------------------------------------------- |

The last TCP packet that we sent confirms the receipt of the data with an `ACK`.

| 5.  | `[ACK]` | `18:28:07.319426 IP 10.10.14.2.59618 > 10.129.2.28.smtp: Flags [.], <SNIP>` |
| --- | ------- | --------------------------------------------------------------------------- |

---
Question:
target: 10.129.2.49

- Enumerate all ports and their services. One of the services contains the flag you have to submit as the answer.
	- HTB{pr0F7pDv3r510nb4nn3r}

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap 10.129.2.49 -p- -sV
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 19:42 EST
Nmap scan report for 10.129.2.49
Host is up (0.046s latency).
Not shown: 65528 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http        Apache httpd 2.4.29 ((Ubuntu))
110/tcp   open  pop3        Dovecot pop3d
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp   open  imap        Dovecot imapd (Ubuntu)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
31337/tcp open  Elite?
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31337-TCP:V=7.95%I=7%D=2/2%Time=67A01129%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,1F,"220\x20HTB{pr0F7pDv3r510nb4nn3r}\r\n");
Service Info: Host: NIX-NMAP-DEFAULT; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 204.74 seconds

```

---

# Nmap Scripting Engine

Nmap Scripting Engine (`NSE`) is another handy feature of `Nmap`. It provides us with the possibility to create scripts in Lua for interaction with certain services. There are a total of 14 categories into which these scripts can be divided:

|**Category**|**Description**|
|---|---|
|`auth`|Determination of authentication credentials.|
|`broadcast`|Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans.|
|`brute`|Executes scripts that try to log in to the respective service by brute-forcing with credentials.|
|`default`|Default scripts executed by using the `-sC` option.|
|`discovery`|Evaluation of accessible services.|
|`dos`|These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services.|
|`exploit`|This category of scripts tries to exploit known vulnerabilities for the scanned port.|
|`external`|Scripts that use external services for further processing.|
|`fuzzer`|This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time.|
|`intrusive`|Intrusive scripts that could negatively affect the target system.|
|`malware`|Checks if some malware infects the target system.|
|`safe`|Defensive scripts that do not perform intrusive and destructive access.|
|`version`|Extension for service detection.|
|`vuln`|Identification of specific vulnerabilities.|

We have several ways to define the desired scripts in `Nmap`.

#### Default Scripts

`crimsonguard@htb[/htb]$ sudo nmap <target> -sC`

#### Specific Scripts Category

`crimsonguard@htb[/htb]$ sudo nmap <target> --script <category>`

#### Defined Scripts

`crimsonguard@htb[/htb]$ sudo nmap <target> --script <script-name>,<script-name>,...`

For example, let us keep working with the target SMTP port and see the results we get with two defined scripts.

#### Nmap - Specifying Scripts


```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-16 23:21 CEST 
Nmap scan report for 10.129.2.28 
Host is up (0.050s latency).  
PORT   STATE SERVICE 
25/tcp open  smtp 
|_banner: 220 inlane ESMTP Postfix (Ubuntu) 
|_smtp-commands: inlane, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 25`|Scans only the specified port.|
|`--script banner,smtp-commands`|Uses specified NSE scripts.|

We see that we can recognize the **Ubuntu** distribution of Linux by using the' banner' script. The `smtp-commands` script shows us which commands we can use by interacting with the target SMTP server. In this example, such information may help us to find out existing users on the target. `Nmap` also gives us the ability to scan our target with the aggressive option (`-A`). This scans the target with multiple options as service detection (`-sV`), OS detection (`-O`), traceroute (`--traceroute`), and with the default NSE scripts (`-sC`).

#### Nmap - Aggressive Scan

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p 80 -A 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-17 01:38 CEST 
Nmap scan report for 10.129.2.28 
Host is up (0.012s latency).  

PORT   STATE SERVICE VERSION 
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu)) 
|_http-generator: WordPress 5.3.4 
|_http-server-header: Apache/2.4.29 (Ubuntu) 
|_http-title: blog.inlanefreight.com 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate) 
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port 
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%),  AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%),  Linux 2.6.32 - 2.6.35 (94%) 
No exact OS matches for host (test conditions non-ideal). 
Network Distance: 1 hop  

TRACEROUTE 
HOP RTT      ADDRESS 
1   11.91 ms 10.129.2.28  
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . 
Nmap done: 1 IP address (1 host up) scanned in 11.36 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 80`|Scans only the specified port.|
|`-A`|Performs service detection, OS detection, traceroute and uses defaults scripts to scan the target.|

With the help of the used scan option (`-A`), we found out what kind of web server (`Apache 2.4.29`) is running on the system, which web application (`WordPress 5.3.4`) is used, and the title (`blog.inlanefreight.com`) of the web page. Also, `Nmap` shows that it is likely to be `Linux` (`96%`) operating system.

## Vulnerability Assessment

Now let us move on to HTTP port 80 and see what information and vulnerabilities we can find using the `vuln` category from `NSE`.

#### Nmap - Vuln Category

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.28 -p 80 -sV --script vuln  

Nmap scan report for 10.129.2.28 
Host is up (0.036s latency). 

PORT   STATE SERVICE VERSION 
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu)) 
| http-enum: 
|   /wp-login.php: Possible admin folder 
|   /readme.html: Wordpress version: 2 
|   /: WordPress version: 5.3.4 
|   /wp-includes/images/rss.png: Wordpress version 2.2 found. 
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found. 
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found. 
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found. 
|   /wp-login.php: Wordpress login page. 
|   /wp-admin/upgrade.php: Wordpress login page. 
|_  /readme.html: Interesting, a readme. 
|_http-server-header: Apache/2.4.29 (Ubuntu) 
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities. 
| http-wordpress-users: 
| Username found: admin 
|_Search stopped at ID #25. Increase the upper limit if necessary with 'http-wordpress-users.limit' 
| vulners: 
|   cpe:/a:apache:http_server:2.4.29: 
|     	CVE-2019-0211	7.2	https://vulners.com/cve/CVE-2019-0211 
|     	CVE-2018-1312	6.8	https://vulners.com/cve/CVE-2018-1312 
|     	CVE-2017-15715	6.8	https://vulners.com/cve/CVE-2017-15715 
<SNIP>
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 80`|Scans only the specified port.|
|`-sV`|Performs service version detection on specified ports.|
|`--script vuln`|Uses all related scripts from specified category.|

The scripts used for the last scan interact with the webserver and its web application to find out more information about their versions and check various databases to see if there are known vulnerabilities. More information about NSE scripts and the corresponding categories we can find at: [https://nmap.org/nsedoc/index.html](https://nmap.org/nsedoc/index.html)

---
Question:
Target: 10.129.2.49

- Use NSE and its scripts to find the flag that one of the services contain and submit it as the answer.
	- HTB{873nniuc71bu6usbs1i96as6dsv26}
```
┌──(kali㉿kali)-[~]
└─$ sudo nmap --script vuln -T5 10.129.2.49 -sV -p 80
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 20:08 EST
Nmap scan report for 10.129.2.49
Host is up (0.029s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-enum: 
|_  /robots.txt: Robots file
| vulners: 
|   cpe:/a:apache:http_server:2.4.29: 
|       95499236-C9FE-56A6-9D7D-E943A24B633A    10.0    https://vulners.com/githubexploit/95499236-C9FE-56A6-9D7D-E943A24B633A      *EXPLOIT*
|       2C119FFA-ECE0-5E14-A4A4-354A2C38071A    10.0    https://vulners.com/githubexploit/2C119FFA-ECE0-5E14-A4A4-354A2C38071A      *EXPLOIT*
|       F607361B-6369-5DF5-9B29-E90FA29DC565    9.8     https://vulners.com/githubexploit/F607361B-6369-5DF5-9B29-E90FA29DC565      *EXPLOIT*
|       EDB-ID:51193    9.8     https://vulners.com/exploitdb/EDB-ID:51193      *EXPLOIT*
|       CVE-2024-38476  9.8     https://vulners.com/cve/CVE-2024-38476
|       CVE-2024-38474  9.8     https://vulners.com/cve/CVE-2024-38474
|       CVE-2023-25690  9.8     https://vulners.com/cve/CVE-2023-25690
|       CVE-2022-31813  9.8     https://vulners.com/cve/CVE-2022-31813
|       CVE-2022-23943  9.8     https://vulners.com/cve/CVE-2022-23943
|       CVE-2022-22720  9.8     https://vulners.com/cve/CVE-2022-22720
|       CVE-2021-44790  9.8     https://vulners.com/cve/CVE-2021-44790
|       CVE-2021-39275  9.8     https://vulners.com/cve/CVE-2021-39275
|       CVE-2021-26691  9.8     https://vulners.com/cve/CVE-2021-26691
|       CVE-2018-1312   9.8     https://vulners.com/cve/CVE-2018-1312
|       B02819DB-1481-56C4-BD09-6B4574297109    9.8     https://vulners.com/githubexploit/B02819DB-1481-56C4-BD09-6B4574297109      *EXPLOIT*
|       A5425A79-9D81-513A-9CC5-549D6321897C    9.8     https://vulners.com/githubexploit/A5425A79-9D81-513A-9CC5-549D6321897C      *EXPLOIT*
|       5C1BB960-90C1-5EBF-9BEF-F58BFFDFEED9    9.8     https://vulners.com/githubexploit/5C1BB960-90C1-5EBF-9BEF-F58BFFDFEED9      *EXPLOIT*
|       3F17CA20-788F-5C45-88B3-E12DB2979B7B    9.8     https://vulners.com/githubexploit/3F17CA20-788F-5C45-88B3-E12DB2979B7B      *EXPLOIT*
|       1337DAY-ID-39214        9.8     https://vulners.com/zdt/1337DAY-ID-39214        *EXPLOIT*
|       CVE-2024-38475  9.1     https://vulners.com/cve/CVE-2024-38475
|       CVE-2022-28615  9.1     https://vulners.com/cve/CVE-2022-28615
|       CVE-2022-22721  9.1     https://vulners.com/cve/CVE-2022-22721
|       CVE-2019-10082  9.1     https://vulners.com/cve/CVE-2019-10082
|       2EF14600-503F-53AF-BA24-683481265D30    9.1     https://vulners.com/githubexploit/2EF14600-503F-53AF-BA24-683481265D30      *EXPLOIT*
|       0486EBEE-F207-570A-9AD8-33269E72220A    9.1     https://vulners.com/githubexploit/0486EBEE-F207-570A-9AD8-33269E72220A      *EXPLOIT*
|       DC06B9EF-3584-5D80-9EEB-E7B637DCF3D6    9.0     https://vulners.com/githubexploit/DC06B9EF-3584-5D80-9EEB-E7B637DCF3D6      *EXPLOIT*
|       CVE-2022-36760  9.0     https://vulners.com/cve/CVE-2022-36760
|       CVE-2021-40438  9.0     https://vulners.com/cve/CVE-2021-40438
|       AE3EF1CC-A0C3-5CB7-A6EF-4DAAAFA59C8C    9.0     https://vulners.com/githubexploit/AE3EF1CC-A0C3-5CB7-A6EF-4DAAAFA59C8C      *EXPLOIT*
|       8AFB43C5-ABD4-52AD-BB19-24D7884FF2A2    9.0     https://vulners.com/githubexploit/8AFB43C5-ABD4-52AD-BB19-24D7884FF2A2      *EXPLOIT*
|       893DFD44-40B5-5469-AC54-A373AEE17F19    9.0     https://vulners.com/githubexploit/893DFD44-40B5-5469-AC54-A373AEE17F19      *EXPLOIT*
|       7F48C6CF-47B2-5AF9-B6FD-1735FB2A95B2    9.0     https://vulners.com/githubexploit/7F48C6CF-47B2-5AF9-B6FD-1735FB2A95B2      *EXPLOIT*
|       4810E2D9-AC5F-5B08-BFB3-DDAFA2F63332    9.0     https://vulners.com/githubexploit/4810E2D9-AC5F-5B08-BFB3-DDAFA2F63332      *EXPLOIT*
|       4373C92A-2755-5538-9C91-0469C995AA9B    9.0     https://vulners.com/githubexploit/4373C92A-2755-5538-9C91-0469C995AA9B      *EXPLOIT*
|       36618CA8-9316-59CA-B748-82F15F407C4F    9.0     https://vulners.com/githubexploit/36618CA8-9316-59CA-B748-82F15F407C4F      *EXPLOIT*
|       CVE-2021-44224  8.2     https://vulners.com/cve/CVE-2021-44224
|       B0A9E5E8-7CCC-5984-9922-A89F11D6BF38    8.2     https://vulners.com/githubexploit/B0A9E5E8-7CCC-5984-9922-A89F11D6BF38      *EXPLOIT*
|       CVE-2024-38473  8.1     https://vulners.com/cve/CVE-2024-38473
|       CVE-2017-15715  8.1     https://vulners.com/cve/CVE-2017-15715
|       249A954E-0189-5182-AE95-31C866A057E1    8.1     https://vulners.com/githubexploit/249A954E-0189-5182-AE95-31C866A057E1      *EXPLOIT*
|       23079A70-8B37-56D2-9D37-F638EBF7F8B5    8.1     https://vulners.com/githubexploit/23079A70-8B37-56D2-9D37-F638EBF7F8B5      *EXPLOIT*
|       EDB-ID:46676    7.8     https://vulners.com/exploitdb/EDB-ID:46676      *EXPLOIT*
|       CVE-2019-0211   7.8     https://vulners.com/cve/CVE-2019-0211
|       PACKETSTORM:176334      7.5     https://vulners.com/packetstorm/PACKETSTORM:176334      *EXPLOIT*
|       PACKETSTORM:171631      7.5     https://vulners.com/packetstorm/PACKETSTORM:171631      *EXPLOIT*
|       F7F6E599-CEF4-5E03-8E10-FE18C4101E38    7.5     https://vulners.com/githubexploit/F7F6E599-CEF4-5E03-8E10-FE18C4101E38      *EXPLOIT*
|       E606D7F4-5FA2-5907-B30E-367D6FFECD89    7.5     https://vulners.com/githubexploit/E606D7F4-5FA2-5907-B30E-367D6FFECD89      *EXPLOIT*
|       E5C174E5-D6E8-56E0-8403-D287DE52EB3F    7.5     https://vulners.com/githubexploit/E5C174E5-D6E8-56E0-8403-D287DE52EB3F      *EXPLOIT*
|       DB6E1BBD-08B1-574D-A351-7D6BB9898A4A    7.5     https://vulners.com/githubexploit/DB6E1BBD-08B1-574D-A351-7D6BB9898A4A      *EXPLOIT*
|       CVE-2024-40898  7.5     https://vulners.com/cve/CVE-2024-40898
|       CVE-2024-39573  7.5     https://vulners.com/cve/CVE-2024-39573
|       CVE-2024-38477  7.5     https://vulners.com/cve/CVE-2024-38477
|       CVE-2024-38472  7.5     https://vulners.com/cve/CVE-2024-38472
|       CVE-2024-27316  7.5     https://vulners.com/cve/CVE-2024-27316
|       CVE-2023-31122  7.5     https://vulners.com/cve/CVE-2023-31122
|       CVE-2022-30556  7.5     https://vulners.com/cve/CVE-2022-30556
|       CVE-2022-29404  7.5     https://vulners.com/cve/CVE-2022-29404
|       CVE-2022-26377  7.5     https://vulners.com/cve/CVE-2022-26377
|       CVE-2022-22719  7.5     https://vulners.com/cve/CVE-2022-22719
|       CVE-2021-34798  7.5     https://vulners.com/cve/CVE-2021-34798
|       CVE-2021-33193  7.5     https://vulners.com/cve/CVE-2021-33193
|       CVE-2021-26690  7.5     https://vulners.com/cve/CVE-2021-26690
|       CVE-2020-9490   7.5     https://vulners.com/cve/CVE-2020-9490
|       CVE-2020-11993  7.5     https://vulners.com/cve/CVE-2020-11993
|       CVE-2019-9517   7.5     https://vulners.com/cve/CVE-2019-9517
|       CVE-2019-10081  7.5     https://vulners.com/cve/CVE-2019-10081
|       CVE-2019-0217   7.5     https://vulners.com/cve/CVE-2019-0217
|       CVE-2019-0215   7.5     https://vulners.com/cve/CVE-2019-0215
|       CVE-2019-0190   7.5     https://vulners.com/cve/CVE-2019-0190
|       CVE-2018-8011   7.5     https://vulners.com/cve/CVE-2018-8011
|       CVE-2018-17199  7.5     https://vulners.com/cve/CVE-2018-17199
|       CVE-2018-1333   7.5     https://vulners.com/cve/CVE-2018-1333
|       CVE-2018-1303   7.5     https://vulners.com/cve/CVE-2018-1303
|       CVE-2017-15710  7.5     https://vulners.com/cve/CVE-2017-15710
|       CVE-2006-20001  7.5     https://vulners.com/cve/CVE-2006-20001
|       CDC791CD-A414-5ABE-A897-7CFA3C2D3D29    7.5     https://vulners.com/githubexploit/CDC791CD-A414-5ABE-A897-7CFA3C2D3D29      *EXPLOIT*
|       C9A1C0C1-B6E3-5955-A4F1-DEA0E505B14B    7.5     https://vulners.com/githubexploit/C9A1C0C1-B6E3-5955-A4F1-DEA0E505B14B      *EXPLOIT*
|       BD3652A9-D066-57BA-9943-4E34970463B9    7.5     https://vulners.com/githubexploit/BD3652A9-D066-57BA-9943-4E34970463B9      *EXPLOIT*
|       B5E74010-A082-5ECE-AB37-623A5B33FE7D    7.5     https://vulners.com/githubexploit/B5E74010-A082-5ECE-AB37-623A5B33FE7D      *EXPLOIT*
|       B0208442-6E17-5772-B12D-B5BE30FA5540    7.5     https://vulners.com/githubexploit/B0208442-6E17-5772-B12D-B5BE30FA5540      *EXPLOIT*
|       A820A056-9F91-5059-B0BC-8D92C7A31A52    7.5     https://vulners.com/githubexploit/A820A056-9F91-5059-B0BC-8D92C7A31A52      *EXPLOIT*
|       A66531EB-3C47-5C56-B8A6-E04B54E9D656    7.5     https://vulners.com/githubexploit/A66531EB-3C47-5C56-B8A6-E04B54E9D656      *EXPLOIT*
|       A0F268C8-7319-5637-82F7-8DAF72D14629    7.5     https://vulners.com/githubexploit/A0F268C8-7319-5637-82F7-8DAF72D14629      *EXPLOIT*
|       9814661A-35A4-5DB7-BB25-A1040F365C81    7.5     https://vulners.com/githubexploit/9814661A-35A4-5DB7-BB25-A1040F365C81      *EXPLOIT*
|       788E0E7C-6F5C-5DAD-9E3A-EE6D8A685F7D    7.5     https://vulners.com/githubexploit/788E0E7C-6F5C-5DAD-9E3A-EE6D8A685F7D      *EXPLOIT*
|       5A864BCC-B490-5532-83AB-2E4109BB3C31    7.5     https://vulners.com/githubexploit/5A864BCC-B490-5532-83AB-2E4109BB3C31      *EXPLOIT*
|       4B14D194-BDE3-5D7F-A262-A701F90DE667    7.5     https://vulners.com/githubexploit/4B14D194-BDE3-5D7F-A262-A701F90DE667      *EXPLOIT*
|       45D138AD-BEC6-552A-91EA-8816914CA7F4    7.5     https://vulners.com/githubexploit/45D138AD-BEC6-552A-91EA-8816914CA7F4      *EXPLOIT*
|       17C6AD2A-8469-56C8-BBBE-1764D0DF1680    7.5     https://vulners.com/githubexploit/17C6AD2A-8469-56C8-BBBE-1764D0DF1680      *EXPLOIT*
|       1337DAY-ID-38427        7.5     https://vulners.com/zdt/1337DAY-ID-38427        *EXPLOIT*
|       1337DAY-ID-35422        7.5     https://vulners.com/zdt/1337DAY-ID-35422        *EXPLOIT*
|       CVE-2023-38709  7.3     https://vulners.com/cve/CVE-2023-38709
|       CVE-2020-35452  7.3     https://vulners.com/cve/CVE-2020-35452
|       EXPLOITPACK:44C5118F831D55FAF4259C41D8BDA0AB    7.2     https://vulners.com/exploitpack/EXPLOITPACK:44C5118F831D55FAF4259C41D8BDA0AB        *EXPLOIT*
|       1337DAY-ID-32502        7.2     https://vulners.com/zdt/1337DAY-ID-32502        *EXPLOIT*
|       FDF3DFA1-ED74-5EE2-BF5C-BA752CA34AE8    6.8     https://vulners.com/githubexploit/FDF3DFA1-ED74-5EE2-BF5C-BA752CA34AE8      *EXPLOIT*
|       0095E929-7573-5E4A-A7FA-F6598A35E8DE    6.8     https://vulners.com/githubexploit/0095E929-7573-5E4A-A7FA-F6598A35E8DE      *EXPLOIT*
|       CVE-2024-24795  6.3     https://vulners.com/cve/CVE-2024-24795
|       CVE-2024-39884  6.2     https://vulners.com/cve/CVE-2024-39884
|       CVE-2020-1927   6.1     https://vulners.com/cve/CVE-2020-1927
|       CVE-2019-10098  6.1     https://vulners.com/cve/CVE-2019-10098
|       CVE-2019-10092  6.1     https://vulners.com/cve/CVE-2019-10092
|       CVE-2023-45802  5.9     https://vulners.com/cve/CVE-2023-45802
|       CVE-2018-1302   5.9     https://vulners.com/cve/CVE-2018-1302
|       CVE-2018-1301   5.9     https://vulners.com/cve/CVE-2018-1301
|       CVE-2018-11763  5.9     https://vulners.com/cve/CVE-2018-11763
|       1337DAY-ID-33577        5.8     https://vulners.com/zdt/1337DAY-ID-33577        *EXPLOIT*
|       CVE-2020-13938  5.5     https://vulners.com/cve/CVE-2020-13938
|       CVE-2022-37436  5.3     https://vulners.com/cve/CVE-2022-37436
|       CVE-2022-28614  5.3     https://vulners.com/cve/CVE-2022-28614
|       CVE-2022-28330  5.3     https://vulners.com/cve/CVE-2022-28330
|       CVE-2020-1934   5.3     https://vulners.com/cve/CVE-2020-1934
|       CVE-2019-17567  5.3     https://vulners.com/cve/CVE-2019-17567
|       CVE-2019-0220   5.3     https://vulners.com/cve/CVE-2019-0220
|       CVE-2019-0196   5.3     https://vulners.com/cve/CVE-2019-0196
|       CVE-2018-17189  5.3     https://vulners.com/cve/CVE-2018-17189
|       CVE-2018-1283   5.3     https://vulners.com/cve/CVE-2018-1283
|       4013EC74-B3C1-5D95-938A-54197A58586D    4.3     https://vulners.com/githubexploit/4013EC74-B3C1-5D95-938A-54197A58586D      *EXPLOIT*
|       1337DAY-ID-33575        4.3     https://vulners.com/zdt/1337DAY-ID-33575        *EXPLOIT*
|_      PACKETSTORM:152441      0.0     https://vulners.com/packetstorm/PACKETSTORM:152441      *EXPLOIT*
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 38.02 seconds

```
- check the robots.txt file (a hunch)
![[Pasted image 20250202201339 1.png]]

---

# Performance

Scanning performance plays a significant role when we need to scan an extensive network or are dealing with low network bandwidth. We can use various options to tell `Nmap` how fast (`-T <0-5>`), with which frequency (`--min-parallelism <number>`), which timeouts (`--max-rtt-timeout <time>`) the test packets should have, how many packets should be sent simultaneously (`--min-rate <number>`), and with the number of retries (`--max-retries <number>`) for the scanned ports the targets should be scanned.

## Timeouts

When Nmap sends a packet, it takes some time (`Round-Trip-Time` - `RTT`) to receive a response from the scanned port. Generally, `Nmap` starts with a high timeout (`--min-RTT-timeout`) of 100ms. Let us look at an example by scanning the whole network with 256 hosts, including the top 100 ports.

#### Default Scan

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.0/24 -F  <SNIP> Nmap done: 256 IP addresses (10 hosts up) scanned in 39.44 seconds`
```

#### Optimized RTT

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms  
<SNIP> 
Nmap done: 256 IP addresses (8 hosts up) scanned in 12.29 seconds`
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.0/24`|Scans the specified target network.|
|`-F`|Scans top 100 ports.|
|`--initial-rtt-timeout 50ms`|Sets the specified time value as initial RTT timeout.|
|`--max-rtt-timeout 100ms`|Sets the specified time value as maximum RTT timeout.|

When comparing the two scans, we can see that we found two hosts less with the optimized scan, but the scan took only a quarter of the time. From this, we can conclude that setting the initial RTT timeout (`--initial-rtt-timeout`) to too short a time period may cause us to overlook hosts.

## Max Retries

Another way to increase scan speed is by specifying the retry rate of sent packets (`--max-retries`). The default value is `10`, but we can reduce it to `0`. This means if Nmap does not receive a response for a port, it won't send any more packets to that port and will skip it.

#### Default Scan

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l  23`
```

#### Reduced Retries

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -l  21`
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.0/24`|Scans the specified target network.|
|`-F`|Scans top 100 ports.|
|`--max-retries 0`|Sets the number of retries that will be performed during the scan.|

Again, we recognize that accelerating can also have a negative effect on our results, which means we can overlook important information.

## Rates

During a white-box penetration test, we may get whitelisted for the security systems to check the systems in the network for vulnerabilities and not only test the protection measures. If we know the network bandwidth, we can work with the rate of packets sent, which significantly speeds up our scans with `Nmap`. When setting the minimum rate (`--min-rate <number>`) for sending packets, we tell `Nmap` to simultaneously send the specified number of packets. It will attempt to maintain the rate accordingly.

#### Default Scan

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.default  
<SNIP> 
Nmap done: 256 IP addresses (10 hosts up) scanned in 29.83 seconds
```

#### Optimized Scan

```
crimsonguard@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300  
<SNIP> 
Nmap done: 256 IP addresses (10 hosts up) scanned in 8.67 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.0/24`|Scans the specified target network.|
|`-F`|Scans top 100 ports.|
|`-oN tnet.minrate300`|Saves the results in normal formats, starting the specified file name.|
|`--min-rate 300`|Sets the minimum number of packets to be sent per second.|

#### Default Scan - Found Open Ports

```
crimsonguard@htb[/htb]`$ cat tnet.default | grep "/tcp" | wc -l  23`
```

#### Optimized Scan - Found Open Ports

```
crimsonguard@htb[/htb]`$ cat tnet.minrate300 | grep "/tcp" | wc -l  23`
```

## Timing

Because such settings cannot always be optimized manually, as in a black-box penetration test, `Nmap` offers six different timing templates (`-T <0-5>`) for us to use. These values (`0-5`) determine the aggressiveness of our scans. This can also have negative effects if the scan is too aggressive, and security systems may block us due to the produced network traffic. The default timing template used when we have defined nothing else is the normal (`-T 3`).

- `-T 0` / `-T paranoid`
- `-T 1` / `-T sneaky`
- `-T 2` / `-T polite`
- `-T 3` / `-T normal`
- `-T 4` / `-T aggressive`
- `-T 5` / `-T insane`

These templates contain options that we can also set manually, and have seen some of them already. The developers determined the values set for these templates according to their best results, making it easier for us to adapt our scans to the corresponding network environment. The exact used options with their values we can find here: [https://nmap.org/book/performance-timing-templates.html](https://nmap.org/book/performance-timing-templates.html)

#### Default Scan

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.0/24 -F -oN tnet.default   <SNIP> Nmap done: 256 IP addresses (10 hosts up) scanned in 32.44 seconds`
```

#### Insane Scan

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5  <SNIP> Nmap done: 256 IP addresses (10 hosts up) scanned in 18.07 seconds`
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.0/24`|Scans the specified target network.|
|`-F`|Scans top 100 ports.|
|`-oN tnet.T5`|Saves the results in normal formats, starting the specified file name.|
|`-T 5`|Specifies the insane timing template.|

#### Default Scan - Found Open Ports

```
crimsonguard@htb[/htb]`$ cat tnet.default | grep "/tcp" | wc -l  23`
```

#### Insane Scan - Found Open Ports

```
crimsonguard@htb[/htb]`$ cat tnet.T5 | grep "/tcp" | wc -l  23`
```

More information about scan performance we can find at [https://nmap.org/book/man-performance.html](https://nmap.org/book/man-performance.html)

# Firewall and IDS/IPS Evasion

`Nmap` gives us many different ways to bypass firewalls rules and IDS/IPS. These methods include the fragmentation of packets, the use of decoys, and others that we will discuss in this section.

## Firewalls

A firewall is a security measure against unauthorized connection attempts from external networks. Every firewall security system is based on a software component that monitors network traffic between the firewall and incoming data connections and decides how to handle the connection based on the rules that have been set. It checks whether individual network packets are being passed, ignored, or blocked. This mechanism is designed to prevent unwanted connections that could be potentially dangerous.

## IDS/IPS

Like the firewall, the intrusion detection system (`IDS`) and intrusion prevention system (`IPS`) are also software-based components. `IDS` scans the network for potential attacks, analyzes them, and reports any detected attacks. `IPS` complements `IDS` by taking specific defensive measures if a potential attack should have been detected. The analysis of such attacks is based on pattern matching and signatures. If specific patterns are detected, such as a service detection scan, `IPS` may prevent the pending connection attempts.

#### Determine Firewalls and Their Rules

We already know that when a port is shown as filtered, it can have several reasons. In most cases, firewalls have certain rules set to handle specific connections. The packets can either be `dropped`, or `rejected`. The `dropped` packets are ignored, and no response is returned from the host.

This is different for `rejected` packets that are returned with an `RST` flag. These packets contain different types of ICMP error codes or contain nothing at all.

Such errors can be:

- Net Unreachable
- Net Prohibited
- Host Unreachable
- Host Prohibited
- Port Unreachable
- Proto Unreachable

Nmap's TCP ACK scan (`-sA`) method is much harder to filter for firewalls and IDS/IPS systems than regular SYN (`-sS`) or Connect scans (`sT`) because they only send a TCP packet with only the `ACK` flag. When a port is closed or open, the host must respond with an `RST` flag. Unlike outgoing connections, all connection attempts (with the `SYN` flag) from external networks are usually blocked by firewalls. However, the packets with the `ACK` flag are often passed by the firewall because the firewall cannot determine whether the connection was first established from the external network or the internal network.

If we look at these scans, we will see how the results differ.

#### SYN-Scan

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p 21,22,25 -sS -Pn -n --disable-arp-ping --packet-trace  
Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 14:56 CEST 
SENT (0.0278s) TCP 10.10.14.2:57347 > 10.129.2.28:22 S ttl=53 id=22412 iplen=44  seq=4092255222 win=1024 <mss 1460> 
SENT (0.0278s) TCP 10.10.14.2:57347 > 10.129.2.28:25 S ttl=50 id=62291 iplen=44  seq=4092255222 win=1024 <mss 1460> 
SENT (0.0278s) TCP 10.10.14.2:57347 > 10.129.2.28:21 S ttl=58 id=38696 iplen=44  seq=4092255222 win=1024 <mss 1460> 
RCVD (0.0329s) ICMP [10.129.2.28 > 10.10.14.2 Port 21 unreachable (type=3/code=3) ] IP [ttl=64 id=40884 iplen=72 ] 
RCVD (0.0341s) TCP 10.129.2.28:22 > 10.10.14.2:57347 SA ttl=64 id=0 iplen=44  seq=1153454414 win=64240 <mss 1460> 
RCVD (1.0386s) TCP 10.129.2.28:22 > 10.10.14.2:57347 SA ttl=64 id=0 iplen=44  seq=1153454414 win=64240 <mss 1460> 
SENT (1.1366s) TCP 10.10.14.2:57348 > 10.129.2.28:25 S ttl=44 id=6796 iplen=44  seq=4092320759 win=1024 <mss 1460> 
Nmap scan report for 10.129.2.28 
Host is up (0.0053s latency).  

PORT   STATE    SERVICE 
21/tcp filtered ftp 
22/tcp open     ssh 
25/tcp filtered smtp 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  
Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
```

#### ACK-Scan

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p 21,22,25 -sA -Pn -n --disable-arp-ping --packet-trace  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 14:57 CEST 
SENT (0.0422s) TCP 10.10.14.2:49343 > 10.129.2.28:21 A ttl=49 id=12381 iplen=40  seq=0 win=1024 
SENT (0.0423s) TCP 10.10.14.2:49343 > 10.129.2.28:22 A ttl=41 id=5146 iplen=40  seq=0 win=1024 
SENT (0.0423s) TCP 10.10.14.2:49343 > 10.129.2.28:25 A ttl=49 id=5800 iplen=40  seq=0 win=1024 
RCVD (0.1252s) ICMP [10.129.2.28 > 10.10.14.2 Port 21 unreachable (type=3/code=3) ] IP [ttl=64 id=55628 iplen=68 ] 
RCVD (0.1268s) TCP 10.129.2.28:22 > 10.10.14.2:49343 R ttl=64 id=0 iplen=40  seq=1660784500 win=0 
SENT (1.3837s) TCP 10.10.14.2:49344 > 10.129.2.28:25 A ttl=59 id=21915 iplen=40  seq=0 win=1024 
Nmap scan report for 10.129.2.28 
Host is up (0.083s latency).  

PORT   STATE      SERVICE 
21/tcp filtered   ftp 
22/tcp unfiltered ssh 
25/tcp filtered   smtp 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)  

Nmap done: 1 IP address (1 host up) scanned in 0.15 seconds`
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 21,22,25`|Scans only the specified ports.|
|`-sS`|Performs SYN scan on specified ports.|
|`-sA`|Performs ACK scan on specified ports.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|

Please pay attention to the RCVD packets and its set flag we receive from our target. With the SYN scan (`-sS`) our target tries to establish the TCP connection by sending a packet back with the SYN-ACK (`SA`) flags set and with the ACK scan (`-sA`) we get the `RST` flag because TCP port 22 is open. For the TCP port 25, we do not receive any packets back, which indicates that the packets will be dropped.

## Detect IDS/IPS

Unlike firewalls and their rules, the detection of IDS/IPS systems is much more difficult because these are passive traffic monitoring systems. `IDS systems` examine all connections between hosts. If the IDS finds packets containing the defined contents or specifications, the administrator is notified and takes appropriate action in the worst case.

`IPS systems` take measures configured by the administrator independently to prevent potential attacks automatically. It is essential to know that IDS and IPS are different applications and that IPS serves as a complement to IDS.

Several virtual private servers (`VPS`) with different IP addresses are recommended to determine whether such systems are on the target network during a penetration test. If the administrator detects such a potential attack on the target network, the first step is to block the IP address from which the potential attack comes. As a result, we will no longer be able to access the network using that IP address, and our Internet Service Provider (`ISP`) will be contacted and blocked from all access to the Internet.

- `IDS systems` alone are usually there to help administrators detect potential attacks on their network. They can then decide how to handle such connections. We can trigger certain security measures from an administrator, for example, by aggressively scanning a single port and its service. Based on whether specific security measures are taken, we can detect if the network has some monitoring applications or not.
    
- One method to determine whether such `IPS system` is present in the target network is to scan from a single host (`VPS`). If at any time this host is blocked and has no access to the target network, we know that the administrator has taken some security measures. Accordingly, we can continue our penetration test with another `VPS`.
    

Consequently, we know that we need to be quieter with our scans and, in the best case, disguise all interactions with the target network and its services.

## Decoys

There are cases in which administrators block specific subnets from different regions in principle. This prevents any access to the target network. Another example is when IPS should block us. For this reason, the Decoy scanning method (`-D`) is the right choice. With this method, Nmap generates various random IP addresses inserted into the IP header to disguise the origin of the packet sent. With this method, we can generate random (`RND`) a specific number (for example: `5`) of IP addresses separated by a colon (`:`). Our real IP address is then randomly placed between the generated IP addresses. In the next example, our real IP address is therefore placed in the second position. Another critical point is that the decoys must be alive. Otherwise, the service on the target may be unreachable due to SYN-flooding security mechanisms.

#### Scan by Using Decoys

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 16:14 CEST 
SENT (0.0378s) TCP 102.52.161.59:59289 > 10.129.2.28:80 S ttl=42 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460> 
SENT (0.0378s) TCP 10.10.14.2:59289 > 10.129.2.28:80 S ttl=59 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460> 
SENT (0.0379s) TCP 210.120.38.29:59289 > 10.129.2.28:80 S ttl=37 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460> 
SENT (0.0379s) TCP 191.6.64.171:59289 > 10.129.2.28:80 S ttl=38 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460> 
SENT (0.0379s) TCP 184.178.194.209:59289 > 10.129.2.28:80 S ttl=39 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460> 
SENT (0.0379s) TCP 43.21.121.33:59289 > 10.129.2.28:80 S ttl=55 id=29822 iplen=44  seq=3687542010 win=1024 <mss 1460> 
RCVD (0.1370s) TCP 10.129.2.28:80 > 10.10.14.2:59289 SA ttl=64 id=0 iplen=44  seq=4056111701 win=64240 <mss 1460> 
Nmap scan report for 10.129.2.28 Host is up (0.099s latency).  

PORT   STATE SERVICE 
80/tcp open  http 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.15 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 80`|Scans only the specified ports.|
|`-sS`|Performs SYN scan on specified ports.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`-D RND:5`|Generates five random IP addresses that indicates the source IP the connection comes from.|

The spoofed packets are often filtered out by ISPs and routers, even though they come from the same network range. Therefore, we can also specify our VPS servers' IP addresses and use them in combination with "`IP ID`" manipulation in the IP headers to scan the target.

Another scenario would be that only individual subnets would not have access to the server's specific services. So we can also manually specify the source IP address (`-S`) to test if we get better results with this one. Decoys can be used for SYN, ACK, ICMP scans, and OS detection scans. So let us look at such an example and determine which operating system it is most likely to be.

#### Testing Firewall Rule

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -n -Pn -p445 -O  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-22 01:23 CEST 
Nmap scan report for 10.129.2.28 
Host is up (0.032s latency).  

PORT    STATE    SERVICE 
445/tcp filtered microsoft-ds 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate) 
Too many fingerprints match this host to give specific OS details 
Network Distance: 1 hop  

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ . 
Nmap done: 1 IP address (1 host up) scanned in 3.14 seconds
```

#### Scan by Using Different Source IP

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-22 01:16 CEST 
Nmap scan report for 10.129.2.28 
Host is up (0.010s latency).  

PORT    STATE SERVICE 
445/tcp open  microsoft-ds 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate) 
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port 
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Linux 2.6.32 - 2.6.35 (94%), Linux 2.6.32 - 3.5 (94%) 
No exact OS matches for host (test conditions non-ideal). 
Network Distance: 1 hop  

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ . 
Nmap done: 1 IP address (1 host up) scanned in 4.11 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-n`|Disables DNS resolution.|
|`-Pn`|Disables ICMP Echo requests.|
|`-p 445`|Scans only the specified ports.|
|`-O`|Performs operation system detection scan.|
|`-S`|Scans the target by using different source IP address.|
|`10.129.2.200`|Specifies the source IP address.|
|`-e tun0`|Sends all requests through the specified interface.|

## DNS Proxying

By default, `Nmap` performs a reverse DNS resolution unless otherwise specified to find more important information about our target. These DNS queries are also passed in most cases because the given web server is supposed to be found and visited. The DNS queries are made over the `UDP port 53`. The `TCP port 53` was previously only used for the so-called "`Zone transfers`" between the DNS servers or data transfer larger than 512 bytes. More and more, this is changing due to IPv6 and DNSSEC expansions. These changes cause many DNS requests to be made via TCP port 53.

However, `Nmap` still gives us a way to specify DNS servers ourselves (`--dns-server <ns>,<ns>`). This method could be fundamental to us if we are in a demilitarized zone (`DMZ`). The company's DNS servers are usually more trusted than those from the Internet. So, for example, we could use them to interact with the hosts of the internal network. As another example, we can use `TCP port 53` as a source port (`--source-port`) for our scans. If the administrator uses the firewall to control this port and does not filter IDS/IPS properly, our TCP packets will be trusted and passed through.

#### SYN-Scan of a Filtered Port

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace  

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 22:50 CEST 
SENT (0.0417s) TCP 10.10.14.2:33436 > 10.129.2.28:50000 S ttl=41 id=21939 iplen=44  seq=736533153 win=1024 <mss 1460> 
SENT (1.0481s) TCP 10.10.14.2:33437 > 10.129.2.28:50000 S ttl=46 id=6446 iplen=44  seq=736598688 win=1024 <mss 1460> 
Nmap scan report for 10.129.2.28 
Host is up. 

PORT      STATE    SERVICE 
50000/tcp filtered ibm-db2  

Nmap done: 1 IP address (1 host up) scanned in 2.06 seconds
```

#### SYN-Scan From DNS Port

```
crimsonguard@htb[/htb]`$ sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53  

SENT (0.0482s) TCP 10.10.14.2:53 > 10.129.2.28:50000 S ttl=58 id=27470 iplen=44  seq=4003923435 win=1024 <mss 1460> 
RCVD (0.0608s) TCP 10.129.2.28:50000 > 10.10.14.2:53 SA ttl=64 id=0 iplen=44  seq=540635485 win=64240 <mss 1460> 
Nmap scan report for 10.129.2.28 
Host is up (0.013s latency).  

PORT      STATE SERVICE 
50000/tcp open  ibm-db2 
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
```

|**Scanning Options**|**Description**|
|---|---|
|`10.129.2.28`|Scans the specified target.|
|`-p 50000`|Scans only the specified ports.|
|`-sS`|Performs SYN scan on specified ports.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
|`--source-port 53`|Performs the scans from specified source port.|

Now that we have found out that the firewall accepts `TCP port 53`, it is very likely that IDS/IPS filters might also be configured much weaker than others. We can test this by trying to connect to this port by using `Netcat`.

#### Connect To The Filtered Port

```
crimsonguard@htb[/htb]`$ ncat -nv --source-port 53 10.129.2.28 50000  

Ncat: Version 7.80 ( https://nmap.org/ncat ) 
Ncat: Connected to 10.129.2.28:50000. 220 ProFTPd`
```

## Firewall and IDS/IPS Evasion Labs

In the next three sections, we get different scenarios to practice where we have to scan our target. Firewall rules and IDS/IPS protect the systems, so we need to use the techniques shown to bypass the firewall rules and do this as quiet as possible. Otherwise, we will be blocked by IPS.

# Firewall and IDS/IPS Evasion - Easy Lab

Now let's get practical. A company hired us to test their IT security defenses, including their `IDS` and `IPS` systems. Our client wants to increase their IT security and will, therefore, make specific improvements to their `IDS/IPS` systems after each successful test. We do not know, however, according to which guidelines these changes will be made. Our goal is to find out specific information from the given situations.

We are only ever provided with a machine protected by `IDS/IPS` systems and can be tested. For learning purposes and to get a feel for how `IDS/IPS` can behave, we have access to a status web page at:

![[Pasted image 20250202203122 1.png]]

This page shows us the number of alerts. We know that if we receive a specific amount of alerts, we will be `banned`. Therefore we have to test the target system as `quietly` as possible.

---
Question:
Target: 10.129.2.80

- Our client wants to know if we can identify which operating system their provided machine is running on. Submit the OS name as the answer.
	- ubuntu
```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sS -O -Pn 10.129.2.80                 
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 20:37 EST
Nmap scan report for 10.129.2.80
Host is up (0.030s latency).
Not shown: 869 closed tcp ports (reset), 128 filtered tcp ports (no-response)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
10001/tcp open  scp-config
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.57 seconds
```

# Firewall and IDS/IPS Evasion - Medium Lab

After we conducted the first test and submitted our results to our client, the administrators made some changes and improvements to the `IDS/IPS` and firewall. We could hear that the administrators were not satisfied with their previous configurations during the meeting, and they could see that the network traffic could be filtered more strictly.

Note: To successfully solve the exercise, we must use the UDP protocol on the VPN.

---
Question:
Target: 10.129.2.48

- After the configurations are transferred to the system, our client wants to know if it is possible to find out our target's DNS server version. Submit the DNS server version of the target as the answer.
	- HTB{GoTtgUnyze9Psw4vGjcuMpHRp}

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap 10.129.2.48 -p53 -sU -Pn -n --disable-arp-ping -sV -T1 --source-port 53 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 21:27 EST
Nmap scan report for 10.129.2.48
Host is up (0.029s latency).

PORT   STATE SERVICE VERSION
53/udp open  domain  (unknown banner: HTB{GoTtgUnyze9Psw4vGjcuMpHRp})
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-UDP:V=7.95%I=7%D=2/2%Time=67A02996%P=x86_64-pc-linux-gnu%r(DNSVe
SF:rsionBindReq,57,"\0\x06\x85\0\0\x01\0\x01\0\x01\0\0\x07version\x04bind\
SF:0\0\x10\0\x03\xc0\x0c\0\x10\0\x03\0\0\0\0\0\x1f\x1eHTB{GoTtgUnyze9Psw4v
SF:GjcuMpHRp}\xc0\x0c\0\x02\0\x03\0\0\0\0\0\x02\xc0\x0c")%r(DNSStatusReque
SF:st,C,"\0\0\x90\x04\0\0\0\0\0\0\0\0")%r(DNS-SD,101,"\0\0\x80\x80\0\x01\0
SF:\0\0\r\0\0\t_services\x07_dns-sd\x04_udp\x05local\0\0\x0c\0\x01\0\0\x02
SF:\0\x01\x006\xee\x80\0\x14\x01B\x0cROOT-SERVERS\x03NET\0\0\0\x02\0\x01\x
SF:006\xee\x80\0\x04\x01G\xc0;\0\0\x02\0\x01\x006\xee\x80\0\x04\x01M\xc0;\
SF:0\0\x02\0\x01\x006\xee\x80\0\x04\x01L\xc0;\0\0\x02\0\x01\x006\xee\x80\0
SF:\x04\x01H\xc0;\0\0\x02\0\x01\x006\xee\x80\0\x04\x01J\xc0;\0\0\x02\0\x01
SF:\x006\xee\x80\0\x04\x01I\xc0;\0\0\x02\0\x01\x006\xee\x80\0\x04\x01C\xc0
SF:;\0\0\x02\0\x01\x006\xee\x80\0\x04\x01D\xc0;\0\0\x02\0\x01\x006\xee\x80
SF:\0\x04\x01A\xc0;\0\0\x02\0\x01\x006\xee\x80\0\x04\x01K\xc0;\0\0\x02\0\x
SF:01\x006\xee\x80\0\x04\x01F\xc0;\0\0\x02\0\x01\x006\xee\x80\0\x04\x01E\x
SF:c0;")%r(RPCCheck,C,"r\xfe\x98\x01\0\0\0\0\0\0\0\0")%r(NBTStat,105,"\x80
SF:\xf0\x80\x90\0\x01\0\0\0\r\0\0\x20CKAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\0\0!
SF:\0\x01\0\0\x02\0\x01\x006\xee\x80\0\x14\x01D\x0cROOT-SERVERS\x03NET\0\0
SF:\0\x02\0\x01\x006\xee\x80\0\x04\x01B\xc0\?\0\0\x02\0\x01\x006\xee\x80\0
SF:\x04\x01K\xc0\?\0\0\x02\0\x01\x006\xee\x80\0\x04\x01J\xc0\?\0\0\x02\0\x
SF:01\x006\xee\x80\0\x04\x01A\xc0\?\0\0\x02\0\x01\x006\xee\x80\0\x04\x01E\
SF:xc0\?\0\0\x02\0\x01\x006\xee\x80\0\x04\x01L\xc0\?\0\0\x02\0\x01\x006\xe
SF:e\x80\0\x04\x01M\xc0\?\0\0\x02\0\x01\x006\xee\x80\0\x04\x01I\xc0\?\0\0\
SF:x02\0\x01\x006\xee\x80\0\x04\x01G\xc0\?\0\0\x02\0\x01\x006\xee\x80\0\x0
SF:4\x01H\xc0\?\0\0\x02\0\x01\x006\xee\x80\0\x04\x01F\xc0\?\0\0\x02\0\x01\
SF:x006\xee\x80\0\x04\x01C\xc0\?");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.23 seconds

```

---
# Firewall and IDS/IPS Evasion - Hard Lab

With our second test's help, our client was able to gain new insights and sent one of its administrators to a `training course` for `IDS/IPS` systems. As our client told us, the training would last `one week`. Now the administrator has taken all the necessary precautions and wants us to test this again because specific services must be changed, and the communication for the provided software had to be modified.

---
Question:
target:

Now our client wants to know if it is possible to find out the version of the running services. Identify the version of service our client was talking about and submit the flag as the answer.

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap 10.129.232.61 -p 22,80,50000 -sV -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53 -e tun0 -D RND:10
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 22:01 EST
SENT (0.2275s) TCP 75.139.108.79:53 > 10.129.232.61:80 S ttl=58 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2281s) TCP 103.21.208.131:53 > 10.129.232.61:80 S ttl=57 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2281s) TCP 10.10.14.3:53 > 10.129.232.61:80 S ttl=54 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2281s) TCP 11.11.215.31:53 > 10.129.232.61:80 S ttl=50 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2281s) TCP 126.157.30.78:53 > 10.129.232.61:80 S ttl=44 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2281s) TCP 130.230.92.4:53 > 10.129.232.61:80 S ttl=38 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 132.66.174.194:53 > 10.129.232.61:80 S ttl=42 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 31.70.242.93:53 > 10.129.232.61:80 S ttl=45 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 92.14.18.61:53 > 10.129.232.61:80 S ttl=56 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 124.234.125.253:53 > 10.129.232.61:80 S ttl=58 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 114.151.113.70:53 > 10.129.232.61:80 S ttl=43 id=27829 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 75.139.108.79:53 > 10.129.232.61:22 S ttl=43 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 103.21.208.131:53 > 10.129.232.61:22 S ttl=38 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 10.10.14.3:53 > 10.129.232.61:22 S ttl=41 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2282s) TCP 11.11.215.31:53 > 10.129.232.61:22 S ttl=45 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2283s) TCP 126.157.30.78:53 > 10.129.232.61:22 S ttl=40 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2283s) TCP 130.230.92.4:53 > 10.129.232.61:22 S ttl=56 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2283s) TCP 132.66.174.194:53 > 10.129.232.61:22 S ttl=37 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2283s) TCP 31.70.242.93:53 > 10.129.232.61:22 S ttl=45 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2283s) TCP 92.14.18.61:53 > 10.129.232.61:22 S ttl=49 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2283s) TCP 124.234.125.253:53 > 10.129.232.61:22 S ttl=45 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2294s) TCP 114.151.113.70:53 > 10.129.232.61:22 S ttl=49 id=33386 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 75.139.108.79:53 > 10.129.232.61:50000 S ttl=46 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 103.21.208.131:53 > 10.129.232.61:50000 S ttl=41 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 10.10.14.3:53 > 10.129.232.61:50000 S ttl=42 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 11.11.215.31:53 > 10.129.232.61:50000 S ttl=40 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 126.157.30.78:53 > 10.129.232.61:50000 S ttl=59 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 130.230.92.4:53 > 10.129.232.61:50000 S ttl=43 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 132.66.174.194:53 > 10.129.232.61:50000 S ttl=41 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 31.70.242.93:53 > 10.129.232.61:50000 S ttl=55 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2295s) TCP 92.14.18.61:53 > 10.129.232.61:50000 S ttl=42 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2296s) TCP 124.234.125.253:53 > 10.129.232.61:50000 S ttl=44 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
SENT (0.2296s) TCP 114.151.113.70:53 > 10.129.232.61:50000 S ttl=48 id=64809 iplen=44  seq=1215970825 win=1024 <mss 1460>
RCVD (0.2571s) TCP 10.129.232.61:80 > 10.10.14.3:53 SA ttl=63 id=0 iplen=44  seq=3367564691 win=64240 <mss 1340>
RCVD (0.2677s) TCP 10.129.232.61:50000 > 10.10.14.3:53 SA ttl=63 id=0 iplen=44  seq=2285185401 win=64240 <mss 1340>
RCVD (0.2678s) TCP 10.129.232.61:22 > 10.10.14.3:53 SA ttl=63 id=0 iplen=44  seq=3790033380 win=64240 <mss 1340>
NSOCK INFO [0.4120s] nsock_iod_new2(): nsock_iod_new (IOD #1)
NSOCK INFO [0.4130s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:22 (IOD #1) EID 8
NSOCK INFO [0.4130s] nsock_iod_new2(): nsock_iod_new (IOD #2)
NSOCK INFO [0.4130s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #2) EID 16
NSOCK INFO [0.4130s] nsock_iod_new2(): nsock_iod_new (IOD #3)
NSOCK INFO [0.4130s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:50000 (IOD #3) EID 24
NSOCK INFO [0.4440s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 8 [10.129.232.61:22]
Service scan sending probe NULL to 10.129.232.61:22 (tcp)
NSOCK INFO [0.4440s] nsock_read(): Read request from IOD #1 [10.129.232.61:22] (timeout: 6000ms) EID 34
NSOCK INFO [0.4440s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 16 [10.129.232.61:80]
Service scan sending probe NULL to 10.129.232.61:80 (tcp)
NSOCK INFO [0.4440s] nsock_read(): Read request from IOD #2 [10.129.232.61:80] (timeout: 6000ms) EID 42
NSOCK INFO [0.4850s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 34 [10.129.232.61:22] (41 bytes): SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.7..
Service scan hard match (Probe NULL matched with NULL line 3560): 10.129.232.61:22 is ssh.  Version: |OpenSSH|7.6p1 Ubuntu 4ubuntu0.7|Ubuntu Linux; protocol 2.0|
NSOCK INFO [0.4850s] nsock_iod_delete(): nsock_iod_delete (IOD #1)
NSOCK INFO [5.4180s] nsock_trace_handler_callback(): Callback: CONNECT TIMEOUT for EID 24 [10.129.232.61:50000]
NSOCK INFO [5.4180s] nsock_iod_delete(): nsock_iod_delete (IOD #3)
NSOCK INFO [6.4450s] nsock_trace_handler_callback(): Callback: READ TIMEOUT for EID 42 [10.129.232.61:80]
Service scan sending probe GetRequest to 10.129.232.61:80 (tcp)
NSOCK INFO [6.4450s] nsock_write(): Write request for 18 bytes to IOD #2 EID 51 [10.129.232.61:80]
NSOCK INFO [6.4450s] nsock_read(): Read request from IOD #2 [10.129.232.61:80] (timeout: 5000ms) EID 58
NSOCK INFO [6.4450s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 51 [10.129.232.61:80]
NSOCK INFO [6.4890s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 58 [10.129.232.61:80] (5312 bytes)
Service scan hard match (Probe GetRequest matched with GetRequest line 10693): 10.129.232.61:80 is http.  Version: |Apache httpd|2.4.29|(Ubuntu)|
NSOCK INFO [6.4890s] nsock_iod_delete(): nsock_iod_delete (IOD #2)
NSOCK INFO [6.5160s] nsock_iod_new2(): nsock_iod_new (IOD #1)
NSOCK INFO [6.5190s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #1) EID 8
NSOCK INFO [6.5190s] nsock_iod_new2(): nsock_iod_new (IOD #2)
NSOCK INFO [6.5190s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #2) EID 16
NSOCK INFO [6.5190s] nsock_iod_new2(): nsock_iod_new (IOD #3)
NSOCK INFO [6.5190s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:50000 (IOD #3) EID 24
NSOCK INFO [6.5190s] nsock_iod_new2(): nsock_iod_new (IOD #4)
NSOCK INFO [6.5190s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:50000 (IOD #4) EID 32
NSOCK INFO [6.5190s] nsock_iod_new2(): nsock_iod_new (IOD #5)
NSOCK INFO [6.5200s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #5) EID 40
NSOCK INFO [6.5530s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 8 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46888 > 10.129.232.61:80 | CONNECT
NSOCK INFO [6.5540s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 40 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46908 > 10.129.232.61:80 | CONNECT
NSOCK INFO [6.5540s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 16 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46904 > 10.129.232.61:80 | CONNECT
NSE: TCP 10.10.14.3:46888 > 10.129.232.61:80 | POST /sdk HTTP/1.1
Host: 10.129.232.61
Content-Length: 441
Connection: close
User-Agent: Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)

<soap:Envelope xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"><soap:Header><operationID>00000001-00000001</operationID></soap:Header><soap:Body><RetrieveServiceContent xmlns="urn:internalvim25"><_this xsi:type="ManagedObjectReference" type="ServiceInstance">ServiceInstance</_this></RetrieveServiceContent></soap:Body></soap:Envelope>
NSOCK INFO [6.5740s] nsock_write(): Write request for 617 bytes to IOD #1 EID 51 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46904 > 10.129.232.61:80 | 00000000: 47 45 54 20 2f 20 48 54 54 50 2f 31 2e 30 0d 0a GET / HTTP/1.0  
00000010: 0d 0a                                             

NSOCK INFO [6.5740s] nsock_write(): Write request for 18 bytes to IOD #2 EID 59 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46908 > 10.129.232.61:80 | 00000000: 47 45 54 20 2f 6e 6d 61 70 6c 6f 77 65 72 63 68 GET /nmaplowerch
00000010: 65 63 6b 31 37 33 38 35 35 31 37 30 36 20 48 54 eck1738551706 HT
00000020: 54 50 2f 31 2e 31 0d 0a 48 6f 73 74 3a 20 31 30 TP/1.1  Host: 10
00000030: 2e 31 32 39 2e 32 33 32 2e 36 31 0d 0a 43 6f 6e .129.232.61  Con
00000040: 6e 65 63 74 69 6f 6e 3a 20 63 6c 6f 73 65 0d 0a nection: close  
00000050: 55 73 65 72 2d 41 67 65 6e 74 3a 20 4d 6f 7a 69 User-Agent: Mozi
00000060: 6c 6c 61 2f 35 2e 30 20 28 63 6f 6d 70 61 74 69 lla/5.0 (compati
00000070: 62 6c 65 3b 20 4e 6d 61 70 20 53 63 72 69 70 74 ble; Nmap Script
00000080: 69 6e 67 20 45 6e 67 69 6e 65 3b 20 68 74 74 70 ing Engine; http
00000090: 73 3a 2f 2f 6e 6d 61 70 2e 6f 72 67 2f 62 6f 6f s://nmap.org/boo
000000a0: 6b 2f 6e 73 65 2e 68 74 6d 6c 29 0d 0a 0d 0a    k/nse.html)    

NSOCK INFO [6.5740s] nsock_write(): Write request for 175 bytes to IOD #5 EID 67 [10.129.232.61:80]
NSOCK INFO [6.5740s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 51 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46888 > 10.129.232.61:80 | SEND
NSOCK INFO [6.5740s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 59 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46904 > 10.129.232.61:80 | SEND
NSOCK INFO [6.5740s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 67 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46908 > 10.129.232.61:80 | SEND
NSOCK INFO [6.6280s] nsock_read(): Read request from IOD #1 [10.129.232.61:80] (timeout: 7000ms) EID 74
NSOCK INFO [6.6280s] nsock_read(): Read request from IOD #2 [10.129.232.61:80] (timeout: 7000ms) EID 82
NSOCK INFO [6.6290s] nsock_read(): Read request from IOD #5 [10.129.232.61:80] (timeout: 7000ms) EID 90
NSOCK INFO [6.6290s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 74 [10.129.232.61:80] (455 bytes)
NSE: TCP 10.10.14.3:46888 < 10.129.232.61:80 | 00000000: 48 54 54 50 2f 31 2e 31 20 34 30 34 20 4e 6f 74 HTTP/1.1 404 Not
00000010: 20 46 6f 75 6e 64 0d 0a 44 61 74 65 3a 20 4d 6f  Found  Date: Mo
00000020: 6e 2c 20 30 33 20 46 65 62 20 32 30 32 35 20 30 n, 03 Feb 2025 0
00000030: 33 3a 30 31 3a 34 37 20 47 4d 54 0d 0a 53 65 72 3:01:47 GMT  Ser
00000040: 76 65 72 3a 20 41 70 61 63 68 65 2f 32 2e 34 2e ver: Apache/2.4.
00000050: 32 39 20 28 55 62 75 6e 74 75 29 0d 0a 43 6f 6e 29 (Ubuntu)  Con
00000060: 74 65 6e 74 2d 4c 65 6e 67 74 68 3a 20 32 37 35 tent-Length: 275
00000070: 0d 0a 43 6f 6e 6e 65 63 74 69 6f 6e 3a 20 63 6c   Connection: cl
00000080: 6f 73 65 0d 0a 43 6f 6e 74 65 6e 74 2d 54 79 70 ose  Content-Typ
00000090: 65 3a 20 74 65 78 74 2f 68 74 6d 6c 3b 20 63 68 e: text/html; ch
000000a0: 61 72 73 65 74 3d 69 73 6f 2d 38 38 35 39 2d 31 arset=iso-8859-1
000000b0: 0d 0a 0d 0a 3c 21 44 4f 43 54 59 50 45 20 48 54     <!DOCTYPE HT
000000c0: 4d 4c 20 50 55 42 4c 49 43 20 22 2d 2f 2f 49 45 ML PUBLIC "-//IE
000000d0: 54 46 2f 2f 44 54 44 20 48 54 4d 4c 20 32 2e 30 TF//DTD HTML 2.0
000000e0: 2f 2f 45 4e 22 3e 0a 3c 68 74 6d 6c 3e 3c 68 65 //EN"> <html><he
000000f0: 61 64 3e 0a 3c 74 69 74 6c 65 3e 34 30 34 20 4e ad> <title>404 N
00000100: 6f 74 20 46 6f 75 6e 64 3c 2f 74 69 74 6c 65 3e ot Found</title>
00000110: 0a 3c 2f 68 65 61 64 3e 3c 62 6f 64 79 3e 0a 3c  </head><body> <
00000120: 68 31 3e 4e 6f 74 20 46 6f 75 6e 64 3c 2f 68 31 h1>Not Found</h1
00000130: 3e 0a 3c 70 3e 54 68 65 20 72 65 71 75 65 73 74 > <p>The request
00000140: 65 64 20 55 52 4c 20 77 61 73 20 6e 6f 74 20 66 ed URL was not f
00000150: 6f 75 6e 64 20 6f 6e 20 74 68 69 73 20 73 65 72 ound on this ser
00000160: 76 65 72 2e 3c 2f 70 3e 0a 3c 68 72 3e 0a 3c 61 ver.</p> <hr> <a
00000170: 64 64 72 65 73 73 3e 41 70 61 63 68 65 2f 32 2e ddress>Apache/2.
00000180: 34 2e 32 39 20 28 55 62 75 6e 74 75 29 20 53 65 4.29 (Ubuntu) Se
00000190: 72 76 65 72 20 61 74 20 31 30 2e 31 32 39 2e 32 rver at 10.129.2
000001a0: 33 32 2e 36 31 20 50 6f 72 74 20 38 30 3c 2f 61 32.61 Port 80</a
000001b0: 64 64 72 65 73 73 3e 0a 3c 2f 62 6f 64 79 3e 3c ddress> </body><
000001c0: 2f 68 74 6d 6c 3e 0a                            /html> 

NSOCK INFO [6.6290s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 82 [10.129.232.61:80] (11595 bytes)
NSE: TCP 10.10.14.3:46904 < 10.129.232.61:80 | HTTP/1.1 200 OK
Date: Mon, 03 Feb 2025 03:01:47 GMT
Server: Apache/2.4.29 (Ubuntu)
Last-Modified: Thu, 10 Sep 2020 02:14:12 GMT
ETag: "2c39-5aeec1fc9d59d"
Accept-Ranges: bytes
Content-Length: 11321
Vary: Accept-Encoding
Connection: close
Content-Type: text/html


<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <!--
    Modified from the Debian original for Ubuntu
    Last updated: 2014-03-19
    See: https://launchpad.net/bugs/1288690
  -->
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works</title>
    <style type="text/css" media="screen">
  * {
    margin: 0px 0px 0px 0px;
    padding: 0px 0px 0px 0px;
  }

  body, html {
    padding: 3px 3px 3px 3px;

    background-color: #D8DBE2;

    font-family: Verdana, sans-serif;
    font-size: 11pt;
    text-align: center;
  }

  div.main_page {
    position: relative;
    display: table;

    width: 800px;

    margin-bottom: 3px;
    margin-left: auto;
    margin-right: auto;
    padding: 0px 0px 0px 0px;

    border-width: 2px;
    border-color: #212738;
    border-style: solid;

    background-color: #FFFFFF;

    text-align: center;
  }

  div.page_header {
    height: 99px;
    width: 100%;

    background-color: #F5F6F7;
  }

  div.page_header span {
    margin: 15px 0px 0px 50px;

    font-size: 180%;
    font-weight: bold;
  }

  div.page_header img {
    margin: 3px 0px 0px 40px;

    border: 0px 0px 0px;
  }

  div.table_of_contents {
    clear: left;

    min-width: 200px;

    margin: 3px 3px 3px 3px;

    background-color: #FFFFFF;

    text-align: left;
  }

  div.table_of_contents_item {
    clear: left;

    width: 100%;

    margin: 4px 0px 0px 0px;

    background-color: #FFFFFF;

    color: #000000;
    text-align: left;
  }

  div.table_of_contents_item a {
    margin: 6px 0px 0px 6px;
  }

  div.content_section {
    margin: 3px 3px 3px 3px;

    background-color: #FFFFFF;

    text-align: left;
  }

  div.content_section_text {
    padding: 4px 8px 4px 8px;

    color: #000000;
    font-size: 100%;
  }

  div.content_section_text pre {
    margin: 8px 0px 8px 0px;
    padding: 8px 8px 8px 8px;

    border-width: 1px;
    border-style: dotted;
    border-color: #000000;

    background-color: #F5F6F7;

    font-style: italic;
  }

  div.content_section_text p {
    margin-bottom: 6px;
  }

  div.content_section_text ul, div.content_section_text li {
    padding: 4px 8px 4px 16px;
  }

  div.section_header {
    padding: 3px 6px 3px 6px;

    background-color: #8E9CB2;

    color: #FFFFFF;
    font-weight: bold;
    font-size: 112%;
    text-align: center;
  }

  div.section_header_red {
    background-color: #CD214F;
  }

  div.section_header_grey {
    background-color: #9F9386;
  }

  .floating_element {
    position: relative;
    float: left;
  }

  div.table_of_contents_item a,
  div.content_section_text a {
    text-decoration: none;
    font-weight: bold;
  }

  div.table_of_contents_item a:link,
  div.table_of_contents_item a:visited,
  div.table_of_contents_item a:active {
    color: #000000;
  }

  div.table_of_contents_item a:hover {
    background-color: #000000;

    color: #FFFFFF;
  }

  div.content_section_text a:link,
  div.content_section_text a:visited,
   div.content_section_text a:active {
    background-color: #DCDFE6;

    color: #000000;
  }

  div.content_section_text a:hover {
    background-color: #000000;

    color: #DCDFE6;
  }

  div.validator {
  }
    </style>
  </head>
  <body>
    <div class="main_page">
      <div class="page_header floating_element">
        <img src="/icons/ubuntu-logo.png" alt="Ubuntu Logo" class="floating_element"/>
        <span class="floating_element">
          Apache2 Ubuntu Default Page
        </span>
      </div>
<!--      <div class="table_of_contents floating_element">
        <div class="section_header section_header_grey">
          TABLE OF CONTENTS
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#about">About</a>
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#changes">Changes</a>
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#scope">Scope</a>
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#files">Config files</a>
        </div>
      </div>
-->
      <div class="content_section floating_element">


        <div class="section_header section_header_red">
          <div id="about"></div>
          It works!
        </div>
        <div class="content_section_text">
          <p>
                This is the default welcome page used to test the correct 
                operation of the Apache2 server after installation on Ubuntu systems.
                It is based on the equivalent page on Debian, from which the Ubuntu Apache
                packaging is derived.
                If you can read this page, it means that the Apache HTTP server installed at
                this site is working properly. You should <b>replace this file</b> (located at
                <tt>/var/www/html/index.html</tt>) before continuing to operate your HTTP server.
          </p>


          <p>
                If you are a normal user of this web site and don't know what this page is
                about, this probably means that the site is currently unavailable due to
                maintenance.
                If the problem persists, please contact the site's administrator.
          </p>

        </div>
        <div class="section_header">
          <div id="changes"></div>
                Configuration Overview
        </div>
        <div class="content_section_text">
          <p>
                Ubuntu's Apache2 default configuration is different from the
                upstream default configuration, and split into several files optimized for
                interaction with Ubuntu tools. The configuration system is
                <b>fully documented in
                /usr/share/doc/apache2/README.Debian.gz</b>. Refer to this for the full
                documentation. Documentation for the web server itself can be
                found by accessing the <a href="/manual">manual</a> if the <tt>apache2-doc</tt>
                package was installed on this server.

          </p>
          <p>
                The configuration layout for an Apache2 web server installation on Ubuntu systems is as follows:
          </p>
          <pre>
/etc/apache2/
|-- apache2.conf
|       `--  ports.conf
|-- mods-enabled
|       |-- *.load
|       `-- *.conf
|-- conf-enabled
|       `-- *.conf
|-- sites-enabled
|       `-- *.conf
          </pre>
          <ul>
                        <li>
                           <tt>apache2.conf</tt> is the main configuration
                           file. It puts the pieces together by including all remaining configuration
                           files when starting up the web server.
                        </li>

                        <li>
                           <tt>ports.conf</tt> is always included from the
                           main configuration file. It is used to determine the listening ports for
                           incoming connections, and this file can be customized anytime.
                        </li>

                        <li>
                           Configuration files in the <tt>mods-enabled/</tt>,
                           <tt>conf-enabled/</tt> and <tt>sites-enabled/</tt> directories contain
                           particular configuration snippets which manage modules, global configuration
                           fragments, or virtual host configurations, respectively.
                        </li>

                        <li>
                           They are activated by symlinking available
                           configuration files from their respective
                           *-available/ counterparts. These should be managed
                           by using our helpers
                           <tt>
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2enmod">a2enmod</a>,
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2dismod">a2dismod</a>,
                           </tt>
                           <tt>
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2ensite">a2ensite</a>,
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2dissite">a2dissite</a>,
                            </tt>
                                and
                           <tt>
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2enconf">a2enconf</a>,
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2disconf">a2disconf</a>
                           </tt>. See their respective man pages for detailed information.
                        </li>

                        <li>
                           The binary is called apache2. Due to the use of
                           environment variables, in the default configuration, apache2 needs to be
                           started/stopped with <tt>/etc/init.d/apache2</tt> or <tt>apache2ctl</tt>.
                           <b>Calling <tt>/usr/bin/apache2</tt> directly will not work</b> with the
                           default configuration.
                        </li>
          </ul>
        </div>

        <div class="section_header">
            <div id="docroot"></div>
                Document Roots
        </div>

        <div class="content_section_text">
            <p>
                By default, Ubuntu does not allow access through the web browser to
                <em>any</em> file apart of those located in <tt>/var/www</tt>,
                <a href="http://httpd.apache.org/docs/2.4/mod/mod_userdir.html">public_html</a>
                directories (when enabled) and <tt>/usr/share</tt> (for web
                applications). If your site is using a web document root
                located elsewhere (such as in <tt>/srv</tt>) you may need to whitelist your
                document root directory in <tt>/etc/apache2/apache2.conf</tt>.
            </p>
            <p>
                The default Ubuntu document root is <tt>/var/www/html</tt>. You
                can make your own virtual hosts under /var/www. This is different
                to previous releases which provides better security out of the box.
            </p>
        </div>

        <div class="section_header">
          <div id="bugs"></div>
                Reporting Problems
        </div>
        <div class="content_section_text">
          <p>
                Please use the <tt>ubuntu-bug</tt> tool to report bugs in the
                Apache2 package with Ubuntu. However, check <a
                href="https://bugs.launchpad.net/ubuntu/+source/apache2">existing
                bug reports</a> before reporting a new bug.
          </p>
          <p>
                Please report bugs specific to modules (such as PHP and others)
                to respective packages, not to the web server itself.
          </p>
        </div>




      </div>
    </div>
    <div class="validator">
    </div>
  </body>
</html>


NSOCK INFO [6.6290s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 90 [10.129.232.61:80] (455 bytes)
NSE: TCP 10.10.14.3:46908 < 10.129.232.61:80 | 00000000: 48 54 54 50 2f 31 2e 31 20 34 30 34 20 4e 6f 74 HTTP/1.1 404 Not
00000010: 20 46 6f 75 6e 64 0d 0a 44 61 74 65 3a 20 4d 6f  Found  Date: Mo
00000020: 6e 2c 20 30 33 20 46 65 62 20 32 30 32 35 20 30 n, 03 Feb 2025 0
00000030: 33 3a 30 31 3a 34 37 20 47 4d 54 0d 0a 53 65 72 3:01:47 GMT  Ser
00000040: 76 65 72 3a 20 41 70 61 63 68 65 2f 32 2e 34 2e ver: Apache/2.4.
00000050: 32 39 20 28 55 62 75 6e 74 75 29 0d 0a 43 6f 6e 29 (Ubuntu)  Con
00000060: 74 65 6e 74 2d 4c 65 6e 67 74 68 3a 20 32 37 35 tent-Length: 275
00000070: 0d 0a 43 6f 6e 6e 65 63 74 69 6f 6e 3a 20 63 6c   Connection: cl
00000080: 6f 73 65 0d 0a 43 6f 6e 74 65 6e 74 2d 54 79 70 ose  Content-Typ
00000090: 65 3a 20 74 65 78 74 2f 68 74 6d 6c 3b 20 63 68 e: text/html; ch
000000a0: 61 72 73 65 74 3d 69 73 6f 2d 38 38 35 39 2d 31 arset=iso-8859-1
000000b0: 0d 0a 0d 0a 3c 21 44 4f 43 54 59 50 45 20 48 54     <!DOCTYPE HT
000000c0: 4d 4c 20 50 55 42 4c 49 43 20 22 2d 2f 2f 49 45 ML PUBLIC "-//IE
000000d0: 54 46 2f 2f 44 54 44 20 48 54 4d 4c 20 32 2e 30 TF//DTD HTML 2.0
000000e0: 2f 2f 45 4e 22 3e 0a 3c 68 74 6d 6c 3e 3c 68 65 //EN"> <html><he
000000f0: 61 64 3e 0a 3c 74 69 74 6c 65 3e 34 30 34 20 4e ad> <title>404 N
00000100: 6f 74 20 46 6f 75 6e 64 3c 2f 74 69 74 6c 65 3e ot Found</title>
00000110: 0a 3c 2f 68 65 61 64 3e 3c 62 6f 64 79 3e 0a 3c  </head><body> <
00000120: 68 31 3e 4e 6f 74 20 46 6f 75 6e 64 3c 2f 68 31 h1>Not Found</h1
00000130: 3e 0a 3c 70 3e 54 68 65 20 72 65 71 75 65 73 74 > <p>The request
00000140: 65 64 20 55 52 4c 20 77 61 73 20 6e 6f 74 20 66 ed URL was not f
00000150: 6f 75 6e 64 20 6f 6e 20 74 68 69 73 20 73 65 72 ound on this ser
00000160: 76 65 72 2e 3c 2f 70 3e 0a 3c 68 72 3e 0a 3c 61 ver.</p> <hr> <a
00000170: 64 64 72 65 73 73 3e 41 70 61 63 68 65 2f 32 2e ddress>Apache/2.
00000180: 34 2e 32 39 20 28 55 62 75 6e 74 75 29 20 53 65 4.29 (Ubuntu) Se
00000190: 72 76 65 72 20 61 74 20 31 30 2e 31 32 39 2e 32 rver at 10.129.2
000001a0: 33 32 2e 36 31 20 50 6f 72 74 20 38 30 3c 2f 61 32.61 Port 80</a
000001b0: 64 64 72 65 73 73 3e 0a 3c 2f 62 6f 64 79 3e 3c ddress> </body><
000001c0: 2f 68 74 6d 6c 3e 0a                            /html> 

NSE: TCP 10.10.14.3:46888 > 10.129.232.61:80 | CLOSE
NSOCK INFO [6.6780s] nsock_iod_delete(): nsock_iod_delete (IOD #1)
NSE: TCP 10.10.14.3:46904 > 10.129.232.61:80 | CLOSE
NSOCK INFO [6.6780s] nsock_iod_delete(): nsock_iod_delete (IOD #2)
NSE: TCP 10.10.14.3:46908 > 10.129.232.61:80 | CLOSE
NSOCK INFO [6.6780s] nsock_iod_delete(): nsock_iod_delete (IOD #5)
NSOCK INFO [6.6780s] nsock_iod_new2(): nsock_iod_new (IOD #6)
NSOCK INFO [6.6820s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #6) EID 96
NSOCK INFO [6.7110s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 96 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46918 > 10.129.232.61:80 | CONNECT
NSE: TCP 10.10.14.3:46918 > 10.129.232.61:80 | 00000000: 47 45 54 20 2f 65 76 6f 78 2f 61 62 6f 75 74 20 GET /evox/about 
00000010: 48 54 54 50 2f 31 2e 31 0d 0a 48 6f 73 74 3a 20 HTTP/1.1  Host: 
00000020: 31 30 2e 31 32 39 2e 32 33 32 2e 36 31 0d 0a 43 10.129.232.61  C
00000030: 6f 6e 6e 65 63 74 69 6f 6e 3a 20 63 6c 6f 73 65 onnection: close
00000040: 0d 0a 55 73 65 72 2d 41 67 65 6e 74 3a 20 4d 6f   User-Agent: Mo
00000050: 7a 69 6c 6c 61 2f 35 2e 30 20 28 63 6f 6d 70 61 zilla/5.0 (compa
00000060: 74 69 62 6c 65 3b 20 4e 6d 61 70 20 53 63 72 69 tible; Nmap Scri
00000070: 70 74 69 6e 67 20 45 6e 67 69 6e 65 3b 20 68 74 pting Engine; ht
00000080: 74 70 73 3a 2f 2f 6e 6d 61 70 2e 6f 72 67 2f 62 tps://nmap.org/b
00000090: 6f 6f 6b 2f 6e 73 65 2e 68 74 6d 6c 29 0d 0a 0d ook/nse.html)   
000000a0: 0a                                               

NSOCK INFO [6.7340s] nsock_write(): Write request for 161 bytes to IOD #6 EID 107 [10.129.232.61:80]
NSOCK INFO [6.7340s] nsock_iod_new2(): nsock_iod_new (IOD #7)
NSOCK INFO [6.7350s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #7) EID 112
NSOCK INFO [6.7350s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 107 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46918 > 10.129.232.61:80 | SEND
NSOCK INFO [6.7660s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 112 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46930 > 10.129.232.61:80 | CONNECT
NSOCK INFO [6.7850s] nsock_read(): Read request from IOD #6 [10.129.232.61:80] (timeout: 7000ms) EID 122
NSE: TCP 10.10.14.3:46930 > 10.129.232.61:80 | 00000000: 47 45 54 20 2f 48 4e 41 50 31 20 48 54 54 50 2f GET /HNAP1 HTTP/
00000010: 31 2e 31 0d 0a 48 6f 73 74 3a 20 31 30 2e 31 32 1.1  Host: 10.12
00000020: 39 2e 32 33 32 2e 36 31 0d 0a 43 6f 6e 6e 65 63 9.232.61  Connec
00000030: 74 69 6f 6e 3a 20 63 6c 6f 73 65 0d 0a 55 73 65 tion: close  Use
00000040: 72 2d 41 67 65 6e 74 3a 20 4d 6f 7a 69 6c 6c 61 r-Agent: Mozilla
00000050: 2f 35 2e 30 20 28 63 6f 6d 70 61 74 69 62 6c 65 /5.0 (compatible
00000060: 3b 20 4e 6d 61 70 20 53 63 72 69 70 74 69 6e 67 ; Nmap Scripting
00000070: 20 45 6e 67 69 6e 65 3b 20 68 74 74 70 73 3a 2f  Engine; https:/
00000080: 2f 6e 6d 61 70 2e 6f 72 67 2f 62 6f 6f 6b 2f 6e /nmap.org/book/n
00000090: 73 65 2e 68 74 6d 6c 29 0d 0a 0d 0a             se.html)    

NSOCK INFO [6.7850s] nsock_write(): Write request for 156 bytes to IOD #7 EID 131 [10.129.232.61:80]
NSOCK INFO [6.7860s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 122 [10.129.232.61:80] (455 bytes)
NSE: TCP 10.10.14.3:46918 < 10.129.232.61:80 | 00000000: 48 54 54 50 2f 31 2e 31 20 34 30 34 20 4e 6f 74 HTTP/1.1 404 Not
00000010: 20 46 6f 75 6e 64 0d 0a 44 61 74 65 3a 20 4d 6f  Found  Date: Mo
00000020: 6e 2c 20 30 33 20 46 65 62 20 32 30 32 35 20 30 n, 03 Feb 2025 0
00000030: 33 3a 30 31 3a 34 37 20 47 4d 54 0d 0a 53 65 72 3:01:47 GMT  Ser
00000040: 76 65 72 3a 20 41 70 61 63 68 65 2f 32 2e 34 2e ver: Apache/2.4.
00000050: 32 39 20 28 55 62 75 6e 74 75 29 0d 0a 43 6f 6e 29 (Ubuntu)  Con
00000060: 74 65 6e 74 2d 4c 65 6e 67 74 68 3a 20 32 37 35 tent-Length: 275
00000070: 0d 0a 43 6f 6e 6e 65 63 74 69 6f 6e 3a 20 63 6c   Connection: cl
00000080: 6f 73 65 0d 0a 43 6f 6e 74 65 6e 74 2d 54 79 70 ose  Content-Typ
00000090: 65 3a 20 74 65 78 74 2f 68 74 6d 6c 3b 20 63 68 e: text/html; ch
000000a0: 61 72 73 65 74 3d 69 73 6f 2d 38 38 35 39 2d 31 arset=iso-8859-1
000000b0: 0d 0a 0d 0a 3c 21 44 4f 43 54 59 50 45 20 48 54     <!DOCTYPE HT
000000c0: 4d 4c 20 50 55 42 4c 49 43 20 22 2d 2f 2f 49 45 ML PUBLIC "-//IE
000000d0: 54 46 2f 2f 44 54 44 20 48 54 4d 4c 20 32 2e 30 TF//DTD HTML 2.0
000000e0: 2f 2f 45 4e 22 3e 0a 3c 68 74 6d 6c 3e 3c 68 65 //EN"> <html><he
000000f0: 61 64 3e 0a 3c 74 69 74 6c 65 3e 34 30 34 20 4e ad> <title>404 N
00000100: 6f 74 20 46 6f 75 6e 64 3c 2f 74 69 74 6c 65 3e ot Found</title>
00000110: 0a 3c 2f 68 65 61 64 3e 3c 62 6f 64 79 3e 0a 3c  </head><body> <
00000120: 68 31 3e 4e 6f 74 20 46 6f 75 6e 64 3c 2f 68 31 h1>Not Found</h1
00000130: 3e 0a 3c 70 3e 54 68 65 20 72 65 71 75 65 73 74 > <p>The request
00000140: 65 64 20 55 52 4c 20 77 61 73 20 6e 6f 74 20 66 ed URL was not f
00000150: 6f 75 6e 64 20 6f 6e 20 74 68 69 73 20 73 65 72 ound on this ser
00000160: 76 65 72 2e 3c 2f 70 3e 0a 3c 68 72 3e 0a 3c 61 ver.</p> <hr> <a
00000170: 64 64 72 65 73 73 3e 41 70 61 63 68 65 2f 32 2e ddress>Apache/2.
00000180: 34 2e 32 39 20 28 55 62 75 6e 74 75 29 20 53 65 4.29 (Ubuntu) Se
00000190: 72 76 65 72 20 61 74 20 31 30 2e 31 32 39 2e 32 rver at 10.129.2
000001a0: 33 32 2e 36 31 20 50 6f 72 74 20 38 30 3c 2f 61 32.61 Port 80</a
000001b0: 64 64 72 65 73 73 3e 0a 3c 2f 62 6f 64 79 3e 3c ddress> </body><
000001c0: 2f 68 74 6d 6c 3e 0a                            /html> 

NSOCK INFO [6.7860s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 131 [10.129.232.61:80]
NSE: TCP 10.10.14.3:46930 > 10.129.232.61:80 | SEND
NSE: TCP 10.10.14.3:46918 > 10.129.232.61:80 | CLOSE
NSOCK INFO [6.8360s] nsock_iod_delete(): nsock_iod_delete (IOD #6)
NSOCK INFO [6.8360s] nsock_read(): Read request from IOD #7 [10.129.232.61:80] (timeout: 7000ms) EID 138
NSOCK INFO [6.8360s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 138 [10.129.232.61:80] (455 bytes)
NSE: TCP 10.10.14.3:46930 < 10.129.232.61:80 | 00000000: 48 54 54 50 2f 31 2e 31 20 34 30 34 20 4e 6f 74 HTTP/1.1 404 Not
00000010: 20 46 6f 75 6e 64 0d 0a 44 61 74 65 3a 20 4d 6f  Found  Date: Mo
00000020: 6e 2c 20 30 33 20 46 65 62 20 32 30 32 35 20 30 n, 03 Feb 2025 0
00000030: 33 3a 30 31 3a 34 37 20 47 4d 54 0d 0a 53 65 72 3:01:47 GMT  Ser
00000040: 76 65 72 3a 20 41 70 61 63 68 65 2f 32 2e 34 2e ver: Apache/2.4.
00000050: 32 39 20 28 55 62 75 6e 74 75 29 0d 0a 43 6f 6e 29 (Ubuntu)  Con
00000060: 74 65 6e 74 2d 4c 65 6e 67 74 68 3a 20 32 37 35 tent-Length: 275
00000070: 0d 0a 43 6f 6e 6e 65 63 74 69 6f 6e 3a 20 63 6c   Connection: cl
00000080: 6f 73 65 0d 0a 43 6f 6e 74 65 6e 74 2d 54 79 70 ose  Content-Typ
00000090: 65 3a 20 74 65 78 74 2f 68 74 6d 6c 3b 20 63 68 e: text/html; ch
000000a0: 61 72 73 65 74 3d 69 73 6f 2d 38 38 35 39 2d 31 arset=iso-8859-1
000000b0: 0d 0a 0d 0a 3c 21 44 4f 43 54 59 50 45 20 48 54     <!DOCTYPE HT
000000c0: 4d 4c 20 50 55 42 4c 49 43 20 22 2d 2f 2f 49 45 ML PUBLIC "-//IE
000000d0: 54 46 2f 2f 44 54 44 20 48 54 4d 4c 20 32 2e 30 TF//DTD HTML 2.0
000000e0: 2f 2f 45 4e 22 3e 0a 3c 68 74 6d 6c 3e 3c 68 65 //EN"> <html><he
000000f0: 61 64 3e 0a 3c 74 69 74 6c 65 3e 34 30 34 20 4e ad> <title>404 N
00000100: 6f 74 20 46 6f 75 6e 64 3c 2f 74 69 74 6c 65 3e ot Found</title>
00000110: 0a 3c 2f 68 65 61 64 3e 3c 62 6f 64 79 3e 0a 3c  </head><body> <
00000120: 68 31 3e 4e 6f 74 20 46 6f 75 6e 64 3c 2f 68 31 h1>Not Found</h1
00000130: 3e 0a 3c 70 3e 54 68 65 20 72 65 71 75 65 73 74 > <p>The request
00000140: 65 64 20 55 52 4c 20 77 61 73 20 6e 6f 74 20 66 ed URL was not f
00000150: 6f 75 6e 64 20 6f 6e 20 74 68 69 73 20 73 65 72 ound on this ser
00000160: 76 65 72 2e 3c 2f 70 3e 0a 3c 68 72 3e 0a 3c 61 ver.</p> <hr> <a
00000170: 64 64 72 65 73 73 3e 41 70 61 63 68 65 2f 32 2e ddress>Apache/2.
00000180: 34 2e 32 39 20 28 55 62 75 6e 74 75 29 20 53 65 4.29 (Ubuntu) Se
00000190: 72 76 65 72 20 61 74 20 31 30 2e 31 32 39 2e 32 rver at 10.129.2
000001a0: 33 32 2e 36 31 20 50 6f 72 74 20 38 30 3c 2f 61 32.61 Port 80</a
000001b0: 64 64 72 65 73 73 3e 0a 3c 2f 62 6f 64 79 3e 3c ddress> </body><
000001c0: 2f 68 74 6d 6c 3e 0a                            /html> 

NSE: TCP 10.10.14.3:46930 > 10.129.232.61:80 | CLOSE
NSOCK INFO [6.8870s] nsock_iod_delete(): nsock_iod_delete (IOD #7)
NSOCK INFO [7.5190s] nsock_trace_handler_callback(): Callback: CONNECT TIMEOUT for EID 32 [10.129.232.61:50000]
NSE: TCP 10.10.14.3:53240 > 10.129.232.61:50000 | CONNECT
NSE: TCP 10.10.14.3:53240 > 10.129.232.61:50000 | CLOSE
NSOCK INFO [7.5540s] nsock_iod_delete(): nsock_iod_delete (IOD #4)
NSOCK INFO [36.5190s] nsock_trace_handler_callback(): Callback: CONNECT TIMEOUT for EID 24 [10.129.232.61:50000]
NSE: TCP 10.10.14.3:53238 > 10.129.232.61:50000 | CONNECT
NSE: TCP 10.10.14.3:53238 > 10.129.232.61:50000 | CLOSE
NSOCK INFO [36.5200s] nsock_iod_delete(): nsock_iod_delete (IOD #3)
NSOCK INFO [36.5210s] nsock_iod_new2(): nsock_iod_new (IOD #8)
NSOCK INFO [36.5230s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #8) EID 144
NSOCK INFO [36.5520s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 144 [10.129.232.61:80]
NSE: TCP 10.10.14.3:54312 > 10.129.232.61:80 | CONNECT
NSE: TCP 10.10.14.3:54312 > 10.129.232.61:80 | 00000000: 47 45 54 20 2f 20 48 54 54 50 2f 31 2e 30 0d 0a GET / HTTP/1.0  
00000010: 0d 0a                                             

NSOCK INFO [36.5540s] nsock_write(): Write request for 18 bytes to IOD #8 EID 155 [10.129.232.61:80]
NSOCK INFO [36.5540s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 155 [10.129.232.61:80]
NSE: TCP 10.10.14.3:54312 > 10.129.232.61:80 | SEND
NSOCK INFO [36.5570s] nsock_read(): Read request from IOD #8 [10.129.232.61:80] (timeout: 7000ms) EID 162
NSOCK INFO [36.5860s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 162 [10.129.232.61:80] (2656 bytes)
NSE: TCP 10.10.14.3:54312 < 10.129.232.61:80 | 00000000: 48 54 54 50 2f 31 2e 31 20 32 30 30 20 4f 4b 0d HTTP/1.1 200 OK 
00000010: 0a 44 61 74 65 3a 20 4d 6f 6e 2c 20 30 33 20 46  Date: Mon, 03 F
00000020: 65 62 20 32 30 32 35 20 30 33 3a 30 32 3a 31 37 eb 2025 03:02:17
00000030: 20 47 4d 54 0d 0a 53 65 72 76 65 72 3a 20 41 70  GMT  Server: Ap
00000040: 61 63 68 65 2f 32 2e 34 2e 32 39 20 28 55 62 75 ache/2.4.29 (Ubu
00000050: 6e 74 75 29 0d 0a 4c 61 73 74 2d 4d 6f 64 69 66 ntu)  Last-Modif
00000060: 69 65 64 3a 20 54 68 75 2c 20 31 30 20 53 65 70 ied: Thu, 10 Sep
00000070: 20 32 30 32 30 20 30 32 3a 31 34 3a 31 32 20 47  2020 02:14:12 G
00000080: 4d 54 0d 0a 45 54 61 67 3a 20 22 32 63 33 39 2d MT  ETag: "2c39-
00000090: 35 61 65 65 63 31 66 63 39 64 35 39 64 22 0d 0a 5aeec1fc9d59d"  
000000a0: 41 63 63 65 70 74 2d 52 61 6e 67 65 73 3a 20 62 Accept-Ranges: b
000000b0: 79 74 65 73 0d 0a 43 6f 6e 74 65 6e 74 2d 4c 65 ytes  Content-Le
000000c0: 6e 67 74 68 3a 20 31 31 33 32 31 0d 0a 56 61 72 ngth: 11321  Var
000000d0: 79 3a 20 41 63 63 65 70 74 2d 45 6e 63 6f 64 69 y: Accept-Encodi
000000e0: 6e 67 0d 0a 43 6f 6e 6e 65 63 74 69 6f 6e 3a 20 ng  Connection: 
000000f0: 63 6c 6f 73 65 0d 0a 43 6f 6e 74 65 6e 74 2d 54 close  Content-T
00000100: 79 70 65 3a 20 74 65 78 74 2f 68 74 6d 6c 0d 0a ype: text/html  
00000110: 0d 0a 0a 3c 21 44 4f 43 54 59 50 45 20 68 74 6d    <!DOCTYPE htm
00000120: 6c 20 50 55 42 4c 49 43 20 22 2d 2f 2f 57 33 43 l PUBLIC "-//W3C
00000130: 2f 2f 44 54 44 20 58 48 54 4d 4c 20 31 2e 30 20 //DTD XHTML 1.0 
00000140: 54 72 61 6e 73 69 74 69 6f 6e 61 6c 2f 2f 45 4e Transitional//EN
00000150: 22 20 22 68 74 74 70 3a 2f 2f 77 77 77 2e 77 33 " "http://www.w3
00000160: 2e 6f 72 67 2f 54 52 2f 78 68 74 6d 6c 31 2f 44 .org/TR/xhtml1/D
00000170: 54 44 2f 78 68 74 6d 6c 31 2d 74 72 61 6e 73 69 TD/xhtml1-transi
00000180: 74 69 6f 6e 61 6c 2e 64 74 64 22 3e 0a 3c 68 74 tional.dtd"> <ht
00000190: 6d 6c 20 78 6d 6c 6e 73 3d 22 68 74 74 70 3a 2f ml xmlns="http:/
000001a0: 2f 77 77 77 2e 77 33 2e 6f 72 67 2f 31 39 39 39 /www.w3.org/1999
000001b0: 2f 78 68 74 6d 6c 22 3e 0a 20 20 3c 21 2d 2d 0a /xhtml">   <!-- 
000001c0: 20 20 20 20 4d 6f 64 69 66 69 65 64 20 66 72 6f     Modified fro
000001d0: 6d 20 74 68 65 20 44 65 62 69 61 6e 20 6f 72 69 m the Debian ori
000001e0: 67 69 6e 61 6c 20 66 6f 72 20 55 62 75 6e 74 75 ginal for Ubuntu
000001f0: 0a 20 20 20 20 4c 61 73 74 20 75 70 64 61 74 65      Last update
00000200: 64 3a 20 32 30 31 34 2d 30 33 2d 31 39 0a 20 20 d: 2014-03-19   
00000210: 20 20 53 65 65 3a 20 68 74 74 70 73 3a 2f 2f 6c   See: https://l
00000220: 61 75 6e 63 68 70 61 64 2e 6e 65 74 2f 62 75 67 aunchpad.net/bug
00000230: 73 2f 31 32 38 38 36 39 30 0a 20 20 2d 2d 3e 0a s/1288690   --> 
00000240: 20 20 3c 68 65 61 64 3e 0a 20 20 20 20 3c 6d 65   <head>     <me
00000250: 74 61 20 68 74 74 70 2d 65 71 75 69 76 3d 22 43 ta http-equiv="C
00000260: 6f 6e 74 65 6e 74 2d 54 79 70 65 22 20 63 6f 6e ontent-Type" con
00000270: 74 65 6e 74 3d 22 74 65 78 74 2f 68 74 6d 6c 3b tent="text/html;
00000280: 20 63 68 61 72 73 65 74 3d 55 54 46 2d 38 22 20  charset=UTF-8" 
00000290: 2f 3e 0a 20 20 20 20 3c 74 69 74 6c 65 3e 41 70 />     <title>Ap
000002a0: 61 63 68 65 32 20 55 62 75 6e 74 75 20 44 65 66 ache2 Ubuntu Def
000002b0: 61 75 6c 74 20 50 61 67 65 3a 20 49 74 20 77 6f ault Page: It wo
000002c0: 72 6b 73 3c 2f 74 69 74 6c 65 3e 0a 20 20 20 20 rks</title>     
000002d0: 3c 73 74 79 6c 65 20 74 79 70 65 3d 22 74 65 78 <style type="tex
000002e0: 74 2f 63 73 73 22 20 6d 65 64 69 61 3d 22 73 63 t/css" media="sc
000002f0: 72 65 65 6e 22 3e 0a 20 20 2a 20 7b 0a 20 20 20 reen">   * {    
00000300: 20 6d 61 72 67 69 6e 3a 20 30 70 78 20 30 70 78  margin: 0px 0px
00000310: 20 30 70 78 20 30 70 78 3b 0a 20 20 20 20 70 61  0px 0px;     pa
00000320: 64 64 69 6e 67 3a 20 30 70 78 20 30 70 78 20 30 dding: 0px 0px 0
00000330: 70 78 20 30 70 78 3b 0a 20 20 7d 0a 0a 20 20 62 px 0px;   }    b
00000340: 6f 64 79 2c 20 68 74 6d 6c 20 7b 0a 20 20 20 20 ody, html {     
00000350: 70 61 64 64 69 6e 67 3a 20 33 70 78 20 33 70 78 padding: 3px 3px
00000360: 20 33 70 78 20 33 70 78 3b 0a 0a 20 20 20 20 62  3px 3px;      b
00000370: 61 63 6b 67 72 6f 75 6e 64 2d 63 6f 6c 6f 72 3a ackground-color:
00000380: 20 23 44 38 44 42 45 32 3b 0a 0a 20 20 20 20 66  #D8DBE2;      f
00000390: 6f 6e 74 2d 66 61 6d 69 6c 79 3a 20 56 65 72 64 ont-family: Verd
000003a0: 61 6e 61 2c 20 73 61 6e 73 2d 73 65 72 69 66 3b ana, sans-serif;
000003b0: 0a 20 20 20 20 66 6f 6e 74 2d 73 69 7a 65 3a 20      font-size: 
000003c0: 31 31 70 74 3b 0a 20 20 20 20 74 65 78 74 2d 61 11pt;     text-a
000003d0: 6c 69 67 6e 3a 20 63 65 6e 74 65 72 3b 0a 20 20 lign: center;   
000003e0: 7d 0a 0a 20 20 64 69 76 2e 6d 61 69 6e 5f 70 61 }    div.main_pa
000003f0: 67 65 20 7b 0a 20 20 20 20 70 6f 73 69 74 69 6f ge {     positio
00000400: 6e 3a 20 72 65 6c 61 74 69 76 65 3b 0a 20 20 20 n: relative;    
00000410: 20 64 69 73 70 6c 61 79 3a 20 74 61 62 6c 65 3b  display: table;
00000420: 0a 0a 20 20 20 20 77 69 64 74 68 3a 20 38 30 30       width: 800
00000430: 70 78 3b 0a 0a 20 20 20 20 6d 61 72 67 69 6e 2d px;      margin-
00000440: 62 6f 74 74 6f 6d 3a 20 33 70 78 3b 0a 20 20 20 bottom: 3px;    
00000450: 20 6d 61 72 67 69 6e 2d 6c 65 66 74 3a 20 61 75  margin-left: au
00000460: 74 6f 3b 0a 20 20 20 20 6d 61 72 67 69 6e 2d 72 to;     margin-r
00000470: 69 67 68 74 3a 20 61 75 74 6f 3b 0a 20 20 20 20 ight: auto;     
00000480: 70 61 64 64 69 6e 67 3a 20 30 70 78 20 30 70 78 padding: 0px 0px
00000490: 20 30 70 78 20 30 70 78 3b 0a 0a 20 20 20 20 62  0px 0px;      b
000004a0: 6f 72 64 65 72 2d 77 69 64 74 68 3a 20 32 70 78 order-width: 2px
000004b0: 3b 0a 20 20 20 20 62 6f 72 64 65 72 2d 63 6f 6c ;     border-col
000004c0: 6f 72 3a 20 23 32 31 32 37 33 38 3b 0a 20 20 20 or: #212738;    
000004d0: 20 62 6f 72 64 65 72 2d 73 74 79 6c 65 3a 20 73  border-style: s
000004e0: 6f 6c 69 64 3b 0a 0a 20 20 20 20 62 61 63 6b 67 olid;      backg
000004f0: 72 6f 75 6e 64 2d 63 6f 6c 6f 72 3a 20 23 46 46 round-color: #FF
00000500: 46 46 46 46 3b 0a 0a 20 20 20 20 74 65 78 74 2d FFFF;      text-
00000510: 61 6c 69 67 6e 3a 20 63 65 6e 74 65 72 3b 0a 20 align: center;  
00000520: 20 7d 0a 0a 20 20 64 69 76 2e 70 61 67 65 5f 68  }    div.page_h
00000530: 65 61 64 65 72 20 7b 0a 20 20 20 20 68 65 69 67 eader {     heig
00000540: 68 74 3a 20 39 39 70 78 3b 0a 20 20 20 20 77 69 ht: 99px;     wi
00000550: 64 74 68 3a 20 31 30 30 25 3b 0a 0a 20 20 20 20 dth: 100%;      
00000560: 62 61 63 6b 67 72 6f 75 6e 64 2d 63 6f 6c 6f 72 background-color
00000570: 3a 20 23 46 35 46 36 46 37 3b 0a 20 20 7d 0a 0a : #F5F6F7;   }  
00000580: 20 20 64 69 76 2e 70 61 67 65 5f 68 65 61 64 65   div.page_heade
00000590: 72 20 73 70 61 6e 20 7b 0a 20 20 20 20 6d 61 72 r span {     mar
000005a0: 67 69 6e 3a 20 31 35 70 78 20 30 70 78 20 30 70 gin: 15px 0px 0p
000005b0: 78 20 35 30 70 78 3b 0a 0a 20 20 20 20 66 6f 6e x 50px;      fon
000005c0: 74 2d 73 69 7a 65 3a 20 31 38 30 25 3b 0a 20 20 t-size: 180%;   
000005d0: 20 20 66 6f 6e 74 2d 77 65 69 67 68 74 3a 20 62   font-weight: b
000005e0: 6f 6c 64 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 2e old;   }    div.
000005f0: 70 61 67 65 5f 68 65 61 64 65 72 20 69 6d 67 20 page_header img 
00000600: 7b 0a 20 20 20 20 6d 61 72 67 69 6e 3a 20 33 70 {     margin: 3p
00000610: 78 20 30 70 78 20 30 70 78 20 34 30 70 78 3b 0a x 0px 0px 40px; 
00000620: 0a 20 20 20 20 62 6f 72 64 65 72 3a 20 30 70 78      border: 0px
00000630: 20 30 70 78 20 30 70 78 3b 0a 20 20 7d 0a 0a 20  0px 0px;   }   
00000640: 20 64 69 76 2e 74 61 62 6c 65 5f 6f 66 5f 63 6f  div.table_of_co
00000650: 6e 74 65 6e 74 73 20 7b 0a 20 20 20 20 63 6c 65 ntents {     cle
00000660: 61 72 3a 20 6c 65 66 74 3b 0a 0a 20 20 20 20 6d ar: left;      m
00000670: 69 6e 2d 77 69 64 74 68 3a 20 32 30 30 70 78 3b in-width: 200px;
00000680: 0a 0a 20 20 20 20 6d 61 72 67 69 6e 3a 20 33 70       margin: 3p
00000690: 78 20 33 70 78 20 33 70 78 20 33 70 78 3b 0a 0a x 3px 3px 3px;  
000006a0: 20 20 20 20 62 61 63 6b 67 72 6f 75 6e 64 2d 63     background-c
000006b0: 6f 6c 6f 72 3a 20 23 46 46 46 46 46 46 3b 0a 0a olor: #FFFFFF;  
000006c0: 20 20 20 20 74 65 78 74 2d 61 6c 69 67 6e 3a 20     text-align: 
000006d0: 6c 65 66 74 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 left;   }    div
000006e0: 2e 74 61 62 6c 65 5f 6f 66 5f 63 6f 6e 74 65 6e .table_of_conten
000006f0: 74 73 5f 69 74 65 6d 20 7b 0a 20 20 20 20 63 6c ts_item {     cl
00000700: 65 61 72 3a 20 6c 65 66 74 3b 0a 0a 20 20 20 20 ear: left;      
00000710: 77 69 64 74 68 3a 20 31 30 30 25 3b 0a 0a 20 20 width: 100%;    
00000720: 20 20 6d 61 72 67 69 6e 3a 20 34 70 78 20 30 70   margin: 4px 0p
00000730: 78 20 30 70 78 20 30 70 78 3b 0a 0a 20 20 20 20 x 0px 0px;      
00000740: 62 61 63 6b 67 72 6f 75 6e 64 2d 63 6f 6c 6f 72 background-color
00000750: 3a 20 23 46 46 46 46 46 46 3b 0a 0a 20 20 20 20 : #FFFFFF;      
00000760: 63 6f 6c 6f 72 3a 20 23 30 30 30 30 30 30 3b 0a color: #000000; 
00000770: 20 20 20 20 74 65 78 74 2d 61 6c 69 67 6e 3a 20     text-align: 
00000780: 6c 65 66 74 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 left;   }    div
00000790: 2e 74 61 62 6c 65 5f 6f 66 5f 63 6f 6e 74 65 6e .table_of_conten
000007a0: 74 73 5f 69 74 65 6d 20 61 20 7b 0a 20 20 20 20 ts_item a {     
000007b0: 6d 61 72 67 69 6e 3a 20 36 70 78 20 30 70 78 20 margin: 6px 0px 
000007c0: 30 70 78 20 36 70 78 3b 0a 20 20 7d 0a 0a 20 20 0px 6px;   }    
000007d0: 64 69 76 2e 63 6f 6e 74 65 6e 74 5f 73 65 63 74 div.content_sect
000007e0: 69 6f 6e 20 7b 0a 20 20 20 20 6d 61 72 67 69 6e ion {     margin
000007f0: 3a 20 33 70 78 20 33 70 78 20 33 70 78 20 33 70 : 3px 3px 3px 3p
00000800: 78 3b 0a 0a 20 20 20 20 62 61 63 6b 67 72 6f 75 x;      backgrou
00000810: 6e 64 2d 63 6f 6c 6f 72 3a 20 23 46 46 46 46 46 nd-color: #FFFFF
00000820: 46 3b 0a 0a 20 20 20 20 74 65 78 74 2d 61 6c 69 F;      text-ali
00000830: 67 6e 3a 20 6c 65 66 74 3b 0a 20 20 7d 0a 0a 20 gn: left;   }   
00000840: 20 64 69 76 2e 63 6f 6e 74 65 6e 74 5f 73 65 63  div.content_sec
00000850: 74 69 6f 6e 5f 74 65 78 74 20 7b 0a 20 20 20 20 tion_text {     
00000860: 70 61 64 64 69 6e 67 3a 20 34 70 78 20 38 70 78 padding: 4px 8px
00000870: 20 34 70 78 20 38 70 78 3b 0a 0a 20 20 20 20 63  4px 8px;      c
00000880: 6f 6c 6f 72 3a 20 23 30 30 30 30 30 30 3b 0a 20 olor: #000000;  
00000890: 20 20 20 66 6f 6e 74 2d 73 69 7a 65 3a 20 31 30    font-size: 10
000008a0: 30 25 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 2e 63 0%;   }    div.c
000008b0: 6f 6e 74 65 6e 74 5f 73 65 63 74 69 6f 6e 5f 74 ontent_section_t
000008c0: 65 78 74 20 70 72 65 20 7b 0a 20 20 20 20 6d 61 ext pre {     ma
000008d0: 72 67 69 6e 3a 20 38 70 78 20 30 70 78 20 38 70 rgin: 8px 0px 8p
000008e0: 78 20 30 70 78 3b 0a 20 20 20 20 70 61 64 64 69 x 0px;     paddi
000008f0: 6e 67 3a 20 38 70 78 20 38 70 78 20 38 70 78 20 ng: 8px 8px 8px 
00000900: 38 70 78 3b 0a 0a 20 20 20 20 62 6f 72 64 65 72 8px;      border
00000910: 2d 77 69 64 74 68 3a 20 31 70 78 3b 0a 20 20 20 -width: 1px;    
00000920: 20 62 6f 72 64 65 72 2d 73 74 79 6c 65 3a 20 64  border-style: d
00000930: 6f 74 74 65 64 3b 0a 20 20 20 20 62 6f 72 64 65 otted;     borde
00000940: 72 2d 63 6f 6c 6f 72 3a 20 23 30 30 30 30 30 30 r-color: #000000
00000950: 3b 0a 0a 20 20 20 20 62 61 63 6b 67 72 6f 75 6e ;      backgroun
00000960: 64 2d 63 6f 6c 6f 72 3a 20 23 46 35 46 36 46 37 d-color: #F5F6F7
00000970: 3b 0a 0a 20 20 20 20 66 6f 6e 74 2d 73 74 79 6c ;      font-styl
00000980: 65 3a 20 69 74 61 6c 69 63 3b 0a 20 20 7d 0a 0a e: italic;   }  
00000990: 20 20 64 69 76 2e 63 6f 6e 74 65 6e 74 5f 73 65   div.content_se
000009a0: 63 74 69 6f 6e 5f 74 65 78 74 20 70 20 7b 0a 20 ction_text p {  
000009b0: 20 20 20 6d 61 72 67 69 6e 2d 62 6f 74 74 6f 6d    margin-bottom
000009c0: 3a 20 36 70 78 3b 0a 20 20 7d 0a 0a 20 20 64 69 : 6px;   }    di
000009d0: 76 2e 63 6f 6e 74 65 6e 74 5f 73 65 63 74 69 6f v.content_sectio
000009e0: 6e 5f 74 65 78 74 20 75 6c 2c 20 64 69 76 2e 63 n_text ul, div.c
000009f0: 6f 6e 74 65 6e 74 5f 73 65 63 74 69 6f 6e 5f 74 ontent_section_t
00000a00: 65 78 74 20 6c 69 20 7b 0a 20 20 20 20 70 61 64 ext li {     pad
00000a10: 64 69 6e 67 3a 20 34 70 78 20 38 70 78 20 34 70 ding: 4px 8px 4p
00000a20: 78 20 31 36 70 78 3b 0a 20 20 7d 0a 0a 20 20 64 x 16px;   }    d
00000a30: 69 76 2e 73 65 63 74 69 6f 6e 5f 68 65 61 64 65 iv.section_heade
00000a40: 72 20 7b 0a 20 20 20 20 70 61 64 64 69 6e 67 3a r {     padding:
00000a50: 20 33 70 78 20 36 70 78 20 33 70 78 20 36 70 78  3px 6px 3px 6px

NSE: TCP 10.10.14.3:54312 > 10.129.232.61:80 | CLOSE
NSOCK INFO [36.5880s] nsock_iod_delete(): nsock_iod_delete (IOD #8)
NSOCK INFO [36.5880s] nsock_iod_new2(): nsock_iod_new (IOD #9)
NSOCK INFO [36.5890s] nsock_connect_tcp(): TCP connection requested to 10.129.232.61:80 (IOD #9) EID 168
NSOCK INFO [36.6320s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 168 [10.129.232.61:80]
NSE: TCP 10.10.14.3:54326 > 10.129.232.61:80 | CONNECT
NSE: TCP 10.10.14.3:54326 > 10.129.232.61:80 | 00000000: 47 45 54 20 2f 20 48 54 54 50 2f 31 2e 31 0d 0a GET / HTTP/1.1  
00000010: 48 6f 73 74 3a 20 31 30 2e 31 32 39 2e 32 33 32 Host: 10.129.232
00000020: 2e 36 31 0d 0a 0d 0a                            .61    

NSOCK INFO [36.6350s] nsock_write(): Write request for 39 bytes to IOD #9 EID 179 [10.129.232.61:80]
NSOCK INFO [36.6350s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 179 [10.129.232.61:80]
NSE: TCP 10.10.14.3:54326 > 10.129.232.61:80 | SEND
NSOCK INFO [36.6370s] nsock_read(): Read request from IOD #9 [10.129.232.61:80] (timeout: 7000ms) EID 186
NSOCK INFO [36.6680s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 186 [10.129.232.61:80] (2656 bytes)
NSE: TCP 10.10.14.3:54326 < 10.129.232.61:80 | 00000000: 48 54 54 50 2f 31 2e 31 20 32 30 30 20 4f 4b 0d HTTP/1.1 200 OK 
00000010: 0a 44 61 74 65 3a 20 4d 6f 6e 2c 20 30 33 20 46  Date: Mon, 03 F
00000020: 65 62 20 32 30 32 35 20 30 33 3a 30 32 3a 31 37 eb 2025 03:02:17
00000030: 20 47 4d 54 0d 0a 53 65 72 76 65 72 3a 20 41 70  GMT  Server: Ap
00000040: 61 63 68 65 2f 32 2e 34 2e 32 39 20 28 55 62 75 ache/2.4.29 (Ubu
00000050: 6e 74 75 29 0d 0a 4c 61 73 74 2d 4d 6f 64 69 66 ntu)  Last-Modif
00000060: 69 65 64 3a 20 54 68 75 2c 20 31 30 20 53 65 70 ied: Thu, 10 Sep
00000070: 20 32 30 32 30 20 30 32 3a 31 34 3a 31 32 20 47  2020 02:14:12 G
00000080: 4d 54 0d 0a 45 54 61 67 3a 20 22 32 63 33 39 2d MT  ETag: "2c39-
00000090: 35 61 65 65 63 31 66 63 39 64 35 39 64 22 0d 0a 5aeec1fc9d59d"  
000000a0: 41 63 63 65 70 74 2d 52 61 6e 67 65 73 3a 20 62 Accept-Ranges: b
000000b0: 79 74 65 73 0d 0a 43 6f 6e 74 65 6e 74 2d 4c 65 ytes  Content-Le
000000c0: 6e 67 74 68 3a 20 31 31 33 32 31 0d 0a 56 61 72 ngth: 11321  Var
000000d0: 79 3a 20 41 63 63 65 70 74 2d 45 6e 63 6f 64 69 y: Accept-Encodi
000000e0: 6e 67 0d 0a 43 6f 6e 74 65 6e 74 2d 54 79 70 65 ng  Content-Type
000000f0: 3a 20 74 65 78 74 2f 68 74 6d 6c 0d 0a 0d 0a 0a : text/html     
00000100: 3c 21 44 4f 43 54 59 50 45 20 68 74 6d 6c 20 50 <!DOCTYPE html P
00000110: 55 42 4c 49 43 20 22 2d 2f 2f 57 33 43 2f 2f 44 UBLIC "-//W3C//D
00000120: 54 44 20 58 48 54 4d 4c 20 31 2e 30 20 54 72 61 TD XHTML 1.0 Tra
00000130: 6e 73 69 74 69 6f 6e 61 6c 2f 2f 45 4e 22 20 22 nsitional//EN" "
00000140: 68 74 74 70 3a 2f 2f 77 77 77 2e 77 33 2e 6f 72 http://www.w3.or
00000150: 67 2f 54 52 2f 78 68 74 6d 6c 31 2f 44 54 44 2f g/TR/xhtml1/DTD/
00000160: 78 68 74 6d 6c 31 2d 74 72 61 6e 73 69 74 69 6f xhtml1-transitio
00000170: 6e 61 6c 2e 64 74 64 22 3e 0a 3c 68 74 6d 6c 20 nal.dtd"> <html 
00000180: 78 6d 6c 6e 73 3d 22 68 74 74 70 3a 2f 2f 77 77 xmlns="http://ww
00000190: 77 2e 77 33 2e 6f 72 67 2f 31 39 39 39 2f 78 68 w.w3.org/1999/xh
000001a0: 74 6d 6c 22 3e 0a 20 20 3c 21 2d 2d 0a 20 20 20 tml">   <!--    
000001b0: 20 4d 6f 64 69 66 69 65 64 20 66 72 6f 6d 20 74  Modified from t
000001c0: 68 65 20 44 65 62 69 61 6e 20 6f 72 69 67 69 6e he Debian origin
000001d0: 61 6c 20 66 6f 72 20 55 62 75 6e 74 75 0a 20 20 al for Ubuntu   
000001e0: 20 20 4c 61 73 74 20 75 70 64 61 74 65 64 3a 20   Last updated: 
000001f0: 32 30 31 34 2d 30 33 2d 31 39 0a 20 20 20 20 53 2014-03-19     S
00000200: 65 65 3a 20 68 74 74 70 73 3a 2f 2f 6c 61 75 6e ee: https://laun
00000210: 63 68 70 61 64 2e 6e 65 74 2f 62 75 67 73 2f 31 chpad.net/bugs/1
00000220: 32 38 38 36 39 30 0a 20 20 2d 2d 3e 0a 20 20 3c 288690   -->   <
00000230: 68 65 61 64 3e 0a 20 20 20 20 3c 6d 65 74 61 20 head>     <meta 
00000240: 68 74 74 70 2d 65 71 75 69 76 3d 22 43 6f 6e 74 http-equiv="Cont
00000250: 65 6e 74 2d 54 79 70 65 22 20 63 6f 6e 74 65 6e ent-Type" conten
00000260: 74 3d 22 74 65 78 74 2f 68 74 6d 6c 3b 20 63 68 t="text/html; ch
00000270: 61 72 73 65 74 3d 55 54 46 2d 38 22 20 2f 3e 0a arset=UTF-8" /> 
00000280: 20 20 20 20 3c 74 69 74 6c 65 3e 41 70 61 63 68     <title>Apach
00000290: 65 32 20 55 62 75 6e 74 75 20 44 65 66 61 75 6c e2 Ubuntu Defaul
000002a0: 74 20 50 61 67 65 3a 20 49 74 20 77 6f 72 6b 73 t Page: It works
000002b0: 3c 2f 74 69 74 6c 65 3e 0a 20 20 20 20 3c 73 74 </title>     <st
000002c0: 79 6c 65 20 74 79 70 65 3d 22 74 65 78 74 2f 63 yle type="text/c
000002d0: 73 73 22 20 6d 65 64 69 61 3d 22 73 63 72 65 65 ss" media="scree
000002e0: 6e 22 3e 0a 20 20 2a 20 7b 0a 20 20 20 20 6d 61 n">   * {     ma
000002f0: 72 67 69 6e 3a 20 30 70 78 20 30 70 78 20 30 70 rgin: 0px 0px 0p
00000300: 78 20 30 70 78 3b 0a 20 20 20 20 70 61 64 64 69 x 0px;     paddi
00000310: 6e 67 3a 20 30 70 78 20 30 70 78 20 30 70 78 20 ng: 0px 0px 0px 
00000320: 30 70 78 3b 0a 20 20 7d 0a 0a 20 20 62 6f 64 79 0px;   }    body
00000330: 2c 20 68 74 6d 6c 20 7b 0a 20 20 20 20 70 61 64 , html {     pad
00000340: 64 69 6e 67 3a 20 33 70 78 20 33 70 78 20 33 70 ding: 3px 3px 3p
00000350: 78 20 33 70 78 3b 0a 0a 20 20 20 20 62 61 63 6b x 3px;      back
00000360: 67 72 6f 75 6e 64 2d 63 6f 6c 6f 72 3a 20 23 44 ground-color: #D
00000370: 38 44 42 45 32 3b 0a 0a 20 20 20 20 66 6f 6e 74 8DBE2;      font
00000380: 2d 66 61 6d 69 6c 79 3a 20 56 65 72 64 61 6e 61 -family: Verdana
00000390: 2c 20 73 61 6e 73 2d 73 65 72 69 66 3b 0a 20 20 , sans-serif;   
000003a0: 20 20 66 6f 6e 74 2d 73 69 7a 65 3a 20 31 31 70   font-size: 11p
000003b0: 74 3b 0a 20 20 20 20 74 65 78 74 2d 61 6c 69 67 t;     text-alig
000003c0: 6e 3a 20 63 65 6e 74 65 72 3b 0a 20 20 7d 0a 0a n: center;   }  
000003d0: 20 20 64 69 76 2e 6d 61 69 6e 5f 70 61 67 65 20   div.main_page 
000003e0: 7b 0a 20 20 20 20 70 6f 73 69 74 69 6f 6e 3a 20 {     position: 
000003f0: 72 65 6c 61 74 69 76 65 3b 0a 20 20 20 20 64 69 relative;     di
00000400: 73 70 6c 61 79 3a 20 74 61 62 6c 65 3b 0a 0a 20 splay: table;   
00000410: 20 20 20 77 69 64 74 68 3a 20 38 30 30 70 78 3b    width: 800px;
00000420: 0a 0a 20 20 20 20 6d 61 72 67 69 6e 2d 62 6f 74       margin-bot
00000430: 74 6f 6d 3a 20 33 70 78 3b 0a 20 20 20 20 6d 61 tom: 3px;     ma
00000440: 72 67 69 6e 2d 6c 65 66 74 3a 20 61 75 74 6f 3b rgin-left: auto;
00000450: 0a 20 20 20 20 6d 61 72 67 69 6e 2d 72 69 67 68      margin-righ
00000460: 74 3a 20 61 75 74 6f 3b 0a 20 20 20 20 70 61 64 t: auto;     pad
00000470: 64 69 6e 67 3a 20 30 70 78 20 30 70 78 20 30 70 ding: 0px 0px 0p
00000480: 78 20 30 70 78 3b 0a 0a 20 20 20 20 62 6f 72 64 x 0px;      bord
00000490: 65 72 2d 77 69 64 74 68 3a 20 32 70 78 3b 0a 20 er-width: 2px;  
000004a0: 20 20 20 62 6f 72 64 65 72 2d 63 6f 6c 6f 72 3a    border-color:
000004b0: 20 23 32 31 32 37 33 38 3b 0a 20 20 20 20 62 6f  #212738;     bo
000004c0: 72 64 65 72 2d 73 74 79 6c 65 3a 20 73 6f 6c 69 rder-style: soli
000004d0: 64 3b 0a 0a 20 20 20 20 62 61 63 6b 67 72 6f 75 d;      backgrou
000004e0: 6e 64 2d 63 6f 6c 6f 72 3a 20 23 46 46 46 46 46 nd-color: #FFFFF
000004f0: 46 3b 0a 0a 20 20 20 20 74 65 78 74 2d 61 6c 69 F;      text-ali
00000500: 67 6e 3a 20 63 65 6e 74 65 72 3b 0a 20 20 7d 0a gn: center;   } 
00000510: 0a 20 20 64 69 76 2e 70 61 67 65 5f 68 65 61 64    div.page_head
00000520: 65 72 20 7b 0a 20 20 20 20 68 65 69 67 68 74 3a er {     height:
00000530: 20 39 39 70 78 3b 0a 20 20 20 20 77 69 64 74 68  99px;     width
00000540: 3a 20 31 30 30 25 3b 0a 0a 20 20 20 20 62 61 63 : 100%;      bac
00000550: 6b 67 72 6f 75 6e 64 2d 63 6f 6c 6f 72 3a 20 23 kground-color: #
00000560: 46 35 46 36 46 37 3b 0a 20 20 7d 0a 0a 20 20 64 F5F6F7;   }    d
00000570: 69 76 2e 70 61 67 65 5f 68 65 61 64 65 72 20 73 iv.page_header s
00000580: 70 61 6e 20 7b 0a 20 20 20 20 6d 61 72 67 69 6e pan {     margin
00000590: 3a 20 31 35 70 78 20 30 70 78 20 30 70 78 20 35 : 15px 0px 0px 5
000005a0: 30 70 78 3b 0a 0a 20 20 20 20 66 6f 6e 74 2d 73 0px;      font-s
000005b0: 69 7a 65 3a 20 31 38 30 25 3b 0a 20 20 20 20 66 ize: 180%;     f
000005c0: 6f 6e 74 2d 77 65 69 67 68 74 3a 20 62 6f 6c 64 ont-weight: bold
000005d0: 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 2e 70 61 67 ;   }    div.pag
000005e0: 65 5f 68 65 61 64 65 72 20 69 6d 67 20 7b 0a 20 e_header img {  
000005f0: 20 20 20 6d 61 72 67 69 6e 3a 20 33 70 78 20 30    margin: 3px 0
00000600: 70 78 20 30 70 78 20 34 30 70 78 3b 0a 0a 20 20 px 0px 40px;    
00000610: 20 20 62 6f 72 64 65 72 3a 20 30 70 78 20 30 70   border: 0px 0p
00000620: 78 20 30 70 78 3b 0a 20 20 7d 0a 0a 20 20 64 69 x 0px;   }    di
00000630: 76 2e 74 61 62 6c 65 5f 6f 66 5f 63 6f 6e 74 65 v.table_of_conte
00000640: 6e 74 73 20 7b 0a 20 20 20 20 63 6c 65 61 72 3a nts {     clear:
00000650: 20 6c 65 66 74 3b 0a 0a 20 20 20 20 6d 69 6e 2d  left;      min-
00000660: 77 69 64 74 68 3a 20 32 30 30 70 78 3b 0a 0a 20 width: 200px;   
00000670: 20 20 20 6d 61 72 67 69 6e 3a 20 33 70 78 20 33    margin: 3px 3
00000680: 70 78 20 33 70 78 20 33 70 78 3b 0a 0a 20 20 20 px 3px 3px;     
00000690: 20 62 61 63 6b 67 72 6f 75 6e 64 2d 63 6f 6c 6f  background-colo
000006a0: 72 3a 20 23 46 46 46 46 46 46 3b 0a 0a 20 20 20 r: #FFFFFF;     
000006b0: 20 74 65 78 74 2d 61 6c 69 67 6e 3a 20 6c 65 66  text-align: lef
000006c0: 74 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 2e 74 61 t;   }    div.ta
000006d0: 62 6c 65 5f 6f 66 5f 63 6f 6e 74 65 6e 74 73 5f ble_of_contents_
000006e0: 69 74 65 6d 20 7b 0a 20 20 20 20 63 6c 65 61 72 item {     clear
000006f0: 3a 20 6c 65 66 74 3b 0a 0a 20 20 20 20 77 69 64 : left;      wid
00000700: 74 68 3a 20 31 30 30 25 3b 0a 0a 20 20 20 20 6d th: 100%;      m
00000710: 61 72 67 69 6e 3a 20 34 70 78 20 30 70 78 20 30 argin: 4px 0px 0
00000720: 70 78 20 30 70 78 3b 0a 0a 20 20 20 20 62 61 63 px 0px;      bac
00000730: 6b 67 72 6f 75 6e 64 2d 63 6f 6c 6f 72 3a 20 23 kground-color: #
00000740: 46 46 46 46 46 46 3b 0a 0a 20 20 20 20 63 6f 6c FFFFFF;      col
00000750: 6f 72 3a 20 23 30 30 30 30 30 30 3b 0a 20 20 20 or: #000000;    
00000760: 20 74 65 78 74 2d 61 6c 69 67 6e 3a 20 6c 65 66  text-align: lef
00000770: 74 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 2e 74 61 t;   }    div.ta
00000780: 62 6c 65 5f 6f 66 5f 63 6f 6e 74 65 6e 74 73 5f ble_of_contents_
00000790: 69 74 65 6d 20 61 20 7b 0a 20 20 20 20 6d 61 72 item a {     mar
000007a0: 67 69 6e 3a 20 36 70 78 20 30 70 78 20 30 70 78 gin: 6px 0px 0px
000007b0: 20 36 70 78 3b 0a 20 20 7d 0a 0a 20 20 64 69 76  6px;   }    div
000007c0: 2e 63 6f 6e 74 65 6e 74 5f 73 65 63 74 69 6f 6e .content_section
000007d0: 20 7b 0a 20 20 20 20 6d 61 72 67 69 6e 3a 20 33  {     margin: 3
000007e0: 70 78 20 33 70 78 20 33 70 78 20 33 70 78 3b 0a px 3px 3px 3px; 
000007f0: 0a 20 20 20 20 62 61 63 6b 67 72 6f 75 6e 64 2d      background-
00000800: 63 6f 6c 6f 72 3a 20 23 46 46 46 46 46 46 3b 0a color: #FFFFFF; 
00000810: 0a 20 20 20 20 74 65 78 74 2d 61 6c 69 67 6e 3a      text-align:
00000820: 20 6c 65 66 74 3b 0a 20 20 7d 0a 0a 20 20 64 69  left;   }    di
00000830: 76 2e 63 6f 6e 74 65 6e 74 5f 73 65 63 74 69 6f v.content_sectio
00000840: 6e 5f 74 65 78 74 20 7b 0a 20 20 20 20 70 61 64 n_text {     pad
00000850: 64 69 6e 67 3a 20 34 70 78 20 38 70 78 20 34 70 ding: 4px 8px 4p
00000860: 78 20 38 70 78 3b 0a 0a 20 20 20 20 63 6f 6c 6f x 8px;      colo
00000870: 72 3a 20 23 30 30 30 30 30 30 3b 0a 20 20 20 20 r: #000000;     
00000880: 66 6f 6e 74 2d 73 69 7a 65 3a 20 31 30 30 25 3b font-size: 100%;
00000890: 0a 20 20 7d 0a 0a 20 20 64 69 76 2e 63 6f 6e 74    }    div.cont
000008a0: 65 6e 74 5f 73 65 63 74 69 6f 6e 5f 74 65 78 74 ent_section_text
000008b0: 20 70 72 65 20 7b 0a 20 20 20 20 6d 61 72 67 69  pre {     margi
000008c0: 6e 3a 20 38 70 78 20 30 70 78 20 38 70 78 20 30 n: 8px 0px 8px 0
000008d0: 70 78 3b 0a 20 20 20 20 70 61 64 64 69 6e 67 3a px;     padding:
000008e0: 20 38 70 78 20 38 70 78 20 38 70 78 20 38 70 78  8px 8px 8px 8px
000008f0: 3b 0a 0a 20 20 20 20 62 6f 72 64 65 72 2d 77 69 ;      border-wi
00000900: 64 74 68 3a 20 31 70 78 3b 0a 20 20 20 20 62 6f dth: 1px;     bo
00000910: 72 64 65 72 2d 73 74 79 6c 65 3a 20 64 6f 74 74 rder-style: dott
00000920: 65 64 3b 0a 20 20 20 20 62 6f 72 64 65 72 2d 63 ed;     border-c
00000930: 6f 6c 6f 72 3a 20 23 30 30 30 30 30 30 3b 0a 0a olor: #000000;  
00000940: 20 20 20 20 62 61 63 6b 67 72 6f 75 6e 64 2d 63     background-c
00000950: 6f 6c 6f 72 3a 20 23 46 35 46 36 46 37 3b 0a 0a olor: #F5F6F7;  
00000960: 20 20 20 20 66 6f 6e 74 2d 73 74 79 6c 65 3a 20     font-style: 
00000970: 69 74 61 6c 69 63 3b 0a 20 20 7d 0a 0a 20 20 64 italic;   }    d
00000980: 69 76 2e 63 6f 6e 74 65 6e 74 5f 73 65 63 74 69 iv.content_secti
00000990: 6f 6e 5f 74 65 78 74 20 70 20 7b 0a 20 20 20 20 on_text p {     
000009a0: 6d 61 72 67 69 6e 2d 62 6f 74 74 6f 6d 3a 20 36 margin-bottom: 6
000009b0: 70 78 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 2e 63 px;   }    div.c
000009c0: 6f 6e 74 65 6e 74 5f 73 65 63 74 69 6f 6e 5f 74 ontent_section_t
000009d0: 65 78 74 20 75 6c 2c 20 64 69 76 2e 63 6f 6e 74 ext ul, div.cont
000009e0: 65 6e 74 5f 73 65 63 74 69 6f 6e 5f 74 65 78 74 ent_section_text
000009f0: 20 6c 69 20 7b 0a 20 20 20 20 70 61 64 64 69 6e  li {     paddin
00000a00: 67 3a 20 34 70 78 20 38 70 78 20 34 70 78 20 31 g: 4px 8px 4px 1
00000a10: 36 70 78 3b 0a 20 20 7d 0a 0a 20 20 64 69 76 2e 6px;   }    div.
00000a20: 73 65 63 74 69 6f 6e 5f 68 65 61 64 65 72 20 7b section_header {
00000a30: 0a 20 20 20 20 70 61 64 64 69 6e 67 3a 20 33 70      padding: 3p
00000a40: 78 20 36 70 78 20 33 70 78 20 36 70 78 3b 0a 0a x 6px 3px 6px;  
00000a50: 20 20 20 20 62 61 63 6b 67 72 6f 75 6e 64 2d 63     background-c

NSE: TCP 10.10.14.3:54326 > 10.129.232.61:80 | CLOSE
NSOCK INFO [36.6700s] nsock_iod_delete(): nsock_iod_delete (IOD #9)
Nmap scan report for 10.129.232.61
Host is up (0.032s latency).

PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http       Apache httpd 2.4.29 ((Ubuntu))
50000/tcp open  tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.67 seconds
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ sudo nmap 10.129.232.62 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-02 22:05 EST
SENT (0.0780s) TCP 10.10.14.3:53 > 10.129.232.62:50000 S ttl=54 id=21300 iplen=44  seq=963393586 win=1024 <mss 1460>
RCVD (0.1070s) TCP 10.129.232.62:50000 > 10.10.14.3:53 SA ttl=63 id=0 iplen=44  seq=251416760 win=64240 <mss 1340>
Nmap scan report for 10.129.232.62
Host is up (0.029s latency).

PORT      STATE SERVICE
50000/tcp open  ibm-db2

Nmap done: 1 IP address (1 host up) scanned in 0.14 seconds
                                                            
```