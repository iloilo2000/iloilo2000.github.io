# Kali specific

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

Usage: `hydra -l <username> -P <password list> <target>`
Example: `hydra -l Root -P /usr/share/wordlists/rockyou.txt ssh://10.10.123.55`

## john the ripper

1. create/download/copy RSA Key file
2. Make key_txt for JohnTheRipper with `ssh2john`
3. Brute force the key password with `john`

## Crack password online

https://crackstation.net/

# Burp Suite

Three version: Community (free), Pro, Enterprise
Community version cannot save projects, the setting reset after every restart.

- Start the GUI
- Configure **proxy**
- Intercept web traffic
  - Attack login page with password dictionary
- Target / Scope: The tartget web traffic can be filtered

