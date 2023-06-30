# Room Information

Room link: https://tryhackme.com/room/bolt

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/29/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.195.64 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt        
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-29 19:36 EDT
Nmap scan report for 10.10.195.64
Host is up (0.090s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f385ec54f201b19440de42e821972080 (RSA)
|   256 77c7c1ae314121e4930e9add0b29e1ff (ECDSA)
|_  256 070543469db23ef04d6967e491d3d37f (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
8000/tcp open  http    (PHP 7.2.32-1)
|_http-title: Bolt | A hero is unleashed
|_http-generator: Bolt
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 Not Found
|     Date: Thu, 29 Jun 2023 23:36:30 GMT
|     Connection: close
|     X-Powered-By: PHP/7.2.32-1+ubuntu18.04.1+deb.sury.org+1
|     Cache-Control: private, must-revalidate
|     Date: Thu, 29 Jun 2023 23:36:30 GMT
|     Content-Type: text/html; charset=UTF-8
|     pragma: no-cache
|     expires: -1
|     X-Debug-Token: a3477f
|     <!doctype html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <title>Bolt | A hero is unleashed</title>
|     <link href="https://fonts.googleapis.com/css?family=Bitter|Roboto:400,400i,700" rel="stylesheet">
|     <link rel="stylesheet" href="/theme/base-2018/css/bulma.css?8ca0842ebb">
|     <link rel="stylesheet" href="/theme/base-2018/css/theme.css?6cb66bfe9f">
|     <meta name="generator" content="Bolt">
|     </head>
|     <body>
|     href="#main-content" class="vis
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Date: Thu, 29 Jun 2023 23:36:29 GMT
|     Connection: close
|     X-Powered-By: PHP/7.2.32-1+ubuntu18.04.1+deb.sury.org+1
|     Cache-Control: public, s-maxage=600
|     Date: Thu, 29 Jun 2023 23:36:29 GMT
|     Content-Type: text/html; charset=UTF-8
|     X-Debug-Token: de258a
|     <!doctype html>
|     <html lang="en-GB">
|     <head>
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <title>Bolt | A hero is unleashed</title>
|     <link href="https://fonts.googleapis.com/css?family=Bitter|Roboto:400,400i,700" rel="stylesheet">
|     <link rel="stylesheet" href="/theme/base-2018/css/bulma.css?8ca0842ebb">
|     <link rel="stylesheet" href="/theme/base-2018/css/theme.css?6cb66bfe9f">
|     <meta name="generator" content="Bolt">
|     <link rel="canonical" href="http://0.0.0.0:8000/">
|     </head>
|_    <body class="front">
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8000-TCP:V=7.93%I=7%D=6/29%Time=649E157B%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,29E1,"HTTP/1\.0\x20200\x20OK\r\nDate:\x20Thu,\x2029\x20Jun\x20
SF:2023\x2023:36:29\x20GMT\r\nConnection:\x20close\r\nX-Powered-By:\x20PHP
SF:/7\.2\.32-1\+ubuntu18\.04\.1\+deb\.sury\.org\+1\r\nCache-Control:\x20pu
SF:blic,\x20s-maxage=600\r\nDate:\x20Thu,\x2029\x20Jun\x202023\x2023:36:29
SF:\x20GMT\r\nContent-Type:\x20text/html;\x20charset=UTF-8\r\nX-Debug-Toke
SF:n:\x20de258a\r\n\r\n<!doctype\x20html>\n<html\x20lang=\"en-GB\">\n\x20\
SF:x20\x20\x20<head>\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20charset=\"u
SF:tf-8\">\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20name=\"viewport\"\x20
SF:content=\"width=device-width,\x20initial-scale=1\.0\">\n\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20<title>Bolt\x20\|\x20A
SF:\x20hero\x20is\x20unleashed</title>\n\x20\x20\x20\x20\x20\x20\x20\x20<l
SF:ink\x20href=\"https://fonts\.googleapis\.com/css\?family=Bitter\|Roboto
SF::400,400i,700\"\x20rel=\"stylesheet\">\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0<link\x20rel=\"stylesheet\"\x20href=\"/theme/base-2018/css/bulma\.css\
SF:?8ca0842ebb\">\n\x20\x20\x20\x20\x20\x20\x20\x20<link\x20rel=\"styleshe
SF:et\"\x20href=\"/theme/base-2018/css/theme\.css\?6cb66bfe9f\">\n\x20\x20
SF:\x20\x20\t<meta\x20name=\"generator\"\x20content=\"Bolt\">\n\x20\x20\x2
SF:0\x20\t<link\x20rel=\"canonical\"\x20href=\"http://0\.0\.0\.0:8000/\">\
SF:n\x20\x20\x20\x20</head>\n\x20\x20\x20\x20<body\x20class=\"front\">\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20<a\x20")%r(FourOhFourRequest,16C3,"HTTP/1
SF:\.0\x20404\x20Not\x20Found\r\nDate:\x20Thu,\x2029\x20Jun\x202023\x2023:
SF:36:30\x20GMT\r\nConnection:\x20close\r\nX-Powered-By:\x20PHP/7\.2\.32-1
SF:\+ubuntu18\.04\.1\+deb\.sury\.org\+1\r\nCache-Control:\x20private,\x20m
SF:ust-revalidate\r\nDate:\x20Thu,\x2029\x20Jun\x202023\x2023:36:30\x20GMT
SF:\r\nContent-Type:\x20text/html;\x20charset=UTF-8\r\npragma:\x20no-cache
SF:\r\nexpires:\x20-1\r\nX-Debug-Token:\x20a3477f\r\n\r\n<!doctype\x20html
SF:>\n<html\x20lang=\"en\">\n\x20\x20\x20\x20<head>\n\x20\x20\x20\x20\x20\
SF:x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0<meta\x20name=\"viewport\"\x20content=\"width=device-width,\x20initial
SF:-scale=1\.0\">\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20<title>Bolt\x20\|\x20A\x20hero\x20is\x20unleashed</title>\n\x2
SF:0\x20\x20\x20\x20\x20\x20\x20<link\x20href=\"https://fonts\.googleapis\
SF:.com/css\?family=Bitter\|Roboto:400,400i,700\"\x20rel=\"stylesheet\">\n
SF:\x20\x20\x20\x20\x20\x20\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"/
SF:theme/base-2018/css/bulma\.css\?8ca0842ebb\">\n\x20\x20\x20\x20\x20\x20
SF:\x20\x20<link\x20rel=\"stylesheet\"\x20href=\"/theme/base-2018/css/them
SF:e\.css\?6cb66bfe9f\">\n\x20\x20\x20\x20\t<meta\x20name=\"generator\"\x2
SF:0content=\"Bolt\">\n\x20\x20\x20\x20</head>\n\x20\x20\x20\x20<body>\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20<a\x20href=\"#main-content\"\x20class=\"v
SF:is");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.54 seconds
```

#
> Scanning for Hiddent Web Directories Using Dirb
4. `dirb http://{TARGET IP} > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.195.64:8000 -r > WebDirectoryScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat WebDirectoryScan.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Jun 29 19:37:01 2023
URL_BASE: http://10.10.195.64:8000/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.195.64:8000/ ----
+ http://10.10.195.64:8000/.htaccess (CODE:200|SIZE:2956)     
+ http://10.10.195.64:8000/entries (CODE:200|SIZE:6661)      
+ http://10.10.195.64:8000/index.php (CODE:200|SIZE:0)      
+ http://10.10.195.64:8000/pages (CODE:200|SIZE:4991)        
+ http://10.10.195.64:8000/search (CODE:200|SIZE:5550)                                                                                                                         
-----------------
END_TIME: Thu Jun 29 19:53:38 2023
DOWNLOADED: 4612 - FOUND: 5
```

#
>
6. `firefox "http://10.10.195.64:8000"`
```bash
┌──(kali㉿kali)-[~]
└─$ firefox "http://10.10.195.64:8000"
```

![]()

7. `firefox "http://10.10.195.64:8000/entries"`
```bash
┌──(kali㉿kali)-[~]
└─$ firefox "http://10.10.195.64:8000/entries"
```

![]()

8. `firefox "http://10.10.195.64:8000/login.php"`
```bash
┌──(kali㉿kali)-[~]
└─$ firefox "http://10.10.195.64:8000/login.php"
```

![]()

9. `firefox "http://10.10.195.64:8000/pages"`
```bash
┌──(kali㉿kali)-[~]
└─$ firefox "http://10.10.195.64:8000/pages"
```

![]()

10. `firefox "http://10.10.195.64:8000/search"`
```bash
┌──(kali㉿kali)-[~]
└─$ firefox "http://10.10.195.64:8000/search"
```

![]()

11. `https://boltcms.io`

![]()

12. `https://docs.boltcms.io/5.0/manual/login`

![]()

13. `firefox "http://10.10.195.64:8000/bolt"` and sign in using the Jake's set of credentials that he mentioned (bolt:bolt1admin23)

![]()

![]()

# Vulnerability Identification
> Vulnerability Identification Using Metasploit
14. `msfconsole`
15. `search Bolt 3.7`

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/


