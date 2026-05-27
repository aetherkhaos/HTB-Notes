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
- 

What command do you use to interact with the free version of Metasploit?
- 