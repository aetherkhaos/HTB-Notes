## machine Description

Sometimes, when we are asked to enumerate the services of specific hosts on the client network, we will be met with file transfer services that may have high chances to be poorly configured. The purpose of this exercise is to familiarize yourself with the File Transfer Protocol (FTP), a native protocol to all host operating systems and used for a long time for simple file transfer tasks, be they automated or manual. FTP can be easily misconfigured if not correctly understood. There are cases where an employee of the client company we are assessing might want to bypass file checks or firewall rules for transferring a file from themselves to their peers. Considering the many different mechanisms for controlling and monitoring data flow within an enterprise network today, this scenario becomes a substantial and viable case we might meet in the wild.

At the same time, FTP can be used to transfer log files from one network device to another or a log collection server. Suppose the network engineer in charge of handling the configuration forgets to secure the receiving FTP server properly or does not put enough importance on the information contained within the logs and decides to leave the FTP service unsecured intentionally. In that case, an attacker could gain leverage of the logs and extract all kinds of information from them, which can later be used to map out the network, enumerate usernames, detect active services, and more.

Let's take a look at what FTP is, according to [definition on Wikipedia](https://en.wikipedia.org/wiki/File_Transfer_Protocol):

```
The File Transfer Protocol (FTP) is a standard communication protocol used to transfer computer files from a server to a client on a computer network. FTP is built on a client–server model architecture using separate control and data connections between the client and the server. FTP users may authenticate themselves with a clear-text sign-in protocol, generally in the form of a username and password. However, they can connect anonymously if the server is configured to allow it. For secure transmission that protects the username and password and encrypts the content, FTP is often secured with SSL/TLS (FTPS) or replaced with SSH File Transfer Protocol (SFTP). 
```

From the first lines of the excerpt above, we can see mention of the client-server model architecture. This refers to the roles hosts in the network have during the act of transferring data between them. Users can download and upload files from the client (their own host) to the server (a centralized data storage device) or vice versa. Conceptually speaking, the client is always the host that downloads and uploads files to the server, and the server always is the host that safely stores the data being transferred.

Clients can also browse the available files on the server when using the FTP protocol. From a user's terminal perspective, this action will seem like browsing their own operating system's directories for files that they need. FTP services also come with a GUI (Graphical User Interface), akin to Windows OS Programs, enabling easier navigation for beginners. An example of a well-known GUI-oriented FTP Service is [FileZilla](https://filezilla-project.org/). However, let's first understand what it means for a port to be running a service openly.

A port running an active service is a reserved space for the IP address of the target to receive requests and send results from. If we only had IP addresses or hostnames, then the hosts could only do 1 task at a time. This means that if you wanted to browse the web and play music from an application on your computer simultaneously, you could not, because the IP address would be used for handling either the first or the latter, but not both at the same time. By having ports, you can have one IP address handling multiple services, as it adds another layer of distinction.

In the case shown below, we can see FTP being active on port 21. However, let's add some extra services like SSH (Secure Shell Protocol) and HTTPD (Web Server) in order to explore a more typical example. With this type of configuration, a network administrator has set up a rudimentary core web server configuration, allowing them to achieve the following, all at the same time if need be:

- Receive and send files that can be used to configure the webserver or serve logs to an external source
- Be able to be logged into for remote management from a distant host, in case any configuration changes are needed
- Serve web content that can be accessed remotely through another host's web browser

The Wiki article shows that it is considered non-standard for FTP to be used without the encryption layer provided by protocols such as SSL/TLS (FTPS) or SSH-tunneling (SFTP). FTP by itself does have the ability to require credentials before allowing access to the stored files. However, the deficiency here is that traffic containing said files can be intercepted with what is known as a Man-in-the-Middle Attack (MitM). The contents of the files can be read in plaintext (meaning unencrypted, human-readable form).

However, if the network administrators choose to wrap the connection with the SSL/TLS protocol or tunnel the FTP connection through SSH (as shown below) to add a layer of encryption that only the source and destination hosts can decrypt, this would successfully foil most Man-in-the-Middle attacks. Notice how port 21 has disappeared, as the FTP protocol gets moved under the SSH protocol on port 22, thus being tunneled through it and secured against any interception.

However, the situation we are dealing with in this case is much simpler. We are only going to interact with the target running a simple, misconfigured FTP service. Let us proceed and analyze how such a service running on an internal host would look like.

---
---

Guided Mode:

- What does the 3-letter acronym FTP stand for?
	- File Transfer Protocol
- Which port does the FTP service listen on usually?
	- 21
- FTP sends data in the clear, without any encryption. what acronym is used for a later protocol designed to provide similar functionality to FTP but securely, as an extension of the ssh protocol?
	- sftp
- What is the command we can use to send an ICMP echo request to test our connection to the target?
```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -n -sV -p21 10.129.1.14 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-07 14:02 EST
Nmap scan report for 10.129.1.14
Host is up (0.028s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.64 seconds

```
- From your scans, what OS type is running on the target?
	- unix
- What is the command we need to run in order to display the 'ftp' client help menu?
	- ftp -?
- What is username that is used over FTP when you want to log in without having an account?
	- anonymous
- What is the response code we get for the FTP message 'Login successful'?
	- 230
- There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.
	- ls
- What is the command used to download the file we found on the FTP server?
	- get
- Submit the flag located on the FTP server.
	- 035db21c881520061c53e0536e44f815