# Introduction to Windows

As a penetration tester, it is important to have knowledge of a wide variety of technologies. A thorough understanding of Windows and Linux operating systems is beneficial in a wide range of assessment types. The majority of systems that we encounter during assessments, whether on-premise or in the cloud, will be based on these two operating systems. It is important to understand how to attack and defend these operating systems and how they can each be used as a platform to perform further penetration testing activities.

## The Windows Operating System

Microsoft first introduced the Windows operating system on November 20, 1985. The first version of Windows was a graphical operating system shell for MS-DOS. Later versions of Windows Desktop introduced the Windows File Manager, Program Manager, and Print Manager programs.

Windows 95 was the first full integration of Windows and DOS and offered built-in Internet support for the first time. This version also debuted the Internet Explorer web browser. Since the initial version, there have been over a dozen versions of Windows released, such as Windows XP, Vista, and 8, up to the current version: Windows 11. Over time, Microsoft has offered various editions of each Windows Desktop release catering to everyone from casual consumers to enterprise customers.

Windows Server was first released in 1993 with the release of Windows NT 3.1 Advanced Server. Windows NT saw several updates over the years, adding in technologies such as Internet Information Services (IIS), various networking protocols, Administrative Wizards to facilitate admin tasks, and more. With the release of Windows 2000, Microsoft debuted Active Directory, originally intended to help sysadmins set up file sharing, data encryption, VPNs, etc. Windows Server 2000 also included the Microsoft Management Console (MMC) and supported dynamic disk volumes.

Windows Server 2003 came next with server roles, a built-in firewall, the Volume Shadow Copy Service, and more. Windows Server 2008 included failover clustering, Hyper-V virtualization software, Server Core, Event Viewer, and major enhancements to Active Directory. Over the years, Microsoft released further Server versions, including Server 2012, Server 2016, and most recently, Server 2019. This latest version added support for Kubernetes, Linux containers, and more advanced security features.

As new versions of Windows are introduced, older versions are deprecated and no longer receive Microsoft updates (unless a long-term support contract is purchased in some cases). Windows Server 2008 and 2012 reached end of life for security updates on January 14, 2020. Currently, only Server 2012 R2 and later are in support. However, Microsoft has released out-of-band patches for earlier versions of Windows in the past few years due to the discovery of the critical SMBv1 vulnerability (EternalBlue).

Many versions of Windows are now deemed "legacy" and are no longer supported. Organizations often find themselves running various older operating systems to support critical applications or due to operational or budgetary concerns. An assessor needs to understand the differences between versions and the various misconfigurations and vulnerabilities inherent to each.

## Windows Versions

The following is a list of the major Windows operating systems and associated version numbers:

|Operating System Names|Version Number|
|---|---|
|Windows NT 4|4.0|
|Windows 2000|5.0|
|Windows XP|5.1|
|Windows Server 2003, 2003 R2|5.2|
|Windows Vista, Server 2008|6.0|
|Windows 7, Server 2008 R2|6.1|
|Windows 8, Server 2012|6.2|
|Windows 8.1, Server 2012 R2|6.3|
|Windows 10, Server 2016, Server 2019|10.0|

We can use the [Get-WmiObject](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1) [cmdlet](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7) to find information about the operating system. This cmdlet can be used to get instances of WMI classes or information about available WMI classes. There are a variety of ways to find the version and build number of our system. We can easily obtain this information using the `win32_OperatingSystem` class, which shows that we are on a Windows 10 host, build number 19041.

```powershell-session
PS C:\htb> Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

Version    BuildNumber
-------    -----------
10.0.19041 19041
```

Some other useful classes that can be used with `Get-WmiObject` are `Win32_Process` to get a process listing, `Win32_Service` to get a listing of services, and `Win32_Bios` to get [Basic Input/Output System](https://en.wikipedia.org/wiki/BIOS) (`BIOS`) information. The BIOS is firmware installed on a computer's motherboard that controls the computer's essential functions, such as power management, input/output interfaces, and system configuration. We can use the `ComputerName` parameter to get information about remote computers. `Get-WmiObject` can be used to start and stop services on local and remote computers, and more. Further information about the cmdlet can be found [here](https://ss64.com/ps/get-wmiobject.html) and [here](https://adamtheautomator.com/get-wmiobject/).

## Accessing Windows

#### Local Access Concepts

If you are reading these words right now, you have local access to a computer of some kind. Be it a smartphone, tablet, laptop, Raspberry Pi, or Desktop. Local access is the most common way to access any computer, including computers running Windows. `Input` is likely happening through a keyboard, trackpad &/or mouse. `Output` is coming from the display screen(s). Organizations with office space where employees work on a day-to-day basis build security policies and security controls around the idea that their employees are working in dedicated workspaces on computers owned by the organization. It is becoming more common to see organizations increasing their support for remote work with their non-technical and technical workforces. This is not a new reality for technical professionals working in IT, Software Development & Infosec. On any given day, a technical professional could be accessing multiple machines locally and remotely. With that, let's discuss the concept of remote access.

#### Remote Access Concepts

Remote Access is accessing a computer over a network. Local access to a computer is needed before one can access another computer remotely. There are countless methods for remote access. In this module, we will mainly use remote access methods to connect to and interact with Windows operating systems. Advances in Networking & Internet technologies have given birth to entire industries that rely completely on remote access & remote administration of computer systems.

Consider [MSPs](https://www.techtarget.com/searchitchannel/definition/managed-service-provider) & [MSSPs](https://www.gartner.com/en/information-technology/glossary/mssp-managed-security-service-provider), both industries are primarily dependent on managing their client's computer systems remotely. This functionality allows them to centralize management, standardize what technologies are used, automate numerous tasks, enable remote work arrangements and allow for quick response time when issues surface, or potential security threats emerge. Remote access is not just limited to MSPs & MSSPs. Organizations with IT, Software Development &/or Security teams use remote access methods daily to build applications, manage servers and administer employee workstations. Some of the most common remote access technologies include but aren't limited to:

- Virtual Private Networks (VPN)
- Secure Shell (SSH)
- File Transfer Protocol (FTP)
- Virtual Network Computing (VNC)
- Windows Remote Management (or PowerShell Remoting) (WinRM)
- Remote Desktop Protocol (RDP)

We will focus primarily on using RDP in this module.

#### Remote Desktop Protocol (RDP)

RDP uses a client/server architecture where a client-side application is used to specify a computer's target IP address or hostname over a network where RDP access is enabled. The target computer where RDP remote access is enabled is considered the server. It is important to note that RDP listens by default on logical port `3389`. Keep in mind that an IP address is used as a logical identifier for a computer on a network, and a logical port is an identifier assigned to an application. In simpler terms, we could consider a network subnet a street in a town (the corporate network), an IP address in that subnet assigned to a host as a house on that street, and logical ports as windows/doors that can be used to access the house.

Once a request (encapsulated inside a packet) has reached a destination computer via its IP address, the request will be directed to an application hosted on the computer based on the port specified in that request (included as a header inside a packet). IP addressing and protocol encapsulation are covered in greater detail in the module [Introduction to Networking](https://enterprise.hackthebox.com/academy-lab/40884/preview/modules/34). From a networking perspective, in this module, we only need to understand that every computer has an IP address assigned to communicate over a network, and applications hosted on target computers listen on specific logical ports.

We can use RDP to connect to a Windows target from an attack host running Linux or Windows. If we are connecting to a Windows target from a Windows host, we can use the built-in RDP client application called `Remote Desktop Connection` ([mstsc.exe](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/mstsc)). Check out the clip below to see basic usage:

![[UsingRemoteDesktopConnection.gif]]

For this to work, remote access must already be [allowed](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access) on the target Windows system. By default, remote access is not allowed on Windows operating systems. The HTB Academy team has configured many of our Windows targets to permit RDP access once connected to the Academy labs via VPN.

Remote Desktop Connection also allows us to save connection profiles. This is a common habit among IT admins because it makes connecting to remote systems more convenient.

![[SavingRDPConnections.gif]]

As pentesters, we can benefit from looking for these saved Remote Desktop Files (`.rdp`) while on an engagement.

Many other Remote Desktop client applications exist, some of which are listed in this Microsoft article called [Remote Desktop clients](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients). We will not cover every Remote Desktop client application in this module.

#### Using xfreerdp

From a Linux-based attack host we can use a tool called [xfreerdp](https://linux.die.net/man/1/xfreerdp) to remotely access Windows targets. You will notice that we use xfreerdp across multiple modules because of its ease of use, feature set, command line utility, and efficiency. Check out the clip below to see basic usage from Pwnbox:

![[ConnectingwithXfreerdp.gif]]

Remember that we can also copy and paste in xfreerdp commands in the command line, so we do not need to enter options manually. There are several options available to us with xfreerdp, such as drive redirection to be able to transfer files to/from the target host, which are worth practicing and we will cover in other modules within HTB Academy.

Other RDP clients exist, such as [Remmina](https://remmina.org/) and [rdesktop](http://www.rdesktop.org/), and we encourage you to experiment with others and see what works best for you. Now that we have covered these concepts let's apply them by spawning the target below and connecting to it using RDP with the credentials provided.

## Connecting to the Windows Target

Connect via Remote Desktop (RDP) using the following command:

crimsonguard@htb[/htb]`$ xfreerdp /v:<targetIp> /u:htb-student /p:Password`

# Operating System Structure

In Windows operating systems, the root directory is <drive_letter>:\ (commonly C drive). The root directory (also known as the boot partition) is where the operating system is installed. Other physical and virtual drives are assigned other letters, for example, Data (E:). The directory structure of the boot partition is as follows:

|Directory|Function|
|---|---|
|Perflogs|Can hold Windows performance logs but is empty by default.|
|Program Files|On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here.|
|Program Files (x86)|32-bit and 16-bit programs are installed here on 64-bit editions of Windows.|
|ProgramData|This is a hidden folder that contains data that is essential for certain installed programs to run. This data is accessible by the program no matter what user is running it.|
|Users|This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default.|
|Default|This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile.|
|Public|This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access.|
|AppData|Per user application data and settings are stored in a hidden user subfolder (i.e., cliff.moore\AppData). Each of these folders contains three subfolders. The Roaming folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. The Local folder is specific to the computer itself and is never synchronized across the network. LocalLow is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode.|
|Windows|The majority of the files required for the Windows operating system are contained here.|
|System, System32, SysWOW64|Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path.|
|WinSxS|The Windows Component Store contains a copy of all Windows components, updates, and service packs.|

## Exploring Directories Using Command Line

We can explore the file system using the [dir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir) command.

```cmd-session
C:\htb> dir c:\ /a
 Volume in drive C has no label.
 Volume Serial Number is F416-77BE

 Directory of c:\

08/16/2020  10:33 AM    <DIR>          $Recycle.Bin
06/25/2020  06:25 PM    <DIR>          $WinREAgent
07/02/2020  12:55 PM             1,024 AMTAG.BIN
06/25/2020  03:38 PM    <JUNCTION>     Documents and Settings [C:\Users]
08/13/2020  06:03 PM             8,192 DumpStack.log
08/17/2020  12:11 PM             8,192 DumpStack.log.tmp
08/27/2020  10:42 AM    37,752,373,248 hiberfil.sys
08/17/2020  12:11 PM    13,421,772,800 pagefile.sys
12/07/2019  05:14 AM    <DIR>          PerfLogs
08/24/2020  10:38 AM    <DIR>          Program Files
07/09/2020  06:08 PM    <DIR>          Program Files (x86)
08/24/2020  10:41 AM    <DIR>          ProgramData
06/25/2020  03:38 PM    <DIR>          Recovery
06/25/2020  03:57 PM             2,918 RHDSetup.log
08/17/2020  12:11 PM        16,777,216 swapfile.sys
08/26/2020  02:51 PM    <DIR>          System Volume Information
08/16/2020  10:33 AM    <DIR>          Users
08/17/2020  11:38 PM    <DIR>          Windows
               7 File(s) 51,190,943,590 bytes
              13 Dir(s)  261,310,697,472 bytes free
```

The [tree](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tree) utility is useful for graphically displaying the directory structure of a path or disk.

```cmd-session
C:\htb> tree "c:\Program Files (x86)\VMware"
Folder PATH listing
Volume serial number is F416-77BE
C:\PROGRAM FILES (X86)\VMWARE
├───VMware VIX
│   ├───doc
│   │   ├───errors
│   │   ├───features
│   │   ├───lang
│   │   │   └───c
│   │   │       └───functions
│   │   └───types
│   ├───samples
│   └───Workstation-15.0.0
│       ├───32bit
│       └───64bit
└───VMware Workstation
    ├───env
    ├───hostd
    │   ├───coreLocale
    │   │   └───en
    │   ├───docroot
    │   │   ├───client
    │   │   └───sdk
    │   ├───extensions
    │   │   └───hostdiag
    │   │       └───locale
    │   │           └───en
    │   └───vimLocale
    │       └───en
    ├───ico
    ├───messages
    │   ├───ja
    │   └───zh_CN
    ├───OVFTool
    │   ├───env
    │   │   └───en
    │   └───schemas
    │       ├───DMTF
    │       └───vmware
    ├───Resources
    ├───tools-upgraders
    └───x64
```

The `tree` command can provide us with a large amount of information. The following command can be used to walk through all the files in the C drive, one screen at a time. This command can be modified to be run against any directory.

```cmd-session
tree c:\ /f | more
```

