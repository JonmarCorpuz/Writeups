# Room Information

Room link: https://tryhackme.com/room/picklerick

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/26/2023

# Scanning and Enumeration
> Port Scanning Using Nmap 
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (**-sC**) and version detection (**-sV**) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.29.218 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-26 21:47 EDT
Nmap scan report for 10.10.29.218
Host is up (0.090s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 425ce2141719498f93765ab4df3162d2 (RSA)
|   256 c8c548584647f7b948b286c84d99e653 (ECDSA)
|_  256 88354fcc3adc12ae0e1e7e536be3fbb2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.83 seconds
```

##
> Scanning for Hiddend Web Directory Using Dirb
4. `dirb http://{TARGET IP} -r > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.29.218 -r > WebDirectoryScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat WebDirectoryScan.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jun 26 21:47:28 2023
URL_BASE: http://10.10.29.218/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.29.218/ ----
==> DIRECTORY: http://10.10.29.218/assets/
+ http://10.10.29.218/index.html (CODE:200|SIZE:1062)
+ http://10.10.29.218/robots.txt (CODE:200|SIZE:17) 
+ http://10.10.29.218/server-status (CODE:403|SIZE:300)

-----------------
END_TIME: Mon Jun 26 21:54:32 2023
DOWNLOADED: 4612 - FOUND: 3
```

##
> Inspecting the Home Page
6.

# Vulnerability Identification
> 

#
> Step

# Vulnerability Exploitation
> Step

#
> Step

# Post Exploitation
> Step

#
> Step

# Contibutions
> Step
