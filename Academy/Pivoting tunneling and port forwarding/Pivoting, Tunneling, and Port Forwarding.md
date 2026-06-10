![[Pasted image 20260610080050.png]]
During a `red team engagement`, `penetration test`, or an `Active Directory assessment`, we will often find ourselves in a situation where we might have already compromised the required `credentials`, `ssh keys`, `hashes`, or `access tokens` to move onto another host, but there may be no other host directly reachable from our attack host. In such cases, we may need to use a `pivot host` that we have already compromised to find a way to our next target. One of the most important things to do when landing on a host for the first time is to check our `privilege level`, `network connections`, and potential `VPN or other remote access software`. If a host has more than one network adapter, we can likely use it to move to a different network segment. Pivoting is essentially the idea of `moving to other networks through a compromised host to find more targets on different network segments`.

There are many different terms used to describe a compromised host that we can use to `pivot` to a previously unreachable network segment. Some of the most common are:

- `Pivot Host`
- `Proxy`
- `Foothold`
- `Beach Head system`
- `Jump Host`

Pivoting's primary use is to defeat segmentation (both physically and virtually) to access an isolated network. `Tunneling`, on the other hand, is a subset of pivoting. Tunneling encapsulates network traffic into another protocol and routes traffic through it. Think of it like this:

We have a `key` we need to send to a partner, but we do not want anyone who sees our package to know it is a key. So we get a stuffed animal toy and hide the key inside with instructions about what it does. We then package the toy up and send it to our partner. Anyone who inspects the box will see a simple stuffed toy, not realizing it contains something else. Only our partner will know that the key is hidden inside and will learn how to access and use it once delivered.

Typical applications like VPNs or specialized browsers are just another form of tunneling network traffic.

We will inevitably come across several different terms used to describe the same thing in IT & the Infosec industry. With pivoting, we will notice that this is often referred to as `Lateral Movement`.

`Isn't it the same thing as pivoting?`

The answer to that is not exactly. Let's take a second to compare and contrast `Lateral Movement` with `Pivoting and Tunneling`, as there can be some confusion as to why some consider them different concepts.

## Lateral Movement, Pivoting, and Tunneling Compared

#### Lateral Movement

Lateral movement can be described as a technique used to further our access to additional `hosts`, `applications`, and `services` within a network environment. Lateral movement can also help us gain access to specific domain resources we may need to elevate our privileges. Lateral Movement often enables privilege escalation across hosts. In addition to the explanation we have provided for this concept, we can also study how other respected organizations explain Lateral Movement. Check out these two explanations when time permits:

[Palo Alto Network's Explanation](https://www.paloaltonetworks.com/cyberpedia/what-is-lateral-movement)

[MITRE's Explanation](https://attack.mitre.org/tactics/TA0008/)

One practical example of `Lateral Movement` would be:

During an assessment, we gained initial access to the target environment and were able to gain control of the local administrator account. We performed a network scan and found three more Windows hosts in the network. We attempted to use the same local administrator credentials, and one of those devices shared the same administrator account. We used the credentials to move laterally to that other device, enabling us to compromise the domain further.

#### Pivoting

Utilizing multiple hosts to cross `network` boundaries you would not usually have access to. This is more of a targeted objective. The goal here is to allow us to move deeper into a network by compromising targeted hosts or infrastructure.

One practical example of `Pivoting` would be:

During one tricky engagement, the target had their network physically and logically separated. This separation made it difficult for us to move around and complete our objectives. We had to search the network and compromise a host that turned out to be the engineering workstation used to maintain and monitor equipment in the operational environment, submit reports, and perform other administrative duties in the enterprise environment. That host turned out to be dual-homed (having more than one physical NIC connected to different networks). Without it having access to both enterprise and operational networks, we would not have been able to pivot as we needed to complete our assessment.

#### Tunneling

We often find ourselves using various protocols to shuttle traffic in/out of a network where there is a chance of our traffic being detected. For example, using HTTP to mask our Command & Control traffic from a server we own to the victim host. The key here is obfuscation of our actions to avoid detection for as long as possible. We utilize protocols with enhanced security measures such as HTTPS over TLS or SSH over other transport protocols. These types of actions also enable tactics like the exfiltration of data out of a target network or the delivery of more payloads and instructions into the network.

One practical example of `Tunneling` would be:

One way we used Tunneling was to craft our traffic to hide in HTTP and HTTPS. This is a common way we maintained Command and Control (C2) of the hosts we had compromised within a network. We masked our instructions inside GET and POST requests that appeared as normal traffic and, to the untrained eye, would look like a web request or response to any old website. If the packet were formed properly, it would be forwarded to our Control server. If it were not, it would be redirected to another website, potentially throwing off the defender checking it out.

To summarize, we should look at these tactics as separate things. Lateral Movement helps us spread wide within a network, elevating our privileges, while Pivoting allows us to delve deeper into the networks accessing previously unreachable environments. Keep this comparison in mind while moving through this module.

Now that we have been introduced to the module and have defined and compared Lateral Movement, Pivoting, and Tunneling, let's dive into some of the networking concepts that enable us to perform these tactics.

# The Networking Behind Pivoting

Being able to grasp the concept of `pivoting` well enough to succeed at it on an engagement requires a solid fundamental understanding of some key networking concepts. This section will be a quick refresher on essential foundational networking concepts to understand pivoting.

## IP Addressing & NICs

Every computer that is communicating on a network needs an IP address. If it doesn't have one, it is not on a network. The IP address is assigned in software and usually obtained automatically from a DHCP server. It is also common to see computers with statically assigned IP addresses. Static IP assignment is common with:

- Servers
- Routers
- Switch virtual interfaces
- Printers
- And any devices that are providing critical services to the network

Whether assigned `dynamically` or `statically`, the IP address is assigned to a `Network Interface Controller` (`NIC`). Commonly, the NIC is referred to as a `Network Interface Card` or `Network Adapter`. A computer can have multiple NICs (physical and virtual), meaning it can have multiple IP addresses assigned, allowing it to communicate on various networks. Identifying pivoting opportunities will often depend on the specific IPs assigned to the hosts we compromise because they can indicate the networks compromised hosts can reach. This is why it is important for us to always check for additional NICs using commands like `ifconfig` (in macOS and Linux) and `ipconfig` (in Windows).

#### Using ifconfig

crimsonguard@htb[/htb]`$ ifconfig  eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500         inet 134.122.100.200  netmask 255.255.240.0  broadcast 134.122.111.255         inet6 fe80::e973:b08d:7bdf:dc67  prefixlen 64  scopeid 0x20<link>         ether 12:ed:13:35:68:f5  txqueuelen 1000  (Ethernet)         RX packets 8844  bytes 803773 (784.9 KiB)         RX errors 0  dropped 0  overruns 0  frame 0         TX packets 5698  bytes 9713896 (9.2 MiB)         TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0  eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500         inet 10.106.0.172  netmask 255.255.240.0  broadcast 10.106.15.255         inet6 fe80::a5bf:1cd4:9bca:b3ae  prefixlen 64  scopeid 0x20<link>         ether 4e:c7:60:b0:01:8d  txqueuelen 1000  (Ethernet)         RX packets 15  bytes 1620 (1.5 KiB)         RX errors 0  dropped 0  overruns 0  frame 0         TX packets 18  bytes 1858 (1.8 KiB)         TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0  lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536         inet 127.0.0.1  netmask 255.0.0.0         inet6 ::1  prefixlen 128  scopeid 0x10<host>         loop  txqueuelen 1000  (Local Loopback)         RX packets 19787  bytes 10346966 (9.8 MiB)         RX errors 0  dropped 0  overruns 0  frame 0         TX packets 19787  bytes 10346966 (9.8 MiB)         TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0  tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500         inet 10.10.15.54  netmask 255.255.254.0  destination 10.10.15.54         inet6 fe80::c85a:5717:5e3a:38de  prefixlen 64  scopeid 0x20<link>         inet6 dead:beef:2::1034  prefixlen 64  scopeid 0x0<global>         unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (UNSPEC)         RX packets 0  bytes 0 (0.0 B)         RX errors 0  dropped 0  overruns 0  frame 0         TX packets 7  bytes 336 (336.0 B)         TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0`

In the output above, each NIC has an identifier (`eth0`, `eth1`, `lo`, `tun0`) followed by addressing information and traffic statistics. The tunnel interface (tun0) indicates a VPN connection is active. When we connect to any of HTB's VPN servers through Pwnbox or our own attack host, we will always notice a tunnel interface gets created and assigned an IP address. The VPN allows us to access the lab network environments hosted by HTB. Keep in mind that these lab networks are not reachable without having a tunnel established. The VPN encrypts traffic and also establishes a tunnel over a public network (often the Internet), through `NAT` on a public-facing network appliance, and into the internal/private network. Also, notice the IP addresses assigned to each NIC. The IP assigned to eth0 (`134.122.100.200`) is a publicly routable IP address. Meaning ISPs will route traffic originating from this IP over the Internet. We will see public IPs on devices that are directly facing the Internet, commonly hosted in DMZs. The other NICs have private IP addresses, which are routable within internal networks but not over the public Internet. At the time of writing, anyone that wants to communicate over the Internet must have at least one public IP address assigned to an interface on the network appliance that connects to the physical infrastructure connecting to the Internet. Recall that NAT is commonly used to translate private IP addresses to public IP addresses.

#### Using ipconfig

```powershell-session
PS C:\Users\htb-student> ipconfig

Windows IP Configuration

Unknown adapter NordLynx:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Ethernet adapter Ethernet0 2:

   Connection-specific DNS Suffix  . : .htb
   IPv6 Address. . . . . . . . . . . : dead:beef::1a9
   IPv6 Address. . . . . . . . . . . : dead:beef::f58b:6381:c648:1fb0
   Temporary IPv6 Address. . . . . . : dead:beef::dd0b:7cda:7118:3373
   Link-local IPv6 Address . . . . . : fe80::f58b:6381:c648:1fb0%8
   IPv4 Address. . . . . . . . . . . : 10.129.221.36
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:df81%8
                                       10.129.0.1

Ethernet adapter Ethernet:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
```

The output directly above is from issuing `ipconfig` on a Windows system. We can see that this system has multiple adapters, but only one of them has IP addresses assigned. There are [IPv6](https://www.cisco.com/c/en/us/solutions/ipv6/overview.html) addresses and an [IPv4](https://en.wikipedia.org/wiki/IPv4) address. This module will primarily focus on networks running IPv4 as it remains the most common IP addressing mechanism in enterprise LANs. We will notice some adapters, like the one in the output above, will have an IPv4 and an IPv6 address assigned in a [dual-stack configuration](https://web.archive.org/web/20220817112619/https://www.cisco.com/c/dam/en_us/solutions/industries/docs/gov/IPV6at_a_glance_c45-625859.pdf) allowing resources to be reached over IPv4 or IPv6.

Every IPv4 address will have a corresponding `subnet mask`. If an IP address is like a phone number, the subnet mask is like the area code. Remember that the subnet mask defines the `network` & `host` portion of an IP address. When network traffic is destined for an IP address located in a different network, the computer will send the traffic to its assigned `default gateway`. The default gateway is usually the IP address assigned to a NIC on an appliance acting as the router for a given LAN. In the context of pivoting, we need to be mindful of what networks a host we land on can reach, so documenting as much IP addressing information as possible on an engagement can prove helpful.

## Routing

It is common to think of a network appliance that connects us to the Internet when thinking about a router, but technically any computer can become a router and participate in routing. Some of the challenges we will face in this module require us to make a pivot host route traffic to another network. One way we will see this is through the use of AutoRoute, which allows our attack box to have `routes` to target networks that are reachable through a pivot host. One key defining characteristic of a router is that it has a routing table that it uses to forward traffic based on the destination IP address. Let's look at this on Pwnbox using the commands `netstat -r` or `ip route`.

#### Routing Table on Pwnbox

crimsonguard@htb[/htb]`$ netstat -r  Kernel IP routing table Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface default         178.62.64.1     0.0.0.0         UG        0 0          0 eth0 10.10.10.0      10.10.14.1      255.255.254.0   UG        0 0          0 tun0 10.10.14.0      0.0.0.0         255.255.254.0   U         0 0          0 tun0 10.106.0.0      0.0.0.0         255.255.240.0   U         0 0          0 eth1 10.129.0.0      10.10.14.1      255.255.0.0     UG        0 0          0 tun0 178.62.64.0     0.0.0.0         255.255.192.0   U         0 0          0 eth0`

We will notice that Pwnbox, Linux distros, Windows, and many other operating systems have a routing table to assist the system in making routing decisions. When a packet is created and has a destination before it leaves the computer, the routing table is used to decide where to send it. For example, if we are trying to connect to a target with the IP `10.129.10.25`, we could tell from the routing table where the packet would be sent to get there. It would be forwarded to a `Gateway` out of the corresponding NIC (`Iface`). Pwnbox is not using any routing protocols (EIGRP, OSPF, BGP, etc...) to learn each of those routes. It learned about those routes via its own directly connected interfaces (eth0, eth1, tun0). Stand-alone appliances designated as routers typically will learn routes using a combination of static route creation, dynamic routing protocols, and directly connected interfaces. Any traffic destined for networks not present in the routing table will be sent to the `default route`, which can also be referred to as the default gateway or gateway of last resort. When looking for opportunities to pivot, it can be helpful to look at the hosts' routing table to identify which networks we may be able to reach or which routes we may need to add.

## Protocols, Services & Ports

`Protocols` are the rules that govern network communications. Many protocols and services have corresponding `ports` that act as identifiers. Logical ports aren't physical things we can touch or plug anything into. They are in software assigned to applications. When we see an IP address, we know it identifies a computer that may be reachable over a network. When we see an open port bound to that IP address, we know that it identifies an application we may be able to connect to. Connecting to specific ports that a device is `listening` on can often allow us to use ports & protocols that are `permitted` in the firewall to gain a foothold on the network.

Let's take, for example, a web server using HTTP (`often listening on port 80`). The administrators should not block traffic coming inbound on port 80. This would prevent anyone from visiting the website they are hosting. This is often a way into the network environment, `through the same port that legitimate traffic is passing`. We must not overlook the fact that a `source port` is also generated to keep track of established connections on the client-side of a connection. We need to remain mindful of what ports we are using to ensure that when we execute our payloads, they connect back to the intended listeners we set up. We will get creative with the use of ports throughout this module.

For further review of fundamental networking concepts, please reference the module [Introduction to Networking](https://academy.hackthebox.com/course/preview/introduction-to-networking).

A tip from LTNB0B: In this module, we will practice many different tools and techniques to pivot through hosts and forward local or remote services to our attack host to access targets connected to different networks. This module gradually increases in difficulty, providing multi-host networks to practice what is learned. I strongly encourage you to practice many different methods in creative ways as you start to understand the concepts. Maybe even try to draw out the network topologies using network diagramming tools as you face challenges. When I am looking for opportunities to pivot, I like to use tools like [Draw.io](https://draw.io/) to build a visual of the network environment I am in, it serves as a great documentation tool as well. This module is a lot of fun and will put your networking skills to the test. Have fun, and never stop learning!

---
## Questions

Answer the question(s) below to complete the section

Reference the Using ifconfig output in the section reading. Which NIC is assigned a public IP address?
eth0

Reference the Routing Table on Pwnbox output shown in the section reading. If a packet is destined for a host with the IP address of 10.129.10.25, out of which NIC will the packet be forwarded?
tun0

Reference the Routing Table on Pwnbox output shown in the section reading. If a packet is destined for www.hackthebox.com what is the IP address of the gateway it will be sent to?
178.62.64.1

---
# Dynamic Port Forwarding with SSH and SOCKS Tunneling

## Port Forwarding in Context

`Port forwarding` is a technique that allows us to redirect a communication request from one port to another. Port forwarding uses TCP as the primary communication layer to provide interactive communication for the forwarded port. However, different application layer protocols such as SSH or even [SOCKS](https://en.wikipedia.org/wiki/SOCKS) (non-application layer) can be used to encapsulate the forwarded traffic. This can be effective in bypassing firewalls and using existing services on your compromised host to pivot to other networks.

## SSH Local Port Forwarding

Let's take an example from the below image.
![[Pasted image 20260610080405.png]]Note: Each network diagram presented in this module is designed to illustrate concepts discussed in the associated section. The IP addressing shown in the diagrams will not always match the lab environments exactly. Be sure to focus on understanding the concept, and you will find the diagrams will prove very useful! After reading this section be sure to reference the above image again to reinforce the concepts.

We have our attack host (10.10.15.x) and a target Ubuntu server (10.129.x.x), which we have compromised. We will scan the target Ubuntu server using Nmap to search for open ports.

#### Scanning the Pivot Target

crimsonguard@htb[/htb]`$ nmap -sT -p22,3306 10.129.202.64  Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-24 12:12 EST Nmap scan report for 10.129.202.64 Host is up (0.12s latency).  PORT     STATE  SERVICE 22/tcp   open   ssh 3306/tcp closed mysql  Nmap done: 1 IP address (1 host up) scanned in 0.68 seconds`

The Nmap output shows that the SSH port is open. To access the MySQL service, we can either SSH into the server and access MySQL from inside the Ubuntu server, or we can port forward it to our localhost on port `1234` and access it locally. A benefit of accessing it locally is if we want to execute a remote exploit on the MySQL service, we won't be able to do it without port forwarding. This is due to MySQL being hosted locally on the Ubuntu server on port `3306`. So, we will use the below command to forward our local port (1234) over SSH to the Ubuntu server.

#### Executing the Local Port Forward

crimsonguard@htb[/htb]`$ ssh -L 1234:localhost:3306 ubuntu@10.129.202.64  ubuntu@10.129.202.64's password:  Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)   * Documentation:  https://help.ubuntu.com  * Management:     https://landscape.canonical.com  * Support:        https://ubuntu.com/advantage    System information as of Thu 24 Feb 2022 05:23:20 PM UTC    System load:             0.0   Usage of /:              28.4% of 13.72GB   Memory usage:            34%   Swap usage:              0%   Processes:               175   Users logged in:         1   IPv4 address for ens192: 10.129.202.64   IPv6 address for ens192: dead:beef::250:56ff:feb9:52eb   IPv4 address for ens224: 172.16.5.129   * Super-optimized for small spaces - read how we shrank the memory    footprint of MicroK8s to make it the smallest full K8s around.     https://ubuntu.com/blog/microk8s-memory-optimisation  66 updates can be applied immediately. 45 of these updates are standard security updates. To see these additional updates run: apt list --upgradable`

The `-L` command tells the SSH client to request the SSH server to forward all the data we send via the port `1234` to `localhost:3306` on the Ubuntu server. By doing this, we should be able to access the MySQL service locally on port 1234. We can use Netstat or Nmap to query our local host on 1234 port to verify whether the MySQL service was forwarded.

#### Confirming Port Forward with Netstat

crimsonguard@htb[/htb]`$ netstat -antp | grep 1234  (Not all processes could be identified, non-owned process info  will not be shown, you would have to be root to see it all.) tcp        0      0 127.0.0.1:1234          0.0.0.0:*               LISTEN      4034/ssh             tcp6       0      0 ::1:1234                :::*                    LISTEN      4034/ssh`     

#### Confirming Port Forward with Nmap

crimsonguard@htb[/htb]`$ nmap -v -sV -p1234 localhost  Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-24 12:18 EST NSE: Loaded 45 scripts for scanning. Initiating Ping Scan at 12:18 Scanning localhost (127.0.0.1) [2 ports] Completed Ping Scan at 12:18, 0.01s elapsed (1 total hosts) Initiating Connect Scan at 12:18 Scanning localhost (127.0.0.1) [1 port] Discovered open port 1234/tcp on 127.0.0.1 Completed Connect Scan at 12:18, 0.01s elapsed (1 total ports) Initiating Service scan at 12:18 Scanning 1 service on localhost (127.0.0.1) Completed Service scan at 12:18, 0.12s elapsed (1 service on 1 host) NSE: Script scanning 127.0.0.1. Initiating NSE at 12:18 Completed NSE at 12:18, 0.01s elapsed Initiating NSE at 12:18 Completed NSE at 12:18, 0.00s elapsed Nmap scan report for localhost (127.0.0.1) Host is up (0.0080s latency). Other addresses for localhost (not scanned): ::1  PORT     STATE SERVICE VERSION 1234/tcp open  mysql   MySQL 8.0.28-0ubuntu0.20.04.3  Read data files from: /usr/bin/../share/nmap Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . Nmap done: 1 IP address (1 host up) scanned in 1.18 seconds`

Similarly, if we want to forward multiple ports from the Ubuntu server to your localhost, you can do so by including the `local port:server:port` argument to your ssh command. For example, the below command forwards the apache web server's port 80 to your attack host's local port on `8080`.

#### Forwarding Multiple Ports

crimsonguard@htb[/htb]`$ ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64`

## Setting up to Pivot

Now, if you type `ifconfig` on the Ubuntu host, you will find that this server has multiple NICs:

- One connected to our attack host (`ens192`)
- One communicating to other hosts within a different network (`ens224`)
- The loopback interface (`lo`).

#### Looking for Opportunities to Pivot using ifconfig

```shell-session
ubuntu@WEB01:~$ ifconfig 

ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.129.202.64  netmask 255.255.0.0  broadcast 10.129.255.255
        inet6 dead:beef::250:56ff:feb9:52eb  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::250:56ff:feb9:52eb  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:b9:52:eb  txqueuelen 1000  (Ethernet)
        RX packets 35571  bytes 177919049 (177.9 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10452  bytes 1474767 (1.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens224: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.16.5.129  netmask 255.255.254.0  broadcast 172.16.5.255
        inet6 fe80::250:56ff:feb9:a9aa  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:b9:a9:aa  txqueuelen 1000  (Ethernet)
        RX packets 8251  bytes 1125190 (1.1 MB)
        RX errors 0  dropped 40  overruns 0  frame 0
        TX packets 1538  bytes 123584 (123.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 270  bytes 22432 (22.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 270  bytes 22432 (22.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Unlike the previous scenario where we knew which port to access, in our current scenario, we don't know which services lie on the other side of the network. So, we can scan smaller ranges of IPs on the network (`172.16.5.1-200`) or the entire subnet (`172.16.5.0/23`). We cannot perform this scan directly from our attack host because it does not have routes to the `172.16.5.0/23` network. To do this, we will have to perform `dynamic port forwarding` and `pivot` our network packets via the Ubuntu server. We can do this by starting a `SOCKS listener` on our `local host` (personal attack host or Pwnbox) and then configure SSH to forward that traffic via SSH to the network (172.16.5.0/23) after connecting to the target host.

This is called `SSH tunneling` over `SOCKS proxy`. SOCKS stands for `Socket Secure`, a protocol that helps communicate with servers where you have firewall restrictions in place. Unlike most cases where you would initiate a connection to connect to a service, in the case of SOCKS, the initial traffic is generated by a SOCKS client, which connects to the SOCKS server controlled by the user who wants to access a service on the client-side. Once the connection is established, network traffic can be routed through the SOCKS server on behalf of the connected client.

This technique is often used to circumvent the restrictions put in place by firewalls, and allow an external entity to bypass the firewall and access a service within the firewalled environment. One more benefit of using SOCKS proxy for pivoting and forwarding data is that SOCKS proxies can pivot via creating a route to an external server from `NAT networks`. SOCKS proxies are currently of two types: `SOCKS4` and `SOCKS5`. SOCKS4 doesn't provide any authentication and UDP support, whereas SOCKS5 does provide that. Let's take an example of the below image where we have a NAT'd network of 172.16.5.0/23, which we cannot access directly.

![[Pasted image 20260610080439.png]]
In the above image, the attack host starts the SSH client and requests the SSH server to allow it to send some TCP data over the ssh socket. The SSH server responds with an acknowledgment, and the SSH client then starts listening on `localhost:9050`. Whatever data you send here will be broadcasted to the entire network (172.16.5.0/23) over SSH. We can use the below command to perform this dynamic port forwarding.

#### Enabling Dynamic Port Forwarding with SSH

crimsonguard@htb[/htb]`$ ssh -D 9050 ubuntu@10.129.202.64`

The `-D` argument requests the SSH server to enable dynamic port forwarding. Once we have this enabled, we will require a tool that can route any tool's packets over the port `9050`. We can do this using the tool `proxychains`, which is capable of redirecting TCP connections through TOR, SOCKS, and HTTP/HTTPS proxy servers and also allows us to chain multiple proxy servers together. Using proxychains, we can hide the IP address of the requesting host as well since the receiving host will only see the IP of the pivot host. Proxychains is often used to force an application's `TCP traffic` to go through hosted proxies like `SOCKS4`/`SOCKS5`, `TOR`, or `HTTP`/`HTTPS` proxies.

To inform proxychains that we must use port 9050, we must modify the proxychains configuration file located at `/etc/proxychains.conf`. We can add `socks4 127.0.0.1 9050` to the last line if it is not already there.

#### Checking /etc/proxychains.conf

crimsonguard@htb[/htb]`$ tail -4 /etc/proxychains.conf  # meanwile # defaults set to "tor" socks4 	127.0.0.1 9050`

Now when you start Nmap with proxychains using the below command, it will route all the packets of Nmap to the local port 9050, where our SSH client is listening, which will forward all the packets over SSH to the 172.16.5.0/23 network.

#### Using Nmap with Proxychains

crimsonguard@htb[/htb]`$ proxychains nmap -v -sn 172.16.5.1-200  ProxyChains-3.1 (http://proxychains.sf.net)  Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-24 12:30 EST Initiating Ping Scan at 12:30 Scanning 10 hosts [2 ports/host] |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.2:80-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.5:80-<><>-OK |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.6:80-<--timeout RTTVAR has grown to over 2.3 seconds, decreasing to 2.0  <SNIP>`

This part of packing all your Nmap data using proxychains and forwarding it to a remote server is called `SOCKS tunneling`. One more important note to remember here is that we can only perform a `full TCP connect scan` over proxychains. The reason for this is that proxychains cannot understand partial packets. If you send partial packets like half connect scans, it will return incorrect results. We also need to make sure we are aware of the fact that `host-alive` checks may not work against Windows targets because the Windows Defender firewall blocks ICMP requests (traditional pings) by default.

[A full TCP connect scan](https://nmap.org/book/scan-methods-connect-scan.html) without ping on an entire network range will take a long time. So, for this module, we will primarily focus on scanning individual hosts, or smaller ranges of hosts we know are alive, which in this case will be a Windows host at `172.16.5.19`.

We will perform a remote system scan using the below command.

#### Enumerating the Windows Target through Proxychains

crimsonguard@htb[/htb]`$ proxychains nmap -v -Pn -sT 172.16.5.19  ProxyChains-3.1 (http://proxychains.sf.net) Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower. Starting Nmap 7.92 ( https://nmap.org ) at 2022-02-24 12:33 EST Initiating Parallel DNS resolution of 1 host. at 12:33 Completed Parallel DNS resolution of 1 host. at 12:33, 0.15s elapsed Initiating Connect Scan at 12:33 Scanning 172.16.5.19 [1000 ports] |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:1720-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:587-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:445-<><>-OK Discovered open port 445/tcp on 172.16.5.19 |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:8080-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:23-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:135-<><>-OK Discovered open port 135/tcp on 172.16.5.19 |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:110-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:21-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:554-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-1172.16.5.19:25-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:5900-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:1025-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:143-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:199-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:993-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:995-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:3389-<><>-OK Discovered open port 3389/tcp on 172.16.5.19 |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:443-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:80-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:113-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:8888-<--timeout |S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:139-<><>-OK Discovered open port 139/tcp on 172.16.5.19`

The Nmap scan shows several open ports, one of which is `RDP port` (3389). Similar to the Nmap scan, we can also pivot `msfconsole` via proxychains to perform vulnerable RDP scans using Metasploit auxiliary modules. We can start msfconsole with proxychains.

## Using Metasploit with Proxychains

We can also open Metasploit using proxychains and send all associated traffic through the proxy we have established.

crimsonguard@htb[/htb]```````$ proxychains msfconsole  ProxyChains-3.1 (http://proxychains.sf.net)                                                          .~+P``````-o+:.                                      -o+:. .+oooyysyyssyyssyddh++os-`````                        ```````````````          ` +++++++++++++++++++++++sydhyoyso/:.````...`...-///::+ohhyosyyosyy/+om++:ooo///o ++++///////~~~~///////++++++++++++++++ooyysoyysosso+++++++++++++++++++///oossosy --.`                 .-.-...-////+++++++++++++++////////~~//////++++++++++++///                                 `...............`              `...-/////...`                                     .::::::::::-.                     .::::::-                                 .hmMMMMMMMMMMNddds\...//M\\.../hddddmMMMMMMNo                                  :Nm-/NMMMMMMMMMMMMM$$NMMMMm&&MMMMMMMMMMMMMMy                                  .sm/`-yMMMMMMMMMMMM$$MMMMMN&&MMMMMMMMMMMMMh`                                   -Nd`  :MMMMMMMMMMM$$MMMMMN&&MMMMMMMMMMMMh`                                    -Nh` .yMMMMMMMMMM$$MMMMMN&&MMMMMMMMMMMm/     `oo/``-hd:  ``                 .sNd  :MMMMMMMMMM$$MMMMMN&&MMMMMMMMMMm/       .yNmMMh//+syysso-``````       -mh` :MMMMMMMMMM$$MMMMMN&&MMMMMMMMMMd     .shMMMMN//dmNMMMMMMMMMMMMs`     `:```-o++++oooo+:/ooooo+:+o+++oooo++/     `///omh//dMMMMMMMMMMMMMMMN/:::::/+ooso--/ydh//+s+/ossssso:--syN///os:           /MMMMMMMMMMMMMMMMMMd.     `/++-.-yy/...osydh/-+oo:-`o//...oyodh+           -hMMmssddd+:dMMmNMMh.     `.-=mmk.//^^^\\\.^^`:++:^^o://^^^\\`::           .sMMmo.    -dMd--:mN/`           ||--X--||          ||--X--|| ........../yddy/:...+hmo-...hdd:............\\=v=//............\\=v=//......... ================================================================================ =====================+--------------------------------+========================= =====================| Session one died of dysentery. |========================= =====================+--------------------------------+========================= ================================================================================                       Press ENTER to size up the situation  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%%%%%%%%%%%%%%%%%%%%%%%%%%%% Date: April 25, 1848 %%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%%%%%%%%%%%%%%%%%%%%%%%%% Weather: It's always cool in the lab %%%%%%%%%%%%%%%% %%%%%%%%%%%%%%%%%%%%%%%%%%% Health: Overweight %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%%%%%%%%%%%%%%%%%%%%%%%% Caffeine: 12975 mg %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%%%%%%%%%%%%%%%%%%%%%%%%%% Hacked: All the things %%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%                          Press SPACE BAR to continue           =[ metasploit v6.1.27-dev                          ] + -- --=[ 2196 exploits - 1162 auxiliary - 400 post       ] + -- --=[ 596 payloads - 45 encoders - 10 nops            ] + -- --=[ 9 evasion                                       ]  Metasploit tip: Adapter names can be used for IP params  set LHOST eth0  msf6 >``````` 

Let's use the `rdp_scanner` auxiliary module to check if the host on the internal network is listening on 3389.

#### Using rdp_scanner Module

```shell-session
msf6 > search rdp_scanner

Matching Modules
================

   #  Name                               Disclosure Date  Rank    Check  Description
   -  ----                               ---------------  ----    -----  -----------
   0  auxiliary/scanner/rdp/rdp_scanner                   normal  No     Identify endpoints speaking the Remote Desktop Protocol (RDP)


Interact with a module by name or index. For example info 0, use 0 or use auxiliary/scanner/rdp/rdp_scanner

msf6 > use 0
msf6 auxiliary(scanner/rdp/rdp_scanner) > set rhosts 172.16.5.19
rhosts => 172.16.5.19
msf6 auxiliary(scanner/rdp/rdp_scanner) > run
|S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:3389-<><>-OK
|S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:3389-<><>-OK
|S-chain|-<>-127.0.0.1:9050-<><>-172.16.5.19:3389-<><>-OK

[*] 172.16.5.19:3389      - Detected RDP on 172.16.5.19:3389      (name:DC01) (domain:DC01) (domain_fqdn:DC01) (server_fqdn:DC01) (os_version:10.0.17763) (Requires NLA: No)
[*] 172.16.5.19:3389      - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

At the bottom of the output above, we can see the RDP port open with the Windows OS version.

Depending on the level of access we have to this host during an assessment, we may try to run an exploit or log in using gathered credentials. For this module, we will log in to the Windows remote host over the SOCKS tunnel. This can be done using `xfreerdp`. The user in our case is `victor,` and the password is `pass@123`

#### Using xfreerdp with Proxychains

crimsonguard@htb[/htb]`$ proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123  ProxyChains-3.1 (http://proxychains.sf.net) [13:02:42:481] [4829:4830] [INFO][com.freerdp.core] - freerdp_connect:freerdp_set_last_error_ex resetting error state [13:02:42:482] [4829:4830] [INFO][com.freerdp.client.common.cmdline] - loading channelEx rdpdr [13:02:42:482] [4829:4830] [INFO][com.freerdp.client.common.cmdline] - loading channelEx rdpsnd [13:02:42:482] [4829:4830] [INFO][com.freerdp.client.common.cmdline] - loading channelEx cliprdr`

The xfreerdp command will require an RDP certificate to be accepted before successfully establishing the session. After accepting it, we should have an RDP session, pivoting via the Ubuntu server.

#### Successful RDP Pivot
![[Pasted image 20260610080506.png]]
Note: When spawning your target, we ask you to wait for 3 - 5 minutes until the whole lab with all the configurations is set up so that the connection to your target works flawlessly.

---
## Questions

Answer the question(s) below to complete the section
You have successfully captured credentials to an external facing Web Server. Connect to the target and list the network interfaces. How many network interfaces does the target web server have? (Including the loopback interface)
3

Apply the concepts taught in this section to pivot to the internal network and use RDP (credentials: victor:pass@123) to take control of the Windows target on 172.16.5.19. Submit the contents of Flag.txt located on the Desktop.