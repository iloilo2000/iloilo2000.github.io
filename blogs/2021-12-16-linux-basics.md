# Own KALI Attackbox
## Tryhackme KALI VPN connect

On KALI install OpenVPN: `sudo apt install openvpn`
On the KALI linux download OVPN File: https://tryhackme.com/vpn/get-config (into the **~/Downloads** folder)
Start OpenVPN connection: `sudo openvpn /Downloads/iloilo2000.ovpn`

## SSH To the target Linux machine

On KALI open a new _Terminal_ Window
In the terminal type: `ssh tryhackme@<target_IP>` and type the password ("_tryhackme_")

### SSH with ssh_key

Before use an ssh key ensure that the key is file system protected:
`chmod 600 <ssh_key_file>`

Login with the SSH key:
`ssh -i <ssh_key_file> user@<target_IP>`

## Take some notes

Install and use **sublimetext**

## Pipe output to TXT file

`<command> | tee output.txt`

# Basic Commands

Mappa listázás: `ls`
Minden file listázása (rejtettek is): `ls -a`
Részletes file adatok listázása: `ls -lh`
Parancs rövid help: `ls --help`
Parancs MAN oldala: `man ls`

Create a file: `touch <filename>`
Create folder: `mkdir <dirname>`
Copy file or folder: `cp <file or folder> <folder>`
Move fil or folder: `mv`
Remove file: `rm <filename>`
Remove directory: `rm -R <dirname>`
Determine the type of a file: `file <filename>`
Show content of a file: `cat <filename>`
Find a file: `find <search folder> -name <filename>`

Switch user and stay in the current folder: `su <username>`
Switch user and go to its home directory: `su -l <username>`

# Root folders

**/etc**
System files of the OS. Some of the important files (`/etc$ ls`):
- Sudoers - kinek van sudo joga
- Passwd, shadow - hogyan tárolja a userek jelszavait az OS

**/var**
Az appok, szervizek által használt gyakran változó file-ok. Pl logfile-ok: */var/log*

**/root**
A "root" nevű system user home mappája

**/tmp**
Reboot esetén törlődik, minden user tudja írni.

# File editors

## nano

Create or edit a file with nano: `nano <filename>`
Exit nano: **^X** (**Ctrl + X**)

# File transfer

## wget

Download a file from the internet:
`wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt`

## scp

Transfer a file to the remote machine:
`scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`

## Start a webserver

Start a webserver
`python3 -m http.server`

# Services

List current user services: `ps`
List all user services: `ps aux`

Kill/terminate service: `kill [pid]`

Start-stop services:
`systemctl <option> <service-name>`
Options:
- Start
- Stop
- Enable (to start on boot)
- Disable

Bring a service to foreground: `fg`

# Ütemezett feladatok

Ütemezett feladatok szeresztése: `crontab -e`
