# Kali notes

Tools directory: `/usr/share`

# Basic Forensics

List users: `cat /etc/passwd`

The home directory of the users always have some interesting stuff: `/home/<username>`

## Find vulnerabilities

Exploit DB: https://www.exploit-db.com

## DirBuster

Find well-known files and directories on a web server.
Parameters:
- web server IP address and port, e.g.: `http://10.10.123.55:80/`
- dictionary file, e.g.: `/usr/share/dirbuster/dictionary/common.txt`

## LinPEAS

LinPEAS (github)
Very comprehensive and detailed command line scanning tool

## LinEnum or Enum4Linux

Find services, users, shares on the target machine

# Network

## Basics

`Ping` és `traceroute`

`Whois` fut Windows powershellből is nem csak Linux konzolon. :)

DNS query: `dig <dnsname> [@dnsserver]`

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

## netcat

Local listener to access remote shell

Example: `nc -l -p 1234`

-l -> listener
-p <port> -> listening port (where the victim should connect to)

# Metasploite

Initialize Metasploite DB:
`msfdb init`

Start Metasploite console: `msfconsole`

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

# Burp Suite

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


