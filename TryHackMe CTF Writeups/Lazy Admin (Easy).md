# Room Information

Room link: https://tryhackme.com/room/lazyadmin

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/27/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sV -sC 10.10.165.242 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-27 18:46 EDT
Nmap scan report for 10.10.165.242
Host is up (0.089s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 497cf741104373da2ce6389586f8e0f0 (RSA)
|   256 2fd7c44ce81b5a9044dfc0638c72ae55 (ECDSA)
|_  256 61846227c6c32917dd27459e29cb905e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.06 seconds
```

#
> Scanning for Hiddent Web Directories Using Dirb
4. `dirb http://{TARGET IP} > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.165.242 > WebDirectoryScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat WebDirectoryScan.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Tue Jun 27 18:53:42 2023
URL_BASE: http://10.10.165.242/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.165.242/ ----
==> DIRECTORY: http://10.10.165.242/content/              
+ http://10.10.165.242/index.html (CODE:200|SIZE:11321)        
+ http://10.10.165.242/server-status (CODE:403|SIZE:278)                                                          
---- Entering directory: http://10.10.165.242/content/ ----
==> DIRECTORY: http://10.10.165.242/content/_themes/             
==> DIRECTORY: http://10.10.165.242/content/as/               
==> DIRECTORY: http://10.10.165.242/content/attachment/       
==> DIRECTORY: http://10.10.165.242/content/images/    
==> DIRECTORY: http://10.10.165.242/content/inc/       
+ http://10.10.165.242/content/index.php (CODE:200|SIZE:2199)      
==> DIRECTORY: http://10.10.165.242/content/js/                                                                   
---- Entering directory: http://10.10.165.242/content/_themes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                               
---- Entering directory: http://10.10.165.242/content/as/ ----
+ http://10.10.165.242/content/as/index.php (CODE:200|SIZE:3678)    
==> DIRECTORY: http://10.10.165.242/content/as/js/     
==> DIRECTORY: http://10.10.165.242/content/as/lib/                                                                 
---- Entering directory: http://10.10.165.242/content/attachment/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                  
---- Entering directory: http://10.10.165.242/content/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                  
---- Entering directory: http://10.10.165.242/content/inc/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                  
---- Entering directory: http://10.10.165.242/content/js/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                  
---- Entering directory: http://10.10.165.242/content/as/js/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                  
---- Entering directory: http://10.10.165.242/content/as/lib/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Tue Jun 27 19:14:26 2023
DOWNLOADED: 13836 - FOUND: 4
```

# 
>
6. `firefox "http://10.10.165.242/content/inc/mysql_backup/"`
![]()

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

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/


