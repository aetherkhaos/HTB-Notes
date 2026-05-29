
### Example Script
CIDR.sh
```bash
#!/bin/bash

# Check for given arguments
if [ $# -eq 0 ]
then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi

# Identify Network range for the specified IP address(es)
function network_range {
	for ip in $ipaddr
	do
		netrange=$(whois $ip | grep "NetRange\|CIDR" | tee -a CIDR.txt)
		cidr=$(whois $ip | grep "CIDR" | awk '{print $2}')
		cidr_ips=$(prips $cidr)
		echo -e "\nNetRange for $ip:"
		echo -e "$netrange"
	done
}

# Ping discovered IP address(es)
function ping_host {
	hosts_up=0
	hosts_total=0
	
	echo -e "\nPinging host(s):"
	for host in $cidr_ips
	do
		stat=1
		while [ $stat -eq 1 ]
		do
			ping -c 2 $host > /dev/null 2>&1
			if [ $? -eq 0 ]
			then
				echo "$host is up."
				((stat--))
				((hosts_up++))
				((hosts_total++))
			else
				echo "$host is down."
				((stat--))
				((hosts_total++))
			fi
		done
	done
	
	echo -e "\n$hosts_up out of $hosts_total hosts are up."
}

# Identify IP address of the specified domain
hosts=$(host $domain | grep "has address" | cut -d" " -f4 | tee discovered_hosts.txt)

echo -e "Discovered IP address:\n$hosts\n"
ipaddr=$(host $domain | grep "has address" | cut -d" " -f4 | tr "\n" " ")

# Available options
echo -e "Additional options available:"
echo -e "\t1) Identify the corresponding network range of target domain."
echo -e "\t2) Ping discovered hosts."
echo -e "\t3) All checks."
echo -e "\t*) Exit.\n"

read -p "Select your option: " opt

case $opt in
	"1") network_range ;;
	"2") ping_host ;;
	"3") network_range && ping_host ;;
	"*") exit 0 ;;
esac
```

As we can see, we have commented here several parts of the script into which we can split it.

1. Check for given arguments
2. Identify network range for the specified IP address(es)
3. Ping discovered IP address(es)
4. Identify IP address(es) of the specified domain
5. Available options

#### 1. Check for given arguments

In the first part of the script, we have an if-else statement that checks if we have specified a domain representing the target company.

#### 2. Identify network range for the specified IP address(es)

Here we have created a function that makes a "whois" query for each IP address and displays the line for the reserved network range, and stores it in the CIDR.txt.

#### 3. Ping discovered IP address(es)

This additional function is used to check if the found hosts are reachable with the respective IP addresses. With the For-Loop, we ping every IP address in the network range and count the results.

#### 4. Identify IP address(es) of the specified domain

As the first step in this script, we identify the IPv4 address of the domain returned to us.

#### 5. Available Options

Then we decide which functions we want to use to find out more information about the infrastructure.


## Conditional Execution
allows us to control the flow of our script by reaching different conditions. This function is one of the essential components. Otherwise, we could only execute one command after another.

Script.sh
```bash
#!/bin/bash

# Check for given argument
if [ $# -eq 0 ]
then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi

<SNIP>
```
In summary, this code section works with the following components:

- `#!/bin/bash` - Shebang.
- `if-else-fi` - Conditional execution.
- `echo` - Prints specific output.
- `$#` / `$0` / `$1` - Special variables.
- `domain` - Variables.

The conditions of the conditional executions can be defined using variables (`$#`, `$0`, `$1`, `domain`), values (`0`), and strings, as we will see in the next examples. These values are compared with the `comparison operators` (`-eq`)

## If-Else-Fi
Psueudo-Code
```
if [ the number of given arguments equals 0 ]
then
	Print: "You need to specify the target domain."
	Print: "<empty line>"
	Print: "Usage:"
	Print: "   <name of the script> <domain>"
	Exit the script with an error
else
	The "domain" variable serves as the alias for the given argument 
finish the if-condition
```
By default, an If-Else condition can only contain a single "if"

If-Only.sh
```
#!/bin/bash

value=$1

if [ $value -gt "10" ]
then
        echo "Given argument is greater than 10."
fi
```

execution:
```bash
@htb[/htb]$ bash if-only.sh 5

@htb[/htb]$ bash if-only.sh 12  

Given argument is greater than 10.
```

when adding Elif or Else, we add alternatives to treat specific values or statuses. if a particular value does not apply to the first case, it will be caught by the others.

If-Elif-Else.sh
```bash
#!/bin/bash

value=$1

if [ $value -gt "10" ]
then
	echo "Given argument is greater than 10."
elif [ $value -lt "10" ]
then
	echo "Given argument is less than 10."
else
	echo "Given argument is not a number."
fi
```

 If-Elif-Else.sh - Execution
```
@htb[/htb]$ bash if-elif-else.sh 5  
Given argument is less than 10.

@htb[/htb]$ bash if-elif-else.sh 12  
Given argument is greater than 10.

@htb[/htb]$ bash if-elif-else.sh HTB
if-elif-else.sh: line 5: [: HTB: integer expression expected 
if-elif-else.sh: line 8: [: HTB: integer expression expected 
Given argument is not a number.
```