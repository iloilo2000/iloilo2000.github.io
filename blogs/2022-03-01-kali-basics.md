# Basics

**Exploit:** Kód ami kihasznál egy sebezhetőséget  
**Vulnerability:** Sebezhetőség, általában kód hiba vagy hibás konfigurációs beállítás  
**Payload:** A célgépen futó kód, amit az exploit juttat be, ez végzi a kártékony tevékenységet (adatlopás, privesc, ...)

## Kali notes

Tools directory: `/usr/share`

---

# Basic Forensics

- Name of the computer: `hostname`
- System information: `uname -a`
- Further system information in the file: `/etc/issue`
- Show processes: `ps`
  - Show all users' processes (**a**), the users started them (**u**) and the non-terminal processes (**x**): `ps aux`
- Display environment variables: `env`
- List commands your user can run with root privileges: `sudo -l`
- Directory listing with hidden files: `ls -la`
- Info about a users' privilege level and group membership: `id <username>`
- Lists users: `cat /etc/passwd`
  - Lists only the usernames: `cat /etc/passwd | cut -d ":" -f 1`
  - Lists only the real users with home dir: `cat /etc/passwd | grep home`
- Previously run commands: `history`
- Network config of the interfaces: `ifconfig`
- The home directory of the users always have some interesting stuff: `/home/<username>`

## Find vulnerabilities

Exploit DB: https://www.exploit-db.com
NVD (National Vuln DB): https://nvd.nist.gov/vuln/full-listing

## DirBuster

Find well-known files and directories on a web server.  
Parameters:
- web server IP address and port, e.g.: `http://10.10.123.55:80/`
- dictionary file, e.g.: `/usr/share/dirbuster/dictionary/common.txt`

Example: `dirb http://10.10.8.172/ /usr/share/wordlists/dirb/common.txt`

## LinPEAS

LinPEAS (github)
Very comprehensive and detailed command line scanning tool

## LinEnum or Enum4Linux

Find services, users, shares on the target machine

---

# Network

## Basics

`Ping` és `traceroute`

`Whois` fut Windows powershellből is nem csak Linux konzolon. :)

DNS query: `dig <dnsname> [@dnsserver]`

**SSH**: `ssh user@<targetIP>`

**RDP**: `rdesktop <targetIP>`

## nmap

Ultimate network scan tool:
- Port scan: `nmap -sV <TargetIP>`
- Operating system version scan `nmap -O <TargetIP>`
- Services scan

**Examples:**
Csekkolja hogy a target gép 21-es portján engedélyezett-e az FTP Anonymous logon ("-Pn" azért kell mert blokkolva a "ping probe")  
`sudo nmap --script=ftp-anon -Pn -p21 <targetIP>`

Sebezhetőségek keresése a célgépen:  
`sudo nmap -sV-vv --script vuln <targetIP>`

SYN Scan a célgép nyitott portok vizsgálatára
- -sS -> SYN Scan
- -sV -> feloldja a service neveket,
- -p- -> minden portot nézzen  
`sudo nmap -sS -sV -p- <TARGET_IP>`

Csak a gépek felderítése portscan nélkül:
- -sn -> nincs portscan
- PR -> ARP scan (vagyis csak a subneten belüli gépek válaszolnak)  
`nmap -PR -sn 10.10.10.0/24`

| Opt | Desc |
|---|---|
| -PR | ARP scan |
| -PE | ICMP Echo scan |
| -PP | ICMP Timestamp scan |
| -PM | ICMP Mask scan |
| -PS[port range] | SYN scan with optional port range (port examples: -PS21, -PS1000-2000, -PS80,443) |
| -PA[port range] | ACK scan with optional port range (port examples: -PS21, -PS1000-2000, -PS80,443) |
| -sn | No portscan |
| -n | No DNS lookup |
| -R | (reverse) DNS lookup for the on/offline hosts |

## netcat

Act as a **client** or a **server**.

- Connect to a remote server (like telnet)
- Local listener (on a remote machine) to provide access to a remote shell

Example: `nc -l -p 1234`

| Opt | Desc |
|---|---|
| -l | listen mode (server) |
| `-p <port>` | listening port (where the attacker should connect to) |
| -n | No DNS resolution |
| -v (-vv) | Verbose (Very Verbose) output |
| -k | Keep listening after client disconnects |

---

# Metasploit Framework
  
Initialize Metasploit DB:
`msfdb init`

Start Metasploit console: `msfconsole`  
Metasploit console is the interface to interact with the modules.  
Modules are small components within the Metasploit Framework.

Modules categories (under `/opt/metasploit.../modules/`):
- **Auxiliary:** Any supporting module, such as scanners, crawlers and fuzzers, can be found here.
- **Encoders:** Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.
- **Evasion:** While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software.
- **Exploits:** Exploits, neatly organized by target system.
- **NOPs:** NOPs (No OPeration) do nothing, literally.
- **Payloads:** Payloads are codes that will run on the target system.
  - Singles: Self-contained payloads that do not need to download an additional component to run.
  - Stagers: Responsible for setting up a connection channel for large (staged) payloads.
  - Stages: Downloaded by the stager. This will allow you to use larger sized payloads.
- **Post:** Post modules will be useful on the final stage of the penetration testing process listed above, post-exploitation.

Metasploit works with variables which can be global (`setg <all|variable-name>`) or module-local (`set <all|variable-name>`). By default the variables are local so when a module is changed, the variable should be set again. (e.g.: `set RHOSTS`)  
Some important variables:
- RHOSTS: Remote host (target machine)
- RPORT: Remote, vulnerable port on the target machine
- LHOST: Local IP address (of the Kali machine)
- LPORT: Local port, the reverse shell will connect to
- PAYLOAD: Payload used by the exploit
- SESSION: Session ID of an open connection, usually used by post-exploitation

To clear a variable, type: `unset <all|variable-name>` or `unsetg <all|variable-name>`
To select (or change) a module, type: `use <module-name>`  
To search a module, type: `search <part-of-the-module-name>`  
After a search you can select a module by its search number (#): `use 12`  
To see the variables of the module, type: `show options`  
To leave the context, type: `back`  
To have further info about the current module: `info`  
- **Rank:** Exploits are rated based on their reliability. A low-ranking exploit may work perfectly, and an excellent ranked exploit may not, or worse, crash the target system.

Launch the exploit with command `exploit` or `run`  
To background the session, start it with `exploit -z` or if it is already started, with the command `background` or `Ctrl+Z`  
The session then will have a session ID. To list the sessions, type: `sessions`  
To interact a session, type `sessions -i <session-number>`

Help: `msfconsole -h`

More detailed help is inside the console:
msf6>`help`

Main Steps to use Metasploit:
- Search vulnerability with NMAP
- Start Metasploit (`sudo msfconsole`)
- Search the MSF Module name for the vulnerability (`search icecast`)
- Use Module (`use <module #>`)
- Get Info about the Module (`show options`)
- Set the value of some variables (LHOST, RHOSTS, payload)
- Run (`exploit`) the Module
- The shell is turning from **msf6>** to **meterpreter>**
  - Help > some other useful commands (sysinfo, getuid, ...)
- Post Module to get shell or find some other exploits
- Background current session with **Ctrl + Z**
- You can check backgrounded sessions: `sessions`
- Run (`exploit`) the recently found Module
  - Switch Session 2
  - Dump password hashes: `hashdump`
  - Run Mimikatz: `load kiwi`
  - Get credentials and steal logged on passwords: `creds_all`, `creds_kerberos`, ...
- Convert the shell to Meterpreter
- Capture the flags :-)

Upgrade normal shell to **meterpreter** shell:
https://infosecwriteups.com/metasploit-upgrade-normal-shell-to-meterpreter-shell-2f09be895646

Other MSF commands:
- `history`: list of the previous commands

---

# Crack passwords

## hashcat

Use **hashcat** to crack a password hash:
_-m 1000_ --> Hash type is NTLM
_-a 0_ --> Attack type: Dictionary (disctionary file is **rockyou.txt**)
`hashcat -m 1000 -a 0 hash.txt rockyou.txt`

## hydra

Usage: `hydra -l <username> -P <password list> <target> [-t <number of threads>]`
* Example1: `hydra -l Root -P /usr/share/wordlists/rockyou.txt ssh://10.10.123.55`
* Example2: `hydra -l username -P passlist.txt ftp://10.10.95.142`
* Example3: `hydra -l username -P passlist.txt 10.10.95.142 -t 4 ssh`

To bruteforce a web form go to the browser **developer tools/network** tab and check the request method (usually GET or POST).

Example4: `hydra -l username -P passlist.txt 10.10.95.142 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V`

|opt|desc|
|---|---|
| -L | User name |
| -P | Password list |
| IP address | target computer or service |
| http-post-form | tells it is a POST request |
| /login:username...&password... | the form url (http://10.10.10.10/login) and the names of the  login and pass variables |
| ^USER^ ^PASS^ | tells that where to append the user and password (the actual values come from -L and -P) |
| F=incorrect | Failed login if the answer contains "incorrect" |
| -V | verbose log |

## john the ripper

1. create/download/copy RSA Key file
2. Make key_txt for JohnTheRipper with `ssh2john`
3. Brute force the key password with `john`

## Crack password online

https://crackstation.net/

---

# Web Site Hacking

## OWASP Juice Shop

Web based OWASP Training site with 90 different challenges:  
https://owasp.org/www-project-juice-shop/

## Reconnaissance

OVASP Favicon Database: https://wiki.owasp.org/index.php/OWASP_favicon_database

Find technologies on a website: https://www.wappalyzer.com/

## Burp Suite

Three versions: Community (free), Pro, Enterprise
Community version cannot save projects, all setting reset after every restart.

- Start the GUI
- Configure **proxy** IP address
- Intercept web traffic on the proxy tab
  - Attack login page with password dictionary
- Target / Scope: The tartget web traffic can be filtered
- Send the captured data to Intruder
- Send the captured data to Repeater
  - You can modify header and other parameters and send the modified request to the web server

---

# Windows Exploitation

## Powerview

Start powerview: `. .\poweview.ps1` (note! the command start with two dots with a space)

