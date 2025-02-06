## Q1: What web application is running on port 80? (two words)
- NMAP Scan of Target
```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sT -Pn 10.129.227.78 -p- -sV   
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-31 12:14 EST
Nmap scan report for 10.129.227.78
Host is up (0.036s latency).
Not shown: 65511 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
53/tcp    open  domain       Simple DNS Plus
80/tcp    open  http
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2025-02-01 01:16:39Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: panda.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds (workgroup: PANDA)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: panda.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
9389/tcp  open  mc-nmf       .NET Message Framing
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49669/tcp open  msrpc        Microsoft Windows RPC
49670/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc        Microsoft Windows RPC
49675/tcp open  msrpc        Microsoft Windows RPC
49702/tcp open  msrpc        Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.95%I=7%D=1/31%Time=679D0513%P=x86_64-pc-linux-gnu%r(GetR
SF:equest,21A,"HTTP/1\.1\x20200\x20OK\r\nCache-Control:\x20private\r\nExpi
SF:res:\x20Thu,\x2001\x20Jan\x201970\x2000:00:00\x20GMT\r\nSet-Cookie:\x20
SF:JSESSIONIDADSSP=3000ECAF271D016BA4209F762ECB148D;\x20Path=/;\x20HttpOnl
SF:y\r\nContent-Type:\x20text/html;charset=UTF-8\r\nContent-Length:\x20259
SF:\r\nDate:\x20Sat,\x2001\x20Feb\x202025\x2001:16:39\x20GMT\r\nConnection
SF::\x20close\r\n\r\n<!--\x20\$Id\$\x20-->\n<html>\n<head>\n\t<META\x20HTT
SF:P-EQUIV=\"CACHE-CONTROL\"\x20CONTENT=\"NO-CACHE\">\n\t<META\x20HTTP-EQU
SF:IV=\"PRAGMA\"\x20CONTENT=\"NO-CACHE\">\n\t<META\x20HTTP-EQUIV=\"Expires
SF:\"\x20CONTENT=\"0\">\n\t<script>\n\t\tlocation\.href\x20=\x20'showLogin
SF:\.cc'\x20\+\x20location\.search;\n\t</script>\n</head>\n</html>\n")%r(H
SF:TTPOptions,AE,"HTTP/1\.1\x20405\x20Method\x20Not\x20Allowed\r\nCache-Co
SF:ntrol:\x20private\r\nExpires:\x20Thu,\x2001\x20Jan\x201970\x2000:00:00\
SF:x20GMT\r\nContent-Length:\x200\r\nDate:\x20Sat,\x2001\x20Feb\x202025\x2
SF:001:16:39\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(RTSPRequest,810,"H
SF:TTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20text/html;charset
SF:=utf-8\r\nContent-Language:\x20en\r\nContent-Length:\x201897\r\nDate:\x
SF:20Sat,\x2001\x20Feb\x202025\x2001:16:39\x20GMT\r\nConnection:\x20close\
SF:r\n\r\n<!doctype\x20html><html\x20lang=\"en\"><head><title>HTTP\x20Stat
SF:us\x20400\x20\xe2\x80\x93\x20Bad\x20Request</title><style\x20type=\"tex
SF:t/css\">body\x20{font-family:Tahoma,Arial,sans-serif;}\x20h1,\x20h2,\x2
SF:0h3,\x20b\x20{color:white;background-color:#525D76;}\x20h1\x20{font-siz
SF:e:22px;}\x20h2\x20{font-size:16px;}\x20h3\x20{font-size:14px;}\x20p\x20
SF:{font-size:12px;}\x20a\x20{color:black;}\x20\.line\x20{height:1px;backg
SF:round-color:#525D76;border:none;}</style></head><body><h1>HTTP\x20Statu
SF:s\x20400\x20\xe2\x80\x93\x20Bad\x20Request</h1><hr\x20class=\"line\"\x2
SF:0/><p><b>Type</b>\x20Exception\x20Report</p><p><b>Message</b>\x20Invali
SF:d\x20character\x20found\x20in\x20the\x20HTTP\x20protocol\x20\[RTSP&#47;
SF:1\.00x0d0x0a0x0d\.\.\.\]</p><p><b>Description</b>\x20The\x20server\x20c
SF:annot\x20or\x20will\x20not\x20process\x20the\x20request\x20due\x20to\x2
SF:0something\x20that\x20is\x20perceived\x20to\x20be\x20a\x20client\x20err
SF:or\x20\(e\.g\.,\x20malformed\x20request\x20syntax,\x20i");
Service Info: Host: ADSELFSERVICE; OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.55 seconds 
```
- NMAP didnt show the application
	- go to the web page

![[ADSelfService.png]]

## What 2021 CVE in ADSelfService Plus allows for Remote Code Execution?
![[ADSelfService-cve.png]]
## What is the name of the HTTP POST parameter that allows for command execution?
