WINDOWS EASY

# Host Scanning
- NMAP
```
┌──(kali㉿kali)-[~/Downloads]
└─$ sudo nmap -Pn -sV  10.129.231.236
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-20 11:29 EST
Nmap scan report for 10.129.231.236
Host is up (0.058s latency).                                                                                        
Not shown: 987 filtered tcp ports (no-response)                                                                     
PORT     STATE SERVICE       VERSION                                                                                
53/tcp   open  domain        Simple DNS Plus                                                                        
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-01-20 16:30:09Z)                         
135/tcp  open  msrpc         Microsoft Windows RPC                                                                  
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn                                                          
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)                                                                                                              
445/tcp  open  microsoft-ds?                                                                                        
464/tcp  open  kpasswd5?                                                                                            
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb0., Site: Default-First-Site-Name)
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 53.65 seconds

```
# 
